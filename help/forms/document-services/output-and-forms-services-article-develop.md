---
title: Entwickeln mit dem Output- und dem Forms-Service in AEM Forms
description: Verwenden der Output- und der Forms-Service-API in AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
duration: 164
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 100%

---

# Entwickeln mit dem Output- und dem Forms-Service in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Verwenden der Output- und der Forms-Service-API in AEM Forms

In diesem Artikel werden wir uns Folgendes ansehen:

* Output-Dienst: In der Regel wird dieser Dienst verwendet, um XML-Daten mit einer XDP-Vorlage oder PDF zusammenzuführen, um eine reduzierte PDF-Datei zu generieren. Weitere Informationen hierzu finden Sie in diesem [Javadoc](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) für den Output-Dienst.
* Forms-Dienst: Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus einer PDF-Datei exportieren und darin importieren können. Weitere Informationen hierzu finden Sie in diesem [Javadoc](https://developer.adobe.com/de/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) für den Forms-Service.


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

[Laden Sie das Paket herunter und installieren Sie es mit Package Manager.](assets/outputandformsservice.zip)




**Nachdem Sie das Paket installiert haben, müssen Sie die folgenden URLs unter „Adobe Granite CSRF Filter“ auf die Zulassungsliste setzen.**

1. Führen Sie die folgenden Schritte aus, um die oben genannten Pfade auf die Zulassungsliste zu setzen.
1. [Melden Sie sich bei configMgr an.](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach „Adobe Granite CSRF Filter“.
1. Fügen Sie die drei folgenden Pfade in den ausgeschlossenen Abschnitten hinzu und speichern Sie
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Suchen Sie nach „Sling Referrer filter“.
1. Aktivieren Sie das Kontrollkästchen „Leere zulassen“. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.)
Es gibt verschiedene Möglichkeiten, den Beispiel-Code zu testen. Am schnellsten und einfachsten lässt sich hier die Postman-App verwenden. Mit Postman können Sie POST-Anfragen an Ihren Server richten. Installieren Sie die Postman-App auf Ihrem System.
Starten Sie die App und geben Sie die folgende URL ein, um die API für den Datenexport zu testen.

Stellen Sie sicher, dass Sie „POST“ aus der Dropdown-Liste ausgewählt haben: 
http://localhost:4502/content/AemFormsSamples/exportdata.html 
Stellen Sie sicher, dass Sie „Autorisierung“ als Standardautorisierung angeben. Geben Sie den Benutzernamen und das Passwort für den AEM-Server an.
Navigieren Sie zur Registerkarte „Hauptteil“ und geben Sie die Anfrageparameter an, wie in der Abbildung unten dargestellt.
![Export](assets/postexport.png)
Klicken Sie dann auf die Schaltfläche „Senden“.

Das Paket enthält drei Beispiele. In den folgenden Absätzen wird erläutert, wann der Output-Dienst oder der Forms-Dienst verwendet werden soll. Außerdem werden die URL des Dienstes und die Eingabeparameter angegeben, die von jedem Dienst erwartet werden.

## Zusammenführen von Daten und Reduzieren der Ausgabe

* Verwenden des Output-Dienstes zum Zusammenführen von Daten mit dem XDP- oder PDF-Dokument, um eine reduzierte PDF-Datei zu generieren
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Anfrageparameter:**

   * **xdp_or_pdf_file**: XDP- oder PDF-Datei, mit der Sie Daten zusammenführen möchten.
   * **xmlfile**: XML-Datendatei, die mit „xdp_or_pdf_file“ zusammengeführt wird.
   * **saveLocation**: Speicherort für das gerenderte Dokument auf Ihrem Dateisystem. Beispiel: „c:\\documents\\sample.pdf“.

### Importieren von Daten in eine PDF-Datei

* Verwenden des Forms-Dienstes zum Importieren von Daten in eine PDF-Datei
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Anfrageparameter:**

   * **pdfile**: PDF-Datei, mit der Sie Daten zusammenführen möchten.
   * **xmlfile**: XML-Datendatei, die mit der PDF-Datei zusammengeführt wird.
   * **saveLocation**: Speicherort für das gerenderte Dokument auf Ihrem Dateisystem. Beispiel `c:\\outputsample.pdf`.

**Exportieren von Daten aus der PDF-Datei**
* Verwenden des Forms-Dienstes zum Exportieren von Daten aus einer PDF-Datei
* **POST URL**: http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Anfrageparameter:**

   * **pdfile**: PDF-Datei, aus der Sie Daten exportieren möchten.
   * **saveLocation**: Speicherort für die exportierten Daten auf Ihrem Dateisystem. Beispiel: „c:\\documents\\exporting_data.xml“.

[Sie könnten diese Postman-Sammlung importieren, um die API zu testen.](assets/document-services-postman-collection.json)
