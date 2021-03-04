---
title: SMS-Zwei-Faktor-Authentifizierung
description: hinzufügen einer zusätzlichen Sicherheitsschicht, um die Identität eines Benutzers zu bestätigen, wenn er bestimmte Aktivitäten durchführen möchte
feature: Adaptive Formulare
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 3%

---



# Benutzer anhand ihrer Handynummern überprüfen

SMS Two Factor Authentication (Dual Factor Authentication) ist ein Verfahren zur Überprüfung der Sicherheit, das durch einen Benutzer ausgelöst wird, der sich bei einer Website, Software oder Anwendung anmeldet. Bei der Anmeldung wird dem Benutzer automatisch eine SMS mit einem eindeutigen numerischen Code an seine Mobilfunknummer gesendet.

Es gibt eine Reihe von Organisationen, die diesen Service anbieten und solange sie über gut dokumentierte REST-APIs verfügen, können Sie AEM Forms mit den Datenintegrationsfunktionen von AEM Forms problemlos integrieren. Für die Zwecke dieses Tutorials habe ich [Nexmo](https://developer.nexmo.com/verify/overview) verwendet, um den SMS 2FA-Anwendungsfall zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um die SMS 2FA mit AEM Forms mithilfe des Nexmo Verify-Dienstes zu implementieren.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/sign-in). Notieren Sie sich den API-Schlüssel und den geheimen API-Schlüssel. Diese Schlüssel werden benötigt, um REST-APIs des Nexmo-Dienstes aufzurufen.

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

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die OTP-Code senden und überprüfen, der mit SMS gesendet wird. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die vollständige Swagger-Datei kann von [hier heruntergeladen werden](assets/two-factore-authentication-swagger.zip)

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [Datenquelle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration erstellen.

## Formulardatenmodell erstellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Arbeiten mit [Formulardatenmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Ein Formulardatenmodell nutzt für den Datenaustausch Datenquellen.
Das ausgefüllte Formulardatenmodell kann [von hier heruntergeladen werden](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um die vom Benutzer im Formular eingegebene Mobiltelefonnummer zu überprüfen. Sie können Ihr eigenes adaptives Formular erstellen und den POST-Aufruf des Formulardatenmodells verwenden, um OTP-Code gemäß Ihren Anforderungen zu senden und zu überprüfen.

Wenn Sie die Beispielelemente mit Ihren API-Schlüsseln verwenden möchten, führen Sie die folgenden Schritte aus:

* [Herunterladen des ](assets/sms-2fa-fdm.zip) Formulardatenmodells und Importieren in AEM mithilfe des  [Paketmanagers](http://localhost:4502/crx/packmgr/index.jsp)
* Das adaptive Beispielformular herunterladen kann [von hier heruntergeladen werden. ](assets/sms-2fa-verification-af.zip) Dieses Musterformular verwendet die Dienstaufrufe des Formulardatenmodells, das als Teil dieses Artikels bereitgestellt wird.
* Importieren Sie das Formular in AEM aus der Benutzeroberfläche [Forms und Dokument](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Öffnen Sie das Formular im Bearbeitungsmodus. Öffnen Sie den Regeleditor für das folgende Feld

![sms-send](assets/check-sms.PNG)

* Bearbeiten Sie die mit dem Feld verknüpfte Regel. Geben Sie die entsprechenden API-Schlüssel an
* Speichern Sie das Formular
* [Vorschau des ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) Formulars und Testen der Funktionalität


