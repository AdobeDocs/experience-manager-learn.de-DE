---
title: Personalisierung mit Visual Experience Composer
description: Erfahren Sie, wie Sie mit Visual Experience Composer eine Adobe Target-Aktivität erstellen.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 71%

---

# Personalisierung mit Visual Experience Composer {#personalization-vec}

Erfahren Sie, wie Sie mit Visual Experience Composer (VEC) eine A/B-Test-Target-Aktivität erstellen.

## Voraussetzungen

Um VEC auf einer AEM Website zu verwenden, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM-Website](./add-target-launch-extension.md)
1. [Auslösen eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Überblick über das Szenario

Auf der WKND-Website-Startseite werden lokale Aktivitäten oder die besten Dinge, die in einer Stadt zu erledigen sind, in Form von Informationskarten angezeigt. Sie als Marketing-Fachkraft haben die Aufgabe erhalten, die Homepage zu ändern, indem Sie Textänderungen am Abschnitts-Teaser für Abenteuer vornehmen und verstehen, wie dieses die Konversion verbessert.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an, tippen Sie auf __Ziel__ und navigieren Sie zur Registerkarte __Tätigkeiten__

   + Wenn Sie __Target__ Stellen Sie im Experience Cloud-Dashboard sicher, dass im Organisationswechsel oben rechts die richtige Adobe-Organisation ausgewählt ist und dass dem Benutzer Zugriff auf Target in [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie die Aktivität **A/B-Test**.

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer** aus, geben Sie die Aktivitäts-URL ein und klicken Sie auf **Weiter**.

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach dem Erstellen einer Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Sie können der Liste neue Erlebnisse hinzufügen, indem Sie die **Erlebnis hinzufügen** Schaltfläche.

   ![Erlebnis A](assets/experience.png)

1. Wählen Sie ein Bild oder einen Text auf Ihrer Seite aus, um Änderungen vorzunehmen, oder verwenden Sie den Code-Editor, um ein HTML-Element auszuwählen.

   ![Element](assets/select-element.png)

1. Ändern Sie den Text von *Campen in Westaustralien* zu *Abenteuer Australiens*. Eine Liste der Änderungen, die einem Erlebnis hinzugefügt wurden, wird unter „Änderungen“ angezeigt. Sie können auf das geänderte Element klicken und es bearbeiten, um dessen CSS-Selektor und den neuen Inhalt anzuzeigen, der ihm hinzugefügt wurde.

   ![Abenteuer](assets/adventures.png)

1. Benennen Sie *Erlebnis A* in *Abenteuer* um
1. Aktualisieren Sie auf ähnliche Weise den Text auf *Erlebnis B* von *Campen in Westaustralien* zu *Erkunde die australische Wildnis*.

   ![Erkunden](assets/explore.png)

1. Klicken Sie auf **Weiter**, um zur Zielgruppenbestimmung zu wechseln. Behalten wir eine manuelle Traffic-Zuordnung von 50-50 zwischen den beiden Erlebnissen bei.

   ![Targeting](assets/targeting.png)

1. Wählen Sie unter „Ziele und Einstellungen“ als Reporting-Quelle „Adobe Target“ und als Zielmetrik „Konversion“ mit Seitenansichtsaktion aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität an und speichern Sie diese.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen live zu schalten.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäts-URL aus Schritt 3) in einer neuen Registerkarte. Sie sollten eines der Erlebnisse (Abenteuer oder Erkunden) aus unserer A/B-Test-Aktivität anzeigen können.

   ![Ziele](assets/publish.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag-and-Drop austauscht und verändert hat, ohne den Code zum Ausführen eines Tests zu ändern.

## Unterstützende Links

+ [Adobe Experience Cloud Debugger – Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger – Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
