---
title: Personalisierung mit Adobe Target
description: Ein Tutorial, das von Anfang bis Ende zeigt, wie mithilfe von Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden können.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 165
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 100%

---

# Personalisierung vollständiger Web-Seiten-Erlebnisse mit Adobe Target

Im vorherigen Kapitel haben wir erfahren, wie man eine Geolokation-basierte Aktivität in Adobe Target mit Inhalten erstellt, die als Experience Fragments erstellt und als HTML-Angebote aus AEM exportiert wurden.

In diesem Kapitel beschäftigen wir uns mit der Erstellung von Aktivitäten, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.

## Überblick über das Szenario

Die Homepage der WKND-Site wurde neu gestaltet, und die Besucherinnen und Besucher der aktuellen Homepage sollen zur neuen Homepage umgeleitet werden. Außerdem gilt es nachzuvollziehen, wie die neu gestaltete Homepage zur Verbesserung der Benutzerinteraktion und Umsatzsteigerung beiträgt. Als Marketing-Fachkraft haben Sie die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucherinnen und Besucher auf die neue Homepage umzuleiten. Lassen Sie uns die WKND-Site-Homepage erkunden und erfahren, wie Aktivitäten mit Adobe Target erstellt werden.

### Beteiligte Benutzerinnen und Benutzer

Für diese Übung müssen die folgenden Benutzenden einbezogen werden. Bei bestimmten Aufgaben benötigen Sie außerdem unter Umständen Administratorzugriff.

* **Inhaltserstellerin/-Inhaltsbearbeiterin bzw. Inhaltsersteller/-Inhaltsbearbeiter** (Adobe Experience Manager)
* **Marketing-Fachkraft** (Adobe Target/Optimierungs-Team)

### Homepage der WKND-Site

![AEM Target-Szenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Voraussetzungen

* **AEM**
   * Die [AEM-Autoren- und -Veröffentlichungsinstanz](./implementation.md#getting-aem) wird auf dem localhost-Port 4502 bzw. 4503 ausgeführt.
   * [AEM ist über Adobe Experience Platform Launch in Adobe Target integriert.](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Es besteht Zugriff auf die Adobe Experience Cloud-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud ist mit folgender Lösung bereitgestellt:
      * [Adobe Target](https://experiencecloud.adobe.com)

## Aktivitäten der Inhaltsbearbeiterin bzw. des -Inhaltsbearbeiters

1. Die Marketing-Fachkraft spricht die AEM-Inhaltsbearbeiterin bzw. den -Inhaltsbearbeiter auf die Neugestaltung der WKND-Homepage an und stellt die Anforderungen im Detail dar.
   * ***Anforderung***: Neugestaltung der Homepage der WKND-Site mit einem kartenbasierten Design
2. Basierend auf den Anforderungen erstellt die AEM-Inhaltsbearbeiterin bzw. der -Inhaltsbearbeiter dann eine neue Homepage für die WKND-Site mit einem kartenbasierten Design und veröffentlicht diese.

## Aktivitäten der Marketing-Fachkraft

1. Die Marketing-Fachkraft erstellt eine A/B-Zielaktivität mit dem Redirect-Angebot als Erlebnis und einen Website-Traffic von 100 % auf die neue Homepage, wobei Erfolgsziel und Metriken hinzugefügt sind.
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
   7. Bewegen Sie den Mauszeiger über **Experience B** und wählen Sie „Andere Optionen anzeigen“ aus.
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url.png)
   8. Wählen Sie **Zu URL umleiten** aus und geben Sie die URL zur neuen WKND-Homepage ein: http://localhost:4503/content/wknd/en1.html
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Speichern** Sie Ihre Änderungen und fahren Sie mit den nächsten Schritten der Aktivitätserstellung fort.
   10. Wählen Sie unter **Traffic-Zuordnungsmethode** die Option „Manuell“ aus und weisen Sie 100 % des Traffics dem **Erlebnis B** zu.
      ![Traffic für Erlebnis B](assets/personalization-use-case-2/traffic.png)
   11. Klicken Sie auf **Weiter**.
   12. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie den A/B-Test.
      ![Zielmetrik für A/B-Test](assets/personalization-use-case-2/goal-metric.png)
   13. Geben Sie einen Namen (**Neugestaltung der WKND-Homepage**) für Ihre Aktivität ein und speichern Sie Ihre Änderungen.
   14. Stellen Sie im Bildschirm mit den Aktivitätsdetails sicher, dass Sie Ihre Aktivität **aktivieren**.
      ![Aktivieren der Aktivität](assets/personalization-use-case-2/ab-activate.png)
   15. Navigieren Sie zur WKND-Homepage (http://localhost:4503/content/wknd/en.html) und Sie werden zur neu gestalteten Homepage der WKND-Site (http://localhost:4503/content/wknd/en1.html) umgeleitet.
      ![Neu gestaltete WKND-Homepage](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Zusammenfassung

In diesem Kapitel konnte eine Marketing-Fachkraft eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.
