---
title: Personalisierung mit Adobe Target Visual Experience Composer
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 2%

---


# Personalisierung mit Visual Experience Composer

In diesem Kapitel werden wir das Erstellen von Erlebnissen mit **Visual Experience Composer** untersuchen, indem wir das Layout und den Inhalt einer Web-Seite in Target per Drag-and-Drop austauschen und ändern.

## Szenario - Überblick

Auf der WKND-Website-Startseite werden lokale Aktivitäten oder das beste, um eine Stadt herum in Form von Kartenlayouts zu erledigen. Marketingexperten haben die Aufgabe erhalten, die Startseite zu ändern, indem sie die Kartenlayouts neu anordnen, um zu sehen, wie sie sich auf die Benutzerinteraktion auswirkt und die Konversion vorantreibt.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise Administratorzugriff benötigen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer**  (Adobe Target/Optimierungsteam)

### WKND-Site-Homepage

![AEM Target-Szenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Voraussetzungen

* **AEM**
   * [AEM ](./implementation.md#getting-aem) Veröffentlichungsinstanz auf 4503
   * [AEM mit Adobe Target mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit [Adobe Target](https://experiencecloud.adobe.com) bereitgestellt wurde

## Marketingaktivitäten

1. Der Marketer erstellt eine A/B-Zielaktivität in Adobe Target.
   1. Navigieren Sie im Adobe Target-Fenster zur Registerkarte **Aktivitäten** .
   2. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie den Aktivitätstyp als **A/B-Test** aus.

      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-ab-activity.png)
   3. Wählen Sie den Kanal **Web** und dann **Visual Experience Composer** aus.
   4. Geben Sie die **Aktivitäts-URL** ein und klicken Sie auf **Weiter**, um den Visual Experience Composer zu öffnen.
      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Damit **Visual Experience Composer** geladen werden kann, aktivieren Sie **Allow Unsafe scripts** in Ihrem Browser und laden Sie Ihre Seite neu.
      ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Beachten Sie, dass die WKND Site-Startseite im Visual Experience Composer-Editor geöffnet ist.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Erlebnis** bietet die standardmäßige WKND-Startseite und lassen Sie uns das Inhaltslayout für  **Erlebnis B** bearbeiten.
      ![Erlebnis B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klicken Sie auf einen der Kartenlayout-Container (*Beste Röster*) und wählen Sie die Option **Neu anordnen** aus.
      ![Container-Auswahl](assets/personalization-use-case-3/container-selection.png)
   9. Klicken Sie auf den Container, den Sie neu anordnen möchten, und ziehen Sie ihn an die gewünschte Position. Ordnen wir den Behälter *Beste Röster* von der ersten Zeile der ersten Spalte in die dritte Zeile der ersten Zeile um. Der *Beste Röster*-Container befindet sich nun neben dem *Fotografikausstellungen* -Container.
      ![Container-Swap](assets/personalization-use-case-3/container-swap.png)

      **Nach dem Tauschen**
      ![Container ersetzt](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Ordnen Sie die Positionen für die anderen Kartencontainer auf die gleiche Weise neu an.
      ![Container ersetzt](assets/personalization-use-case-3/after-swap-all.png)
   11. Fügen wir auch einen Kopfzeilentext unter der Karussellkomponente und über dem Kartenlayout hinzu.
   12. Klicken Sie auf den Karussellbehälter und wählen Sie die Option **Einfügen nach > HTML** aus, um HTML hinzuzufügen.
      ![Text hinzufügen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Text hinzufügen](assets/personalization-use-case-3/after-changes.png)
   13. Klicken Sie auf **Weiter** , um mit Ihrer Aktivität fortzufahren.
   14. Wählen Sie die **Traffic-Zuordnungsmethode** als manuell aus und weisen Sie **Erlebnis B** 100 % Traffic zu.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   15. Klicken Sie auf **Weiter**.
   16. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie Ihren A/B-Test.
      ![Metrik für A/B-Test-Ziel](assets/personalization-use-case-2/goal-metric.png)
   17. Geben Sie einen Namen (**WKND-Startseitenaktualisierung**) für Ihre Aktivität an und speichern Sie Ihre Änderungen.
   18. Stellen Sie im Bildschirm &quot;Aktivitätsdetails&quot;sicher, dass Sie **Aktivieren** Ihre Aktivität auswählen.
      ![Aktivität aktivieren](assets/personalization-use-case-3/save-activity.png)
   19. Navigieren Sie zur WKND-Startseite (http://localhost:4503/content/wknd/en.html) und Sie sehen die Änderungen, die wir zur Aktivität WKND-Startseite aktualisieren A/B-Test hinzugefügt haben.
      ![WKND-Homepage aktualisiert](assets/personalization-use-case-3/activity-result.png)
   20. Öffnen Sie Ihre Browser-Konsole und überprüfen Sie die Registerkarte &quot;Netzwerk&quot;, um nach Zielantworten für die Aktivität WKND-Homepage-Update A/B-Test zu suchen.
      ![Netzwerkaktivität](assets/personalization-use-case-3/activity-result.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag-and-Drop, Tausch und Änderung durchführte, ohne den Code zum Ausführen eines Tests zu ändern.
