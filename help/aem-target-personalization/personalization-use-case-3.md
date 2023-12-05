---
title: Personalisierung mit Adobe Target Visual Experience Composer
description: Ein Tutorial, das von Anfang bis Ende zeigt, wie mithilfe von Adobe Target Visual Experience Composer (VEC) personalisierte Erlebnisse erstellt und bereitgestellt werden können.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 167
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 100%

---

# Personalisierung mit Visual Experience Composer

In diesem Kapitel beschäftigen wir uns mit Erstellung von Erlebnissen mit **Visual Experience Composer** durch Ziehen und Ablegen, Tauschen und Ändern des Layouts sowie Inhalts einer Web-Seite in Target.

## Überblick über das Szenario

Auf der Homepage der WKND-Site werden lokale Aktivitäten oder die besten Unternehmungen in einer Stadt in Form von Karten-Layouts präsentiert. Als Marketing-Fachkraft haben Sie die Aufgabe erhalten, die Homepage zu ändern, indem Sie die Karten-Layouts neu anordnen, um zu sehen, wie sich dies auf die Benutzerinteraktion auswirkt und die Konversion vorantreibt.

### Beteiligte Benutzerinnen und Benutzer

Für diese Übung müssen die folgenden Benutzenden einbezogen werden. Bei bestimmten Aufgaben benötigen Sie außerdem unter Umständen Administratorzugriff.

* **Inhaltserstellerin/-Inhaltsbearbeiterin bzw. Inhaltsersteller/-Inhaltsbearbeiter** (Adobe Experience Manager)
* **Marketing-Fachkraft** (Adobe Target/Optimierungs-Team)

### Homepage der WKND-Site

![AEM Target-Szenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Voraussetzungen

* **AEM**
   * Die [AEM-Veröffentlichungsinstanz](./implementation.md#getting-aem) wird auf „localhost:4503“ ausgeführt.
   * [AEM ist über Adobe Experience Platform Launch in Adobe Target integriert.](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Es besteht Zugriff auf die Adobe Experience Cloud-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud wurde mit [Adobe Target](https://experiencecloud.adobe.com) bereitgestellt.

## Aktivitäten der Marketing-Fachkraft

1. Die Marketing-Fachkraft erstellt eine A/B-Zielaktivität in Adobe Target.
   1. Navigieren Sie im Adobe Target-Fenster zur Registerkarte **Aktivitäten**.
   2. Klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie **A/B-Test** als Aktivitätstyp aus.
      ![Adobe Target – Aktivität erstellen](assets/personalization-use-case-2/create-ab-activity.png)
   3. Wählen Sie den **Web-Kanal** und dann **Visual Experience Composer** aus.
   4. Geben Sie die **Aktivitäts-URL** ein und klicken Sie auf **Weiter**, um Visual Experience Composer zu öffnen.
      ![Adobe Target – Aktivität erstellen](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Zum Laden von **Visual Experience Composer** aktivieren Sie in Ihrem Browser die Option **Unsichere Skripte laden** und laden Sie die Seite neu.
      ![Experience Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Die Homepage der WKND-Site wird im Visual Experience Composer-Editor geöffnet.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Mit **Erlebnis A** wird die standardmäßige WKND-Homepage bereitgestellt. Ändern Sie nun das Inhalts-Layout für **Erlebnis B**.
      ![Erlebnis B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klicken Sie auf einen der Karten-Layout-Container (*Best Roaster*) und wählen Sie die Option **Neu anordnen** aus.
      ![Container-Auswahl](assets/personalization-use-case-3/container-selection.png)
   9. Klicken Sie auf den Container, der neu angeordnet werden soll, und ziehen Sie ihn an die gewünschte Position. Ordnen Sie den Container *Best Roaster* neu an, sodass er nicht mehr in der ersten Zeile der ersten Spalte, sondern in der ersten Zeile der dritten Spalte erscheint. Der Container *Best Roaster* befindet sich nun neben dem Container *Photography Exhibitions*.
      ![Container-Tausch](assets/personalization-use-case-3/container-swap.png)
      **Nach dem Tausch**
      ![Getauschte Container](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Ordnen Sie die Positionen für die anderen Karten-Container auf die gleiche Weise neu an.
      ![Getauschte Container](assets/personalization-use-case-3/after-swap-all.png)
   11. Fügen Sie auch einen Kopfzeilentext unter der Karussellkomponente und über dem Karten-Layout hinzu.
   12. Klicken Sie auf den Karussell-Container und wählen Sie die Option **Einfügen nach > HTML** aus, um HTML hinzuzufügen.
      ![Text hinzufügen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Text hinzufügen](assets/personalization-use-case-3/after-changes.png)
   13. Klicken Sie auf **Weiter**, um mit der Aktivität fortzufahren.
   14. Wählen Sie unter **Traffic-Zuordnungsmethode** die Option „Manuell“ aus und weisen Sie 100 % des Traffics dem **Erlebnis B** zu.
      ![Traffic für Erlebnis B](assets/personalization-use-case-2/traffic.png)
   15. Klicken Sie auf **Weiter**.
   16. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie den A/B-Test.
      ![Zielmetrik für A/B-Test](assets/personalization-use-case-2/goal-metric.png)
   17. Geben Sie einen Namen (**Aktualisierung der WKND-Homepage**) für Ihre Aktivität ein und speichern Sie Ihre Änderungen.
   18. Stellen Sie im Bildschirm mit den Aktivitätsdetails sicher, dass Sie Ihre Aktivität **aktivieren**.
      ![Aktivieren der Aktivität](assets/personalization-use-case-3/save-activity.png)
   19. Navigieren Sie zur WKND-Homepage (http://localhost:4503/content/wknd/en.html). Ihnen werden die Änderungen auffallen, die wir zur A/B-Testaktivität im Rahmen der WKND-Homepage-Aktualisierung hinzugefügt haben.
      ![Aktualisierte WKND-Homepage](assets/personalization-use-case-3/activity-result.png)
   20. Öffnen Sie Ihre Browser-Konsole und überprüfen Sie die Registerkarte „Netzwerk“, um sich die Zielreaktion auf die A/B-Testaktivität im Rahmen der WKND-Homepage-Aktualisierung anzusehen.
      ![Netzwerkaktivität](assets/personalization-use-case-3/activity-result.png)

## Zusammenfassung

In diesem Kapitel konnte eine Marketing-Fachkraft mithilfe von Visual Experience Composer durch Ziehen und Ablegen, Tauschen und Ändern des Layouts sowie Inhalts einer Web-Seite ein Erlebnis erstellen, ohne den Code zum Ausführen eines Tests zu ändern.
