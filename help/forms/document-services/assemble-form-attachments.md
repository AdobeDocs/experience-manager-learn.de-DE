---
title: Zusammenführen von Formularanlagen
description: Zusammenführen von Formularanlagen in der angegebenen Reihenfolge
feature: Assembler
version: 6.4,6.5
jira: KT-6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 100%

---

# Zusammenführen von Formularanlagen

Dieser Artikel stellt Assets bereit, um adaptive Formularanlagen in einer angegebenen Reihenfolge zusammenzuführen. Die Formularanhänge müssen im PDF-Format vorliegen, damit dieser Beispiel-Code funktioniert. Im Folgenden finden Sie den Anwendungsfall.
Benutzerin bzw. Benutzer, die bzw. der ein adaptives Formular ausfüllt, hängt ein oder mehrere PDF-Dokumente an das Formular an.
Bei der Übermittlung des Formulars werden die Anhänge des Formulars zu einer PDF-Datei zusammengefügt. Sie können die Reihenfolge angeben, in der die Anlagen zusammengestellt werden, um das endgültige PDF zu generieren.

## Erstellen einer OSGi-Komponente, die die WorkflowProcess-Schnittstelle implementiert

Erstellen Sie eine OSGi-Komponente, die die [com.adobe.granite.workflow.exec.WorkflowProcess-Schnittstelle](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html) implementiert. Der Code in dieser Komponente kann mit der Prozessschrittkomponente im AEM-Workflow verknüpft werden. Die Ausführungsmethode der com.adobe.granite.workflow.exec.WorkflowProcess-Schnittstelle wird in dieser Komponente implementiert.

Wenn ein adaptives Formular übermittelt wird, um einen AEM-Workflow auszulösen, werden die übermittelten Daten in der angegebenen Datei im Payload-Ordner gespeichert. Dies ist beispielsweise die gesendete Datendatei. Wir müssen die Anlagen zusammenführen, die unter den Tags „idcard und „bankstatement“ angegeben sind.
![submitted-data](assets/submitted-data.JPG).

### Abrufen der Tag-Namen

Die Reihenfolge der Anlagen wird als Prozessschrittargumente im Workflow angegeben, wie im Screenshot unten dargestellt. Hier stellen wir die Anlagen zusammen, die dem Feld „idcard“ hinzugefügt werden, gefolgt von „bankstatements“

![process-step](assets/process-step.JPG)

Das folgende Codesnippet extrahiert die Anlagennamen aus den Prozessargumenten:

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Erstellen von DDX aus den Anlagennamen

Anschließend müssen wir das Dokument [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) erstellen, das vom Assembler-Dienst zum Zusammenstellen von Dokumenten verwendet wird. Im Folgenden finden Sie die DDX, die aus den Prozessargumenten erstellt wurde. Mit dem Element „NoForms“ können Sie XFA-basierte Dokumente verkleinern, bevor sie zusammengestellt werden. Beachten Sie, dass sich die PDF-Quellelemente in der richtigen Reihenfolge befinden, wie in den Prozessargumenten angegeben.

![ddx-xml](assets/ddx.PNG)

### Erstellen von Dokumentzuordnungen

Anschließend erstellen wir eine Zuordnung von Dokumenten mit dem Anlagennamen als Schlüssel und der Anlage als Wert. Der Query Builder-Dienst wurde verwendet, um die Anlagen unter dem Payload-Pfad abzufragen und die Zuordnung der Dokumente zu erstellen. Diese Dokumentzuordnung zusammen mit dem DDX ist erforderlich, damit der Assembler-Dienst das endgültige PDF zusammenstellen kann.

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

### Verwenden des AssemblerService zum Zusammenführen der Dokumente

Nachdem das DDX und die Dokumentzuordnung erstellt wurden, ist der nächste Schritt die Verwendung des AssemblerService zum Zusammenführen der Dokumente.
Der folgende Code setzt die Datei zusammen und gibt das zusammengesetzte PDF zurück.

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

### Speichern der zusammengestellten PDF-Datei im Payload-Ordner

Der letzte Schritt besteht darin, die zusammengestellte PDF-Datei im Payload-Ordner zu speichern. Auf diese PDF-Datei kann dann in den nachfolgenden Schritten des Workflows zur weiteren Bearbeitung zugegriffen werden.
Das folgende Codesnippet wurde verwendet, um die Datei im Payload-Ordner zu speichern:

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

Im Folgenden finden Sie die Struktur des Payload-Ordners, nachdem die Formularanlagen zusammengestellt und gespeichert wurden.

![payload-structure](assets/payload-structure.JPG)

### Sie können diese Funktion wie folgt auf Ihrem AEM-Server verwenden:

* Laden Sie das [Assemble Form Attachments Form](assets/assemble-form-attachments-af.zip) auf Ihr lokales System herunter.
* Importieren Sie das Formular von der Seite[Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Laden Sie [Workflow](assets/assemble-form-attachments.zip) herunter und importieren Sie dies mit Package Manager in AEM.
* Laden Sie das [benutzerspezifische Bundle](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar) herunter
* Stellen Sie das Bundle mithilfe der [Web-Konsole](http://localhost:4502/system/console/bundles) bereit und führen Sie es aus
* Lassen Sie Ihren Browser auf das [AssembleAttachments-Formular](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled) verweisen
* Fügen Sie im ID-Dokument einen Anhang und im Abschnitt mit den Kontoauszügen ein paar PDF-Dokumente hinzu
* Senden Sie das Formular ab, um den Workflow auszulösen
* Überprüfen Sie das zusammengestellte PDF-Dokument im [Payload-Ordner im crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) des Workflows

>[!NOTE]
> Wenn Sie die Protokollfunktion für das benutzerdefinierte Bundle aktiviert haben, werden DDX und die zusammengestellte Datei in den Ordner Ihrer AEM-Installation geschrieben.
