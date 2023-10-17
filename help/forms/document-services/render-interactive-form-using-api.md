---
title: Rendern interaktiver PDF-Dateien mit Forms-Diensten in AEM Forms
description: Verwenden der Forms Service-API in AEM Forms zum Rendern interaktiver PDF-Dateien
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '364'
ht-degree: 100%

---

# Rendern interaktiver PDF-Dateien mit Forms-Diensten in AEM Forms

Verwenden der Forms Service-API in AEM Forms zum Rendern interaktiver PDF-Dateien

In diesem Artikel beschäftigen wir uns mit dem folgenden Dienst:

* FormsService: Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren und durch Zusammenführen von XML-Daten in einer XDP-Vorlage interaktive PDF-Dateien generieren können.

Das offizielle [Javadoc für die AEM Forms-API finden Sie hier](https://helpx.adobe.com/de/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html).

Das folgende Codesnippet rendert interaktive PDF-Dateien mithilfe des FormsService-Vorgangs „renderPDFForm“. Die Vorlage „schengen.xdp“ wird zum Zusammenführen der XML-Daten verwendet.

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

Zeile 1: Speicherort des Ordners, der die XDP-Vorlage enthält

Zeile 2–4: Erstellen von PDFFormRenderOptions und Festlegen der zugehörigen Eigenschaften

Zeile 7: Generieren der interaktiven PDF-Datei mithilfe des FormsService-Vorgangs „renderPDFForm“ 

Zeile 11: Zurückgeben der generierten interaktiven PDF-Datei an die aufrufende Anwendung

**So testen Sie das Beispielpaket auf Ihrem System:**
1. [Laden Sie „DevelopingWithServiceUserBundle“ herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Laden Sie das DocumentServices-Beispiel-Bundle herunter und installieren Sie es mithilfe der Felix-Web-Konsole.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Laden Sie das Paket herunter und installieren Sie es mit Package Manager.](assets/downloadinteractivepdffrommobileform.zip)

1. [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach „Adobe Granite CSRF Filter“.
1. Fügen Sie den folgenden Pfad in den ausgeschlossenen Abschnitten hinzu und speichern Sie
1. /bin/generateinteractivepdf.
1. Suchen Sie nach _Apache Sling Service User Mapper Service_ und klicken Sie darauf, um die Eigenschaften zu öffnen.
   1. Klicken Sie auf das Symbol *+* (Plus), um die folgende Dienstzuordnung hinzuzufügen.
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Klicken Sie auf „Speichern“.
1. [Öffnen Sie das Mobile-Formular.](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Füllen Sie einige Felder aus und klicken Sie auf ***Herunterladen und Ausfüllen*** Schaltfläche
1. Die interaktive PDF-Datei sollte auf Ihr lokales System heruntergeladen werden.


Das Beispielpaket enthält das benutzerdefinierte Profil, das mit dem Mobile-Formular verknüpft ist. Sehen Sie sich die Datei [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) an. Diese JSP-Datei extrahiert die Daten aus dem Mobile-Formular und sendet eine POST-Anfrage an das im Pfad ***/bin/generateinteractivepdf*** bereitgestellte Servlet. Das Servlet gibt die interaktive PDF-Datei an die aufrufende Anwendung zurück. Mit dem Code in „customtoolbar.jsp“ wird dann die Datei auf Ihr lokales System heruntergeladen.
