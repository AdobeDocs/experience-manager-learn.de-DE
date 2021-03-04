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
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 2%

---


# Personalisierung mit Visual Experience Composer {#personalization-vec}

Erfahren Sie, wie Sie mit Visual Experience Composer (VEC) eine Aktivität zur A/B-Zielgruppe erstellen.

## Voraussetzungen

Zur Verwendung von VEC auf einer AEM Website muss die folgende Einrichtung abgeschlossen sein:

1. [hinzufügen von Adobe Target zu Ihrer AEM Website](./add-target-launch-extension.md)
1. [Trigger und Adobe Target-Aufruf von Launch](./load-and-fire-target.md)

## Szenario-Übersicht

Die WKND-Site-Startseite zeigt Aktivitäten oder das Beste, was man in einer Stadt mit Informationskarten machen kann. Als Vermarkter wurde Ihnen die Aufgabe zugewiesen, die Startseite zu ändern, indem Sie Textänderungen am Abenteuerabschnitt-Teaser vornehmen und verstehen, wie die Konvertierung verbessert wird.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an, tippen Sie auf __Zielgruppe__ und navigieren Sie zur Registerkarte __Aktivitäten__

   + Wenn __Zielgruppe__ im Experience Cloud-Dashboard nicht angezeigt wird, stellen Sie sicher, dass im Organisationswechsel oben rechts die richtige Organisation für die Adobe ausgewählt ist und dass Sie unter [Adobe Admin Console](https://adminconsole.adobe.com/) Zugriff auf die Zielgruppe erhalten haben.

1. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie dann **A/B-Test** Aktivität

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer**, geben Sie die Aktivitäten-URL ein und klicken Sie auf **Weiter**

   ![Aktivitäten-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste. Mit der Schaltfläche **Hinzufügen Erlebnis** können Sie der Liste neue Erlebnisse hinzufügen.

   ![Erlebnis A](assets/experience.png)

1. Wählen Sie ein Bild oder einen Text auf Ihrer Seite aus, um Beginn, die Änderungen vornehmen, zu ändern oder den Code-Editor zum Auswählen und HTML-Element zu verwenden.

   ![Element](assets/select-element.png)

1. Ändern Sie den Text von *Camping in Westaustralien* in *Abenteuer in Australien*. Eine Liste der Änderungen, die zu einem Erlebnis hinzugefügt wurden, wird unter Änderungen angezeigt. Sie können auf das geänderte Element klicken und es bearbeiten, um dessen CSS-Selektor und den ihm hinzugefügten neuen Inhalt Ansicht.

   ![Abenteuer](assets/adventures.png)

1. Umbenennen von *Erlebnis A* in *Abenteuer*
1. Aktualisieren Sie den Text auf *Erlebnis B* von *Camping in Westaustralien* auf *Die australische Wildnis*.

   ![Entdecken](assets/explore.png)

1. Klicken Sie auf **Weiter**, um zum Targeting zu wechseln, und bewahren Sie die manuelle Traffic-Zuordnung von 50-50 zwischen den beiden Erlebnissen auf.

   ![Targeting](assets/targeting.png)

1. Wählen Sie für &quot;Ziele und Einstellungen&quot;die Quelle des Berichte als Adobe Target und wählen Sie die Metrik &quot;Ziel&quot;als &quot;Konversion&quot;mit einer Seitenaktion &quot;Ansicht&quot;aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität und Speichern ein.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen zu aktivieren.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäten-URL ab Schritt 3) in einer neuen Registerkarte, und Sie sollten in der Lage sein, eines der Erlebnisse (Abenteuer oder Erforschen) aus unserer A/B-Test-Aktivität Ansicht.

   ![Ziele](assets/publish.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag &amp; Drop verschieben, austauschen und ändern konnte, ohne den Code zum Ausführen eines Tests zu ändern.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
