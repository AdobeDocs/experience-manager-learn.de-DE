---
title: Benutzer mit OTP überprüfen
description: Überprüfen Sie mithilfe von OTP die mit der App-Nummer verknüpfte Mobiltelefonnummer.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 2%

---

# Benutzer mit OTP überprüfen

SMS Two Factor Authentication (Dual Factor Authentication) ist ein Verfahren zur Überprüfung der Sicherheit, das durch die Anmeldung eines Benutzers auf einer Website, Software oder Anwendung ausgelöst wird. Beim Anmeldungsprozess erhält der Benutzer automatisch eine SMS mit einem eindeutigen numerischen Code an seine Mobiltelefonnummer.

Es gibt eine Reihe von Organisationen, die diesen Dienst bereitstellen. Solange sie über gut dokumentierte REST-APIs verfügen, können Sie AEM Forms mithilfe der Datenintegrationsfunktionen von AEM Forms einfach integrieren. Im Rahmen dieses Tutorials habe ich [Nexmo](https://developer.nexmo.com/verify/overview) um den Anwendungsfall für SMS 2FA zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um den SMS 2FA mit AEM Forms mithilfe des Nexmo Verify-Dienstes zu implementieren.

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/sign-in). Notieren Sie sich den API-Schlüssel und den API-Geheimschlüssel. Diese Schlüssel sind erforderlich, um REST-APIs des Nexmo-Dienstes aufzurufen.

## Erstellen der Swagger/OpenAPI-Datei

Die OpenAPI-Spezifikation (früher Swagger Specification) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie Ihre gesamte API beschreiben, einschließlich:

* Verfügbare Endpunkte (/users) und Vorgänge für jeden Endpunkt (GET /users, POST /users)
* Aktionsparameter Eingabe und Ausgabe für jede Vorgangsauthentifizierungsmethode
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und sonstige Informationen.
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist für Menschen und Maschinen leicht zu erlernen und lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, folgen Sie dem [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms unterstützt OpenAPI Specification Version 2.0 (fka Swagger).

Verwenden Sie die [Swagger-Editor](https://editor.swagger.io/) , um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die mit SMS gesendeten OTP-Code senden und überprüfen. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann von heruntergeladen werden. [here](assets/two-factore-authentication-swagger.zip)

## Datenquelle erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [REST-basierte Datenquelle mit der Swagger-Datei](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration. Die abgeschlossene Datenquelle wird Ihnen als Teil dieses Kurs-Assets bereitgestellt.

## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodelle](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Ein Formulardatenmodell nutzt Datenquellen für den Datenaustausch.
Das ausgefüllte Formulardatenmodell kann [heruntergeladen von hier](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
