---
title: Personalisierung mit Visual Experience Composer
description: Erfahren Sie, wie Sie mit Visual Experience Composer eine Adobe Target-Aktivität erstellen.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 6%

---

# Personalisierung mit Visual Experience Composer {#personalization-vec}

Erfahren Sie, wie Sie mit Visual Experience Composer (VEC) eine A/B-Test-Target-Aktivität erstellen.

## Voraussetzungen

Um VEC auf einer AEM Website verwenden zu können, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM-Website](./add-target-launch-extension.md)
1. [Auslösen eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Szenario - Überblick

Auf der WKND-Website-Startseite werden lokale Aktivitäten oder das beste, um eine Stadt herum in Form von Informationskarten angezeigt. Marketingexperten haben die Aufgabe erhalten, die Startseite zu ändern, indem Sie Textänderungen am Teaser für Abenteuerabschnitte vornehmen und verstehen, wie diese die Konversion verbessert.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Anmelden bei [Adobe Experience Cloud](https://experience.adobe.com/), tippen Sie auf __Target__, navigieren Sie zum __Tätigkeiten__ tab

   + Wenn Sie __Target__ Vergewissern Sie sich im Experience Cloud-Dashboard, dass im Organisationswechsel oben rechts die richtige Organisation für Adoben ausgewählt ist und dass Ihnen der Zugriff auf Target in [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Klicken **Aktivität erstellen** und wählen Sie **A/B-Test** activity

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die **Visual Experience Composer** Geben Sie die Aktivitäts-URL ein und klicken Sie auf **Nächste**

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach der Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Sie können der Liste neue Erlebnisse hinzufügen, indem Sie die **Erlebnis hinzufügen** Schaltfläche.

   ![Erlebnis A](assets/experience.png)

1. Wählen Sie ein Bild oder Text auf Ihrer Seite aus, um Änderungen vorzunehmen, oder verwenden Sie den Code-Editor, um Elemente auszuwählen und HTML.

   ![Element](assets/select-element.png)

1. Text ändern von *Campen in Westaustralien* nach *Abenteuer in Australien*. Eine Liste der Änderungen, die einem Erlebnis hinzugefügt wurden, wird unter Änderungen angezeigt. Sie können auf das geänderte Element klicken und es bearbeiten, um dessen CSS-Selektor und den neuen Inhalt anzuzeigen, der ihm hinzugefügt wurde.

   ![Abenteuer](assets/adventures.png)

1. Umbenennen *Erlebnis A* nach *Abenteuer*
1. Aktualisieren Sie auf ähnliche Weise den Text auf *Erlebnis B* von *Campen in Westaustralien* nach *Die australische Wildnis*.

   ![Erkunden](assets/explore.png)

1. Klicken **Nächste** , um zu Targeting zu wechseln, und lassen Sie uns eine manuelle Traffic-Zuordnung von 50 bis 50 zwischen den beiden Erlebnissen beibehalten.

   ![Targeting](assets/targeting.png)

1. Wählen Sie für Ziele und Einstellungen als Berichtsquelle Adobe Target und als Zielmetrik &quot;Konversion mit Seitenansichtsaktion&quot;aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität an und speichern Sie.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen live zu schalten.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäts-URL aus Schritt 3) in einer neuen Registerkarte. Sie sollten eines der Erlebnisse (Abenteuer oder Erkunden) aus unserer A/B-Test-Aktivität anzeigen können.

   ![Ziele](assets/publish.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag-and-Drop, Tausch und Änderung durchführte, ohne den Code zum Ausführen eines Tests zu ändern.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
