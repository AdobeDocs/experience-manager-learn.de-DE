---
title: Entwicklung mit Output- und Forms-Diensten in AEM Forms
seo-title: Entwicklung mit Output- und Forms-Diensten in AEM Forms
description: Verwenden der Output- und Forms-Dienst-API in AEM Forms
seo-description: Verwenden der Output- und Forms-Dienst-API in AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: Ausgabe-Service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 2%

---


# Entwickeln mit Output- und Forms-Diensten in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Verwenden der Output- und Forms-Dienst-API in AEM Forms

In diesem Artikel werden wir uns folgende Themen ansehen:

* Output-Dienst - In der Regel wird dieser Dienst zum Zusammenführen von XML-Daten mit einer XDP-Vorlage oder PDF-Datei verwendet, um reduzierte PDF-Dateien zu generieren
* FormsService - Dies ist ein sehr vielseitiger Dienst, mit dem Sie Daten aus und in eine PDF-Datei exportieren/importieren können

Die offizielle JavaScript-API für AEM Forms ist [hier](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) aufgeführt

Der folgende Codeausschnitt exportiert Daten aus der PDF-Datei

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

Zeile 1 extrahiert die PDF-Datei aus der Anforderung

Line2 extrahiert saveLocation aus der Anforderung

Zeile 5 enthält FormsService

Zeile 6 exportiert die xmlData aus der PDF-Datei

**So testen Sie das Musterpaket auf Ihrem System**

[Herunterladen und Installieren des Pakets mit dem AEM Package Manager](assets/outputandformsservice.zip)




**Nach der Installation des Pakets müssen Sie die folgenden URLs in Zulassungsliste Adobe Granite CSRF Filter.**

1. Gehen Sie wie folgt vor, um die oben genannten Pfade in Zulassungsliste zu setzen.
1. [Bei configMgr anmelden](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF-Filter suchen
1. hinzufügen Sie die folgenden 3 Pfade in den ausgeschlossenen Abschnitten und speichern Sie
1. /content/AEMFormsSamples/mergedata
1. /content/AEMFormsSamples/exportdata
1. /content/AEMFormsSamples/outputservice
1. Suchen Sie nach &quot;Sling Werber filter&quot;.
1. Aktivieren Sie das Kontrollkästchen &quot;Leeres Feld zulassen&quot;. (Diese Einstellung sollte nur zu Testzwecken verwendet werden.)
Es gibt verschiedene Möglichkeiten, den Beispielcode zu testen. Am schnellsten und einfachsten ist die Verwendung der Postman-App. Postman ermöglicht es Ihnen, POST an Ihren Server zu senden. Installieren Sie die Postman-App auf Ihrem System.
App starten und folgende URL eingeben, um die Export-Daten-API zu testen

Vergewissern Sie sich, dass Sie in der Dropdown-Liste &quot;POST&quot;ausgewählt haben
http://localhost:4502/content/AemFormsSamples/exportdata.html
Stellen Sie sicher, dass Sie &quot;Autorisierung&quot;als &quot;Grundlegende Auth&quot;angeben. AEM Server-Benutzernamen und -Kennwort angeben
Navigieren Sie zur Registerkarte &quot;Haupttext&quot;und geben Sie die Anforderungsparameter an, wie in der Abbildung unten dargestellt
![export](assets/postexport.png)
Klicken Sie dann auf Senden

Das Paket enthält 3 Beispiele. In den folgenden Absätzen wird erläutert, wann der Output-Dienst oder der Forms-Dienst, die URL des Dienstes, Eingabeparameter, die jeder Dienst erwartet, verwendet werden soll

**Daten zusammenführen und Ausgabe reduzieren:**

* Verwenden Sie den Output-Dienst zum Zusammenführen von Daten mit dem xdp- oder pdf-Dokument, um reduzierte PDF-Dateien zu erstellen
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Anforderungsparameter -**

   * xdp_or_pdf_file : Die XDP- oder PDF-Datei, mit der Sie Daten zusammenführen möchten
   * xmlfile: Die XML-Datendatei, die mit der Datei xdp_or_pdf_file zusammengeführt wird
   * saveLocation: Der Speicherort des gerenderten Dokuments im Dateisystem

**Daten in PDF-Datei importieren:**
* FormsService zum Importieren von Daten in die PDF-Datei verwenden
* **POST-URL**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Anforderungsparameter:**

   * pdfile: Die PDF-Datei, mit der Sie Daten zusammenführen möchten
   * xmlfile: Die XML-Datendatei, die mit der PDF-Datei zusammengeführt wird
   * saveLocation: Der Speicherort des gerenderten Dokuments im Dateisystem. Beispiel: c:\\\outputsample.pdf.

**Daten aus PDF-Datei exportieren**
* FormsService zum Exportieren von Daten aus der PDF-Datei verwenden
* **POST-** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Anforderungsparameter:**

   * pdfile: Die PDF-Datei, aus der Sie Daten exportieren möchten
   * saveLocation: Speicherort der exportierten Daten im Dateisystem

[Sie können diese Postman-Sammlung importieren, um die API zu testen](assets/document-services-postman-collection.json)

