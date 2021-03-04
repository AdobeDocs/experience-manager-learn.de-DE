---
title: Entwicklung mit Output- und Forms-Diensten in AEM Forms
seo-title: Entwicklung mit Output- und Forms-Diensten in AEM Forms
description: Verwenden der Output- und Forms-Dienst-API in AEM Forms
seo-description: Verwenden der Output- und Forms-Dienst-API in AEM Forms
feature: Formularservice
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 2%

---


# Rendern von interaktiven PDF-Dateien mit Forms Services in AEM Forms

Verwenden der Forms-Dienst-API in AEM Forms zum Rendern interaktiver PDF

In diesem Artikel sehen Sie sich den folgenden Dienst an:

* FormsService - Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren und interaktive PDF-Dateien generieren können, indem Sie XML-Daten in eine XDP-Vorlage zusammenführen

Die offizielle JavaScript-API für AEM Forms ist [hier](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) aufgeführt

Im folgenden Codefragment wird interaktives PDF-Dokument mit dem Vorgang renderPDFForm des FormsService gerendert. Die Vorlage &quot;schengen.xdp&quot;wird zum Zusammenführen der XML-Daten verwendet.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Zeile 1: Speicherort des Ordners, der die xdp-Vorlage enthält

Linie 2-4: Erstellen von PDFFormRenderOptions und Festlegen der Eigenschaften

Zeile 7: Interaktive PDF-Dateien mit dem Vorgang renderPDFForm des FormsService generieren

Zeile 11: Gibt die generierte interaktive PDF-Datei an die aufrufende Anwendung zurück

**So testen Sie das Musterpaket auf Ihrem System**
1. [Herunterladen und Installieren des DocumentServices-Beispielpakets mithilfe der Felix-Webkonsole](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Herunterladen und Installieren des Pakets mit dem AEM Package Manager](assets/downloadinteractivepdffrommobileform.zip)



1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF-Filter suchen
1. hinzufügen Sie den folgenden Pfad in den ausgeschlossenen Abschnitten und speichern Sie
1. /bin/generateinteractivepdf
1. [Öffnen des mobilen Formulars](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Füllen Sie einige Felder aus und klicken Sie dann auf ***Herunterladen und füllen ....*** button
1. Die interaktive PDF sollte auf Ihr lokales System heruntergeladen werden


Das Musterpaket enthält das benutzerdefinierte Profil, das mit dem Mobile Form verknüpft ist. Bitte entdecken Sie die Datei [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Diese JSP extrahiert die Daten aus dem mobilen Formular und stellt eine POST zum Servlet auf dem Pfad ***/bin/generateinteractivepdf*** bereit. Das Servlet gibt die interaktive PDF an die aufrufende Anwendung zurück. Der Code in customtoolbar.jsp lädt dann die Datei auf Ihr lokales System herunter


