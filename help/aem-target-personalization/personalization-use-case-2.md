---
title: Personalisierung mit Adobe Target
seo-title: Personalisierung mit Adobe Target
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden.
feature: Experience Fragments
topic: 'Personalisierung '
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Personalisierung vollständiger Webseiten-Erlebnisse mit Adobe Target

Im vorherigen Kapitel haben wir erfahren, wie Sie eine standortbasierte Aktivität in Adobe Target mit Inhalten erstellen, die als Experience Fragments erstellt und aus AEM als HTML-Angebote exportiert wurden.

In diesem Kapitel werden wir die Erstellung von Aktivitäten untersuchen, um Ihre Site-Seiten, die auf AEM gehostet werden, mit Adobe Target auf eine neue Seite umzuleiten.

## Szenario - Überblick

Die WKND-Site hat ihre Homepage neu gestaltet und möchte die aktuellen Besucher der Homepage auf die neue Homepage umleiten. Gleichzeitig sollten Sie auch verstehen, wie die neu gestaltete Startseite zur Verbesserung der Benutzerinteraktion und des Umsatzes beiträgt. Marketingexperten haben die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucher auf die neue Startseite umzuleiten. Lassen Sie uns die WKND-Site-Homepage durchsuchen und erfahren, wie Sie eine Aktivität mit Adobe Target erstellen.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise Administratorzugriff benötigen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer**  (Adobe Target/Optimierungsteam)

### WKND-Site-Homepage

![AEM Target-Szenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Voraussetzungen

* **AEM**
   * [AEM Autoren- und ](./implementation.md#getting-aem) Veröffentlichungsinstanz auf localhost 4502 bzw. 4503.
   * [AEM mit Adobe Target mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Target](https://experiencecloud.adobe.com)

## Content Editor-Aktivitäten

1. Der Marketingexperte initiiert mit dem AEM Content Editor die Neugestaltungsdiskussion der WKND-Homepage und stellt die Anforderungen im Detail dar.
   * ***Anforderung*** : WKND Site Home Page mit kartenbasiertem Design neu gestalten.
2. Basierend auf den Anforderungen erstellt AEM Inhaltseditor dann eine neue WKND Site-Homepage mit einem kartenbasierten Design und veröffentlicht die neue Startseite.

## Marketingaktivitäten

1. Marketer erstellt eine A/B-Zielaktivität mit dem Umleitungsangebot als Erlebnis und 100 % Website-Traffic auf die neue Startseite, wobei Erfolgsziel und Metriken hinzugefügt werden.
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
   7. Bewegen Sie den Mauszeiger über **Erlebnis B** und wählen Sie Andere Optionen anzeigen aus.
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url.png)
   8. Wählen Sie die Option **Zur URL umleiten** aus und geben Sie die URL zur neuen WKND-Homepage ein. (http://localhost:4503/content/wknd/en1.html)
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** Speichern Sie Ihre Änderungen und fahren Sie mit den nächsten Schritten der Aktivitätserstellung fort.
   10. Wählen Sie die **Traffic-Zuordnungsmethode** als manuell aus und weisen Sie **Erlebnis B** 100 % Traffic zu.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   11. Klicken Sie auf **Weiter**.
   12. Geben Sie **Zielmetriken** für Ihre Aktivität an und speichern und schließen Sie Ihren A/B-Test.
      ![Metrik für A/B-Test-Ziel](assets/personalization-use-case-2/goal-metric.png)
   13. Geben Sie einen Namen (**WKND-Homepage-Neugestaltung**) für Ihre Aktivität an und speichern Sie Ihre Änderungen.
   14. Stellen Sie im Bildschirm &quot;Aktivitätsdetails&quot;sicher, dass Sie **Aktivieren** Ihre Aktivität auswählen.
      ![Aktivität aktivieren](assets/personalization-use-case-2/ab-activate.png)
   15. Navigieren Sie zur WKND-Homepage (http://localhost:4503/content/wknd/en.html) und Sie werden zur neu gestalteten WKND-Site-Homepage (http://localhost:4503/content/wknd/en1.html) weitergeleitet.
      ![WKND-Homepage neu gestaltet](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketing-Experte eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.
