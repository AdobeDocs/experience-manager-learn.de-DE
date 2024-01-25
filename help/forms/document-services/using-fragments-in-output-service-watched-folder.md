---
title: Verwenden von Fragmenten im Ausgabedienst mit überwachten Ordnern
description: Generieren von PDF-Dokumenten mit Fragmenten im CRX-Repository
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 100
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 100%

---

# Erstellen von PDF-Dokument mit Fragmenten mithilfe des ECMA-Skripts{#developing-with-output-and-forms-services-in-aem-forms}


In diesem Artikel verwenden wir den Output-Dienst, um PDF-Dateien mit XDP-Fragmenten zu generieren. Die XDP-Hauptdatei und die Fragmente befinden sich im CRX-Repository. Es ist wichtig, die Dateisystem-Ordnerstruktur in AEM nachzuahmen. Wenn Sie beispielsweise ein Fragment im Fragmentordner in Ihrer XDP verwenden, müssen Sie einen Ordner mit dem Namen **fragments** unter Ihrem Basisordner in AEM erstellen. Der Basisordner enthält Ihre XDP-Basisvorlage. Folgendes gilt, wenn beispielsweise die folgende Struktur auf Ihrem Dateisystem vorhanden ist:
* c:\xdptemplates: Dieser Ordner enthält Ihre XDP-Basisvorlage.
* c:\xdptemplates\fragments: Dieser Ordner enthält Fragmente und die Hauptvorlage verweist auf das Fragment, wie unten dargestellt.
  ![Fragment-XDP](assets/survey-fragment.png).
* Die Ordner „xdpdocuments“ enthält Ihre Basisvorlage und die Fragmente im Ordner **fragments**.

Sie können die erforderliche Struktur mithilfe der Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) erstellen.

Im Folgenden finden Sie die Ordnerstruktur für das XDP-Beispiel, das 2 Fragmente verwendet.
![Formulare und Documente](assets/fragment-folder-structure-ui.png)


* Output-Dienst: In der Regel wird dieser Dienst verwendet, um XML-Daten mit einer XDP-Vorlage oder PDF zusammenzuführen, um eine reduzierte PDF-Datei zu generieren. Weitere Informationen finden Sie im [Javadoc](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) für den Output-Dienst. In diesem Beispiel verwenden wir Fragmente, die sich im CRX-Repository befinden.


Das folgende ECMA-Skript wurde verwendet, um ein PDF zu generieren. Beachten Sie die Verwendung von ResourceResolver und ResourceResolverHelper im Code. Der ResourceResolver ist erforderlich, da dieser Code außerhalb jedes Benutzerkontexts ausgeführt wird.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**So testen Sie das Beispielpaket auf Ihrem System:**
* [Stellen Sie das Bundle „DevelopingWithServiceUser“ bereit.](assets/DevelopingWithServiceUser.jar)
* Fügen Sie den Eintrag **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** in die Änderung des User-Mapper-Dienstes ein, wie im folgenden Screenshot gezeigt:
  ![Änderung des User-Mappers](assets/user-mapper-service-amendment.png)
* [Laden Sie die XDP-Beispieldateien und ECMA-Skripte herunter und importieren Sie sie](assets/watched-folder-fragments-ecma.zip).
Dadurch wird eine überwachte Ordnerstruktur im Ordner „c:/fragments/outputService“ erstellt.

* [Extrahieren Sie die Beispieldatendatei](assets/usingFragmentsSampleData.zip) und legen Sie sie in den Installationsordner Ihres überwachten Ordners (c:\fragmentsandoutputservice\install).

* Überprüfen Sie den Ergebnisordner Ihrer überwachten Ordnerkonfiguration auf die generierte PDF-Datei.
