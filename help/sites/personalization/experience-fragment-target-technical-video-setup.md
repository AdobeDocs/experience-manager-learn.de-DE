---
title: Einrichten von Experience Fragments und Adobe Target-Integration in AEM
description: Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketing-Fachleute Inhalte kanalübergreifend nahtlos testen und personalisieren.
feature: Experience Fragments
version: 6.4, 6.5
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
doc-type: Technical Video
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
duration: 257
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 100%

---

# Einrichten von Experience Fragments und Adobe Target-Integration{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketing-Fachleute Inhalte kanalübergreifend nahtlos testen und personalisieren.

>[!VIDEO](https://video.tv.adobe.com/v/22380?quality=12&learn=on)

>[!NOTE]
>
>Es wird empfohlen, die at.js-Client-Bibliothek zu verwenden. Die Best Practise besteht darin, Tag-Management-Lösungen wie Adobe Experience Platform Launch, Adobe DTM oder beliebige Drittanbieter-Tag-Management-Lösungen zu verwenden, um Ihren Site-Seiten Zielbibliotheken hinzuzufügen

* Die auf den Experience Fragment-Ordner angewendete Target Cloud Service-Konfiguration erbt alle Experience Fragments, die direkt unter dem übergeordneten Ordner erstellt wurden. Der untergeordnete Ordner übernimmt nicht die übergeordnete Cloud Service-Konfiguration.
* Der Target-Client-Code kann unter „Adobe Experience Cloud“ > „Launch Target“ > unter der Registerkarte „Einrichtung“> „Implementierung“ > „at.js-Einstellungen bearbeiten“ abgerufen werden.
* Den Benutzernamen und das Passwort der Target-API können Sie abrufen, indem Sie ein Ticket mit einer Anfrage an die Kundenunterstützung senden, um die Funktion für die Experience Fragment-Target-Integration zu aktivieren.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zu Experience Fragments](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Verwenden von Experience Fragments](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
