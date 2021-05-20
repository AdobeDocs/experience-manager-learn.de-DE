---
title: Implementieren eines benutzerdefinierten Prozessschritts
seo-title: Implementieren eines benutzerdefinierten Prozessschritts
description: Schreiben von adaptiven Formularanlagen in das Dateisystem mithilfe eines benutzerdefinierten Prozessschritts
seo-description: Schreiben von adaptiven Formularanlagen in das Dateisystem mithilfe eines benutzerdefinierten Prozessschritts
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 1%

---


# Benutzerdefinierter Prozessschritt

Dieses Tutorial richtet sich an AEM Forms-Kunden, die benutzerdefinierte Prozessschritte implementieren müssen. Ein Prozessschritt kann ein ECMA-Skript ausführen oder benutzerdefinierten Java-Code aufrufen, um Vorgänge auszuführen. In diesem Tutorial werden die Schritte erläutert, die zur Implementierung von WorkflowProcess erforderlich sind, der vom Prozessschritt ausgeführt wird.

Der Hauptgrund für die Implementierung eines benutzerdefinierten Prozessschritts besteht darin, den AEM Workflow zu erweitern. Wenn Sie beispielsweise AEM Forms-Komponenten in Ihrem Workflow-Modell verwenden, sollten Sie die folgenden Vorgänge durchführen

* Speichern Sie die Anlagen des adaptiven Formulars im Dateisystem
* Bearbeiten der gesendeten Daten

Um den oben genannten Anwendungsfall durchzuführen, schreiben Sie normalerweise einen OSGi-Dienst, der vom Prozessschritt ausgeführt wird.

## Maven-Projekt erstellen

Der erste Schritt besteht darin, ein Maven-Projekt mit dem entsprechenden Adobe-Maven-Archetyp zu erstellen. Die detaillierten Schritte finden Sie in diesem [Artikel](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en). Sobald Sie Ihr Maven-Projekt in Eclipse importiert haben, können Sie mit dem Schreiben Ihrer ersten OSGi-Komponente beginnen, die in Ihrem Prozessschritt verwendet werden kann.


### Erstellen einer Klasse, die WorkflowProcess implementiert

Öffnen Sie das Maven-Projekt in Ihrer Eclipse IDE. Erweitern Sie den Ordner **Projektname** > **core** . Erweitern Sie den Ordner src/main/java . Sie sollten ein Paket sehen, das mit &quot;core&quot;endet. Erstellen Sie eine Java-Klasse, die WorkflowProcess in diesem Paket implementiert. Sie müssen die Ausführungsmethode überschreiben. Die Signatur der execute-Methode lautet wie folgt
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException
Die execute -Methode bietet Zugriff auf die folgenden 3 Variablen

**WorkItem**: Die Variable &quot;workItem&quot;gibt Zugriff auf Daten im Zusammenhang mit dem Workflow. Die öffentliche API-Dokumentation ist [hier verfügbar.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Diese Variable workflowSession bietet Ihnen die Möglichkeit, den Workflow zu steuern. Die öffentliche API-Dokumentation ist [hier](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html) verfügbar.

**MetaDataMap**: Alle mit dem Workflow verknüpften Metadaten. Alle Prozessargumente, die an den Prozessschritt übergeben werden, sind mit dem MetaDataMap -Objekt verfügbar.[API-Dokumentation](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

In diesem Tutorial werden wir die Anlagen schreiben, die dem adaptiven Formular als Teil des AEM-Workflows zum Dateisystem hinzugefügt wurden.

Um diesen Anwendungsfall durchzuführen, wurde die folgende Java-Klasse geschrieben

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

Zeile 1 - definiert die Eigenschaften für unsere Komponente. Die Eigenschaft &quot;process.label&quot;wird angezeigt, wenn Sie die OSGi-Komponente mit dem Prozessschritt verknüpfen, wie in einem der folgenden Screenshots dargestellt.

Zeilen 13-15 - Die an diese OSGi-Komponente übergebenen Prozessargumente werden mithilfe des Trennzeichens &quot;,&quot;aufgeteilt. Die Werte für attachmentPath und saveToLocation werden dann aus dem String-Array extrahiert.

* attachmentPath - Dies ist der gleiche Speicherort, den Sie beim Konfigurieren der Sendeaktion für das adaptive Formular für den Aufruf AEM Workflows im adaptiven Formular angegeben haben. Dies ist ein Name des Ordners, in dem die Anlagen AEM der Payload des Workflows gespeichert werden sollen.

* saveToLocation - Dies ist der Speicherort, an dem die Anlagen im Dateisystem Ihres AEM-Servers gespeichert werden sollen.

Diese beiden Werte werden als Prozessargumente übergeben, wie im folgenden Screenshot gezeigt.

![ProcessStep](assets/implement-process-step.gif)

Der QueryBuilder-Dienst wird zum Abfragen von Knoten des Typs nt:file im Ordner attachmentsPath verwendet. Der Rest des Codes durchläuft die Suchergebnisse, um das Dokumentobjekt zu erstellen und im Dateisystem zu speichern


>[!NOTE]
>
>Da wir ein AEM Forms-spezifisches Dokumentobjekt verwenden, müssen Sie die aemfd-client-sdk-Abhängigkeit in Ihr Maven-Projekt einbeziehen. Die Gruppen-ID lautet com.adobe.aemfd und die Artefakt-ID aemfd-client-sdk.

#### Erstellen und Bereitstellen

[Erstellen Sie das Bundle wie ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[hier beschrieben. Stellen Sie sicher, dass das Bundle bereitgestellt ist und sich im aktiven Status befindet.](http://localhost:4502/system/console/bundles)

Workflow-Modell erstellen. Ziehen Sie den Prozessschritt in das Workflow-Modell. Verknüpfen Sie den Prozessschritt mit &quot;Speichern von adaptiven Formularanlagen im Dateisystem&quot;.

Geben Sie die erforderlichen Prozessargumente getrennt durch Kommas an. Beispiel: Anlagen, c:\\scrappp\\. Das erste Argument ist der Ordner, in dem Ihre adaptiven Formularanlagen relativ zur Workflow-Nutzlast gespeichert werden. Dieser Wert muss mit dem Wert übereinstimmen, den Sie beim Konfigurieren der Sendeaktion des adaptiven Formulars angegeben haben. Das zweite Argument ist der Speicherort, an dem die Anlagen gespeichert werden sollen.

Erstellen Sie ein adaptives Formular. Ziehen Sie die Komponente Dateianlagen in das Formular. Konfigurieren Sie die Sendeaktion des Formulars, um den in den vorherigen Schritten erstellten Workflow aufzurufen. Geben Sie den entsprechenden Anlagenpfad an.

Speichern Sie die Einstellungen.

Vorschau des Formulars Fügen Sie einige Anhänge hinzu und senden Sie das Formular. Die Anlagen sollten im Dateisystem an dem Speicherort gespeichert werden, den Sie im Workflow angegeben haben.

