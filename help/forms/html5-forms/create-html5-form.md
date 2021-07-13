---
title: HTML5 Forms erstellen
description: Erstellen und Konfigurieren von HTML5-Formularen
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Entwicklung
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 12%

---


# HTML5-Formulare erstellen

HTML5-Formulare sind eine neue Funktion in Adobe Experience Manager, die die Wiedergabe von XFA-Formularvorlagen (xdp) im HTML5-Format bietet. Diese Funktion ermöglicht das Anzeigen von Formularen auf mobilen Geräten und Desktop-Browsern, auf denen XFA-basierte PDF nicht unterstützt werden. „HTML5-Formulare“ Forms unterstützt nicht nur vorhandene Funktionen XFA-basierter Formularvorlagen, sondern bietet auch neue Funktionen für mobile Geräte wie die Scribble-Signatur.

## Voraussetzung

Stellen Sie sicher, dass Sie über eine funktionierende Instanz von AEM Forms verfügen. Folgen Sie dem [Installationshandbuch](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html), um AEM Forms zu installieren und zu konfigurieren.

## Erstellen Sie Ihr erstes HTML5-Formular

1. [Laden Sie den Inhalt der ZIP-Datei herunter und extrahieren Sie ihn](assets/assets.zip). Die ZIP-Datei enthält xdp und die Datendatei
2. [Navigieren zu Forms und Dokumenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klicken Sie auf Erstellen > Datei-Upload .
4. Wählen Sie die in Schritt 2 heruntergeladene xdp-Vorlage aus.

## Vorschau als HTML

Die xdp kann im HTML5- oder PDF-Format als Vorschau angezeigt werden. Gehen Sie wie folgt vor, um eine Vorschau des XDP im HTML5-Format anzuzeigen

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau als HTML_. Sie sollten sehen, dass xdp als HTML5 gerendert wird.

>[!NOTE]
>Wenn Sie die Option _Vorschau als PDF_ auswählen, wird die gerenderte PDF-Datei nicht im Browser angezeigt, da AEM Forms dynamische PDFs rendert, die das Acrobat-Plug-in erfordern. Sie müssen die PDF-Datei herunterladen und mit Adobe Acrobat/Reader öffnen, um sie anzuzeigen


## Vorschau mit Daten

Gehen Sie wie folgt vor, um eine Vorschau der xdp-Datei im HTML5-Format mit der Datendatei anzuzeigen:

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau mit Daten_. Suchen Sie die Datendatei und wählen Sie sie aus und klicken Sie auf _Vorschau_.
* Sie sollten eine Vorlage sehen, die im HTML5-Format gerendert und mit den Daten vorausgefüllt ist

## Erweiterte Eigenschaften der xdp-Vorlage durchsuchen

Die erweiterten Eigenschaften der xdp-Vorlage ermöglichen es Ihnen, Veröffentlichungsdatum, Sende-Handler, Renderprofil für Ihr Formular, Vorbefüllungs-Service usw. anzugeben. Um die erweiterten Eigenschaften der Vorlage anzuzeigen, tippen Sie auf die xdp und klicken Sie auf _properties -> Advanced_. Hier finden Sie eine Reihe von Immobilien. Einige dieser Eigenschaften werden hier behandelt.

**Sende-URL**  - Dies ist die URL, die die Übermittlung Ihres HTML5-Formulars verarbeitet. Wir werden dies in der nächsten Lektion behandeln. Wenn hier keine Sende-URL angegeben ist, wird der standardmäßige Sende-Handler aufgerufen, der die Formulardaten an den Browser zurückgibt.

**HTML Render Profile**  - HTML5-Formulare haben die Vorstellung von Profilen, die als REST-Endpunkte bereitgestellt werden, um die mobile Wiedergabe von Formularvorlagen zu ermöglichen. Die Mehrzahl der Fälle, in denen das standardmäßige Renderprofil ausreichend sein sollte, um das Formular wiederzugeben. Wenn das standardmäßige Renderprofil Ihre Anforderungen nicht erfüllt, kann ein [benutzerdefiniertes Profil](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) erstellt und mit dem Formular verknüpft werden.

**Vorbefüllungs-Dienst**  - Der Vorbefüllungs-Dienst wird normalerweise verwendet, um Ihr Formular mit Daten zu füllen, die aus einer Backend-Datenquelle abgerufen werden.

