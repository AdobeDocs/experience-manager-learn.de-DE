---
title: Erstellen mehrerer PDFs aus einer Datendatei
description: OutputService bietet eine Reihe von Methoden zur Erstellung von Dokumenten unter Verwendung eines Formularentwurfs und von Daten, die mit dem Formularentwurf zusammengeführt werden sollen. Erfahren Sie, wie Sie mehrere PDF-Dateien aus einer großen XML-Datei generieren, die mehrere einzelne Datensätze enthält.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
duration: 148
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '486'
ht-degree: 100%

---

# Generieren eines Satzes von PDF-Dokumenten aus einer XML-Datendatei

OutputService bietet eine Reihe von Methoden zur Erstellung von Dokumenten unter Verwendung eines Formularentwurfs und von Daten, die mit dem Formularentwurf zusammengeführt werden sollen. Im folgenden Artikel wird der Anwendungsfall erläutert, mit dem mehrere PDF-Dateien aus einer großen XML-Datei mit mehreren einzelnen Datensätzen generiert werden.
Im Folgenden finden Sie den Screenshot einer XML-Datei mit mehreren Datensätzen.

![multi-record-xml](assets/multi-record-xml.PNG)

Die Daten-XML-Datei enthält zwei Datensätze. Jeder Datensatz wird durch das Element „form1“ dargestellt. Diese XML-Datei wird an die OutputService-Methode [generatePDFOutputBatch](https://helpx.adobe.com/de/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) übergeben und wir erhalten eine Liste von PDF-Dokumenten (eines pro Datensatz)
Die Signatur der Methode „generatePDFOutputBatch“ nimmt die folgenden Parameter:

* templates – Zuordnung, die die Vorlage enthält, durch einen Schlüssel identifiziert
* data – Zuordnung mit XML-Datendokumenten, durch einen Schlüssel identifiziert
* pdfOutputOptions – Optionen zum Konfigurieren der PDF-Generierung
* batchOptions – Optionen zum Konfigurieren des Batches



## Anwendungsfalldetails{#use-case-details}

In diesem Anwendungsbeispiel stellen wir eine einfache Web-Oberfläche zum Hochladen der Vorlage und der Daten(XML)-Datei bereit. Sobald der Upload der Dateien abgeschlossen ist, wird eine POST-Anfrage an AEM Servlet gesendet. Dieses Servlet extrahiert die Dokumente und ruft die Methode „generatePDFOutputBatch“ des OutputService auf. Die generierten PDFs werden in eine ZIP-Datei komprimiert und den Endbenutzenden zum Herunterladen über den Webbrowser zur Verfügung gestellt.

## Servlet-Code{#servlet-code}

Im Folgenden finden Sie das Codesnippet aus dem Servlet. Der Code extrahiert die Vorlage (XDP) und die Datendatei (XML) aus der Anfrage. Die Vorlagendatei wird im Dateisystem gespeichert. Zwei Zuordnungen werden erstellt: templateMap und dataFileMap; sie enthalten die Vorlage bzw. die XML(data)-Dateien. Anschließend wird die Methode „generateMultipleRecords“ des DocumentServices-Dienstes aufgerufen.

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

### Implementierungs-Code der Schnittstelle{#Interface-Implementation-Code}

Der folgende Code generiert mehrere PDF-Dateien mithilfe von „generatePDFOutputBatch“ des OutputService und gibt eine ZIP-Datei mit den PDF-Dateien an das aufrufende Servlet zurück

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

* [Laden Sie den Inhalt der ZIP-Datei herunter und entpacken Sie ihn in Ihr Dateisystem](assets/mult-records-template-and-xml-file.zip). Diese ZIP-Datei enthält die Vorlage und die XML-Datendatei.
* [Lassen Sie Ihren Browser auf die Felix-Web-Konsole verweisen](http://localhost:4502/system/console/bundles).
* [Stellen Sie das DevelopingWithServiceUser-Bundle bereit](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Stellen Sie das benutzerdefinierte AEMFormsDocumentServices-Bundle bereit](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Das benutzerdefinierte Bundle generiert die PDFs mithilfe der OutputService-API.
* [Lassen Sie Ihren Browser auf Package Manager verweisen](http://localhost:4502/crx/packmgr/index.jsp).
* [Importieren und installieren Sie das Paket](assets/generate-multiple-pdf-from-xml.zip). Dieses Paket enthält eine HTML-Seite, auf der Sie die Vorlage und die Datendateien ablegen können.
* [Lassen Sie Ihren Browser auf „MultiRecords.html“ verweisen](http://localhost:4502/content/DocumentServices/Multirecord.html?).
* Ziehen Sie die Vorlage und die XML-Datendatei per Drag-and-Drop zusammen.
* Laden Sie die erstellte ZIP-Datei herunter. Diese ZIP-Datei enthält die vom Output-Dienst generierten PDF-Dateien.

>[!NOTE]
>Es gibt mehrere Möglichkeiten, diese Funktion auszulösen. In diesem Beispiel haben wir eine Web-Schnittstelle verwendet, um die Vorlage und die Datendatei abzulegen und die Möglichkeiten zu demonstrieren.
