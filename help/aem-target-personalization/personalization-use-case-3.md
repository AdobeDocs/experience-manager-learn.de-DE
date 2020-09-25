---
title: Personalisierung mit Adobe Target Visual Experience Composer
seo-title: Personalisierung mit Adobe Target Visual Experience Composer (VEC)
description: Ein durchgehendes Lernprogramm, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: Ein durchgehendes Lernprogramm, in dem gezeigt wird, wie mit dem Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# Personalisierung mit Visual Experience Composer

In diesem Kapitel werden wir das Erstellen von Erlebnissen mit **Visual Experience Composer** untersuchen, indem wir das Layout und den Inhalt einer Webseite innerhalb der Zielgruppe per Drag &amp; Drop verschieben, austauschen und ändern.

## Szenario-Übersicht

Die WKND-Site-Startseite zeigt lokale Aktivitäten oder das beste, was Sie in einer Stadt tun sollten, in Form von Kartenlayouts. Als Marketingspezialist wurde Ihnen die Aufgabe zugewiesen, die Startseite zu ändern, indem Sie die Kartenlayouts neu anordnen, um zu sehen, wie sich dies auf die Benutzerinteraktion auswirkt und die Konversion antreibt.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise administrativen Zugriff benötigen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimierungsteam)

### WKND-Site-Startseite

![AEM Zielgruppe Szenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Voraussetzungen

* **AEM**
   * [AEM Veröffentlichungsinstanz](./implementation.md#getting-aem) , die auf 4503 ausgeführt wird
   * [AEM mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Mit [Adobe Target bereitgestelltes Experience Cloud](https://experiencecloud.adobe.com)

## Aktivitäten von Marketingexperten

1. Der Marketingexperte erstellt innerhalb von Adobe Target eine Aktivität zur A/B-Zielgruppe.
   1. Navigieren Sie im Adobe Target-Fenster zur Registerkarte &quot; **Aktivitäten** &quot;.
   2. Klicken Sie auf **Aktivität** erstellen und wählen Sie den Typ der Aktivität als **A/B-Test**

      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-ab-activity.png)
   3. Wählen Sie den **Web** Kanal und dann **Visual Experience Composer**.
   4. Geben Sie die **Aktivitäten-URL** ein und klicken Sie auf **Weiter** , um den Visual Experience Composer zu öffnen.
      ![Adobe Target - Aktivität erstellen](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Damit **Visual Experience Composer** geladen wird, aktivieren Sie **Zulassen von nicht sicheren Skripten** in Ihrem Browser und laden Sie Ihre Seite neu.
      ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Beachten Sie, dass die WKND-Site-Startseite im Visual Experience Composer-Editor geöffnet ist.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Erlebnis A** bietet die Standard-WKND-Startseite und bearbeiten Sie das Inhaltslayout für **Erlebnis B**.
      ![Erlebnis B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klicken Sie auf einen der Kartenlayout-Container (*Beste Röster*) und wählen Sie die Option &quot; **Neu anordnen** &quot;aus.
      ![Auswahl des Containers](assets/personalization-use-case-3/container-selection.png)
   9. Klicken Sie auf den Container, den Sie neu anordnen möchten, und ziehen Sie ihn an die gewünschte Position. Ändern wir die Anordnung des Containers *Beste Röster* von der ersten Zeile in die dritte Zeile. Der *Best Roasters* Container wird nun neben dem Container *Fotografieausstellungen* stehen.
      ![Container austauschen](assets/personalization-use-case-3/container-swap.png)

      **Nach dem Austauschen**
      ![Container ausgetauscht](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Ordnen Sie die Positionen für die anderen Container der Karte entsprechend neu an.
      ![Container ausgetauscht](assets/personalization-use-case-3/after-swap-all.png)
   11. Fügen wir auch einen Kopfzeilentext unter der Karussellkomponente und über dem Kartenlayout hinzu.
   12. Klicken Sie auf den Karussell-Container und wählen Sie die Option &quot; **Einzug nach&quot;> &quot;HTML** &quot;, um HTML hinzuzufügen.
      ![Text hinzufügen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Text hinzufügen](assets/personalization-use-case-3/after-changes.png)
   13. Klicken Sie auf **Weiter** , um Ihre Aktivität fortzusetzen.
   14. Wählen Sie die **Traffic-Zuordnungsmethode** als manuell aus und weisen Sie **Erlebnis B**100 % Traffic zu.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   15. Klicken Sie auf **Weiter**.
   16. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie Ihren A/B-Test.
      ![A/B-Testziel-Metrik](assets/personalization-use-case-2/goal-metric.png)
   17. Geben Sie einen Namen (**WKND-Startseite aktualisieren**) für Ihre Aktivität ein und speichern Sie Ihre Änderungen.
   18. Achten Sie im Bildschirm &quot;Aktivität&quot; darauf, Ihre Aktivität zu **aktivieren** .
      ![Aktivität aktivieren](assets/personalization-use-case-3/save-activity.png)
   19. Navigieren Sie zur WKND-Startseite (http://localhost:4503/content/wknd/en.html), und Sie sehen die Änderungen, die wir zur Aktivität A/B-Test aktualisieren der WKND-Startseite hinzugefügt haben.
      ![WKND-Startseite aktualisiert](assets/personalization-use-case-3/activity-result.png)
   20. Öffnen Sie Ihre Browser-Konsole und überprüfen Sie die Registerkarte &quot;Netzwerk&quot;, um nach Zielgruppen für die WKND Startseite Refresh A/B Test Aktivität zu suchen.
      ![Netzwerk-Aktivität](assets/personalization-use-case-3/activity-result.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketingexperte mithilfe von Visual Experience Composer ein Erlebnis erstellen, indem er das Layout und den Inhalt einer Webseite per Drag &amp; Drop verschieben, austauschen und ändern konnte, ohne den Code zum Ausführen eines Tests zu ändern.
