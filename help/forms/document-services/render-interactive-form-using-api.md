---
title: Entwickeln mit Output- und Forms-Diensten in AEM Forms
seo-title: Entwickeln mit Output- und Forms-Diensten in AEM Forms
description: Verwenden der Output- und Forms Service-API in AEM Forms
seo-description: Verwenden der Output- und Forms Service-API in AEM Forms
feature: Formularservice
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---


# Rendern von interaktiven PDF-Dateien mit Forms Services in AEM Forms

Verwenden der Forms Service-API in AEM Forms zum Rendern interaktiver PDF-Dateien

In diesem Artikel werden wir uns folgende Dienste ansehen

* FormsService - Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren und durch Zusammenführen von XML-Daten in einer xdp-Vorlage interaktive PDF generieren können

Das offizielle JavaScript für die AEM Forms-API ist [hier](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) aufgeführt.

Das folgende Codefragment rendert interaktive PDF-Dateien mithilfe des Vorgangs renderPDFForm des FormsService. Die Datei schengen.xdp ist eine Vorlage, die zum Zusammenführen der XML-Daten verwendet wird.

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

Zeile2-4: Erstellen von PDFFormRenderOptions und Festlegen der Eigenschaften

Zeile 7: Generieren interaktiver PDF-Dateien mit dem renderPDFForm-Dienstvorgang von FormsService

Zeile 11: Gibt das generierte interaktive PDF-Dokument an die aufrufende Anwendung zurück

**So testen Sie das Beispielpaket auf Ihrem System**
1. [Laden Sie das Document Services-Beispielpaket mithilfe der Felix Web Console herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Laden Sie das Paket herunter und installieren Sie es mithilfe des AEM Paketmanagers.](assets/downloadinteractivepdffrommobileform.zip)



1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Adobe Granite CSRF Filter .
1. Fügen Sie den folgenden Pfad in die ausgeschlossenen Abschnitte hinzu und speichern Sie
1. /bin/generateinteractivepdf
1. [Öffnen Sie das Mobile-Formular](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Füllen Sie einige Felder aus und klicken Sie dann auf ***Herunterladen und Ausfüllen ....*** button
1. Das interaktive PDF-Dokument sollte auf Ihr lokales System heruntergeladen werden


Das Beispielpaket enthält das benutzerdefinierte Profil, das mit dem Mobile Form verknüpft ist. Lesen Sie die Datei [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) . Diese JSP extrahiert die Daten aus dem mobilen Formular und sendet eine POST-Anfrage an das Servlet, das auf dem Pfad ***/bin/generateinteractivepdf*** bereitgestellt wird. Das Servlet gibt die interaktive PDF-Datei an die aufrufende Anwendung zurück. Der Code in customtoolbar.jsp lädt dann die Datei auf Ihr lokales System herunter


