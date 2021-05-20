---
title: Erstellen mehrerer PDFs aus einer Datendatei
seo-title: Erstellen mehrerer PDFs aus einer Datendatei
feature: Ausgabe-Service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 2%

---


# Generieren eines Satzes von PDF-Dokumenten aus einer XML-Datendatei

OutputService bietet eine Reihe von Methoden zum Erstellen von Dokumenten mit einem Formularentwurf und Daten zum Zusammenführen mit dem Formularentwurf. Im folgenden Artikel wird der Anwendungsfall erläutert, mit dem mehrere PDF-Dateien aus einer großen XML-Datei mit mehreren einzelnen Datensätzen generiert werden.
Im Folgenden finden Sie den Screenshot einer XML-Datei mit mehreren Datensätzen.

![multi-record-xml](assets/multi-record-xml.PNG)

Die Daten-XML enthält 2 Datensätze. Jeder Datensatz wird durch das Element form1 dargestellt. Diese XML-Datei wird an die OutputService-Methode [generatePDFOutputBatch-Methode](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) übergeben. Es wird eine Liste von PDF-Dokumenten (eines pro Datensatz) abgerufen.
Die Signatur der generatePDFOutputBatch -Methode akzeptiert die folgenden Parameter

* templates - Zuordnung, die die Vorlage enthält, durch einen Schlüssel identifiziert
* data - Zuordnung mit XML-Datendokumenten, identifiziert durch Schlüssel
* pdfOutputOptions - Optionen zum Konfigurieren der PDF-Generierung
* batchOptions - Optionen zum Konfigurieren des Batches

>[!NOTE]
>
>Dieses Anwendungsbeispiel ist als Live-Beispiel auf dieser [Website](https://forms.enablementadobe.com/content/samples/samples.html?query=0) verfügbar.

## Anwendungsfalldetails{#use-case-details}

In diesem Anwendungsbeispiel stellen wir eine einfache Web-Oberfläche zum Hochladen der Vorlage und der Datei data(xml) bereit. Sobald der Upload der Dateien abgeschlossen ist und die POST-Anfrage an AEM Servlet gesendet wird. Dieses Servlet extrahiert die Dokumente und ruft die Methode generatePDFOutputBatch des OutputService auf. Die generierten PDFs werden in eine ZIP-Datei komprimiert und dem Endbenutzer zum Herunterladen vom Webbrowser zur Verfügung gestellt.

## Servlet-Code{#servlet-code}

Im Folgenden finden Sie das Code-Snippet aus dem Servlet. Code extrahiert die Vorlage (xdp) und die Datendatei (xml) aus der Anforderung. Die Vorlagendatei wird im Dateisystem gespeichert. Zwei Karten werden erstellt - templateMap und dataFileMap , die die Vorlage und die XML(data)-Dateien enthalten. Anschließend wird die generateMultipleRecords -Methode des DocumentServices-Dienstes aufgerufen.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Code für die Implementierung der Benutzeroberfläche{#Interface-Implementation-Code}

Der folgende Code generiert mehrere PDF-Dateien mithilfe des generatePDFOutputBatch des OutputService und gibt eine ZIP-Datei mit den PDF-Dateien an das aufrufende Servlet zurück

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Bereitstellen auf Ihrem Server{#Deploy-on-your-server}

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen:

* [Laden Sie den Inhalt der ZIP-Datei herunter und extrahieren Sie ihn in Ihr Dateisystem](assets/mult-records-template-and-xml-file.zip). Diese ZIP-Datei enthält die Vorlage und die XML-Datendatei.
* [Verweisen Sie Ihren Browser auf die Felix-Webkonsole.](http://localhost:4502/system/console/bundles)
* [Bereitstellen des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Bereitstellen des benutzerdefinierten Bundles AEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Custom , das die PDF-Dateien mithilfe der OutputService-API generiert
* [Verweisen Sie Ihren Browser auf Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Importieren und installieren Sie das Paket](assets/generate-multiple-pdf-from-xml.zip). Dieses Paket enthält eine HTML-Seite, auf der Sie die Vorlage und die Datendateien ablegen können.
* [Verweisen Sie Ihren Browser auf MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Ziehen Sie die Vorlage und die XML-Datendatei zusammen.
* Laden Sie die erstellte ZIP-Datei herunter. Diese ZIP-Datei enthält die vom Output-Dienst generierten PDF-Dateien.

>[!NOTE]
>Es gibt mehrere Möglichkeiten, diese Funktion Trigger. In diesem Beispiel haben wir eine Web-Oberfläche verwendet, um die Vorlage und die Datendatei abzulegen, um die Funktion zu demonstrieren.

