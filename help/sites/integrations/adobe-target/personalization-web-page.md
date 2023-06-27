---
title: Personalisierung des gesamten Web-Seiten-Erlebnisses
description: Erfahren Sie, wie Sie eine Target-Aktivität erstellen, um Ihre AEM Webseiten mithilfe von Adobe Target auf neue Seiten umzuleiten.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 4%

---

# Personalisierung des gesamten Web-Seiten-Erlebnisses {#personalization-fpe}

Erfahren Sie, wie Sie eine Aktivität erstellen, um die auf AEM gehosteten Seiten Ihrer Site mit Adobe Target auf eine neue Seite umzuleiten.

## Voraussetzungen

Um die vollständigen Seiten einer AEM Website zu personalisieren, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM-Website](./add-target-launch-extension.md)
1. [Auslösen eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Übersicht über das Szenario

Die WKND-Site hat ihre Homepage neu gestaltet und möchte die aktuellen Besucher der Homepage auf die neue Homepage umleiten. Gleichzeitig sollten Sie auch verstehen, wie die neu gestaltete Startseite zur Verbesserung der Benutzerinteraktion und des Umsatzes beiträgt. Marketingexperten haben die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucher auf die neue Startseite umzuleiten. Lassen Sie uns die WKND-Site-Homepage durchsuchen und erfahren, wie Sie eine Aktivität mit Adobe Target erstellen.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei Adobe Target an und navigieren Sie zur Registerkarte Aktivitäten .
1. Klicken **Aktivität erstellen** und wählen Sie **A/B-Test** activity

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die **Visual Experience Composer** Geben Sie die Aktivitäts-URL ein und klicken Sie auf **Nächste**

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach der Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Sie können der Liste neue Erlebnisse hinzufügen, indem Sie die **Erlebnis hinzufügen** Schaltfläche.

   ![Erlebnisoptionen](assets/experience-options.png)

1. Die für Erlebnis A verfügbaren Anzeigeoptionen und wählen Sie dann die **Zu URL umleiten** und geben Sie eine URL für die neue WKND Site-Startseite an.

   ![Umleitungs-URL](assets/redirect-url.png)

1. Umbenennen *Erlebnis A* nach *Neue WKND-Homepage* und *Erlebnis B* nach *WKND-Homepage*

   ![Abenteuer](assets/new-experiences.png)

1. Klicken **Nächste** , um zum Targeting zu wechseln und eine manuelle Traffic-Zuordnung von 50 bis 50 zwischen den beiden Erlebnissen beizubehalten.

   ![Targeting](assets/targeting.png)

1. Wählen Sie für Ziele und Einstellungen als Berichtsquelle Adobe Target und als Zielmetrik &quot;Konversion mit Seitenansichtsaktion&quot;aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität an und speichern Sie.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen live zu schalten.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäts-URL aus Schritt 3) in einer neuen Registerkarte. Sie sollten eines der Erlebnisse (WKND-Homepage oder neue WKND-Homepage) aus unserer A/B-Test-Aktivität anzeigen können. `us/en.html` umleitet auf `us/home.html`.

   ![Ziele](assets/redirect-test.png)

## Zusammenfassung

Als Marketer konnten Sie eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.

## Unterstützende Links

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
