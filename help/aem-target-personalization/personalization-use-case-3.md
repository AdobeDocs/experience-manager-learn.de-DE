---
title: Personalisierung mit Adobe Target Visual Experience Composer
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 3%

---

# Personalisierung mit Visual Experience Composer

In diesem Kapitel wird die Erstellung von Erlebnissen mithilfe von **Visual Experience Composer** durch Ziehen und Ablegen, Austauschen und Ändern des Layouts und Inhalts einer Webseite aus Target heraus.

## Szenario - Überblick

Auf der WKND-Website-Startseite werden lokale Aktivitäten oder das beste, um eine Stadt herum in Form von Kartenlayouts zu erledigen, angezeigt. Marketingexperten haben die Aufgabe erhalten, die Startseite zu ändern, indem sie die Kartenlayouts neu anordnen, um zu sehen, wie sie sich auf die Benutzerinteraktion auswirkt und die Konversion vorantreibt.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise Administratorzugriff benötigen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimierungsteam)

### WKND-Site-Homepage

![AEM Target-Szenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Voraussetzungen

* **AEM**
   * [AEM Veröffentlichungsinstanz](./implementation.md#getting-aem) läuft auf 4503
   * [AEM mit Adobe Target mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Unternehmen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit [Adobe Target](https://experiencecloud.adobe.com)

## Marketingaktivitäten

1. Der Marketer erstellt eine A/B-Zielaktivität in Adobe Target.
   1. Navigieren Sie im Adobe Target-Fenster zu **Tätigkeiten** Registerkarte.
   2. Klicken **Aktivität erstellen** und wählen Sie den Aktivitätstyp als **A/B-Test**
      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-ab-activity.png)
   3. Wählen Sie die **Web** Kanal und wählen Sie die **Visual Experience Composer**.
   4. Geben Sie die **Aktivitäts-URL** und klicken **Nächste** , um den Visual Experience Composer zu öffnen.
      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Für **Visual Experience Composer** zum Laden aktivieren **Unsichere Skripte laden zulassen** in Ihrem Browser und laden Sie Ihre Seite neu.
      ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Beachten Sie, dass die WKND Site-Startseite im Visual Experience Composer-Editor geöffnet ist.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Erlebnis A** stellt die standardmäßige WKND-Homepage bereit und lassen Sie uns das Inhaltslayout für **Erlebnis B**.
      ![Erlebnis B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klicken Sie auf einen der Kartenlayout-Container (*Beste Röster*) und wählen Sie **Neu anordnen** -Option.
      ![Container-Auswahl](assets/personalization-use-case-3/container-selection.png)
   9. Klicken Sie auf den Container, den Sie neu anordnen möchten, und ziehen Sie ihn an die gewünschte Position. Ordnen wir die *Beste Röster* Container aus der ersten Zeile Spalte der ersten Zeile in die dritte Spalte der ersten Zeile. Jetzt *Beste Röster* Container befindet sich neben *Fotografierausstellungen* Container.
      ![Container-Swap](assets/personalization-use-case-3/container-swap.png)
      **Nach dem Tauschen**
      ![Container ersetzt](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Ordnen Sie die Positionen für die anderen Kartencontainer auf die gleiche Weise neu an.
      ![Container ersetzt](assets/personalization-use-case-3/after-swap-all.png)
   11. Fügen wir auch einen Kopfzeilentext unter der Karussellkomponente und über dem Kartenlayout hinzu.
   12. Klicken Sie auf den Karussellbehälter und wählen Sie den **Einfügen nach > HTML** -Option zum Hinzufügen von HTML.
      ![Text hinzufügen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Text hinzufügen](assets/personalization-use-case-3/after-changes.png)
   13. Klicken **Nächste** , um mit Ihrer Aktivität fortzufahren.
   14. Wählen Sie die **Traffic-Zuordnungsmethode** als manuellen und 100 %-igen Traffic auf **Erlebnis B**.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   15. Klicken Sie auf **Weiter**.
   16. Bereitstellung **Zielmetriken** für Ihre Aktivität und Speichern und schließen Sie Ihren A/B-Test.
      ![Metrik für A/B-Test-Ziel](assets/personalization-use-case-2/goal-metric.png)
   17. Geben Sie einen Namen (**WKND-Homepage-Aktualisierung**) für Ihre Aktivität und speichern Sie Ihre Änderungen.
   18. Stellen Sie im Bildschirm &quot;Aktivitätsdetails&quot;sicher, dass Sie **Aktivieren** Ihre Aktivität.
      ![Aktivität aktivieren](assets/personalization-use-case-3/save-activity.png)
   19. Navigieren Sie zur WKND-Startseite (http://localhost:4503/content/wknd/en.html) und Sie sehen die Änderungen, die wir zur Aktivität WKND-Startseite aktualisieren A/B-Test hinzugefügt haben.
      ![WKND-Homepage aktualisiert](assets/personalization-use-case-3/activity-result.png)
   20. Öffnen Sie Ihre Browser-Konsole und überprüfen Sie die Registerkarte &quot;Netzwerk&quot;, um nach Zielantworten für die Aktivität WKND-Homepage-Update A/B-Test zu suchen.
      ![Netzwerkaktivität](assets/personalization-use-case-3/activity-result.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag-and-Drop, Tausch und Änderung durchführte, ohne den Code zum Ausführen eines Tests zu ändern.
