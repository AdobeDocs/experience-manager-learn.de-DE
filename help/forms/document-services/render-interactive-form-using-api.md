---
title: Rendern interaktiver PDF mit Forms Services in AEM Forms
description: Verwenden der Forms-Dienst-API in AEM Forms zum Rendern interaktiver PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 1%

---

# Rendern interaktiver PDF mit Forms Services in AEM Forms

Verwenden der Forms-Dienst-API in AEM Forms zum Rendern interaktiver PDF

In diesem Artikel werden wir uns folgende Dienste ansehen

* FormsService - Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren und durch Zusammenführen von XML-Daten in einer xdp-Vorlage interaktive PDF generieren können.

Das offizielle JavaScript für die AEM Forms-API wird aufgelistet [here](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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

Zeile 7: Generieren Sie interaktive PDF mit dem renderPDFForm-Dienstvorgang von FormsService

Zeile 11: Gibt das generierte interaktive PDF-Dokument an die aufrufende Anwendung zurück

**So testen Sie das Beispielpaket auf Ihrem System**
1. [Laden Sie das Document Services-Beispielpaket mithilfe der Felix Web Console herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Laden Sie das Paket herunter und installieren Sie es mithilfe des AEM Paketmanagers.](assets/downloadinteractivepdffrommobileform.zip)



1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Adobe Granite CSRF Filter .
1. Fügen Sie den folgenden Pfad in die ausgeschlossenen Abschnitte hinzu und speichern Sie
1. /bin/generateinteractivepdf
1. [Öffnen Sie das Mobile-Formular](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Füllen Sie einige Felder aus und klicken Sie auf die Schaltfläche ***Herunterladen und Ausfüllen ....*** Schaltfläche
1. Das interaktive PDF-Dokument sollte auf Ihr lokales System heruntergeladen werden


Das Beispielpaket enthält das benutzerdefinierte Profil, das mit dem Mobile-Formular verknüpft ist. Weitere Informationen finden Sie unter [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) -Datei. Diese JSP extrahiert die Daten aus dem mobilen Formular und sendet eine POST-Anfrage an das Servlet, das auf bereitgestellt wird. ***/bin/generateinteractivepdf*** Pfad. Das Servlet gibt die interaktive PDF-Datei an die aufrufende Anwendung zurück. Der Code in customtoolbar.jsp lädt dann die Datei auf Ihr lokales System herunter
