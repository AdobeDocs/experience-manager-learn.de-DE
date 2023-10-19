---
title: Personalisierung des gesamten Web-Seitenerlebnisses
description: Erfahren Sie, wie Sie eine Target-Aktivität erstellen, um die Seiten Ihrer AEM-Website mithilfe von Adobe Target auf neue Seiten umzuleiten.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
version: Cloud Service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 89%

---

# Personalisierung des gesamten Web-Seitenerlebnisses {#personalization-fpe}

Erfahren Sie, wie Sie eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.

## Voraussetzungen

Um die vollständigen Seiten einer AEM Website zu personalisieren, muss die folgende Einrichtung abgeschlossen sein:

1. [Hinzufügen von Adobe Target zu Ihrer AEM-Website](./add-target-launch-extension.md)
1. [Auslösen eines Adobe Target-Aufrufs von Launch](./load-and-fire-target.md)

## Überblick über das Szenario

Die Homepage der WKND-Site wurde neu gestaltet, und die Besucherinnen und Besucher der aktuellen Homepage sollen zur neuen Homepage umgeleitet werden. Außerdem gilt es nachzuvollziehen, wie die neu gestaltete Homepage zur Verbesserung der Benutzerinteraktion und Umsatzsteigerung beiträgt. Als Marketing-Fachkraft haben Sie die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucherinnen und Besucher auf die neue Homepage umzuleiten. Lassen Sie uns die WKND-Site-Homepage erkunden und erfahren, wie Aktivitäten mit Adobe Target erstellt werden.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei Adobe Target an und navigieren Sie zur Registerkarte „Aktivitäten“.
1. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie die Aktivität **A/B-Test**.

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer** aus, geben Sie die Aktivitäts-URL ein und klicken Sie auf **Weiter**.

   ![Aktivitäts-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach dem Erstellen einer Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste aus. Sie können der Liste neue Erlebnisse hinzufügen, indem Sie die **Erlebnis hinzufügen** Schaltfläche.

   ![Erlebnisoptionen](assets/experience-options.png)

1. Rufen Sie die für Erlebnis A verfügbaren Optionen auf, wählen Sie dann **Zu URL umleiten** aus und geben Sie eine URL für die neue WKND-Site-Homepage an.

   ![Umleitungs-URL](assets/redirect-url.png)

1. Benennen Sie *Erlebnis A* in *Neue WKND-Homepage* und *Erlebnis B* in *WKND-Homepage* um.

   ![Abenteuer](assets/new-experiences.png)

1. Klicken Sie auf **Weiter**, um zum Targeting zu wechseln und eine manuelle Traffic-Zuordnung von 50-50 zwischen den beiden Erlebnissen beizubehalten.

   ![Targeting](assets/targeting.png)

1. Wählen Sie unter „Ziele und Einstellungen“ als Reporting-Quelle „Adobe Target“ und als Zielmetrik „Konversion“ mit Seitenansichtsaktion aus.

   ![Ziele](assets/goals.png)

1. Geben Sie einen Namen für Ihre Aktivität an und speichern Sie diese.
1. Aktivieren Sie Ihre gespeicherte Aktivität, um Ihre Änderungen live zu schalten.

   ![Ziele](assets/activate.png)

1. Öffnen Sie Ihre Site-Seite (Aktivitäts-URL aus Schritt 3) in einer neuen Registerkarte. Sie sollten eines der Erlebnisse („WKND-Homepage“ oder „Neue WKND-Homepage“) aus unserer A/B-Test-Aktivität anzeigen können. `us/en.html` leitet zu `us/home.html` um.

   ![Ziele](assets/redirect-test.png)

## Zusammenfassung

Als Marketing-Fachkraft konnten Sie eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.

## Unterstützende Links

* [Adobe Experience Cloud Debugger – Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
* [Adobe Experience Cloud Debugger – Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
