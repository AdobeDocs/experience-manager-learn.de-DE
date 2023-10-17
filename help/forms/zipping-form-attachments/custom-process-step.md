---
title: Benutzerdefinierter Prozessschritt zum Komprimieren von Dateianlagen
description: Benutzerdefinierter Prozessschritt zum Hinzufügen der adaptiven Formularanhänge zu einer Zip-Datei und Speichern der Zip-Datei in einer Workflow-Variablen
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: 1131dca8-882d-4904-8691-95468fb708b7
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: ht
source-wordcount: '159'
ht-degree: 100%

---

# Benutzerdefinierter Prozessschritt


Ein benutzerdefinierter Prozessschritt wurde implementiert, um die ZIP-Datei mit den Formularanlagen zu erstellen. Wenn Sie mit der Erstellung von OSGi-Bundles nicht vertraut sind, folgen Sie bitte [der folgenden Anleitung](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=de)

Der Code im benutzerdefinierten Prozessschritt führt Folgendes aus:

* Erstellt Abfragen aller Anlagen des adaptiven Formulars im Payload-Ordner Übergibt den Ordnernamen als Prozessargument an den Prozessschritt

* Erstellt eine Zip-Datei mit den Formularanhängen und speichert sie im Payload-Ordner
* Legt den Wert der Workflow-Variablen (no_of_attachments) fest





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
> Bitte stellen Sie sicher, dass Sie eine Variable namens *no_of_attachments* vom Typ „Double“ in Ihrem Workflow haben, damit dieser Code funktioniert.

## Nächste Schritte

[Füllen von ArrayList-Workflow-Variablen mit Anlagen und Anlagennamen](./custom-process-step.md)
