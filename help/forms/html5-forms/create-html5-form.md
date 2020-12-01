---
title: HTML5-Forms erstellen
description: Erstellen und Konfigurieren von HTML5-Formularen
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 11%

---


# Erstellen von HTML5-Formularen

HTML5-Formulare sind eine neue Funktion in Adobe Experience Manager, mit der Angebot XFA-Formularvorlagen(xdp) im HTML5-Format wiedergeben können. Diese Funktion ermöglicht das Anzeigen von Formularen auf mobilen Geräten und Desktop-Browsern, auf denen XFA-basierte PDF nicht unterstützt werden. „HTML5-Formulare“ Forms unterstützt nicht nur vorhandene Funktionen XFA-basierter Formularvorlagen, sondern bietet auch neue Funktionen für mobile Geräte wie die Scribble-Signatur.

## Voraussetzung

Vergewissern Sie sich bitte, dass Sie eine funktionierende Instanz von AEM Forms haben. Folgen Sie dem [Installationshandbuch](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html), um AEM Forms zu installieren und zu konfigurieren.

## Erstellen des ersten HTML5-Formulars

1. [Laden Sie den Inhalt der ZIP-Datei](assets/assets.zip) herunter und extrahieren Sie ihn. Die ZIP-Datei enthält xdp und die Datendatei
2. [Navigieren Sie zu Forms und Dokumenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klicken Sie auf Erstellen -> Datei-Upload
4. Wählen Sie die in Schritt 2 heruntergeladene xdp-Vorlage aus.

## Vorschau als HTML

Die xdp-Datei kann im HTML5- oder PDF-Format in der Vorschau angezeigt werden. Gehen Sie wie folgt vor, um die xdp-Datei im HTML5-Format Vorschau

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau als HTML_. Sie sollten den xdp sehen, der als HTML5 gerendert wird

>[!NOTE]
>Wenn Sie die Option _Vorschau als PDF_ auswählen, wird die gerenderte PDF nicht im Browser angezeigt, da AEM Forms dynamische PDFs wiedergibt, für die Acrobat-Plugin erforderlich ist.Sie müssen die PDF-Datei herunterladen und mit Adobe Acrobat/Reader zur Ansicht öffnen


## Vorschau mit Daten

Gehen Sie wie folgt vor, um die xdp-Datei im HTML5-Format mit der Datendatei Vorschau:

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau mit Daten_. Suchen Sie die Datendatei und wählen Sie sie aus und klicken Sie auf _Vorschau_.
* Sie sollten eine Vorlage sehen, die im HTML5-Format wiedergegeben wird und mit den Daten vorausgefüllt ist

## Erweiterte Eigenschaften der xdp-Vorlage

Mit den erweiterten Eigenschaften der xdp-Vorlage können Sie Veröffentlichungsdatum, Übermittlungs-Handler, Rendering-Profil für Ihr Formular, Vorfülldienst usw. angeben. Um die erweiterten Eigenschaften der Vorlage Ansicht, tippen Sie auf den xdp und klicken Sie auf _Eigenschaften -> Erweitert_. Hier finden Sie eine Reihe von Immobilien. Einige dieser Eigenschaften werden hier behandelt.

**Sende-URL** : Diese URL verarbeitet die Übermittlung des HTML5-Formulars. Wir werden dies in der nächsten Lektion behandeln. Wenn hier keine Sende-URL angegeben ist, wird der standardmäßige Sende-Handler aufgerufen, der die Formulardaten an den Browser zurückgibt.

**HTML-Render-Profil**  - HTML5-Formulare haben die Vorstellung von Profilen, die als REST-Endpunkte bereitgestellt werden, um die mobile Wiedergabe von Formularvorlagen zu aktivieren. Die Mehrzahl der Profile, bei denen das Standard-Rendering ausreicht, um das Formular wiederzugeben. Wenn das standardmäßige Render-Profil Ihren Anforderungen nicht entspricht, kann ein [benutzerdefiniertes Profil](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) erstellt und mit dem Formular verknüpft werden.

**Dienst**  zum Vorausfüllen: Der Dienst zum Vorausfüllen wird in der Regel verwendet, um Ihr Formular mit Daten zu füllen, die von einer Back-End-Datenquelle abgerufen werden.

