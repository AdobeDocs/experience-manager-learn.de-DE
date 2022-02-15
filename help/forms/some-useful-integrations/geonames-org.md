---
title: Kaskadierende Dropdown-Listen
description: Füllen von Dropdownlisten basierend auf einer vorherigen Dropdown-Listenauswahl.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Kaskadierende Dropdown-Listen

Eine Dropdown-Liste für die Kaskadierung ist eine Reihe von abhängigen DropDownList-Steuerelementen, in denen ein DropDownList-Steuerelement vom übergeordneten oder vorherigen DropDownList-Steuerelement abhängt. Die Elemente im DropDownList-Steuerelement werden basierend auf einem Element gefüllt, das vom Benutzer aus einem anderen DropDownList-Steuerelement ausgewählt wird.

## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

Im Rahmen dieses Tutorials habe ich [Geonames REST API](http://api.geonames.org/) , um diese Funktion zu demonstrieren.
Es gibt eine Reihe von Organisationen, die diese Art von Dienst bereitstellen. Solange sie über gut dokumentierte REST-APIs verfügen, können Sie mit der Datenintegrationsfunktion einfach in AEM Forms integrieren

Die folgenden Schritte wurden ausgeführt, um kaskadierende Dropdown-Listen in AEM Forms zu implementieren

## Entwicklerkonto erstellen

Erstellen Sie ein Entwicklerkonto mit [Geonames](https://www.geonames.org/login). Notieren Sie sich den Benutzernamen. Dieser Benutzername ist erforderlich, um REST-APIs der geonames.org aufzurufen.

## Erstellen der Swagger/OpenAPI-Datei

Die OpenAPI-Spezifikation (früher Swagger Specification) ist ein API-Beschreibungsformat für REST-APIs. Mit einer OpenAPI-Datei können Sie Ihre gesamte API beschreiben, einschließlich:

* Verfügbare Endpunkte (/users) und Vorgänge für jeden Endpunkt (GET /users, POST /users)
* Aktionsparameter Eingabe und Ausgabe für jede Vorgangsauthentifizierungsmethode
* Kontaktinformationen, Lizenz, Nutzungsbedingungen und sonstige Informationen.
* API-Spezifikationen können in YAML oder JSON geschrieben werden. Das Format ist für Menschen und Maschinen leicht zu erlernen und lesbar.

Um Ihre erste Swagger/OpenAPI-Datei zu erstellen, folgen Sie dem [OpenAPI-Dokumentation](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms unterstützt OpenAPI Specification Version 2.0 (FKA Swagger).

Verwenden Sie die [Swagger-Editor](https://editor.swagger.io/) um Ihre Swagger-Datei zu erstellen, um die Vorgänge zu beschreiben, die alle Länder und untergeordneten Elemente des Landes oder Staates abrufen. Die Swagger-Datei kann im JSON- oder YAML-Format erstellt werden. Die fertige Swagger-Datei kann von heruntergeladen werden. [here](assets/swagger-files.zip)
Die Swagger-Dateien beschreiben die folgende REST-API
* [Alle Länder abrufen](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Abrufen von untergeordneten Elementen des Geoname-Objekts](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## Data Sources erstellen

Um AEM/AEM Forms mit Drittanbieteranwendungen zu integrieren, müssen wir [Datenquelle erstellen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) in der Cloud-Services-Konfiguration. Bitte verwenden Sie [Swagger-Dateien](assets/swagger-files.zip) , um Ihre Datenquellen zu erstellen.
Sie müssen zwei Datenquellen erstellen (eine zum Abrufen aller Länder und eine andere zum Abrufen untergeordneter Elemente).


## Erstellen von Formulardatenmodellen

Die AEM Forms-Datenintegration bietet eine intuitive Benutzeroberfläche zum Erstellen und Verwenden von [Formulardatenmodelle](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Stützen Sie das Formulardatenmodell auf die Datenquellen, die im vorherigen Schritt erstellt wurden. Formulardatenmodell mit 2 Datenquellen

![fdm](assets/geonames-fdm.png)


## Adaptives Formular erstellen

Integrieren Sie die GET Aufrufe des Formulardatenmodells in Ihr adaptives Formular, um die Dropdown-Listen zu füllen.
Erstellen Sie ein adaptives Formular mit 2 Dropdownlisten. Eins zur Auflistung der Länder und eins zur Auflistung der Bundesländer/-provinzen je nach Land.

### Dropdownliste &quot;Länder ausfüllen&quot;

Die Länderliste wird ausgefüllt, wenn das Formular zum ersten Mal initialisiert wird. Der folgende Screenshot zeigt den Regeleditor, der zum Ausfüllen der Optionen der Dropdown-Liste &quot;Land&quot;konfiguriert ist. Dazu müssen Sie Ihren Benutzernamen mit dem Geonames-Konto angeben.
![get-countries](assets/get-countries-rule-editor.png)

#### Füllen der Dropdownliste Bundesland/Provinz

Die Dropdown-Liste Bundesland muss auf der Grundlage des ausgewählten Landes ausgefüllt werden. Der folgende Screenshot zeigt die Konfiguration des Regeleditors
![state-provinze-options](assets/state-province-options.png)

### Übung

Fügen Sie im Formular 2 Dropdownlisten mit dem Namen &quot;Counties and Cities&quot;hinzu, um je nach ausgewähltem Land und Bundesland die Bezirke und Städte aufzulisten.
![Übung](assets/cascading-drop-down-exercise.png)





