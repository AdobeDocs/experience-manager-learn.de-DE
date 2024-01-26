---
title: OCR-Datenextraktion
description: Extrahieren Sie Daten aus staatlich ausgestellten Dokumenten, um Formulare auszufüllen.
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 179
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 100%

---

# OCR-Datenextraktion

Sie können automatisch Daten aus einer Vielzahl von staatlich ausgestellten Dokumenten extrahieren, um Ihre adaptiven Formulare zu befüllen.

Es gibt eine Reihe von Organisationen, die diesen Dienst bereitstellen. Sofern diese über gut dokumentierte REST-APIs verfügen, können Sie AEM Forms mithilfe der Datenintegrationsfunktionen problemlos integrieren. Im Rahmen dieses Tutorials wird [ID Analyzer](https://www.idanalyzer.com/) verwendet, um die OCR-Datenextraktion hochgeladener Dokumente zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um die OCR-Datenextraktion mit AEM Forms mithilfe des ID Analyzer-Diensts zu implementieren.

## Erstellen eines Entwicklerkontos

Erstellen Sie ein Entwicklerkonto mit [ID Analyzer](https://portal.idanalyzer.com/signin.html). Notieren Sie sich den API-Schlüssel. Dieser Schlüssel ist erforderlich, um REST-APIs des ID Analyzer-Diensts aufzurufen.

## Erstellen einer Swagger/OpenAPI-Datei

Die OpenAPI-Spezifikation (früher Swagger-Spezifikation) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie Ihre gesamte API beschreiben, einschließlich:

* verfügbarer Endpunkte (/users) und der Vorgänge für jeden Endpunkt (GET /users, POST /users)
* Aktionsparameter-Eingabe und -Ausgabe für jeden Vorgang 
Authentifizierungsmethoden
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und sonstiger Informationen
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist sowohl für Menschen als auch für Maschinen leicht zu erlernen und lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, befolgen Sie die [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/).

>[!NOTE]
> AEM Forms unterstützt die OpenAPI-Spezifikationsversion 2.0 (früher Swagger).

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen und die Vorgänge zu beschreiben, bei denen per SMS ein OTP-Code gesendet und überprüft wird. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann [hier](assets/drivers-license-swagger.zip) heruntergeladen werden.

## Aspekte beim Definieren der Swagger-Datei

* Definitionen sind erforderlich.
* $ref muss für Methodendefinitionen verwendet werden.
* Ziehen Sie es vor, Verbrauchs- und Produktionsabschnitte zu definieren.
* Definieren Sie keine Inline-Anfragetext- oder -Antwortparameter. Versuchen Sie, so viel wie möglich zu modularisieren. So wird etwa die folgende Definition nicht unterstützt:

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

Folgendes wird mit einem Verweis auf die requestBody-Definition unterstützt.

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Beispiel einer Swagger-Datei für Ihre Referenz](assets/sample-swagger.json)

## Erstellen einer Datenquelle

Um AEM/AEM Forms in Drittanbieteranwendungen zu integrieren, ist die [Erstellung einer Datenquelle](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=de) in der Cloud-Service-Konfiguration erforderlich. Verwenden Sie die [Swagger-Datei](assets/drivers-license-swagger.zip), um Ihre Datenquelle zu erstellen.

## Erstellen eines Formulardatenmodells

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen von und Arbeiten mit [Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Stützen Sie das Formulardatenmodell auf die Datenquelle, die im vorherigen Schritt erstellt wurde.

![FDM](assets/test-dl-fdm.PNG)

## Erstellen einer Client-Bibliothek

Wir müssen eine base64-codierte Zeichenfolge des hochgeladenen Dokuments abrufen. Diese base64-codierte Zeichenfolge wird dann als ein Parameter unseres REST-Aufrufs weitergeben.
Die Client-Bibliothek kann [hier](assets/drivers-license-client-lib.zip) heruntergeladen werden.

## Erstellen eines adaptiven Formulars

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um Daten aus dem von der Person hochgeladenen Dokument für das Formular zu extrahieren. Sie können Ihr eigenes adaptives Formular erstellen und den POST-Aufruf des Formulardatenmodells verwenden, um die base64-codierte Zeichenfolge des hochgeladenen Dokuments zu senden.

## Bereitstellen auf Ihrem Server

Wenn Sie die Beispiel-Assets mit Ihrem API-Schlüssel verwenden möchten, gehen Sie wie folgt vor:

* [Laden Sie die Datenquelle herunter](assets/drivers-license-source.zip) und importieren Sie sie mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) in AEM.
* [Laden Sie das Formulardatenmodell herunter](assets/drivers-license-fdm.zip) und importieren Sie es mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) in AEM.
* [Laden Sie die Client-Bibliothek herunter.](assets/drivers-license-client-lib.zip)
* Laden Sie das adaptive Beispielformular [hier](assets/adaptive-form-dl.zip) herunter. Dieses Beispielformular nutzt die Dienstaufrufe des Formulardatenmodells, das im Rahmen dieses Artikels bereitgestellt wird.
* Importieren Sie das Formular über die Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) in AEM.
* Öffnen Sie das Formular im [Bearbeitungsmodus](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html).
* Geben Sie Ihren API-Schlüssel als Standardwert im Feld „apikey“ an und speichern Sie Ihre Änderungen.
* Öffnen Sie den Regeleditor für das Feld für die base64-Zeichenfolge. Achten Sie auf den Dienstaufruf, wenn der Wert dieses Felds geändert wird.
* Speichern Sie das Formular
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled) und laden Sie ein Bild von der Vorderseite Ihres Personalausweises hoch.
