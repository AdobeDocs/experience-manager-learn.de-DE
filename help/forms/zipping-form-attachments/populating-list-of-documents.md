---
title: Benutzerdefinierter Prozessschritt zum Auffüllen von Listenvariablen
description: Erfahren Sie, wie Sie einen benutzerdefinierten Prozessschritt erstellen, um Listenvariablen vom Typ „Dokument“ und „Zeichenfolge“ in Adobe Experience Manager aufzufüllen.
feature: Workflow
topic: Development
version: Experience Manager 6.5
role: Developer
level: Beginner
kt: kt-8063
exl-id: 09d9eabf-4815-4159-b6c7-cf2ebc8a2df5
duration: 68
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '170'
ht-degree: 100%

---


# Benutzerdefinierter Prozessschritt

Dieses Handbuch führt Sie durch die Erstellung eines benutzerdefinierten Prozessschritts zum Auffüllen von Listenvariablen vom Typ „Array-Liste“ mit Anhängen und Namen von Anhängen in Adobe Experience Manager. Diese Variablen sind für die Workflow-Komponente „E-Mail senden“ von entscheidender Bedeutung.

Wenn Sie mit der Erstellung von OSGi-Bundles nicht vertraut sind, folgen Sie bitte der folgenden [Anleitung](https://experienceleague.adobe.com/de/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle).

Der Code im benutzerdefinierten Prozessschritt führt folgende Aktionen aus:

1. Erstellt Abfragen aller Anhänge des adaptiven Formulars im Ordner „Payload“. Übergibt den Ordnernamen als Prozessargument an den Schritt.
2. Füllt die Workflow-Variable `listOfDocuments` aus.
3. Füllt die Workflow-Variable `attachmentNames` aus.
4. Legt den Wert der Workflow-Variablen `no_of_attachments` fest.

```java
package com.aemforms.formattachments.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
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
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=PopulateListOfDocuments",
        "process.label=PopulateListOfDocuments"
})
public class PopulateListOfDocuments implements WorkflowProcess {

        private static final Logger log = LoggerFactory.getLogger(PopulateListOfDocuments.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path is" + payloadPath);
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
                int no_of_attachments = result.getHits().size();
                Document[] listOfDocuments = new Document[no_of_attachments];
                String[] attachmentNames = new String[no_of_attachments];
                int i = 0;
                for (Hit hit: result.getHits()) {
                        try {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                listOfDocuments[i] = new Document(attachmentStream);
                                attachmentNames[i] = new String(hit.getTitle());
                                log.debug("Added " + hit.getTitle() + "to the list");
                                i++;
                        } catch (Exception e) {
                                log.error("Unable to obtain attachment", e);
                        }
                }

                metaDataMap.put("no_of_attachments", no_of_attachments);
                metaDataMap.put("listOfDocuments", listOfDocuments);
                metaDataMap.put("attachmentNames", attachmentNames);

                log.debug("Updated workflow");
        }

}
```

>[!NOTE]
>
> Stellen Sie sicher, dass die folgenden Variablen in Ihrem Workflow definiert sind, damit der Code funktioniert:
> 
> - `listOfDocuments`: Variable vom Typ „ArrayList of Documents“
> - `attachmentNames`: Variable vom Typ „ArrayList of String“
> - `no_of_attachments`: Variable vom Typ „Double“

## Nächste Schritte

[Testen der Lösung auf Ihrem lokalen System](./test.md)