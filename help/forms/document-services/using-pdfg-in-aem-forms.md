---
title: Verwenden von PDFG in AEM Forms
description: Demonstrieren der Drag-and-Drop-Funktion zum Erstellen von PDF mithilfe von AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 3%

---

# Verwenden von PDFG in AEM Forms{#using-pdfg-in-aem-forms}

Demonstrieren der Drag-and-Drop-Funktion zum Erstellen von PDF mithilfe von AEM Forms

PDFG steht für PDF Generation. Dadurch können Sie eine Vielzahl von Dateiformaten in PDF konvertieren. Die häufigsten sind Microsoft Office-Dokumente. PDFG ist seit 6.1 Teil von AEM Forms.
[Die Javadoc für die PDFG-API ist hier aufgeführt.](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Die mit diesem Artikel verknüpften Assets ermöglichen es Ihnen, MS-Office-Dokumente oder JPG-Dateien in die Dropzone der HTML-Seite zu ziehen. Nachdem das Dokument abgelegt wurde, ruft es den PDFG-Dienst auf, konvertiert das Dokument in PDF und speichert es im Dateisystem AEM Servers.

Um die Demo-Assets zu installieren, führen Sie die folgenden Schritte aus

1. PDFG wie in diesem Dokument beschrieben konfigurieren [here](https://helpx.adobe.com/de/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Befolgen Sie die entsprechende Dokumentation zu Ihrer AEM Forms-Version.
1. [Importieren und installieren Sie Assets, die sich auf diesen Artikel beziehen, mit dem Package Manager.](assets/createpdfgdemov2.zip)
1. [Navigieren Sie zu post.jsp .](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) in Ihrem CRX
1. Speicherort gemäß Ihrer Voreinstellung ändern (Zeile 9)
1. Speichern Sie Ihre Änderungen.
1. Öffnen Sie die [HTML-Seite](http://localhost:4502/content/DocumentServices/CreatePDFG.html) zum Ziehen und Ablegen von Dateien zur Konvertierung.
1. Legen Sie eine Wortdatei oder JPG in der Dropzone ab.
1. Das Eingabedokument wird in PDF konvertiert und an dem unter Nummer 4 angegebenen Speicherort gespeichert.

Das folgende Codefragment zeigt die Verwendung des PDFG-Dienstes zum Konvertieren von Dateien in PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
