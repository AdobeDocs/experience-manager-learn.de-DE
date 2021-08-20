---
title: Verwenden von PDFG in AEM Forms
description: Demonstrieren der Drag & Drop-Funktion zum Erstellen von PDF-Dateien mit AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 4%

---


# Verwenden von PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Demonstrieren der Drag &amp; Drop-Funktion zum Erstellen von PDF-Dateien mit AEM Forms

PDFG steht für PDF Generation. Dies bedeutet, dass Sie eine Vielzahl von Dateiformaten in PDF konvertieren können. Die häufigsten sind Microsoft Office-Dokumente. PDFG ist seit 6.1 Teil von AEM Forms.
[Das JavaScript für die PDFG-API ist hier](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService) aufgeführt.

Die mit diesem Artikel verknüpften Assets ermöglichen es Ihnen, MS Office-Dokumente oder JPG-Dateien in die Dropzone der HTML-Seite zu ziehen. Nachdem das Dokument abgelegt wurde, wird der PDFG-Dienst aufgerufen, das Dokument in PDF konvertiert und im Dateisystem von AEM Server gespeichert.

Um die Demo-Assets zu installieren, führen Sie die folgenden Schritte aus

1. Konfigurieren Sie PDFG wie in diesem Dokument [hier](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/install-configure-pdf-generator.html) beschrieben.
1. Befolgen Sie die entsprechende Dokumentation zu Ihrer AEM Forms-Version.
1. [Importieren und installieren Sie Assets, die sich auf diesen Artikel beziehen, mit dem Package Manager.](assets/createpdfgdemov2.zip)
1. [Navigieren Sie zu post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin Ihrem CRX.
1. Speicherort gemäß Ihrer Voreinstellung ändern (Zeile 9)
1. Speichern Sie Ihre Änderungen.
1. Öffnen Sie die HTML-Seite [](http://localhost:4502/content/DocumentServices/CreatePDFG.html) zum Ziehen und Ablegen von Dateien zur Konvertierung.
1. Legen Sie eine Wortdatei oder JPG in der Dropzone ab.
1. Das Eingabedokument wird in PDF konvertiert und an dem unter Nummer 4 angegebenen Speicherort gespeichert.

Das folgende Codefragment zeigt die Verwendung des PDFG-Dienstes zum Konvertieren von Dateien in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

