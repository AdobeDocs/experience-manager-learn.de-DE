---
title: Erstellen von HTML5-Formularen
description: Erstellen und Konfigurieren von HTML5-Formularen
feature: Mobile Forms
doc-type: article
version: 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 133
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 100%

---

# Erstellen von HTML5-Formularen

„HTML5-Formulare“ ist eine neue Funktion in Adobe Experience Manager, die das Rendern von XFA-Formularvorlagen (XDP) im HTML5-Format ermöglicht. Diese Funktion ermöglicht das Rendern von Formularen auf Mobilgeräten und Desktop-Browsern, auf denen XFA-basierte PDF nicht unterstützt werden. „HTML5-Formulare“ unterstützt nicht nur vorhandene Funktionen XFA-basierter Formularvorlagen, sondern bietet auch neue Funktionen für mobile Geräte wie die Scribble-Signatur.

## Voraussetzung

Stellen Sie sicher, dass Sie über eine funktionierende AEM Forms-Instanz verfügen. Folgen Sie dem [Installationshandbuch](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=de), um AEM Forms zu installieren und zu konfigurieren.

## Erstellen Ihres ersten HTML5-Formulars

1. [Laden Sie die ZIP-Datei herunter und extrahieren Sie deren Inhalt](assets/assets.zip). Die ZIP-Datei enthält eine XDP- und eine Datendatei.
2. [Navigieren Sie zu „Formulare und Dokumente“](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
3. Klicken Sie auf „Erstellen“ > „Datei hochladen“.
4. Wählen Sie die in Schritt 2 heruntergeladene XDP-Vorlage aus.

## Anzeigen als HTML-Vorschau

Die XDP-Datei kann im HTML5- oder PDF-Format in einer Vorschau angezeigt werden. Gehen Sie wie folgt vor, um eine Vorschau der XDP-Datei im HTML5-Format anzuzeigen.

* Klicken Sie auf die neu hochgeladene XDP-Datei und dann auf _Vorschau > Vorschau als HTML_. Die XDP-Datei sollte als HTML5 gerendert werden.

>[!NOTE]
>Wenn Sie die Option _Vorschau als PDF_ auswählen, wird die gerenderte PDF nicht im Browser angezeigt, da AEM Forms dynamische PDF-Dateien rendert, für die das Acrobat-Plug-in erforderlich ist. Zur Anzeige müssen Sie die PDF-Datei herunterladen und mit Adobe Acrobat/Reader öffnen.


## Vorschau mit Daten

Gehen Sie wie folgt vor, um eine Vorschau der XDP-Datei im HTML5-Format mit einer Datendatei anzuzeigen:

* Klicken Sie auf die neu hochgeladene XDP-Datei und dann auf _Vorschau > Vorschau mit Daten_. Suchen Sie die Datendatei, wählen Sie sie aus und klicken Sie auf _Vorschau_.
* Sie sollten eine Vorlage sehen, die im HTML5-Format gerendert und mit den Daten vorausgefüllt ist.

## Erkunden erweiterter Eigenschaften der XDP-Vorlage

Die erweiterten Eigenschaften der XDP-Vorlage ermöglichen es Ihnen, das Veröffentlichungsdatum, den Übermittlungs-Handler, das Render-Profil für Ihr Formular, den Vorbefüllungsdienst usw. anzugeben. Um die erweiterten Eigenschaften der Vorlage anzuzeigen, klicken Sie auf die XDP-Datei und dann auf _Eigenschaften > Erweitert_. Hier finden Sie eine Reihe von Eigenschaften. Einige dieser Eigenschaften werden im Folgenden behandelt.

**Sende-URL**: Dies ist die URL, über die die Übermittlung Ihres HTML5-Formulars erfolgt. In der nächsten Lektion werden wir näher darauf eingehen. Wenn hier keine Sende-URL angegeben ist, wird der standardmäßige Übermittlungs-Handler aufgerufen, der die Formulardaten an den Browser zurückgibt.

**HTML-Render-Profil**: HTML5-Formulare umfassen das Konzept der Profile, die als REST-Endpunkte bereitgestellt werden, um Formularvorlagen auf Mobilgeräten rendern zu können. In den meisten Fällen sollte das standardmäßige Render-Profil zum Rendern des Formulars reichen. Wenn das standardmäßige Render-Profil Ihre Anforderungen nicht erfüllt, kann ein [benutzerdefiniertes Profil](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=de) erstellt und mit dem Formular verknüpft werden.

**Vorbefüllungsdienst**: Der Vorbefüllungsdienst wird normalerweise verwendet, um ein Formular mit Daten aufzufüllen, die aus einer Backend-Datenquelle abgerufen werden.
