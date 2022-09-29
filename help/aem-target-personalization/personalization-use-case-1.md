---
title: Personalisierung mit AEM Experience Fragments und Adobe Target
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: Ein durchgehendes Tutorial, in dem gezeigt wird, wie mit Adobe Experience Manager Experience Fragments und Adobe Target personalisierte Erlebnisse erstellt und bereitgestellt werden.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1691'
ht-degree: 1%

---

# Personalisierung mit AEM Experience Fragments und Adobe Target

Mit der Möglichkeit, AEM Experience Fragments als HTML-Angebote in Adobe Target zu exportieren, können Sie die Benutzerfreundlichkeit und Leistungsfähigkeit von AEM mit leistungsstarken Funktionen der automatisierten Intelligenz (AI) und des maschinellen Lernens (ML) in Target kombinieren, um Erlebnisse bedarfsgerecht zu testen und zu personalisieren.

AEM führt all Ihre Inhalte und Assets an einem zentralen Ort zusammen, um Ihre Personalisierungsstrategie zu unterstützen. Mit AEM können Sie mühelos Inhalte für Desktops, Tablets und Mobilgeräte an einem Ort erstellen, ohne Code zu schreiben. Es ist nicht erforderlich, Seiten für jedes Gerät zu erstellen. AEM passt jedes Erlebnis automatisch an Ihren Inhalt an.

Mit Target können Sie personalisierte Erlebnisse bedarfsgerecht bereitstellen. Dies erfolgt auf der Grundlage einer Kombination aus regelbasierten und KI-gestützten Ansätzen des maschinellen Lernens, zu denen Verhaltens-, Kontext- und Offline-Variablen zählen.  Mit Target können Sie mühelos A/B- und Multivarianz-Aktivitäten (MVT) einrichten und ausführen, um die besten Angebote, Inhalte und Erlebnisse zu ermitteln.

Experience Fragments sind ein großer Schritt vorwärts, um Ersteller von Inhalten mit Marketingexperten zu verknüpfen, die Geschäftsergebnisse mit Target optimieren.

## Szenario - Überblick

Die WKND-Site plant, eine **SkateFest Challenge** über ihre Website in ganz Amerika und möchten, dass sich ihre Site-Benutzer für die Vorführung in jedem Staat anmelden. Marketingexperten haben die Aufgabe erhalten, eine Kampagne auf der WKND-Site-Startseite auszuführen, wobei Bannernachrichten für den Standort der Benutzer und ein Link zur Seite mit den Ereignisdetails angezeigt werden. Lassen Sie uns die WKND-Site-Startseite durchsuchen und erfahren, wie ein personalisiertes Erlebnis für einen Benutzer basierend auf seinem aktuellen Standort erstellt und bereitgestellt wird.

### Betroffene Benutzer

Für diese Übung müssen die folgenden Benutzer beteiligt sein und einige Aufgaben ausführen, für die Sie möglicherweise Administratorzugriff benötigen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimierungsteam)

### Voraussetzungen

