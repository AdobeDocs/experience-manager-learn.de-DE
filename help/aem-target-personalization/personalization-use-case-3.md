---
title: Personalisierung mit Adobe Target Visual Experience Composer
seo-title: Personalisierung mit Adobe Target Visual Experience Composer (VEC)
description: Ein durchgehendes Lernprogramm, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: Ein durchgehendes Lernprogramm, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
feature: Experience Fragments
topic: 'Personalisierung '
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 3%

---


# Personalisierung mit Visual Experience Composer

In diesem Kapitel werden wir das Erstellen von Erlebnissen mithilfe von **Visual Experience Composer** untersuchen, indem wir das Layout und den Inhalt einer Webseite innerhalb der Zielgruppe per Drag &amp; Drop verschieben, austauschen und ändern.

## Szenario-Übersicht

Die WKND-Site-Startseite zeigt lokale Aktivitäten oder das beste, was Sie in einer Stadt tun sollten, in Form von Kartenlayouts. Als Marketingspezialist wurde Ihnen die Aufgabe zugewiesen, die Startseite zu ändern, indem Sie die Kartenlayouts neu anordnen, um zu sehen, wie sich dies auf die Benutzerinteraktion auswirkt und die Konversion antreibt.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise administrativen Zugriff benötigen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimierungsteam)

### WKND-Site-Startseite

![AEM Zielgruppe Szenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Voraussetzungen

* **AEM-**
   * [AEM ](./implementation.md#getting-aem) Installation am 4503 veröffentlichen
   * [AEM mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud bereitgestellt mit [Adobe Target](https://experiencecloud.adobe.com)

## Aktivitäten von Marketingexperten

1. Der Marketingexperte erstellt innerhalb von Adobe Target eine Aktivität zur A/B-Zielgruppe.
   1. Navigieren Sie im Adobe Target-Fenster zur Registerkarte **Aktivitäten**.
   2. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie den Typ der Aktivität als **A/B-Test**

      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-ab-activity.png)
   3. Wählen Sie den Kanal **Web** und wählen Sie **Visual Experience Composer**.
   4. Geben Sie die **Aktivitäten-URL** ein und klicken Sie auf **Weiter**, um den Visual Experience Composer zu öffnen.
      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Damit **Visual Experience Composer** geladen werden kann, aktivieren Sie **Unsichere Skripte** in Ihrem Browser laden und laden Sie Ihre Seite neu.
      ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Beachten Sie, dass die WKND-Site-Startseite im Visual Experience Composer-Editor geöffnet ist.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Erlebnis-** Hilfe bietet die standardmäßige WKND-Startseite und bearbeiten Sie das Inhaltslayout für  **Erlebnis B**.
      ![Erlebnis B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klicken Sie auf einen der Kartenlayout-Container (*Beste Röster*) und wählen Sie **Neuanordnen**.
      ![Auswahl des Containers](assets/personalization-use-case-3/container-selection.png)
   9. Klicken Sie auf den Container, den Sie neu anordnen möchten, und ziehen Sie ihn an die gewünschte Position. Ordnen wir den Container *Beste Röster* von der ersten Zeile in die dritte Zeile um. Der Container *Beste Röster* befindet sich nun neben dem Container *Fotografieausstellungen*.
      ![Container austauschen](assets/personalization-use-case-3/container-swap.png)

      **Nach dem Austauschen**
      ![Container ausgetauscht](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Ordnen Sie die Positionen für die anderen Container der Karte entsprechend neu an.
      ![Container ausgetauscht](assets/personalization-use-case-3/after-swap-all.png)
   11. Fügen wir auch einen Kopfzeilentext unter der Karussellkomponente und über dem Kartenlayout hinzu.
   12. Klicken Sie auf den Karussell-Container und wählen Sie die Option **Einsetzen nach > HTML**, um HTML hinzuzufügen.
      ![Text hinzufügen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Text hinzufügen](assets/personalization-use-case-3/after-changes.png)
   13. Klicken Sie auf **Weiter**, um mit Ihrer Aktivität fortzufahren.
   14. Wählen Sie die **Traffic-Zuordnungsmethode** als manuell aus und weisen Sie **Erlebnis B** 100 % Traffic zu.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   15. Klicken Sie auf **Weiter**.
   16. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie Ihren A/B-Test.
      ![A/B-Testziel-Metrik](assets/personalization-use-case-2/goal-metric.png)
   17. Geben Sie einen Namen (**Aktualisierung der WKND-Startseite**) für Ihre Aktivität ein und speichern Sie Ihre Änderungen.
   18. Vergewissern Sie sich im Bildschirm &quot;Aktivität Details&quot;auf **Aktivieren** Ihrer Aktivität.
      ![Aktivität aktivieren](assets/personalization-use-case-3/save-activity.png)
   19. Navigieren Sie zur WKND-Startseite (http://localhost:4503/content/wknd/en.html), und Sie sehen die Änderungen, die wir der Aktivität WKND Startseite Refresh A/B Test  hinzugefügt haben.
      ![WKND-Startseite aktualisiert](assets/personalization-use-case-3/activity-result.png)
   20. Öffnen Sie Ihre Browser-Konsole und überprüfen Sie die Registerkarte &quot;Netzwerk&quot;, um nach Zielgruppen für die WKND Startseite Refresh A/B Test Aktivität zu suchen.
      ![Netzwerk-Aktivität](assets/personalization-use-case-3/activity-result.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag &amp; Drop verschieben, austauschen und ändern konnte, ohne den Code zum Ausführen eines Tests zu ändern.
