---
title: Personalisierung mit Visual Experience Composer
description: Erfahren Sie, wie Sie mit Visual Experience Composer eine Adobe Target-Aktivität erstellen.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 1%

---


# Personalisierung mit Visual Experience Composer {#personalization-vec}

Erfahren Sie, wie Sie mit Visual Experience Composer (VEC) eine Aktivität zur A/B-Zielgruppe erstellen.

Bevor Sie eine Aktivität in der Zielgruppe erstellen, müssen Sie Folgendes einrichten:

1. [Experience Platform Launch und AEM integrieren](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
2. [Integrieren von Adobe Experience Manager mit Adobe Target mithilfe von Cloud Services](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/target/setup-aem-target-cloud-service.html)

## Szenario-Übersicht

Die WKND-Site-Startseite zeigt Aktivitäten oder das Beste, was man in einer Stadt mit Informationskarten machen kann. Als Vermarkter wurde Ihnen die Aufgabe zugewiesen, die Startseite zu ändern, indem Sie Textänderungen am Abenteuerabschnitt-Teaser vornehmen und verstehen, wie die Konvertierung verbessert wird.

## Schritte zum Erstellen eines A/B-Tests mit Visual Experience Composer (VEC)

1. Melden Sie sich bei Adobe Target an und navigieren Sie zur Registerkarte &quot;Aktivitäten&quot;.
1. Klicken Sie auf **Aktivität** erstellen und wählen Sie dann **A/B-Test** -Aktivität

   ![A/B-Aktivität](assets/ab-target-activity.png)

1. Wählen Sie die Option **Visual Experience Composer** , geben Sie die Aktivitäten-URL ein und klicken Sie auf **Weiter**

   ![Aktivitäten-URL](assets/ab-test-url.png)

1. Der Visual Experience Composer zeigt nach Erstellung einer neuen Aktivität auf der linken Seite zwei Registerkarten an: *Erlebnis A* und *Erlebnis B*. Wählen Sie ein Erlebnis aus der Liste. Sie können der Liste mithilfe der Schaltfläche **Hinzufügen Erlebnis** neue Erlebnisse hinzufügen.

   ![Erlebnis A](assets/experience.png)

1. Wählen Sie ein Bild oder einen Text auf Ihrer Seite aus, um Beginn, die Änderungen vornehmen, zu ändern oder den Code-Editor zum Auswählen und HTML-Element zu verwenden.

   ![Element](assets/select-element.png)

1. Ändern Sie den Text von *Camping in Westaustralien* in *Abenteuer in Australien*. Eine Liste der Änderungen, die zu einem Erlebnis hinzugefügt wurden, wird unter Änderungen angezeigt. Sie können auf das geänderte Element klicken und es bearbeiten, um dessen CSS-Selektor und den ihm hinzugefügten neuen Inhalt Ansicht.

   ![Abenteuer](assets/adventures.png)

1. Umbenennen von *Erlebnis A* zu *Abenteuer*
1. Ebenso aktualisieren Sie den Text zu *Erlebnis B* vom *Camping in Westaustralien* , um die australische Wildnis zu *entdecken*.

   ![Entdecken](assets/explore.png)

1. Klicken Sie auf **Weiter** , um zum Targeting zu wechseln, und bewahren Sie die manuelle Traffic-Zuordnung zwischen den beiden Erlebnissen auf 50-50.

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

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
