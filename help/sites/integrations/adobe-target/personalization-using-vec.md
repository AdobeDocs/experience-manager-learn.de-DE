---
title: Personalisierung mit Visual Experience Composer
description: Erfahren Sie, wie Sie mit Visual Experience Composer eine Adobe Target-Aktivität erstellen.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrationen
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 1%

---


# Personalisierung mit Visual Experience Composer {#personalization-vec}

Erfahren Sie, wie Sie mit Visual Experience Composer (VEC) eine A/B-Test-Target-Aktivität erstellen.

## Voraussetzungen

Um VEC auf einer AEM Website verwenden zu können, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM Website](./add-target-launch-extension.md)
1. [Trigger eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Szenario - Überblick

Auf der WKND-Website-Startseite werden lokale Aktivitäten oder das beste, um eine Stadt herum in Form von Informationskarten angezeigt. Marketingexperten haben die Aufgabe erhalten, die Startseite zu ändern, indem Sie Textänderungen am Teaser für Abenteuerabschnitte vornehmen und verstehen, wie diese die Konversion verbessert.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an, tippen Sie auf __Target__ und navigieren Sie zur Registerkarte __Aktivitäten__ .

   + Wenn __Target__ nicht im Experience Cloud-Dashboard angezeigt wird, stellen Sie sicher, dass im Organisationswechsel oben rechts die richtige Organisationsstruktur ausgewählt ist und dass Ihnen in [Adobe Admin Console](https://adminconsole.adobe.com/) Zugriff auf Target gewährt wurde.

1. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie dann **A/B-Test** -Aktivität

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer**, geben Sie die Aktivitäts-URL ein und klicken Sie dann auf **Weiter**

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach der Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Mithilfe der Schaltfläche **Erlebnis hinzufügen** können Sie der Liste neue Erlebnisse hinzufügen.

   ![Erlebnis A](assets/experience.png)

1. Wählen Sie ein Bild oder Text auf Ihrer Seite aus, um Änderungen vorzunehmen, oder verwenden Sie den Code-Editor, um HTML-Elemente auszuwählen und zu verwenden.

   ![Element](assets/select-element.png)

1. Ändern Sie den Text von *Camping in Westaustralien* in *Abenteuer in Australien*. Eine Liste der Änderungen, die einem Erlebnis hinzugefügt wurden, wird unter Änderungen angezeigt. Sie können auf das geänderte Element klicken und es bearbeiten, um dessen CSS-Selektor und den neuen Inhalt anzuzeigen, der ihm hinzugefügt wurde.

   ![Abenteuer](assets/adventures.png)

1. Benennen Sie *Erlebnis A* in *Abenteuer* um.
1. Aktualisieren Sie auf ähnliche Weise den Text von *Erlebnis B* von *Campen in Westaustralien* auf *Erkunden Sie die australische Wildnis*.

   ![Erkunden](assets/explore.png)

1. Klicken Sie auf **Weiter** , um zum Targeting zu wechseln. Behalten wir eine manuelle Traffic-Zuordnung von 50-50 zwischen den beiden Erlebnissen bei.

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
