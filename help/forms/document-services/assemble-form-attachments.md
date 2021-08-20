---
title: Zusammenführen von Formularanlagen
description: Zusammenführen von Formularanlagen in der angegebenen Reihenfolge
feature: Assembler
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Zusammenführen von Formularanlagen

Dieser Artikel stellt Assets bereit, um adaptive Formularanhänge in einer angegebenen Reihenfolge zusammenzuführen. Die Formularanhänge müssen im PDF-Format vorliegen, damit dieser Beispielcode funktioniert. Im Folgenden finden Sie den Anwendungsfall.
Benutzer, die ein adaptives Formular ausfüllen, hängt ein oder mehrere PDF-Dokumente an das Formular an.
Assemblieren Sie beim Senden des Formulars die Formularanhänge, um ein PDF-Dokument zu generieren. Sie können die Reihenfolge angeben, in der die Anlagen zusammengestellt werden, um das endgültige PDF zu generieren.

## Erstellen einer OSGi-Komponente, die die WorkflowProcess-Oberfläche implementiert

Erstellen Sie eine OSGi-Komponente, die die [com.adobe.granite.workflow.exec.WorkflowProcess -Schnittstelle](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html) implementiert. Der Code in dieser Komponente kann mit der Prozessschrittkomponente im AEM Workflow verknüpft werden. Die Ausführungsmethode der Schnittstelle com.adobe.granite.workflow.exec.WorkflowProcess wird in dieser Komponente implementiert.

Wenn ein adaptives Formular an Trigger gesendet wird, werden die gesendeten Daten in der angegebenen Datei im Payload-Ordner gespeichert. Dies ist beispielsweise die gesendete Datendatei. Wir müssen die Anlagen zusammenführen, die unter dem Tag idcard und bankstatement angegeben sind.
![submit-data](assets/submitted-data.JPG).

### Abrufen der Tag-Namen

Die Reihenfolge der Anlagen wird als Prozessschrittargumente im Workflow angegeben, wie im Screenshot unten dargestellt. Hier stellen wir die Anlagen zusammen, die dem Feld-IDcard hinzugefügt werden, gefolgt von Bankausweisen

![Prozessschritt](assets/process-step.JPG)

Das folgende Codefragment extrahiert die Anlagennamen aus den Prozessargumenten

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Erstellen von DDX aus den Anlagennamen

Anschließend müssen wir das Dokument [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) erstellen, das vom Assembler-Dienst zum Zusammenführen von Dokumenten verwendet wird. Im Folgenden finden Sie die DDX, die aus den Prozessargumenten erstellt wurde. Mit dem Element NoForms können Sie XFA-basierte Dokumente reduzieren, bevor sie zusammengestellt werden. Beachten Sie, dass sich die PDF-Quellelemente in der richtigen Reihenfolge befinden, wie in den Prozessargumenten angegeben.

![ddx-xml](assets/ddx.PNG)

### Erstellen von Dokumentzuordnungen

Anschließend erstellen wir eine Zuordnung von Dokumenten mit dem Anlagennamen als Schlüssel und dem Anhang als Wert. Der Query Builder-Dienst wurde verwendet, um die Anlagen unter dem Payload-Pfad abzufragen und die Zuordnung der Dokumente zu erstellen. Diese Dokumentzuordnung zusammen mit dem DDX ist erforderlich, damit der Assembler-Dienst das endgültige PDF zusammenstellen kann.

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

Nachdem das DDX und die Dokumentzuordnung erstellt wurden, ist der nächste Schritt die Verwendung des AssemblerService zum Zusammenführen der Dokumente.
Der folgende Code assembliert und gibt das assemblierte PDF-Dokument zurück.

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

### Speichern Sie das assemblierte PDF unter dem Payload-Ordner

Der letzte Schritt besteht darin, das zusammengestellte PDF-Dokument im Payload-Ordner zu speichern. Auf dieses PDF-Dokument können Sie dann in den nachfolgenden Schritten des Workflows zur weiteren Verarbeitung zugreifen.
Das folgende Codefragment wurde verwendet, um die Datei im Payload-Ordner zu speichern

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

### So können Sie diese Funktion auf Ihrem AEM-Server verwenden

* Laden Sie das Formular [Formularanhänge zusammenführen](assets/assemble-form-attachments-af.zip) auf Ihr lokales System herunter.
* Importieren Sie das Formular von der Seite [Forms und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) .
* Laden Sie [workflow](assets/assemble-form-attachments.zip) herunter und importieren Sie mit Package Manager in AEM.
* Laden Sie das [benutzerdefinierte Bundle](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar) herunter
* Stellen Sie das Bundle mithilfe der [Web-Konsole](http://localhost:4502/system/console/bundles) bereit und starten Sie es.
* Zeigen Sie Ihren Browser auf [AssembleAttachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Fügen Sie im ID-Dokument einen Anhang und im Abschnitt Bankübersichten ein paar PDF-Dokumente hinzu.
* Senden des Formulars an den Trigger des Workflows
* Überprüfen Sie den Ordner [payload des Workflows in crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) auf das assemblierte PDF

>[!NOTE]
> Wenn Sie die Protokollierung für das benutzerdefinierte Bundle aktiviert haben, werden DDX und die assemblierte Datei in den Ordner Ihrer AEM-Installation geschrieben.

