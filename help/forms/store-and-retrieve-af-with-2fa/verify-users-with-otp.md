---
title: Benutzer mit OTP überprüfen
description: Überprüfen Sie die mit der Anwendungsnummer verknüpfte Mobiltelefonnummer mithilfe von OTP.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# Benutzer mit OTP überprüfen

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

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [REST-basierte Datenquelle mit der Swagger-Datei](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration verwenden. Die abgeschlossene Datenquelle wird Ihnen als Teil dieser Kursmaterialien zur Verfügung gestellt.

## Formulardatenmodell erstellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Arbeiten mit [Formulardatenmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Ein Formulardatenmodell nutzt für den Datenaustausch Datenquellen.
Das ausgefüllte Formulardatenmodell kann [von hier heruntergeladen werden](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
