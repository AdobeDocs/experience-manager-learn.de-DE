---
title: Verwenden von PDFG in AEM Forms
description: Demonstrieren der Drag-and-Drop-Funktionen zum Erstellen von PDFs mithilfe von AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 63
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# Verwenden von PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Demonstrieren der Drag-and-Drop-Funktionen zum Erstellen von PDFs mithilfe von AEM Forms

PDFG steht für PDF-Generierung. Das bedeutet, dass Sie eine Vielzahl von Dateiformaten in PDF konvertieren können. Die häufigsten sind Microsoft Office-Dokumente. PDFG ist seit 6.1 Teil von AEM Forms.
[Die Javadoc für die PDFG-API ist hier aufgeführt.](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Die mit diesem Artikel verknüpften Assets ermöglichen es Ihnen, MS Office-Dokumente oder JPG-Dateien in den Ablagebereich der HTML-Seite zu ziehen. Sobald das Dokument abgelegt wird, wird der PDFG-Dienst aufgerufen, der das Dokument in eine PDF-Datei konvertiert und im Dateisystem von AEM Server speichert.

Um die Demo-Assets zu installieren, führen Sie die folgenden Schritte aus:

1. Konfigurieren Sie PDFG wie in dem Dokument [hier](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/install-configure-pdf-generator.html) beschrieben
1. Befolgen Sie die entsprechende Dokumentation zu Ihrer AEM Forms-Version
1. [Importieren und installieren Sie Assets, die sich auf diesen Artikel beziehen, mit Package Manager](assets/createpdfgdemov2.zip)
1. [Navigieren Sie zu post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) in Ihrem CRX
1. Ändern Sie den Speicherort nach Ihren Wünschen (Zeile 9)
1. Speichern Sie Ihre Änderungen.
1. Öffnen Sie die [HTML-Seite](http://localhost:4502/content/DocumentServices/CreatePDFG.html) zum Ziehen und Ablegen von Dateien zur Konvertierung
1. Legen Sie eine Word- oder JPG-Datei im Ablagebereich ab
1. Das Eingabedokument wird in PDF konvertiert und an dem unter Nummer 4 angegebenen Speicherort gespeichert

Das folgende Code-Fragment zeigt die Verwendung des PDFG-Dienstes zum Konvertieren von Dateien in PDF:

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
