---
title: Entwickeln mit Output- und Forms-Diensten in AEM Forms
description: Verwenden der Output- und Forms Service-API in AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 2%

---

# Entwickeln mit Output- und Forms-Diensten in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Verwenden der Output- und Forms Service-API in AEM Forms

In diesem Artikel werden wir uns Folgendes ansehen:

* Output-Dienst - In der Regel wird dieser Dienst verwendet, um XML-Daten mit einer xdp-Vorlage oder PDF zusammenzuführen, um eine reduzierte PDF-Datei zu generieren. Weitere Informationen hierzu finden Sie in diesem [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) für den Output-Dienst.
* FormsService - Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren können. Weitere Informationen hierzu finden Sie in diesem [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) für den Forms-Dienst.


Der folgende Codeausschnitt exportiert Daten aus der PDF-Datei

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

Zeile 1 extrahiert die pdfile aus der Anforderung

Line2 extrahiert die saveLocation aus der Anfrage

Zeile 5 hält FormsService ein

Zeile 6 exportiert die xmlData aus der PDF-Datei

**So testen Sie das Beispielpaket auf Ihrem System**

[Laden Sie das Paket herunter und installieren Sie es mithilfe des AEM Paketmanagers.](assets/outputandformsservice.zip)




**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs in Adobe Granite CSRF Filter in Zulassungsliste setzen.**

1. Führen Sie die unten genannten Schritte aus, um die oben genannten Pfade in Zulassungslisten anzuzeigen.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach Adobe Granite CSRF Filter .
1. Fügen Sie die folgenden 3 Pfade in die ausgeschlossenen Abschnitte hinzu und speichern Sie
1. /content/AEMFormsSamples/mergedata
1. /content/AEMFormsSamples/exportdata
1. /content/AemFormsSamples/outputService
1. Suchen Sie nach &quot;Sling Referrer filter&quot;.
1. Aktivieren Sie das Kontrollkästchen &quot;Leere erlauben&quot;. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.) Es gibt verschiedene Möglichkeiten, den Beispielcode zu testen. Am schnellsten und einfachsten ist die Verwendung der Postman-App. Mit Postman können Sie POST-Anfragen an Ihren Server richten. Installieren Sie das Postman-Programm auf Ihrem System.
Starten Sie die App und geben Sie die folgende URL ein, um die Export-Daten-API zu testen.

Stellen Sie sicher, dass Sie &quot;POST&quot;aus der Dropdown-Liste http://localhost:4502/content/AemFormsSamples/exportdata.html ausgewählt haben. Stellen Sie sicher, dass Sie &quot;Autorisierung&quot;als &quot;Einfache Autorisierung&quot;angeben. Geben Sie den Benutzernamen und das Kennwort des AEM-Servers an Navigieren Sie zur Registerkarte &quot;Hauptteil&quot;und geben Sie die Anforderungsparameter an, wie in der Abbildung unten dargestellt
![export](assets/postexport.png)
Klicken Sie dann auf die Schaltfläche Senden .

Die Packung enthält 3 Proben. In den folgenden Absätzen wird erläutert, wann der Output-Dienst oder der Forms-Dienst, die URL des Dienstes, Eingabeparameter, die von jedem Dienst erwartet werden, verwendet werden soll

## Daten zusammenführen und Ausgabe reduzieren

* Verwenden Sie Output Service zum Zusammenführen von Daten mit dem XDP- oder PDF-Dokument, um reduzierte PDF-Dateien zu generieren
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Anforderungsparameter -**

   * **xdp_or_pdf_file** : Die XDP- oder PDF-Datei, mit der Sie Daten zusammenführen möchten
   * **xmlfile**: Die XML-Datendatei, die mit xdp_or_pdf_file zusammengeführt wird
   * **saveLocation**: Der Speicherort für das wiedergegebene Dokument auf Ihrem Dateisystem. Beispiel: c:\\documents\\sample.pdf

### Daten in PDF-Datei importieren

* Verwenden von FormsService zum Importieren von Daten in eine PDF-Datei
* **POST-URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Anforderungsparameter:**

   * **pdfile** : Die PDF-Datei, mit der Sie Daten zusammenführen möchten
   * **xmlfile**: Die XML-Datendatei, die mit der PDF-Datei zusammengeführt wird
   * **saveLocation**: Der Speicherort für das wiedergegebene Dokument auf Ihrem Dateisystem. Beispiel `c:\\outputsample.pdf`.

**Daten aus einer PDF-Datei exportieren**
* Verwenden Sie FormsService zum Exportieren von Daten aus einer PDF-Datei
* **POST UR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Anforderungsparameter:**

   * **pdfile** : Die PDF-Datei, aus der Sie Daten exportieren möchten
   * **saveLocation**: Der Speicherort, unter dem die exportierten Daten in Ihrem Dateisystem gespeichert werden sollen. Beispiel: c:\\documents\\exported_data.xml

[Sie können diese Postman-Sammlung importieren, um die API zu testen](assets/document-services-postman-collection.json)
