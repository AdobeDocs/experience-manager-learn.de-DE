---
title: Personalisierung mit Adobe Target
seo-title: Personalization using Adobe Target
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Personalisierung vollständiger Webseiten-Erlebnisse mit Adobe Target

Im vorherigen Kapitel haben wir erfahren, wie Sie eine standortbasierte Aktivität in Adobe Target mit Inhalten erstellen, die als Experience Fragments erstellt und aus AEM als HTML-Angebote exportiert wurden.

In diesem Kapitel werden wir die Erstellung von Aktivitäten untersuchen, um Ihre Site-Seiten, die auf AEM gehostet werden, mit Adobe Target auf eine neue Seite umzuleiten.

## Szenario - Überblick

Die WKND-Site hat ihre Homepage neu gestaltet und möchte die aktuellen Besucher der Homepage auf die neue Homepage umleiten. Gleichzeitig sollten Sie auch verstehen, wie die neu gestaltete Startseite zur Verbesserung der Benutzerinteraktion und des Umsatzes beiträgt. Marketingexperten haben die Aufgabe erhalten, eine Aktivität zu erstellen, um die Besucher auf die neue Startseite umzuleiten. Lassen Sie uns die WKND-Site-Homepage durchsuchen und erfahren, wie Sie eine Aktivität mit Adobe Target erstellen.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise Administratorzugriff benötigen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimierungsteam)

### WKND-Site-Homepage

![AEM Target-Szenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Voraussetzungen

* **AEM**
   * [AEM der Autoren- und Veröffentlichungsinstanz](./implementation.md#getting-aem) auf localhost 4502 bzw. 4503 ausgeführt werden.
   * [AEM mit Adobe Target mit Adobe Experience Platform Launch integriert](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Zugriff auf Ihre Unternehmen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Target](https://experiencecloud.adobe.com)

## Content Editor-Aktivitäten

1. Der Marketingexperte initiiert mit dem AEM Content Editor die Neugestaltungsdiskussion der WKND-Homepage und stellt die Anforderungen im Detail dar.
   * ***Anforderung*** : WKND Site Home Page mit kartenbasiertem Design neu gestalten.
2. Basierend auf den Anforderungen erstellt AEM Inhaltseditor dann eine neue WKND Site-Homepage mit einem kartenbasierten Design und veröffentlicht die neue Startseite.

## Marketingaktivitäten

1. Marketer erstellt eine A/B-Zielaktivität mit dem Umleitungsangebot als Erlebnis und 100 % Website-Traffic auf die neue Startseite, wobei Erfolgsziel und Metriken hinzugefügt werden.
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
   7. Bewegen **Erlebnis B** und wählen Sie Andere Optionen anzeigen aus.
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url.png)
   8. Auswählen **Zu URL umleiten** und geben Sie die URL zur neuen WKND-Homepage ein. (http://localhost:4503/content/wknd/en1.html)
      ![Erlebnis B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Speichern** Ihre Änderungen vornehmen und mit den nächsten Schritten der Aktivitätserstellung fortfahren.
   10. Wählen Sie die **Traffic-Zuordnungsmethode** als manuellen und 100 %-igen Traffic auf **Erlebnis B**.
      ![Erlebnis B-Traffic](assets/personalization-use-case-2/traffic.png)
   11. Klicken Sie auf **Weiter**.
   12. Bereitstellung **Zielmetriken** für Ihre Aktivität und Speichern und schließen Sie Ihren A/B-Test.
      ![Metrik für A/B-Test-Ziel](assets/personalization-use-case-2/goal-metric.png)
   13. Geben Sie einen Namen (**WKND-Homepage-Neugestaltung**) für Ihre Aktivität und speichern Sie Ihre Änderungen.
   14. Stellen Sie im Bildschirm &quot;Aktivitätsdetails&quot;sicher, dass Sie **Aktivieren** Ihre Aktivität.
      ![Aktivität aktivieren](assets/personalization-use-case-2/ab-activate.png)
   15. Navigieren Sie zur WKND-Homepage (http://localhost:4503/content/wknd/en.html) und Sie werden zur neu gestalteten WKND-Site-Homepage (http://localhost:4503/content/wknd/en1.html) weitergeleitet.
      ![WKND-Homepage neu gestaltet](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Zusammenfassung

In diesem Kapitel konnte ein Marketing-Experte eine Aktivität erstellen, um Ihre auf AEM gehosteten Site-Seiten mit Adobe Target auf eine neue Seite umzuleiten.
