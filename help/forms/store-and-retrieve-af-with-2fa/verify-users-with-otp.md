---
title: Benutzer mit OTP überprüfen
description: Überprüfen Sie mithilfe von OTP die mit der App-Nummer verknüpfte Mobiltelefonnummer.
feature: Adaptive Formulare
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---



# Benutzer mit OTP überprüfen

SMS Two Factor Authentication (Dual Factor Authentication) ist ein Verfahren zur Überprüfung der Sicherheit, das durch die Anmeldung eines Benutzers auf einer Website, Software oder Anwendung ausgelöst wird. Beim Anmeldungsprozess erhält der Benutzer automatisch eine SMS mit einem eindeutigen numerischen Code an seine Mobiltelefonnummer.

Es gibt eine Reihe von Organisationen, die diesen Dienst bereitstellen. Solange sie über gut dokumentierte REST-APIs verfügen, können Sie AEM Forms mithilfe der Datenintegrationsfunktionen von AEM Forms einfach integrieren. Im Rahmen dieses Tutorials habe ich [Nexmo](https://developer.nexmo.com/verify/overview) verwendet, um den Anwendungsfall für SMS 2FA zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um den SMS 2FA mit AEM Forms mithilfe des Nexmo Verify-Dienstes zu implementieren.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/sign-in). Notieren Sie sich den API-Schlüssel und den API-Geheimschlüssel. Diese Schlüssel sind erforderlich, um REST-APIs des Nexmo-Dienstes aufzurufen.

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

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die mit SMS gesendeten OTP-Code senden und überprüfen. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann von [hier](assets/two-factore-authentication-swagger.zip) heruntergeladen werden

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir eine [REST-basierte Datenquelle verwenden, die die Swagger-Datei](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud Services-Konfiguration verwendet. Die abgeschlossene Datenquelle wird Ihnen als Teil dieses Kurs-Assets bereitgestellt.

## Formulardatenmodell erstellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodellen](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Ein Formulardatenmodell nutzt Datenquellen für den Datenaustausch.
Das ausgefüllte Formulardatenmodell kann [von hier heruntergeladen werden](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
