---
title: Personalisierung des gesamten Web-Seiten-Erlebnisses
description: Erfahren Sie, wie Sie eine Target-Aktivität erstellen, um Ihre AEM Webseiten mithilfe von Adobe Target auf neue Seiten umzuleiten.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrationen
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 1%

---


# Personalisierung der vollständigen Webseite Erlebnis {#personalization-fpe}

Erfahren Sie, wie Sie eine Aktivität erstellen, um die auf AEM gehosteten Seiten Ihrer Site mit Adobe Target auf eine neue Seite umzuleiten.

## Voraussetzungen

Um die vollständigen Seiten einer AEM Website zu personalisieren, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM Website](./add-target-launch-extension.md)
1. [Trigger eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Übersicht über das Szenario

Die WKND-Site hat ihre Homepage neu gestaltet und möchte die aktuellen Besucher der Homepage auf die neue Homepage umleiten. Gleichzeitig sollten Sie auch verstehen, wie die neu gestaltete Startseite zur Verbesserung der Benutzerinteraktion und des Umsatzes beiträgt. Marketingexperten haben die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucher auf die neue Startseite umzuleiten. Lassen Sie uns die WKND-Site-Homepage durchsuchen und erfahren, wie Sie eine Aktivität mit Adobe Target erstellen.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei Adobe Target an und navigieren Sie zur Registerkarte Aktivitäten .
1. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie dann **A/B-Test** -Aktivität

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer**, geben Sie die Aktivitäts-URL ein und klicken Sie dann auf **Weiter**

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach der Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Mithilfe der Schaltfläche **Erlebnis hinzufügen** können Sie der Liste neue Erlebnisse hinzufügen.

   ![Erlebnisoptionen](assets/experience-options.png)

1. Für Erlebnis A verfügbare Anzeigeoptionen, die Option **Zur URL umleiten** auswählen und eine URL für die neue WKND-Site-Startseite angeben.

   ![Umleitungs-URL](assets/redirect-url.png)

1. Benennen Sie *Erlebnis A* in *Neue WKND-Homepage* und *Erlebnis B* in *WKND-Homepage* um.

   ![Abenteuer](assets/new-experiences.png)

1. Klicken Sie auf **Weiter** , um zum Targeting zu wechseln und eine manuelle Traffic-Zuordnung von 50-50 zwischen den beiden Erlebnissen beizubehalten.

   ![Targeting](assets/targeting.png)

1. Wählen Sie für Ziele und Einstellungen als Berichtsquelle Adobe Target und als Zielmetrik &quot;Konversion mit Seitenansichtsaktion&quot;aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität an und speichern Sie.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen live zu schalten.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäts-URL aus Schritt 3) in einer neuen Registerkarte. Sie sollten eines der Erlebnisse (WKND-Homepage oder neue WKND-Homepage) aus unserer A/B-Test-Aktivität anzeigen können. `us/en.html` leitet zu  `us/home.html`um.

   ![Ziele](assets/redirect-test.png)

## Zusammenfassung

Als Marketer konnten Sie eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.

## Unterstützende Links

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

