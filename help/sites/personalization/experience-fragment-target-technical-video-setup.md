---
title: Einrichten von Experience Fragments und Adobe Target-Integration in AEM
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketingexperten Inhalte kanalübergreifend nahtlos testen und personalisieren.
seo-description: Adobe Experience Manager 6.4 reimagines the personalization workflow between AEM and Target. Experiences created within AEM can now be delivered directly to Adobe Target as HTML Offers. It allows Marketers to seamlessly test and personalize content across different channels.
feature: Experience Fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 2%

---

# Einrichten von Experience Fragments und Adobe Target-Integration{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketingexperten Inhalte kanalübergreifend nahtlos testen und personalisieren.

>[!VIDEO](https://video.tv.adobe.com/v/22380?quality=12&learn=on)

>[!NOTE]
>
>Es wird empfohlen, die at.js-Client-Bibliothek zu verwenden. Es empfiehlt sich daher, Tag-Management-Lösungen wie Launch by Adobe, Adobe DTM oder beliebige Drittanbieter-Tag-Management-Lösungen zu verwenden, um Ihren Siteseiten Zielbibliotheken hinzuzufügen

* Die auf den Experience Fragment-Ordner angewendete Target Cloud-Dienstkonfiguration erbt alle Experience Fragments, die direkt im übergeordneten Ordner erstellt wurden. Der untergeordnete Ordner übernimmt nicht die übergeordnete Cloud Services-Konfiguration.
* Der Target-Client-Code kann unter Adobe Experience Cloud > Launch-Target > unter &quot;Registerkarte &quot;Einrichten&quot;> &quot;Implementierung&quot;> &quot;at.js-Einstellungen bearbeiten&quot;abgerufen werden.
* Benutzername und Kennwort der Target-API können Sie abrufen, indem Sie ein Ticket mit einer Anfrage an die Kundenunterstützung senden, um die Funktion für die Experience Fragment-Target-Integration zu aktivieren.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zu Experience Fragments](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Verwenden von Experience Fragments](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
