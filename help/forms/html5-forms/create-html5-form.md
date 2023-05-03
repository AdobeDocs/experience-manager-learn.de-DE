---
title: HTML5 Forms erstellen
description: Erstellen und Konfigurieren von HTML5-Formularen
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 2%

---

# Erstellen von HTML5-Formularen

HTML5 forms ist eine neue Funktion in Adobe Experience Manager, die die Wiedergabe von XFA-Formularvorlagen (xdp) im HTML5-Format ermöglicht. Diese Funktion ermöglicht die Wiedergabe von Formularen auf Mobilgeräten und Desktopbrowsern, auf denen XFA-basierte PDF nicht unterstützt werden. HTML5 forms unterstützt nicht nur die vorhandenen Funktionen von XFA-Formularvorlagen, sondern bietet auch neue Funktionen wie die Freihandsignatur für Mobilgeräte.

## Voraussetzung

Stellen Sie sicher, dass Sie über eine funktionierende Instanz von AEM Forms verfügen. Bitte folgen Sie dem [Installationshandbuch](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) Installieren und Konfigurieren von AEM Forms

## Erstellen Sie Ihr erstes HTML5-Formular

1. [Herunterladen und Extrahieren des Inhalts der ZIP-Datei](assets/assets.zip). Die ZIP-Datei enthält xdp und die Datendatei
2. [Navigieren zu Forms und Dokumenten](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Klicken Sie auf Erstellen > Datei-Upload .
4. Wählen Sie die in Schritt 2 heruntergeladene xdp-Vorlage aus.

## Vorschau als HTML

Die xdp kann im HTML5-Format oder im PDF-Format in der Vorschau angezeigt werden. Gehen Sie wie folgt vor, um eine Vorschau des XDP im HTML5-Format anzuzeigen

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau als HTML anzeigen_. Die xdp sollte als HTML5 gerendert werden.

>[!NOTE]
>Wenn Sie _Vorschau als PDF_ -Option wird die gerenderte PDF nicht im Browser angezeigt, da AEM Forms dynamische PDFs rendert, die das Acrobat-Plug-in erfordern. Sie müssen die PDF herunterladen und mit Adobe Acrobat/Reader öffnen, um sie anzuzeigen


## Vorschau mit Daten

Gehen Sie wie folgt vor, um eine Vorschau des XDP im HTML5-Format mit einer Datendatei anzuzeigen:

* Tippen Sie auf die neu hochgeladene xdp und klicken Sie auf _Vorschau -> Vorschau mit Daten_. Durchsuchen und wählen Sie die Datendatei aus und klicken Sie auf _Vorschau_.
* Sie sollten eine Vorlage sehen, die im HTML 5-Format gerendert wird und mit den Daten vorausgefüllt ist

## Erweiterte Eigenschaften der xdp-Vorlage durchsuchen

Die erweiterten Eigenschaften der xdp-Vorlage ermöglichen es Ihnen, Veröffentlichungsdatum, Sende-Handler, Renderprofil für Ihr Formular, Vorbefüllungs-Service usw. anzugeben. Um die erweiterten Eigenschaften der Vorlage anzuzeigen, tippen Sie auf die XDP-Datei und klicken Sie auf _properties -> Erweitert_. Hier finden Sie eine Reihe von Immobilien. Einige dieser Eigenschaften werden hier behandelt.

**Sende-URL** - Dies ist die URL, die die Übermittlung Ihres HTML5-Formulars verarbeitet. Wir werden dies in der nächsten Lektion behandeln. Wenn hier keine Sende-URL angegeben ist, wird der standardmäßige Sende-Handler aufgerufen, der die Formulardaten an den Browser zurückgibt.

**HTML Render Profile** - HTML5-Formulare haben den Begriff &quot;Profile&quot;, die als REST-Endpunkte verfügbar gemacht werden, um die mobile Wiedergabe von Formularvorlagen zu ermöglichen. Die Mehrzahl der Fälle, in denen das standardmäßige Renderprofil ausreichend sein sollte, um das Formular wiederzugeben. Wenn das standardmäßige Renderprofil Ihre Anforderungen nicht erfüllt, wird ein [Benutzerdefiniertes Profil](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) kann erstellt und mit dem Formular verknüpft werden.

**Vorbefüllungs-Dienst** - Der Vorbefüllungs-Dienst wird normalerweise verwendet, um Ihr Formular mit Daten zu füllen, die aus einer Backend-Datenquelle abgerufen werden.
