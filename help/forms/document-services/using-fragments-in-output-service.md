---
title: Fragmente im Output-Dienst verwenden
description: PDF-Dokumente mit Fragmenten im CRX-Repository erstellen
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 2%

---

# PDF-Dokumente mit Fragmenten erstellen{#developing-with-output-and-forms-services-in-aem-forms}


In diesem Artikel verwenden wir den Output-Dienst, um PDF-Dateien mit XDP-Fragmenten zu generieren. Die Haupt-XDP und die Fragmente befinden sich im CRX-Repository. Es ist wichtig, die Dateisystemordnerstruktur in AEM zu imitieren. Wenn Sie beispielsweise ein Fragment im Fragmentordner in Ihrer xdp verwenden, müssen Sie einen Ordner mit dem Namen **Fragmente** unter Ihrem Basisordner in AEM. Der Basisordner enthält Ihre Basis-XDP-Vorlage. Wenn Sie beispielsweise die folgende Struktur auf Ihrem Dateisystem haben
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* Die Ordner-xdpdocuments enthalten Ihre Basisvorlage und die Fragmente in **Fragmente** Ordner

Sie können die erforderliche Struktur mithilfe der [Formulare und Dokument-Benutzeroberfläche](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Im Folgenden finden Sie die Ordnerstruktur für das Beispiel-XDP, das 2 Fragmente verwendet
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Output-Dienst - In der Regel wird dieser Dienst verwendet, um XML-Daten mit einer xdp-Vorlage oder PDF zusammenzuführen, um eine reduzierte PDF-Datei zu generieren. Weitere Informationen finden Sie im Abschnitt [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) für den Output-Dienst. In diesem Beispiel verwenden wir Fragmente, die sich im CRX-Repository befinden.


Der folgende Code wurde verwendet, um Fragmente in die PDF-Datei einzuschließen

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**So testen Sie das Beispielpaket auf Ihrem System**

* [Herunterladen und Importieren der Beispiel-XDP-Dateien in AEM](assets/xdp-templates-fragments.zip)
* [Laden Sie das Paket herunter und installieren Sie es mithilfe des AEM Paketmanagers.](assets/using-fragments-assets.zip)
* [Die Beispiel-XDP und -Fragmente können hier heruntergeladen werden](assets/xdptemplates.zip)

**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs in Adobe Granite CSRF Filter in Zulassungsliste setzen.**

1. Führen Sie die unten genannten Schritte aus, um die oben genannten Pfade in Zulassungslisten anzuzeigen.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Adobe Granite CSRF Filter .
1. Fügen Sie den folgenden Pfad in die ausgeschlossenen Abschnitte hinzu und speichern Sie
1. /content/AEMFormsSamples/usingFragments

Es gibt verschiedene Möglichkeiten, den Beispielcode zu testen. Am schnellsten und einfachsten ist die Verwendung der Postman-App. Mit Postman können Sie POST-Anfragen an Ihren Server richten. Installieren Sie das Postman-Programm auf Ihrem System.
Starten Sie die App und geben Sie die folgende URL ein, um die Export-Daten-API zu testen.

Stellen Sie sicher, dass Sie &quot;POST&quot;aus der Dropdown-Liste http://localhost:4502/content/AemFormsSamples/usingfragments.html ausgewählt haben. Stellen Sie sicher, dass Sie &quot;Autorisierung&quot;als &quot;Einfache Autorisierung&quot;angeben. Geben Sie den Benutzernamen und das Kennwort des AEM-Servers an Navigieren Sie zur Registerkarte &quot;Hauptteil&quot;und geben Sie die Anforderungsparameter an, wie in der Abbildung unten dargestellt
![export](assets/using-fragment-postman.png)
Klicken Sie dann auf die Schaltfläche Senden .

[Sie können diese Postman-Sammlung importieren, um die API zu testen](assets/usingfragments.postman_collection.json)
