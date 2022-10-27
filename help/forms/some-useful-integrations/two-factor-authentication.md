---
title: SMS-Zwei-Faktor-Authentifizierung
description: Fügen Sie eine zusätzliche Sicherheitsebene hinzu, um die Identität eines Benutzers zu bestätigen, wenn er bestimmte Aktivitäten durchführen möchte
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 4%

---

# Benutzer anhand ihrer Mobiltelefonnummern überprüfen

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

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [Datenquelle erstellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration.

## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodelle](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Ein Formulardatenmodell nutzt Datenquellen für den Datenaustausch.
Das ausgefüllte Formulardatenmodell kann [heruntergeladen von hier](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um die vom Benutzer im Formular eingegebene Mobiltelefonnummer zu überprüfen. Sie können Ihr eigenes adaptives Formular erstellen und den Aufruf des Formulardatenmodells verwenden, um OTP-POST gemäß Ihren Anforderungen zu senden und zu überprüfen.

Wenn Sie die Beispiel-Assets mit Ihren API-Schlüsseln verwenden möchten, führen Sie die folgenden Schritte aus:

* [Herunterladen des Formulardatenmodells](assets/sms-2fa-fdm.zip) und importieren Sie in AEM mithilfe von [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Das adaptive Beispielformular herunterladen kann [heruntergeladen von hier](assets/sms-2fa-verification-af.zip). Dieses Beispielformular verwendet die Dienstaufrufe des Formulardatenmodells, die als Teil dieses Artikels bereitgestellt werden.
* Importieren Sie das Formular in AEM aus dem [Benutzeroberfläche von Forms und Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Öffnen Sie das Formular im Bearbeitungsmodus. Öffnen Sie den Regeleditor für das folgende Feld.

![sms-send](assets/check-sms.PNG)

* Bearbeiten Sie die mit dem Feld verknüpfte Regel. Bereitstellen der entsprechenden API-Schlüssel
* Speichern Sie das Formular
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) und testen Sie die Funktionalität
