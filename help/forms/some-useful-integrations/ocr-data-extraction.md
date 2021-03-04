---
title: OCR Data Extraktion
description: Extrahieren Sie Daten aus staatlich ausgegebenen Dokumenten, um Formulare auszufüllen.
feature: Integrationen
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 3%

---



# OCR Data Extraktion

Extrahieren Sie automatisch Daten aus einer Vielzahl von staatlich ausgegebenen Dokumenten, um Ihre adaptiven Formulare zu füllen.

Es gibt eine Reihe von Organisationen, die diesen Dienst anbieten und solange diese über gut dokumentierte REST-APIs verfügen, können Sie mit der Datenintegrationsfunktion problemlos in AEM Forms integrieren. Für diese Übung habe ich [ID Analyzer](https://www.idanalyzer.com/) verwendet, um die OCR-Extraktion von hochgeladenen Dokumenten zu demonstrieren.

Die folgenden Schritte wurden zur Implementierung der OCR-Extraktion mit AEM Forms mithilfe des ID-Analyzer-Dienstes ausgeführt.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [ID Analyzer](https://portal.idanalyzer.com/signin.html). Notieren Sie sich den API-Schlüssel. Dieser Schlüssel wird benötigt, um REST-APIs des ID-Analyzer-Dienstes aufzurufen.

## Swagger/OpenAPI-Datei erstellen

OpenAPI Specification (früher Swagger Specification) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie die gesamte API beschreiben, einschließlich:

* Verfügbare Endpunkte (/users) und Vorgänge für jeden Endpunkt (GET /users, POST/users)
* Betriebsparameter Eingabe und Ausgabe für jeden Vorgang
Authentifizierungsmethoden
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und andere Informationen.
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist sowohl für Menschen als auch für Maschinen leicht lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, befolgen Sie die [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms unterstützt OpenAPI Specification Version 2.0 (fka Swagger).

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die OTP-Code senden und überprüfen, der mit SMS gesendet wird. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die vollständige Swagger-Datei kann von [hier heruntergeladen werden](assets/drivers-license-swagger.zip)

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [Datenquelle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration erstellen. Verwenden Sie die Datei [swagger](assets/drivers-license-swagger.zip), um Ihre Datenquelle zu erstellen.

## Formulardatenmodell erstellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Arbeiten mit [Formulardatenmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Stützen Sie das Formulardatenmodell auf der Datenquelle, die im vorherigen Schritt erstellt wurde.

![fdm](assets/test-dl-fdm.PNG)

## Client-Bibliothek erstellen

Wir müssten eine Base64-kodierte Zeichenfolge des hochgeladenen Dokuments abrufen. Diese base64-kodierte Zeichenfolge wird dann als einer der Parameter unseres REST-Aufrufs übergeben.
Die Client-Bibliothek kann von hier heruntergeladen werden.[](assets/drivers-license-client-lib.zip)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um Daten aus dem hochgeladenen Dokument vom Benutzer im Formular zu extrahieren. Sie können Ihr eigenes adaptives Formular erstellen und die Base64-kodierte Zeichenfolge des hochgeladenen Dokuments mit dem Formulardatenmodell-POST-Aufruf senden.

## Auf dem Server bereitstellen

Wenn Sie die Beispielelemente mit Ihrem API-Schlüssel verwenden möchten, führen Sie die folgenden Schritte aus:

* [Laden Sie die Datenquellen ](assets/drivers-license-source.zip) herunter und importieren Sie sie mit dem  [Paketmanager in AEM](http://localhost:4502/crx/packmgr/index.jsp)
* [Herunterladen des ](assets/drivers-license-fdm.zip) Formulardatenmodells und Importieren in AEM mithilfe des  [Paketmanagers](http://localhost:4502/crx/packmgr/index.jsp)
* [Herunterladen der Client-Bibliothek](assets/drivers-license-client-lib.zip)
* Das adaptive Beispielformular herunterladen kann [von hier heruntergeladen werden. ](assets/adaptive-form-dl.zip) Dieses Musterformular verwendet die Dienstaufrufe des Formulardatenmodells, das als Teil dieses Artikels bereitgestellt wird.
* Importieren Sie das Formular in AEM aus der Benutzeroberfläche [Forms und Dokument](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Öffnen Sie das Formular im Bearbeitungsmodus [](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Geben Sie den API-Schlüssel als Standardwert im Feld apikey an und speichern Sie die Änderungen
* Öffnen Sie den Regeleditor für das Feld Base 64 String. Beachten Sie den Dienstaufruf, wenn der Wert dieses Felds geändert wird.
* Speichern Sie das Formular
* [Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), Hochladen des Titelbildes Ihres Führerscheinführerscheins


