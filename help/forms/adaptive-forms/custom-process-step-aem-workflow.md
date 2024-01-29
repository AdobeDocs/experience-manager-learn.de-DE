---
title: Implementieren eines benutzerdefinierten Prozessschritts
description: Schreiben von adaptiven Formularanlagen in das Dateisystem mithilfe eines benutzerdefinierten Prozessschritts
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 226
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '769'
ht-degree: 100%

---

# Benutzerdefinierter Prozessschritt

Dieses Tutorial richtet sich an AEM Forms-Kundinnen und -Kunden, die einen benutzerdefinierten Prozessschritt implementieren müssen. Ein Prozessschritt kann ein ECMA-Skript ausführen oder benutzerdefinierten Java-Code aufrufen, um Vorgänge auszuführen. In diesem Tutorial werden die Schritte erläutert, die zur Implementierung des vom Prozessschritt ausgeführten WorkflowProcess erforderlich sind.

Der Hauptgrund für die Implementierung eines benutzerdefinierten Prozessschritts besteht darin, den AEM-Workflow zu erweitern. Wenn Sie beispielsweise AEM Forms-Komponenten in Ihrem Workflow-Modell verwenden, sollten Sie die folgenden Vorgänge durchführen:

* Speichern der Anhänge eines adaptiven Formulars im Dateisystem
* Ändern der übermittelten Daten

Für den oben genannten Anwendungsfall schreiben Sie normalerweise einen OSGi-Dienst, der vom Prozessschritt ausgeführt wird.

## Erstellen eines Maven-Projekts

Der erste Schritt besteht darin, ein Maven-Projekt mit dem entsprechenden Adobe-Maven-Archetyp zu erstellen. Die detaillierten Schritte finden Sie in diesem [Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=de). Sobald Sie Ihr Maven-Projekt in Eclipse importiert haben, können Sie mit dem Schreiben Ihrer ersten OSGi-Komponente beginnen, die in Ihrem Prozessschritt verwendet werden kann.


### Erstellen einer Klasse, die WorkflowProcess implementiert

Öffnen Sie das Maven-Projekt in Ihrer Eclipse-IDE. Erweitern Sie den Ordner **Projektname** > **core**.  Erweitern Sie den Ordner „src/main/java“. Sie sollten ein Paket sehen, das mit „core“ endet. Erstellen Sie eine Java-Klasse, die WorkflowProcess in diesem Paket implementiert. Sie müssen die Ausführungsmethode überschreiben. Die Signatur der execute-Methode lautet wie folgt:
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException
Die execute-Methode gibt Zugriff auf die drei folgenden Variablen:

**WorkItem**: Die workItem-Variable gibt Zugriff auf Daten im Zusammenhang mit dem Workflow. Die Dokumentation zur öffentlichen API ist [hier](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html) verfügbar.

**WorkflowSession**: Diese workflowSession-Variable bietet Ihnen die Möglichkeit, den Workflow zu steuern. Die Dokumentation zur öffentlichen API ist [hier](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html) verfügbar.

