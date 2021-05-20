---
title: OCR-Datenextraktion
description: Extrahieren Sie Daten aus staatlich ausgestellten Dokumenten, um Formulare auszufüllen.
feature: Barcoded Forms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 3%

---



# OCR-Datenextraktion

Sie können automatisch Daten aus einer Vielzahl von staatlich ausgestellten Dokumenten extrahieren, um Ihre adaptiven Formulare zu füllen.

Es gibt eine Reihe von Organisationen, die diesen Dienst bereitstellen. Solange sie über gut dokumentierte REST-APIs verfügen, können Sie mithilfe der Datenintegrationsfunktion einfach mit AEM Forms integrieren. Für diese Anleitung habe ich [ID Analyzer](https://www.idanalyzer.com/) verwendet, um die OCR-Datenextraktion hochgeladener Dokumente zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um die OCR-Datenextraktion mit AEM Forms mithilfe des ID Analyzer-Diensts zu implementieren.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [ID Analyzer](https://portal.idanalyzer.com/signin.html). Notieren Sie sich den API-Schlüssel. Dieser Schlüssel ist erforderlich, um REST-APIs des ID Analyzer-Diensts aufzurufen.

## Erstellen der Swagger/OpenAPI-Datei

Die OpenAPI-Spezifikation (früher Swagger Specification) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie Ihre gesamte API beschreiben, einschließlich:

* Verfügbare Endpunkte (/users) und Vorgänge für jeden Endpunkt (GET /users, POST /users)
* Aktionsparameter Eingabe und Ausgabe für jeden Vorgang
Authentifizierungsmethoden
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und sonstige Informationen.
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist für Menschen und Maschinen leicht zu erlernen und lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, befolgen Sie die [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms unterstützt OpenAPI Specification Version 2.0 (fka Swagger).

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die mit SMS gesendeten OTP-Code senden und überprüfen. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann von [hier](assets/drivers-license-swagger.zip) heruntergeladen werden

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [eine Datenquelle ](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration erstellen. Verwenden Sie die [Swagger-Datei](assets/drivers-license-swagger.zip) , um Ihre Datenquelle zu erstellen.

## Formulardatenmodell erstellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Stützen Sie das Formulardatenmodell auf die Datenquelle, die im vorherigen Schritt erstellt wurde.

![fdm](assets/test-dl-fdm.PNG)

## Client-Bibliothek erstellen

Wir müssen eine base64-kodierte Zeichenfolge des hochgeladenen Dokuments abrufen. Diese base64-kodierte Zeichenfolge wird dann als einer der Parameter unseres REST-Aufrufs übergeben.
Die Client-Bibliothek kann [von hier heruntergeladen werden.](assets/drivers-license-client-lib.zip)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um Daten vom Benutzer aus dem hochgeladenen Dokument im Formular zu extrahieren. Sie können Ihr eigenes adaptives Formular erstellen und den Aufruf des Formulardatenmodells verwenden, um die base64-kodierte Zeichenfolge des hochgeladenen Dokuments zu senden.

## Auf Ihrem Server bereitstellen

Wenn Sie die Beispiel-Assets mit Ihrem API-Schlüssel verwenden möchten, führen Sie die folgenden Schritte aus:

* [Laden Sie die ](assets/drivers-license-source.zip) Datenquelle herunter und importieren Sie sie mithilfe des  [Paketmanagers in AEM.](http://localhost:4502/crx/packmgr/index.jsp)
* [Laden Sie das ](assets/drivers-license-fdm.zip) Formulardatenmodell herunter und importieren Sie es mithilfe des  [Paketmanagers in AEM.](http://localhost:4502/crx/packmgr/index.jsp)
* [Client-Bibliothek herunterladen](assets/drivers-license-client-lib.zip)
* Laden Sie das adaptive Beispielformular herunter, indem Sie [es von hier herunterladen](assets/adaptive-form-dl.zip). Dieses Beispielformular verwendet die Dienstaufrufe des Formulardatenmodells, die als Teil dieses Artikels bereitgestellt werden.
* Importieren Sie das Formular aus der [Forms- und Document-Benutzeroberfläche](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) in AEM.
* Öffnen Sie das Formular im Bearbeitungsmodus [](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html).
* Geben Sie Ihren API-Schlüssel als Standardwert im Feld apikey an und speichern Sie Ihre Änderungen.
* Öffnen Sie den Regeleditor für das Feld Base 64 String . Beachten Sie den Dienstaufruf, wenn der Wert dieses Felds geändert wird.
* Speichern Sie das Formular
* [Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled) anzeigen, Titelbild Ihrer Führerscheinnummer hochladen


