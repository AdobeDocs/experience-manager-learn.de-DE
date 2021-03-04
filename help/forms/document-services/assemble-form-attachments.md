---
title: Zusammenführen von Formularanlagen
description: Zusammenführen von Formularanlagen in der angegebenen Reihenfolge
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 1%

---


# Zusammenführen von Formularanlagen

Dieser Artikel enthält Assets, um adaptive Formularanhänge in einer bestimmten Reihenfolge zusammenzustellen. Die Formularanhänge müssen im PDF-Format vorliegen, damit dieser Beispielcode funktioniert. Im Folgenden wird der Verwendungsfall beschrieben.
Beim Ausfüllen eines adaptiven Formulars wird ein oder mehrere PDF-Dokumente an das Formular angehängt.
Stellen Sie beim Senden des Formulars die Formularanlagen zusammen, um ein PDF-Dokument zu erstellen. Sie können die Reihenfolge angeben, in der die Anlagen zusammengestellt werden, um die endgültige PDF zu generieren.

## OSGi-Komponente erstellen, die die WorkflowProcess-Schnittstelle implementiert

Erstellen Sie eine OSGi-Komponente, die die [com.adobe.granite.workflow.exec.WorkflowProcess-Schnittstelle](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html) implementiert. Der Code in dieser Komponente kann mit der Prozessschrittkomponente im AEM Workflow verknüpft werden. Die Ausführungsmethode der Schnittstelle com.adobe.granite.workflow.exec.WorkflowProcess wird in dieser Komponente implementiert.

Wenn ein adaptives Formular an Trigger und AEM Workflow gesendet wird, werden die gesendeten Daten in der angegebenen Datei im Payload-Ordner gespeichert. Dies ist beispielsweise die gesendete Datendatei. Wir müssen die Anlagen zusammenstellen, die unter dem Tag &quot;idcard&quot;und &quot;bankstatement&quot;angegeben sind.
![gesendete Daten](assets/submitted-data.JPG).

### Abrufen der Tag-Namen

Die Reihenfolge der Anlagen wird als Prozessschrittargument im Workflow angegeben, wie im Screenshot unten dargestellt. Hier montieren wir die Anlagen, die dem Feldausweis hinzugefügt werden, gefolgt von Bankausweisen

![process-step](assets/process-step.JPG)

Im folgenden Codefragment werden die Anlagennamen aus den Prozessargumenten extrahiert

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Erstellen von DDX aus den Anlagennamen

Anschließend müssen wir ein [Dokument Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf)-Dokument erstellen, das vom Assembler-Dienst zum Zusammenführen von Dokumenten verwendet wird. Im Folgenden finden Sie das DDX, das aus den Prozessargumenten erstellt wurde. Mit dem NoForms-Element können Sie XFA-basierte Dokumente reduzieren, bevor sie assembliert werden. Beachten Sie, dass sich die PDF-Quellelemente in der richtigen Reihenfolge befinden, wie in den Prozessargumenten angegeben.

![ddx-xml](assets/ddx.PNG)

### Imagemap von Dokumenten erstellen

Anschließend erstellen wir eine Zuordnung von Dokumenten mit dem Anlagennamen als Schlüssel und der Anlage als Wert. Der Abfrage Builder-Dienst wurde verwendet, um die Anlagen unter dem Payload-Pfad Abfrage und die Zuordnung der Dokumente zu erstellen. Diese Karte des Dokuments zusammen mit dem DDX wird benötigt, damit der Assembler-Dienst die endgültige PDF zusammenstellen kann.

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### AssemblerService zum Zusammenführen der Dokumente verwenden

Nachdem DDX und die Dokument-Map erstellt wurden, besteht der nächste Schritt darin, die Dokumente mit AssemblerService zusammenzustellen.
Mit dem folgenden Code wird die zusammengeführte PDF zusammengestellt und zurückgegeben.

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### Speichern Sie die assemblierte PDF unter dem Payload-Ordner

Der letzte Schritt besteht darin, die zusammengesetzte PDF-Datei im Payload-Ordner zu speichern. Diese PDF-Datei kann dann in den nachfolgenden Schritten des Workflows zur weiteren Verarbeitung aufgerufen werden.
Das folgende Codefragment wurde zum Speichern der Datei im Ordner &quot;Payload&quot;verwendet

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

Im Folgenden wird die Struktur des Payload-Ordners dargestellt, nachdem die Formularanhänge zusammengestellt und gespeichert wurden.

![payload-Struktur](assets/payload-structure.JPG)

### So funktioniert diese Funktion auf Ihrem AEM

* Laden Sie das Formular [Formularanhänge zusammenstellen](assets/assemble-form-attachments-af.zip) auf Ihr lokales System herunter.
* Importieren Sie das Formular von der Seite [Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Laden Sie [workflow](assets/assemble-form-attachments.zip) herunter und importieren Sie mit dem Package Manager in AEM.
* [benutzerdefiniertes Bundle](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar) herunterladen
* Stellen Sie das Bundle mit der [Webkonsole](http://localhost:4502/system/console/bundles) bereit und Beginn
* Verweisen Sie Ihren Browser auf [AssembleAttachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* hinzufügen einer Anlage im ID-Dokument und ein paar PDF-Dokumente zum Abschnitt &quot;Bankauszüge&quot;
* Senden des Formulars an den Trigger des Workflows
* Überprüfen Sie den Ordner [Payload im Workflow](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) auf die assemblierte PDF-Datei

>[!NOTE]
> Wenn Sie die Protokollfunktion für das benutzerdefinierte Bundle aktiviert haben, wird die DDX- und die assemblierte Datei in den Ordner Ihrer AEM-Installation geschrieben.