**MetaDataMap**: Alle mit dem Workflow verknüpften Metadaten. Alle Prozessargumente, die an den Prozessschritt weitergegeben werden, sind über das MetaDataMap-Objekt verfügbar.[API-Dokumentation](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In diesem Tutorial werden wir die Anlagen schreiben, die dem adaptiven Formular als Teil des AEM-Workflows zum Dateisystem hinzugefügt wurden.

Um diesen Anwendungsfall durchzuführen, wurde die folgende Java-Klasse geschrieben:

Sehen wir uns diesen Code an

```java
package com.learningaemforms.adobe.core;

import java.io.File;
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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
        map.put("path", payloadPath + "/" + attachmentsPath);
        File saveLocationFolder = new File(saveToLocation);
        if (!saveLocationFolder.exists()) {
            saveLocationFolder.mkdirs();
        }

        map.put("type", "nt:file");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
        query.setStart(0);
        query.setHitsPerPage(20);

        SearchResult result = query.getResult();
        log.debug("Got  " + result.getHits().size() + " attachments ");
        Node attachmentNode = null;
        for (Hit hit: result.getHits()) {
            try {
                String path = hit.getPath();
                log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
                attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
                InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                Document attachmentDoc = new Document(documentStream);
                attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
                attachmentDoc.close();
            } catch (Exception e) {
                log.debug("Error saving file " + e.getMessage());
            }
```

Zeile 1: Definiert die Eigenschaften für unsere Komponente. Die Eigenschaft „process.label“ wird angezeigt, wenn Sie die OSGi-Komponente mit dem Prozessschritt verknüpfen, wie in einem der folgenden Screenshots dargestellt.

Zeilen 13–15: Die an diese OSGi-Komponente weitergegebenen Prozessargumente werden mithilfe eines Trennzeichens (&quot;,&quot;) getrennt. Die Werte für „attachmentPath“ und „saveToLocation“ werden dann aus dem Zeichenfolgen-Array extrahiert.

* attachmentPath: Dies ist derselbe Speicherort, den Sie im adaptiven Formular angegeben haben, als Sie die Übermittlungsaktion für das adaptive Formular zum Aufruf des AEM-Workflows konfiguriert haben. Dies ist der Name des Ordners, in dem die Anhänge in AEM gespeichert werden sollen, bezogen auf die Payload des Workflows.

* saveToLocation: Dies ist der Speicherort, an dem die Anhänge im Dateisystem Ihres AEM-Servers gespeichert werden sollen.

Diese beiden Werte werden als Prozessargumente weitergeben, wie im folgenden Screenshot dargestellt.

![Prozessschritt](assets/implement-process-step.gif)

Der QueryBuilder-Dienst wird zum Abfragen von Knoten des Typs „nt:file“ im Ordner „attachmentsPath“ verwendet. Der Rest des Codes durchläuft die Suchergebnisse, um das Dokumentobjekt zu erstellen und im Dateisystem zu speichern


>[!NOTE]
>
>Da wir ein AEM Forms-spezifisches Dokumentobjekt verwenden, müssen Sie die Abhängigkeit „aemfd-client-sdk“ in Ihr Maven-Projekt einschließen. Die Gruppen-ID lautet „com.adobe.aemfd“ und die Artefakt-ID „aemfd-client-sdk“.

#### Erstellen und Bereitstellen

[Erstellen Sie das Bundle wie hier beschrieben.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=de)
[Stellen Sie sicher, dass das Bundle bereitgestellt ist und sich in einem aktiven Status befindet.](http://localhost:4502/system/console/bundles)

Erstellen Sie ein Workflow-Modell. Ziehen Sie den Prozessschritt in das Workflow-Modell. Verknüpfen Sie den Prozessschritt mit dem Vorgang zum Speichern von Anhängen adaptiver Formulare im Dateisystem.

Geben Sie die erforderlichen Prozessargumente getrennt durch Kommas an, z. B. „Attachments,c:\\scrappp\\“. Das erste Argument bezeichnet den Ordner, in dem Ihre adaptiven Formularanhänge relativ zur Workflow-Payload gespeichert werden. Dieser Wert muss mit dem Wert übereinstimmen, den Sie beim Konfigurieren der Übermittlungsaktion des adaptiven Formulars angegeben haben. Das zweite Argument bezeichnet den Speicherort, an dem die Anhänge gespeichert werden sollen.

Erstellen eines adaptiven Formulars. Ziehen Sie die Komponente „Dateianhänge“ in das Formular. Konfigurieren Sie die Übermittlungsaktion des Formulars, um den in den vorherigen Schritten erstellten Workflow aufzurufen. Geben Sie den entsprechenden Anhangspfad an.

Speichern Sie die Einstellungen.

Zeigen Sie das Formular in einer Vorschau an. Fügen Sie einige Anhänge hinzu und übermitteln Sie das Formular. Die Anhänge sollten im Dateisystem an dem Speicherort gespeichert werden, den Sie im Workflow angegeben haben.
