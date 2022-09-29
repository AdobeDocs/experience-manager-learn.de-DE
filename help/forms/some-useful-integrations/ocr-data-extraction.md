---
title: OCR-Datenextraktion
description: Extrahieren Sie Daten aus staatlich ausgestellten Dokumenten, um Formulare auszufüllen.
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 4%

---

# OCR-Datenextraktion

Sie können automatisch Daten aus einer Vielzahl von staatlich ausgestellten Dokumenten extrahieren, um Ihre adaptiven Formulare zu füllen.

Es gibt eine Reihe von Organisationen, die diesen Dienst bereitstellen. Solange sie über gut dokumentierte REST-APIs verfügen, können Sie mithilfe der Datenintegrationsfunktion einfach mit AEM Forms integrieren. Im Rahmen dieses Tutorials habe ich [ID Analyzer](https://www.idanalyzer.com/) , um die OCR-Datenextraktion hochgeladener Dokumente zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um die OCR-Datenextraktion mit AEM Forms mithilfe des ID Analyzer-Diensts zu implementieren.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [ID Analyzer](https://portal.idanalyzer.com/signin.html). Notieren Sie sich den API-Schlüssel. Dieser Schlüssel ist erforderlich, um REST-APIs des ID Analyzer-Diensts aufzurufen.

## Erstellen der Swagger/OpenAPI-Datei

Die OpenAPI-Spezifikation (früher Swagger Specification) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie Ihre gesamte API beschreiben, einschließlich:

* Verfügbare Endpunkte (/users) und Vorgänge für jeden Endpunkt (GET /users, POST /users)
* Aktionsparameter Eingabe und Ausgabe für jede Vorgangsauthentifizierungsmethode
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und sonstige Informationen.
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist für Menschen und Maschinen leicht zu erlernen und lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, folgen Sie dem [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms unterstützt OpenAPI Specification Version 2.0 (fka Swagger).

Verwenden Sie die [Swagger-Editor](https://editor.swagger.io/) , um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die mit SMS gesendeten OTP-Code senden und überprüfen. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann von heruntergeladen werden. [here](assets/drivers-license-swagger.zip)

## Überlegungen beim Definieren der Swagger-Datei

* Definitionen sind erforderlich
* $ref muss für Methodendefinitionen verwendet werden
* Stellen Sie sich vor, dass Konsumenten und Produktionsabschnitte definiert sind.
* Definieren Sie keine Parameter für den Inline-Anforderungstext oder Antwortparameter. Versuchen Sie, so viel wie möglich modularisieren. Beispielsweise wird die folgende Definition nicht unterstützt

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

Folgendes wird mit einem Verweis auf die Definition von requestBody unterstützt

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Beispiel-Swagger-Datei für Ihre Referenz](assets/sample-swagger.json)

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [Datenquelle erstellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration. Bitte verwenden Sie [Swagger-Datei](assets/drivers-license-swagger.zip) , um Ihre Datenquelle zu erstellen.

## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodelle](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Stützen Sie das Formulardatenmodell auf die Datenquelle, die im vorherigen Schritt erstellt wurde.

![fdm](assets/test-dl-fdm.PNG)

## Client-Bibliothek erstellen

Wir müssen eine base64-kodierte Zeichenfolge des hochgeladenen Dokuments abrufen. Diese base64-kodierte Zeichenfolge wird dann als einer der Parameter unseres REST-Aufrufs übergeben.
Die Client-Bibliothek kann heruntergeladen werden [von hier aus.](assets/drivers-license-client-lib.zip)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um Daten vom Benutzer aus dem hochgeladenen Dokument im Formular zu extrahieren. Sie können Ihr eigenes adaptives Formular erstellen und den Aufruf des Formulardatenmodells verwenden, um die base64-kodierte Zeichenfolge des hochgeladenen Dokuments zu senden.

## Auf Ihrem Server bereitstellen

Wenn Sie die Beispiel-Assets mit Ihrem API-Schlüssel verwenden möchten, führen Sie die folgenden Schritte aus:

* [Datenquelle herunterladen](assets/drivers-license-source.zip) und importieren Sie in AEM mithilfe von [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Herunterladen des Formulardatenmodells](assets/drivers-license-fdm.zip) und importieren Sie in AEM mithilfe von [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [Client-Bibliothek herunterladen](assets/drivers-license-client-lib.zip)
* Das adaptive Beispielformular herunterladen kann [heruntergeladen von hier](assets/adaptive-form-dl.zip). Dieses Beispielformular verwendet die Dienstaufrufe des Formulardatenmodells, die als Teil dieses Artikels bereitgestellt werden.
* Importieren Sie das Formular in AEM aus dem [Benutzeroberfläche von Forms und Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Öffnen Sie das Formular in [Bearbeitungsmodus.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Geben Sie Ihren API-Schlüssel als Standardwert im Feld apikey an und speichern Sie Ihre Änderungen.
* Öffnen Sie den Regeleditor für das Feld Base 64 String . Beachten Sie den Dienstaufruf, wenn der Wert dieses Felds geändert wird.
* Speichern Sie das Formular
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), laden Sie ein Titelbild Ihres Führerscheins hoch.
