---
title: Einrichten von Erlebnisfragmenten und Adobe Target-Integration in AEM
seo-title: Einrichten von Erlebnisfragmenten und Adobe Target-Integration in AEM
description: Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.
seo-description: Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.
sub-product: content-services
feature: Experience Fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: 'Personalisierung '
role: Administrator, Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 3%

---


# Einrichten von Erlebnisfragmenten und Adobe Target-Integration{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.

>[!VIDEO](https://video.tv.adobe.com/v/22380/?quality=9&learn=on)

>[!NOTE]
>
>Es wird empfohlen, die at.js-Client-Bibliothek zu verwenden. Es empfiehlt sich, Tag-Management-Lösungen wie Launch by Adobe, Adobe DTM oder eine Drittanbieter-Tag-Management-Lösung zu verwenden, um Ihren Siteseiten Zielgruppe-Bibliotheken hinzuzufügen.

* Die auf den Experience Fragment-Ordner angewendete Konfiguration des Zielgruppe Cloud-Dienstes erbt alle Erlebnisfragmente, die direkt im übergeordneten Ordner erstellt wurden. Die Konfiguration der übergeordneten Cloud-Dienste wird nicht vom untergeordneten Ordner übernommen.
* Zielgruppe Client-Code erhalten Sie unter Adobe Experience Cloud > Zielgruppe starten > Unter Einrichten > Implementierung > at.js-Einstellungen bearbeiten.
* Benutzername und Kennwort der Zielgruppe-API können Sie abrufen, indem Sie eine Eintrittskarte mit einer Anfrage an den Kundendienst senden, um die Funktion zur Integration der Experience Fragment-Zielgruppe zu aktivieren.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zu Erlebnisfragmenten](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Verwenden von Experience Fragments](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
