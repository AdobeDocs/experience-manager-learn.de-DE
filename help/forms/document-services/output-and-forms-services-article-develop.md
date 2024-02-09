---
title: Entwickeln mit dem Output- und dem Forms-Service in AEM Forms
description: Erfahren Sie mehr über die Entwicklung mit der Output- und Forms Service-API in AEM Forms.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
source-git-commit: 8e9bf8001e4bb7341aeadd65ffd2543da359e061
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 61%

---

# Entwickeln mit dem Output- und dem Forms-Service in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Erfahren Sie mehr über die Entwicklung mit der Output- und Forms Service-API in AEM Forms.

In diesem Artikel werden wir uns Folgendes ansehen:

* [Output-Dienst](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) - In der Regel wird dieser Dienst verwendet, um XML-Daten mit einer xdp-Vorlage oder PDF zusammenzuführen, um reduzierte PDF-Dateien zu generieren.
* [FormsService](https://developer.adobe.com/de/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) - Dies ist ein sehr vielseitiger Dienst, mit dem Sie XDP als PDF rendern und Daten aus und in die PDF-Datei exportieren/importieren können.


Mit dem folgenden Codesnippet werden Daten aus der PDF-Datei exportiert:

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

Zeile 1 extrahiert die PDF-Datei aus der Anfrage.

Zeile 2 extrahiert den Speicherort aus der Anfrage.

Zeile 5 ruft den Forms-Dienst ab.

Zeile 6 exportiert die XML-Daten aus der PDF-Datei.

**So testen Sie das Beispielpaket auf Ihrem System:**

[Laden Sie das Paket herunter und installieren Sie es mit Package Manager.](assets/using-output-and-form-service-api.zip)




**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs unter „Adobe Granite CSRF Filter“ auf die Zulassungsliste setzen.**

1. Führen Sie die folgenden Schritte aus, um die oben genannten Pfade auf die Zulassungsliste zu setzen.
1. [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach „Adobe Granite CSRF Filter“.
1. Fügen Sie die drei folgenden Pfade in den ausgeschlossenen Abschnitten hinzu und speichern Sie
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. Suchen Sie nach „Sling Referrer filter“.
1. Aktivieren Sie das Kontrollkästchen „Leere zulassen“. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.)

## Testen der Proben

Es gibt verschiedene Möglichkeiten, den Beispiel-Code zu testen. Am schnellsten und einfachsten lässt sich hier die Postman-App verwenden. Mit Postman können Sie POST-Anfragen an Ihren Server richten.

* Installieren Sie das Postman-Programm auf Ihrem System.
* Starten Sie die App und geben Sie die entsprechende URL ein.
* Vergewissern Sie sich, dass Sie in der Dropdownliste &quot;POST&quot;ausgewählt haben.
* Stellen Sie sicher, dass Sie &quot;Authorization&quot;als &quot;Basic Auth&quot;angeben. Geben Sie den Benutzernamen und das Kennwort des AEM-Servers an
* Angeben der Anforderungsparameter auf der Registerkarte &quot;Hauptteil&quot;
* Klicken Sie auf die Schaltfläche Senden .

Die Packung enthält 4 Proben. In den folgenden Absätzen wird erläutert, wann der Output-Dienst oder der Forms-Dienst verwendet werden soll. Außerdem werden die URL des Dienstes und die Eingabeparameter angegeben, die von jedem Dienst erwartet werden.

## Verwenden von OutputService zum Zusammenführen von Daten mit der xdp-Vorlage

* Verwenden des Output-Dienstes zum Zusammenführen von Daten mit dem XDP- oder PDF-Dokument, um eine reduzierte PDF-Datei zu generieren
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Anfrageparameter:**

   * **xdp_or_pdf_file**: XDP- oder PDF-Datei, mit der Sie Daten zusammenführen möchten.
   * **xmlfile**: XML-Datendatei, die mit „xdp_or_pdf_file“ zusammengeführt wird.
   * **saveLocation**: Speicherort für das gerenderte Dokument auf Ihrem Dateisystem. Beispiel: „c:\\documents\\sample.pdf“.

### Verwenden der FormsService-API

#### Daten importieren

* Verwenden Sie FormsService importData , um Daten in eine PDF-Datei zu importieren
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/mergedata.html

* **Anfrageparameter:**

   * **pdfile**: PDF-Datei, mit der Sie Daten zusammenführen möchten.
   * **xmlfile**: XML-Datendatei, die mit der PDF-Datei zusammengeführt wird.
   * **saveLocation**: Speicherort für das gerenderte Dokument auf Ihrem Dateisystem. Beispiel `c:\\outputsample.pdf`.

#### Daten exportieren

* FormsService exportData API zum Exportieren von Daten aus einer PDF-Datei verwenden
* **POST-URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Anfrageparameter:**

   * **pdfile**: PDF-Datei, aus der Sie Daten exportieren möchten.
   * **saveLocation**: Speicherort für die exportierten Daten auf Ihrem Dateisystem. Beispiel: „c:\\documents\\exporting_data.xml“.

#### Render XDP

* XDP-Vorlage als statisches/dynamisches PDF rendern
* Verwenden der FormsService renderPDFForm-API zum Rendern der XDP-Vorlage als PDF
* **POST-URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* Anforderungsparameter:
   * xdpName: Name der xdp-Datei, die als pdf gerendert werden soll

[Sie könnten diese Postman-Sammlung importieren, um die API zu testen.](assets/UsingDocumentServicesInAEMForms.postman_collection.json)
