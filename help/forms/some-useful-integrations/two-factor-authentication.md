---
title: SMS-Zwei-Faktor-Authentifizierung
description: Hinzufügen einer zusätzlichen Sicherheitsebene, um die Identität einer Benutzerin oder eines Benutzers beim Durchführen bestimmter Aktivitäten zu bestätigen
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 127
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 100%

---

# Überprüfen von Benutzenden anhand ihrer Mobiltelefonnummern

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

Um AEM/AEM Forms in Drittanbieteranwendungen zu integrieren, ist die [Erstellung einer Datenquelle](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=de) in der Cloud-Service-Konfiguration erforderlich. 

## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen von und Arbeiten mit [Formulardatenmodellen](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=de). Ein Formulardatenmodell benötigt Datenquellen für den Datenaustausch.
Das fertige Formulardatenmodell kann [hier](assets/sms-2fa-fdm.zip) heruntergeladen werden.

![FDM](assets/2FA-fdm.PNG)

## Erstellen eines adaptiven Formulars

Integrieren Sie die POST-Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um die von Benutzenden in das Formular eingegebenen Mobiltelefonnummern zu überprüfen. Sie können Ihr eigenes adaptives Formular erstellen und den POST-Aufruf des Formulardatenmodells verwenden, um den OTP-Code gemäß Ihren Anforderungen zu senden und zu überprüfen.

Wenn Sie die Beispiel-Assets mit Ihren API-Schlüsseln verwenden möchten, gehen Sie wie folgt vor:

* [Laden Sie das Formulardatenmodell herunter](assets/sms-2fa-fdm.zip) und importieren Sie es mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp) in AEM.
* Laden Sie das adaptive Beispielformular [hier](assets/sms-2fa-verification-af.zip) herunter. Dieses Beispielformular nutzt die Dienstaufrufe des Formulardatenmodells, das im Rahmen dieses Artikels bereitgestellt wird.
* Importieren Sie das Formular über die Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) in AEM.
* Öffnen Sie das Formular im Bearbeitungsmodus. Öffnen Sie den Regeleditor für das folgende Feld.

![SMS-Versand](assets/check-sms.PNG)

* Bearbeiten Sie die mit dem Feld verknüpfte Regel. Stellen Sie die entsprechenden API-Schlüssel bereit.
* Speichern Sie das Formular
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) und testen Sie die Funktionalität.
