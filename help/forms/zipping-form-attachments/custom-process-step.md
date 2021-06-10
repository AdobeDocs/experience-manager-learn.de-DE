---
title: Benutzerdefinierter Prozessschritt zum ZIP von Dateianlagen
description: Benutzerdefinierter Prozessschritt zum Hinzufügen der Anlagen des adaptiven Formulars zu einer ZIP-Datei und zum Speichern der ZIP-Datei in einer Workflow-Variablen
feature: Workflow
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 1%

---


# Benutzerdefinierter Prozessschritt


Ein benutzerdefinierter Prozessschritt wurde implementiert, um die ZIP-Datei mit den Formularanlagen zu erstellen. Wenn Sie nicht mit dem Erstellen eines OSGi-Bundles vertraut sind, befolgen Sie diese Anweisungen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)[

Der Code im benutzerdefinierten Prozessschritt führt Folgendes aus:

* Abfragen aller Anlagen des adaptiven Formulars im Payload-Ordner. Der Ordnername wird als Prozessargument an den Prozessschritt übergeben.

* Erstellen Sie eine ZIP-Datei mit den Formularanlagen und speichern Sie sie im Payload-Ordner.
* Wert der Workflow-Variablen festlegen (no_of_attachments)





```java
 package com.aemforms.formattachments.core;
import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;

import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit
import com.day.cq.search.result.SearchResult;


@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})


public class ZipFormAttachments implements WorkflowProcess {

	 private static final Logger log = LoggerFactory.getLogger(ZipFormAttachments.class);
     @Reference
     QueryBuilder queryBuilder;

     @Override
     public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
     {
             String payloadPath = workItem.getWorkflowData().getPayload().toString();
             log.debug("The payload path  is" + payloadPath);
             MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
             Session session = workflowSession.adaptTo(Session.class);
             Map < String, String > map = new HashMap < String, String > ();
             map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
             map.put("type", "nt:file");
             Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
             query.setStart(0);
             query.setHitsPerPage(20);
             SearchResult result = query.getResult();
             log.debug("Get result hits " + result.getHits().size());
             ByteArrayOutputStream baos = new ByteArrayOutputStream();
             ZipOutputStream zipOut = new ZipOutputStream(baos);
             int no_of_attachments = result.getHits().size();
             for (Hit hit: result.getHits())
             {
                     try
                     {
                             String attachmentPath = hit.getPath();
                             log.debug("The hit path is" + hit.getPath());
                             Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                             InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                             ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                             int nRead;
                             byte[] data = new byte[1024];
                             while ((nRead = attachmentStream.read(data, 0, data.length)) != -1)
                             {
                                     buffer.write(data, 0, nRead);
                             }

                             buffer.flush();
                             byte[] byteArray = buffer.toByteArray();
                             ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                             zipOut.putNextEntry(zipEntry);
                             zipOut.write(byteArray);
                             zipOut.closeEntry();

                     } 
                     catch (Exception e)
                     {
                             log.debug("The error message is " + e.getMessage());
                     }
             }
             try
             {
                    zipOut.close();
                    Node payloadNode = session.getNode(payloadPath);
                    Node zippedFileNode =  payloadNode.addNode("zipped_attachments.zip", "nt:file");
                    javax.jcr.Node resNode = zippedFileNode.addNode("jcr:content", "nt:resource");
        		
        			ValueFactory valueFactory = session.getValueFactory();
        			Document zippedDocument = new Document(baos.toByteArray());

        			Binary contentValue = valueFactory.createBinary(zippedDocument.getInputStream());
        			metaDataMap.put("no_of_attachments", no_of_attachments);

                    workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                    log.debug("Updated workflow");
        			resNode.setProperty("jcr:data", contentValue);
        			session.save();
        			zippedDocument.close();



             }
             catch (IOException | RepositoryException e)
             {
                     
                     log.error("Error in closing zipout", e);
             }
             
            
             

     }

}
```

>[!NOTE]
>
> Stellen Sie sicher, dass in Ihrem Workflow eine Variable namens *no_of_attachments* vom Typ Double vorhanden ist, damit dieser Code funktioniert.