* **AEM**
   * [AEM der Autoren- und Veröffentlichungsinstanz](./implementation.md#getting-aem) auf localhost 4502 bzw. 4503 ausgeführt werden.
* **Experience Cloud**
   * Zugriff auf Ihre Unternehmen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND-Site-Homepage

![AEM Target-Szenario 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Marketer startet die WKND SkateFest-Kampagnendiskussion mit AEM Content Editor und erläutert die Anforderungen.
   * ***Anforderung***: Bewerben Sie die WKND SkateFest-Kampagne auf der WKND-Site-Homepage mit personalisierten Inhalten für Besucher aus allen US-Bundesstaaten. Fügen Sie unter dem Karussell der Homepage einen neuen Inhaltsbaustein hinzu, der ein Hintergrundbild, Text und eine Schaltfläche enthält.
      * **Hintergrundbild**: Das Bild sollte für den Status relevant sein, von dem aus der Benutzer die WKND-Site-Seite besucht.
      * **Text**: &quot;Registrieren Sie sich für die Auditionen&quot;
      * **Schaltfläche**: &quot;Ereignisdetails&quot;mit Verweis auf die WKND-SkateFest-Seite
      * **WKND SkateFest-Seite**: eine neue Seite mit Ereignisdetails, einschließlich Veranstaltungsort, Datum und Uhrzeit.
1. Basierend auf den Anforderungen erstellt AEM Inhaltseditor ein Experience Fragment für den Inhaltsbaustein und exportiert es als Angebot in Adobe Target. Um personalisierte Inhalte für alle US-Bundesstaaten bereitzustellen, kann der Inhaltsautor eine Übergeordnete Variante des Experience Fragment erstellen und dann 50 weitere Varianten erstellen, eine für jeden Bundesstaat. Inhalte für jede Statusvariante mit relevanten Bildern und Text können dann manuell bearbeitet werden. Beim Erstellen eines Experience Fragments können Inhaltseditoren schnell auf alle in AEM Assets verfügbaren Assets zugreifen, indem sie die Asset-Finder-Option verwenden. Wenn ein Experience Fragment nach Adobe Target exportiert wird, werden alle Varianten auch als Angebote an Adobe Target gesendet.

1. Nach dem Exportieren von Experience Fragment aus AEM in Adobe Target als Angebote können Marketing-Experten in Target mithilfe dieser Angebote Aktivitäten erstellen. Basierend auf der SkateFest-Kampagne auf der WKND-Site muss der Marketing-Experte ein personalisiertes Erlebnis für WKND-Site-Besucher aus jedem Bundesstaat erstellen und bereitstellen. Um eine Erlebnis-Targeting-Aktivität zu erstellen, muss der Marketing-Experte die Zielgruppen identifizieren. Für unsere WKND SkateFest-Kampagne müssen wir 50 separate Zielgruppen erstellen, basierend auf ihrem Standort, von dem aus sie die WKND-Website besuchen.
   * [Zielgruppen](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) Definieren Sie die Zielgruppe für Ihre Aktivität und werden überall dort verwendet, wo Targeting verfügbar ist. Zielgruppen sind ein festgelegter Satz von Besucherkriterien. Angebote können auf bestimmte Zielgruppen (oder Segmente) ausgerichtet werden. Nur Besucher, die zu dieser Zielgruppe gehören, sehen das Erlebnis, das für sie bestimmt ist.  Sie können beispielsweise ein Angebot für eine Zielgruppe bereitstellen, das aus Besuchern besteht, die einen bestimmten Browser oder einen bestimmten geografischen Standort verwenden.
   * Ein [Angebot](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) ist der Inhalt, der während Kampagnen oder Aktivitäten auf Ihren Webseiten angezeigt wird. Wenn Sie Ihre Webseiten testen, messen Sie den Erfolg jedes Erlebnisses mit verschiedenen Angeboten an Ihren Standorten. Ein Angebot kann verschiedene Inhaltstypen enthalten, darunter:
      * Bild
      * Text
      * **HTML**
         * *HTML-Angebote werden für die Aktivität dieses Szenarios verwendet*
      * Verknüpfung
      * Schaltfläche

## Content Editor-Aktivitäten

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Veröffentlichen Sie das Experience Fragment, bevor Sie es in Adobe Target exportieren.

## Marketingaktivitäten

### Erstellen einer Zielgruppe mit Geotargeting {#marketer-audience}

1. Navigieren zu Ihren Organisationen [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Melden Sie sich mit Ihrer Adobe ID an und stellen Sie sicher, dass Sie sich in der richtigen Organisation befinden.
1. Klicken Sie im Lösungsmenü auf **Target** und dann **launch** Adobe Target.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navigieren Sie zum **Angebote** und suchen Sie nach &quot;WKND&quot;-Angeboten. Sie sollten die Liste der Experience Fragments-Varianten sehen können, die aus AEM als HTML-Angebote exportiert wurden. Jedes Angebot entspricht einem Status. Beispiel: *WKND SkateFest Kalifornien* ist das Angebot, das einem WKND Site-Besucher aus Kalifornien bereitgestellt wird.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Klicken Sie im Hauptnavigationsmenü auf **Zielgruppen**.

   Ein Marketer muss 50 separate Zielgruppen für WKND-Site-Besucher aus jedem Bundesstaat in den USA erstellen.

1. Um eine Audience zu erstellen, klicken Sie auf **Zielgruppe erstellen** und geben Sie einen Namen für Ihre Zielgruppe an.

   **Format des Zielgruppennamens : WKND-\&lt;*state*\>**

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Klicken **Regel hinzufügen > Geo**.
1. Klicken **Auswählen** und wählen Sie dann eine der folgenden Optionen aus:
   * Land
   * **state** *(Status für WKND Site SkateFest-Kampagne auswählen)*
   * Stadt
   * Postleitzahl
   * Breite
   * Länge
   * DMA
   * Mobilnetzbetreiber

   **Geo** - Verwenden Sie Zielgruppen, um Benutzer anhand ihres geografischen Standorts auszuwählen, einschließlich Land, Bundesland, Stadt, Postleitzahl, DMA oder Mobilnetzbetreiber. Mit Geolocation-Parametern können Sie Aktivitäten und Erlebnisse auf Basis der geografischen Daten Ihrer Besucher auswählen. Diese Daten werden mit jeder Target-Anfrage gesendet und basieren auf der IP-Adresse des Besuchers. Wählen Sie diese Parameter wie beliebige Zielgruppenwerte aus.

   >[!NOTE]
   >Die IP-Adresse eines Besuchers wird einmal pro Besuch (Sitzung) mit einer Mbox-Anfrage übergeben, um Geotargeting-Parameter für diesen Besucher aufzulösen.

1. Wählen Sie den Operator als **matches**, geben Sie einen geeigneten Wert an (z. B.: Kalifornien) und **Speichern** Ihre Änderungen. Geben Sie in unserem Fall den Statusnamen an.

   ![Adobe Target - Geo-Regel](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Sie können einer Zielgruppe mehrere Regeln zuweisen.

1. Wiederholen Sie die Schritte 6 bis 9, um Zielgruppen für die anderen Status zu erstellen.

   ![Adobe Target - WKND-Zielgruppen](assets/personalization-use-case-1/adobe-target-audiences-50.png)

An dieser Stelle haben wir erfolgreich Zielgruppen für alle WKND Site-Besucher aus verschiedenen Bundesstaaten in den USA erstellt und haben auch das entsprechende HTML-Angebot für jeden Bundesstaat. Erstellen wir nun eine Erlebnis-Targeting-Aktivität, um die Zielgruppe mit einem entsprechenden Angebot für die WKND-Site-Startseite anzusprechen.

### Erstellen einer Aktivität mit Geotargeting

1. Navigieren Sie im Adobe Target-Fenster zu **Tätigkeiten** Registerkarte.
1. Klicken **Aktivität erstellen** und wählen Sie die **Erlebnis-Targeting** Aktivitätstyp.
1. Wählen Sie die **Web** Kanal und wählen Sie die **Visual Experience Composer**.
1. Geben Sie die **Aktivitäts-URL** und klicken Sie auf **Nächste** , um den Visual Experience Composer zu öffnen.

   WKND Site Home Page Publish URL: http://localhost:4503/content/wknd/en.html

   ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/target-activity.png)

1. Für **Visual Experience Composer** zum Laden aktivieren **Unsichere Skripte laden zulassen** in Ihrem Browser und laden Sie Ihre Seite neu.

   ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Beachten Sie, dass die WKND Site-Startseite im Visual Experience Composer-Editor geöffnet ist.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Um dem VEC eine Zielgruppe hinzuzufügen, klicken Sie auf **Hinzufügen von Erlebnis-Targeting** unter Zielgruppen , wählen Sie die Zielgruppe WKND-California aus und klicken Sie auf **Nächste**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Klicken Sie auf die WKND-Site-Seite in VEC, wählen Sie das HTML-Element aus, um das Angebot für die WKND-California-Zielgruppe hinzuzufügen, und wählen Sie aus. **Ersetzen durch** und wählen Sie dann die **HTML-Angebot**.

   ![Erlebnis-Targeting-Aktivität](assets/personalization-use-case-1/vec-selecting-div.png)

1. Wählen Sie die **WKND SkateFest Kalifornien** HTML-Angebot für **WKND-Kalifornien** Zielgruppe aus der ausgewählten Benutzeroberfläche und Klicken **Fertig**.
1. Sie sollten jetzt die **WKND SkateFest Kalifornien** Das HTML-Angebot wurde Ihrer WKND-Site-Seite für die WKND-California-Zielgruppe hinzugefügt.
1. Wiederholen Sie die Schritte 7 bis 10, um Erlebnis-Targeting für die anderen Status hinzuzufügen und das entsprechende HTML-Angebot auszuwählen.
1. Klicken **Nächste** um fortzufahren, und Sie sehen eine Zuordnung für Zielgruppen zu Erlebnissen.
1. Klicken **Nächste** , um zu Ziele und Einstellungen zu wechseln.
1. Wählen Sie Ihre Berichtsquelle aus und bestimmen Sie ein Hauptziel für Ihre Aktivität. Wählen Sie für unser Szenario als Berichtsquelle **Adobe Target** Messaktivität als **Konversion**, Aktion, die eine Seite angezeigt hat, und URL, die auf die Seite &quot;WKND SkateFest Details&quot;verweist.

   ![Ziel und Targeting - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Sie können auch Adobe Analytics als Berichtsquelle auswählen.

1. Bewegen Sie den Mauszeiger über den aktuellen Aktivitätsnamen und benennen Sie ihn um in **WKND SkateFest - USA**, und dann **Speichern und schließen** Ihre Änderungen.
1. Stellen Sie im Bildschirm &quot;Aktivitätsdetails&quot;sicher, dass Sie **Aktivieren** Ihre Aktivität.

   ![Aktivität aktivieren](assets/personalization-use-case-1/activate-activity.png)

1. Ihre WKND SkateFest-Kampagne ist jetzt für alle WKND Site-Besucher live.
1. Navigieren Sie zum [WKND-Site-Homepage](http://localhost:4503/content/wknd/en.html)und Sie sollten das WKND SkateFest-Angebot basierend auf Ihrem geografischen Standort (*state: Kalifornien*).

   ![Aktivitäts-QA](assets/personalization-use-case-1/wknd-california.png)

### Target-Aktivitäts-QA

1. under **Aktivitätsdetails > Übersicht** klicken Sie auf die **Aktivitäts-QA** und Sie erhalten den direkten QA-Link zu allen Ihren Erlebnissen.

   ![Aktivitäts-QA](assets/personalization-use-case-1/activity-qa.png)

1. Navigieren Sie zum [WKND-Site-Homepage](http://localhost:4503/content/wknd/en.html)und Sie sollten das WKND SkateFest-Angebot basierend auf Ihrem geografischen Standort (Status) sehen können.
1. Sehen Sie sich das folgende Video an, um zu verstehen, wie ein Angebot an Ihre Seite gesendet wird, wie Sie Antwort-Token anpassen und eine Qualitätsprüfung durchführen können.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Zusammenfassung

In diesem Kapitel konnte ein Inhaltseditor den gesamten Inhalt erstellen, um die WKND SkateFest-Kampagne in Adobe Experience Manager zu unterstützen, und ihn als HTML-Angebote in Adobe Target exportieren, um Erlebnis-Targeting zu erstellen, basierend auf dem geografischen Standort des Benutzers.
