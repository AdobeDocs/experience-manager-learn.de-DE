---
title: Überprüfen von Benutzenden mit OTP
description: Überprüfen Sie mithilfe von OTP die mit der Anwendungsnummer verknüpfte Mobiltelefonnummer.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 104
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '403'
ht-degree: 100%

---

# Überprüfen von Benutzenden mit OTP

Die SMS-Zwei-Faktor-Authentifizierung (SMS 2FA) ist ein Sicherheitsprüfverfahren, das durch die Anmeldung einer Benutzerin oder eines Benutzers bei einer Website, Software oder Anwendung ausgelöst wird. Beim Anmeldevorgang erhält die Benutzerin bzw. der Benutzer über die angegebene Mobiltelefonnummer automatisch eine SMS mit einem eindeutigen numerischen Code.

Es gibt eine Reihe von Unternehmen und Organisationen, die diesen Dienst bereitstellen. Sofern diese über gut dokumentierte REST-APIs verfügen, können Sie AEM Forms mithilfe der Datenintegrationsfunktionen problemlos integrieren. Im Rahmen dieses Tutorials wird [Nexmo](https://developer.nexmo.com/verify/overview) verwendet, um den SMS 2FA-Anwendungsfall zu demonstrieren.

Die folgenden Schritte wurden ausgeführt, um das SMS 2FA-Verfahren mit AEM Forms über den Nexmo Verify-Dienst zu implementieren.

## Erstellen eines Entwicklerkontos

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/sign-in). Notieren Sie sich den API-Schlüssel und den geheimen API-Schlüssel. Diese Schlüssel sind erforderlich, um REST-APIs des Nexmo-Dienstes aufzurufen.

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

Verwenden Sie den [Swagger-Editor](https://editor.swagger.io/), um Ihre Swagger-Datei zu erstellen und die Vorgänge zu beschreiben, bei denen per SMS ein OTP-Code gesendet und überprüft wird. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann [hier](assets/two-factore-authentication-swagger.zip) heruntergeladen werden.

## Erstellen einer Datenquelle

Um AEM/AEM Forms in Drittanbieteranwendungen zu integrieren, ist eine [REST-basierte Datenquelle unter Verwendung der Swagger-Datei](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=de) in der Cloud-Service-Konfiguration erforderlich. Die fertige Datenquelle wird Ihnen als Teil dieser Kurs-Assets zur Verfügung gestellt.

## Erstellen eines Formulardatenmodells

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen von und Arbeiten mit [Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Ein Formulardatenmodell benötigt Datenquellen für den Datenaustausch.
Das fertige Formulardatenmodell kann [hier](assets/sms-2fa-fdm.zip) heruntergeladen werden.

![FDM](assets/2FA-fdm.PNG)

[Erstellen des Hauptformulars](./create-the-main-adaptive-form.md)
