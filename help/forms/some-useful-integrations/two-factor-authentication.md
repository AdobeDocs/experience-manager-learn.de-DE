---
title: SMS-Zwei-Faktor-Authentifizierung
description: Fügen Sie eine zusätzliche Sicherheitsebene hinzu, um die Identität eines Benutzers zu bestätigen, wenn er bestimmte Aktivitäten durchführen möchte
feature: Adaptive Formulare
version: 6.4,6.5
kt: 6317
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 3%

---



# Benutzer anhand ihrer Mobiltelefonnummern überprüfen

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

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [eine Datenquelle ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration erstellen.

## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Ein Formulardatenmodell nutzt Datenquellen für den Datenaustausch.
Das ausgefüllte Formulardatenmodell kann [von hier heruntergeladen werden](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Adaptives Formular erstellen

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um die vom Benutzer im Formular eingegebene Mobiltelefonnummer zu überprüfen. Sie können Ihr eigenes adaptives Formular erstellen und den Aufruf des Formulardatenmodells verwenden, um OTP-POST gemäß Ihren Anforderungen zu senden und zu überprüfen.

Wenn Sie die Beispiel-Assets mit Ihren API-Schlüsseln verwenden möchten, führen Sie die folgenden Schritte aus:

* [Laden Sie das ](assets/sms-2fa-fdm.zip) Formulardatenmodell herunter und importieren Sie es mithilfe des  [Paketmanagers in AEM.](http://localhost:4502/crx/packmgr/index.jsp)
* Laden Sie das adaptive Beispielformular herunter, indem Sie [es von hier herunterladen](assets/sms-2fa-verification-af.zip). Dieses Beispielformular verwendet die Dienstaufrufe des Formulardatenmodells, die als Teil dieses Artikels bereitgestellt werden.
* Importieren Sie das Formular aus der [Forms- und Document-Benutzeroberfläche](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) in AEM.
* Öffnen Sie das Formular im Bearbeitungsmodus. Öffnen Sie den Regeleditor für das folgende Feld.

![sms-send](assets/check-sms.PNG)

* Bearbeiten Sie die mit dem Feld verknüpfte Regel. Bereitstellen der entsprechenden API-Schlüssel
* Speichern Sie das Formular
* [Vorschau des ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) Formulars anzeigen und Funktionalität testen


