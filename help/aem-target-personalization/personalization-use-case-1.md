---
title: Personalisierung mit AEM Experience Fragments und Adobe Target
description: Ein Tutorial, das von Anfang bis Ende zeigt, wie mithilfe von Adobe Experience Manager Experience Fragments und Adobe Target personalisierte Erlebnisse geschaffen und bereitgestellt werden können.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1149
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 100%

---

# Personalisierung mit AEM Experience Fragments und Adobe Target

Durch die Möglichkeit, AEM Experience Fragments als HTML-Angebote in Adobe Target zu exportieren, können Sie die Benutzerfreundlichkeit und Leistungsfähigkeit von AEM mit leistungsstarken Funktionen für automatisierte Intelligenz (AI) und maschinelles Lernen (ML) in Target kombinieren, um Erlebnisse bedarfsgerecht zu testen und zu personalisieren.

AEM führt all Ihre Inhalte und Assets an einem zentralen Ort zusammen, um Ihre Personalisierungsstrategie zu unterstützen. Mit AEM können Sie mühelos Inhalte für Desktop-Computer, Tablets und Mobilgeräte an einem Ort erstellen, ohne Code zu schreiben. Es ist nicht erforderlich, Seiten für jedes Gerät zu erstellen – AEM passt jedes Erlebnis automatisch an Ihre Inhalte an. 

Mit Target können Sie personalisierte Erlebnisse bedarfsgerecht bereitstellen, und zwar basierend auf einer Kombination aus regelbasierten und auf automatisierter Intelligenz gestützten Ansätzen des maschinellen Lernens, die Verhaltens-, Kontext- und Offline-Variablen beinhalten. Mit Target können Sie mühelos A/B- und Multivarianz-Aktivitäten (MVT) einrichten und ausführen, um die besten Angebote, Inhalte und Erlebnisse zu ermitteln.

Das Experience Fragments-Konzept stellt einen großen Schritt nach vorne dar, um Erstellerinnen und Ersteller von Inhalten mit Marketing-Fachkräften zu verbinden, die Geschäftsergebnisse mit Target optimieren.

## Überblick über das Szenario

Auf der WKND-Site soll eine **SkateFest Challenge** für ganz Amerika bekannt gegeben werden. Die Benutzenden der Site sollen sich dabei für die in jedem US-Bundesstaat durchgeführten Auditionen anmelden. Als Marketing-Fachkraft haben Sie die Aufgabe erhalten, eine Kampagne auf der Homepage der WKND-Site zu starten und dabei Bannernachrichten einzurichten, die für den Standort der Benutzenden relevant sind, sowie einen Link zur Seite mit den Ereignisdetails bereitzustellen. Untersuchen wir die Homepage der WKND-Site, um zu erfahren, wie ein personalisiertes Erlebnis für eine Benutzerin oder einen Benutzer basierend auf ihrem bzw. seinem aktuellen Standort geschaffen und bereitgestellt wird.

### Beteiligte Benutzerinnen und Benutzer

Für diese Übung müssen die folgenden Benutzenden einbezogen werden. Bei bestimmten Aufgaben benötigen Sie außerdem unter Umständen Administratorzugriff.

* **Inhaltserstellerin/-bearbeiterin bzw. Inhaltsersteller/-bearbeiter** (Adobe Experience Manager)
* **Marketing-Fachkraft** (Adobe Target/Optimierungs-Team)

### Voraussetzungen

* **AEM**
   * Die [AEM-Autoren- und -Veröffentlichungsinstanz](./implementation.md#getting-aem) wird auf dem localhost-Port 4502 bzw. 4503 ausgeführt.
* **Experience Cloud**
   * Es besteht Zugriff auf die Adobe Experience Cloud-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud ist mit folgender Lösung bereitgestellt:
      * [Adobe Target](https://experiencecloud.adobe.com)

### Homepage der WKND-Site

![AEM Target-Szenario 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Die Marketing-Fachkraft spricht die AEM-Inhaltsbearbeiterin bzw. den AEM-Inhaltsbearbeiter auf die WKND SkateFest-Kampagne an und stellt die Anforderungen im Detail dar.
   * ***Anforderung***: Werbung für die WKND SkateFest-Kampagne auf der WKND-Site-Homepage mit personalisierten Inhalten für Besucherinnen und Besucher aus allen US-Bundesstaaten. Hinzufügen eines neuen Inhaltsblocks unter dem Homepage-Karussell mit Hintergrundbild, Text und einer Schaltfläche.
      * **Hintergrundbild**: Das Bild sollte für den Bundesstaat relevant sein, von dem aus die Person die WKND-Site-Seite besucht.
      * **Text**: „Jetzt für die Auditionen anmelden“.
      * **Schaltfläche**: „Ereignisdetails“ mit einem Verweis auf die WKND-SkateFest-Seite.
      * **WKND SkateFest-Seite**: Eine neue Seite mit Ereignisdetails, einschließlich Ort, Datum und Uhrzeit der Audition.
1. Basierend auf den Anforderungen erstellt die AEM-Inhaltsbearbeiterin bzw. der AEM-Inhaltsbearbeiter ein Experience Fragment für den Inhaltsblock und exportiert es als Angebot in Adobe Target. Um personalisierte Inhalte für alle US-Bundesstaaten bereitzustellen, kann die Inhaltsautorin bzw. der Inhaltsautor eine Experience Fragment-Primärvariante kreieren und dann 50 weitere Varianten erstellen – eine für jeden Bundesstaat. Inhalte für jede Bundesstaatvariante mit relevanten Bildern und Texten können dann manuell bearbeitet werden. Beim Erstellen von Experience Fragments können Inhaltsbearbeiterinnen und -bearbeiter über die Asset-Suchoption schnell auf alle in AEM Assets verfügbaren Assets zugreifen. Wenn ein Experience Fragment nach Adobe Target exportiert wird, werden alle zugehörigen Varianten ebenfalls als Angebote an Adobe Target übertragen.

1. Nachdem das Experience Fragment aus AEM nach Adobe Target in Angebotsform exportiert wurde, kann die Marketing-Fachkraft in Target mithilfe dieser Angebote Aktivitäten erstellen. Basierend auf der SkateFest-Kampagne auf der WKND-Site muss die Marketing-Fachkraft ein personalisiertes Erlebnis für Besucherinnen und Besucher der WKND-Site aus jedem Bundesstaat schaffen und bereitstellen. Um eine Experience Targeting-Aktivität zu erstellen, muss die Marketing-Fachkraft die Zielgruppen identifizieren. Für unsere WKND SkateFest-Kampagne müssen wir 50 separate Zielgruppen erstellen, basierend auf ihrem Standort, von dem aus sie die WKND-Website besuchen.
   * [Zielgruppen](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=de#section_3F32DA46BDF947878DD79DBB97040D01) definieren das Ziel für Ihre Aktivität und werden überall dort verwendet, wo Targeting verfügbar ist. Bei Zielgruppen handelt es sich um einen festgelegten Satz von Besucherkriterien. Angebote können auf bestimmte Zielgruppen (oder Segmente) ausgerichtet werden. Nur Besuchende, die zu dieser Zielgruppe gehören, sehen das Erlebnis, das für sie bestimmt ist. Sie können beispielsweise ein Angebot für eine Zielgruppe bereitstellen, die aus Besuchenden mit einem bestimmten Browser oder geografischen Standort besteht.
   * Ein [Angebot](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=de#section_973D4CC4CEB44711BBB9A21BF74B89E9) ist der Inhalt, der während Kampagnen oder Aktivitäten auf Ihren Web-Seiten angezeigt wird. Wenn Sie Ihre Web-Seiten testen, messen Sie den Erfolg jedes Erlebnisses mit verschiedenen Angeboten an Ihren Standorten. Ein Angebot kann verschiedene Inhaltstypen enthalten, darunter:
      * Bild
      * Text
      * **HTML**
         * *HTML-Angebote werden für die Aktivität dieses Szenarios verwendet.*
      * Link
      * Schaltfläche

## Aktivitäten der Inhaltsbearbeiterin bzw. des -Inhaltsbearbeiters

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Veröffentlichen Sie das Experience Fragment, bevor Sie es nach Adobe Target exportieren.

## Aktivitäten der Marketing-Fachkraft

### Erstellen einer Zielgruppe mit Geotargeting {#marketer-audience}

1. Navigieren Sie zur [Adobe Experience Cloud](https://experiencecloud.adobe.com/)-Bereitstellung Ihres Unternehmens: `<https://<yourcompany>.experiencecloud.adobe.com`.
1. Melden Sie sich mit Ihrer Adobe ID an und stellen Sie sicher, dass Sie sich in der richtigen Organisation befinden.
1. Klicken Sie in der Lösungsauswahl auf **Target** und **starten** Sie dann Adobe Target.

   ![Experience Cloud – Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navigieren Sie zur Registerkarte **Angebote** und suchen Sie nach WKND-Angeboten. Sie sollten die Liste der Experience Fragments-Varianten sehen können, die aus AEM als HTML-Angebote exportiert wurden. Jedes Angebot entspricht einem Bundesstaat. Beispielsweise ist *WKND SkateFest California* das Angebot, das Besucherinnen und Besuchern der WKND-Site aus Kalifornien präsentiert wird.

   ![Experience Cloud – Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Klicken Sie in der Hauptnavigation auf **Zielgruppen**.

   Eien Marketing-Fachkraft muss 50 separate Zielgruppen für Besucherinnen und Besucher der WKND-Site aus jedem US-Bundesstaat erstellen.

1. Um eine Zielgruppe zu erstellen, klicken Sie auf die Schaltfläche **Zielgruppe erstellen** und geben Sie einen Namen für Ihre Zielgruppe an.

   **Format des Zielgruppennamens: WKND-\&lt;*Bundesstaat*\>**

   ![Experience Cloud – Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Klicken Sie auf **Regel hinzufügen > Geo**.
1. Klicken Sie auf **Auswählen** und wählen Sie dann eine der folgenden Optionen aus:
   * Land
   * **Bundesstaat** *(„Bundesstaat“ für die SkateFest-Kampagne der WKND-Site auswählen)*
   * Stadt
   * Postleitzahl
   * Breitengrad
   * Längengrad
   * DMA
   * Mobilnetzbetreiber

   **Geo**: Verwenden Sie Zielgruppen, um Benutzende anhand ihres geografischen Standorts anzusprechen, einschließlich Land, Bundesstaat (oder abhängig vom Land: Region, Bundesland oder Kanton), Stadt, Postleitzahl, DMA oder Mobilnetzbetreiber. Mit Geolokationsparametern können Sie Aktivitäten und Erlebnisse auf Basis der geografischen Daten Ihrer Besuchenden ausrichten. Diese Daten werden mit jeder Target-Anfrage gesendet und basieren auf der IP-Adresse der Besucherin oder des Besuchers. Wählen Sie diese Parameter wie beliebige andere Zielgruppenbestimmungswerte aus.

   >[!NOTE]
   >Die IP-Adresse einer Besucherin oder eines Besuchers wird einmal pro Besuch (Sitzung) mit einer Mbox-Anfrage weitergegeben, um Geotargeting-Parameter für diese Person aufzulösen.

1. Wählen Sie als Operator **Stimmt überein** aus, geben Sie einen geeigneten Wert an (z. B. Kalifornien) und **speichern** Sie Ihre Änderungen. Geben Sie in unserem Fall den Namen des Bundesstaates an.

   ![Adobe Target – Geo-Regel](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Sie können einer Zielgruppe mehrere Regeln zuweisen.

1. Wiederholen Sie die Schritte 6 bis 9, um Zielgruppen für die anderen Bundesstaaten zu erstellen.

   ![Adobe Target – WKND-Zielgruppen](assets/personalization-use-case-1/adobe-target-audiences-50.png)

An dieser Stelle haben wir erfolgreich Zielgruppen für alle Besucherinnen und Besucher der WKND-Site aus verschiedenen US-Bundesstaaten erstellt und verfügen zudem über das entsprechende HTML-Angebot für jeden Bundesstaat. Erstellen wir nun eine Experience Targeting-Aktivität, um die Zielgruppe mit einem entsprechenden Angebot für die Homepage der WKND-Site anzusprechen.

### Erstellen einer Aktivität mit Geotargeting

1. Navigieren Sie im Adobe Target-Fenster zur Registerkarte **Aktivitäten**.
1. Klicken Sie auf **Aktivität erstellen** und wählen Sie den Aktivitätstyp **Experience Targeting** aus.
1. Wählen Sie den **Web-Kanal** und dann **Visual Experience Composer** aus.
1. Geben Sie die **Aktivitäts-URL** ein und klicken Sie auf **Weiter**, um Visual Experience Composer zu öffnen.

   Veröffentlichungs-URL der Homepage der WKND-Site: http://localhost:4503/content/wknd/en.html

   ![Experience Targeting-Aktivität](assets/personalization-use-case-1/target-activity.png)

1. Zum Laden von **Visual Experience Composer** aktivieren Sie in Ihrem Browser die Option **Unsichere Skripte laden** und laden Sie die Seite neu.

   ![Experience Targeting-Aktivität](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Die Homepage der WKND-Site wird im Visual Experience Composer-Editor geöffnet.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Um VEC eine Zielgruppe hinzuzufügen, klicken Sie unter „Zielgruppen“ auf **Experience Targeting hinzufügen**, wählen Sie die Zielgruppe „WKND-California“ aus und klicken Sie auf **Weiter**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Klicken Sie auf die WKND-Site-Seite in VEC und wählen Sie das HTML-Element aus, um das Angebot für die Zielgruppe „WKND-California“ hinzuzufügen. Wählen Sie dann die Option **Ersetzen durch** und schließlich das **HTML-Angebot** aus.

   ![Experience Targeting-Aktivität](assets/personalization-use-case-1/vec-selecting-div.png)

1. Wählen Sie das HTML-Angebot **WKND SkateFest California** für die Zielgruppe **WKND-California** aus der Benutzeroberfläche für die Angebotsauswahl aus und klicken Sie auf **Fertig**.
1. Sie sollten jetzt das HTML-Angebot **WKND SkateFest California** sehen, das Ihrer WKND-Site-Seite für die Zielgruppe „WKND-California“ hinzugefügt wurde.
1. Wiederholen Sie die Schritte 7 bis 10, um Experience Targeting für die anderen Bundesstaaten hinzuzufügen und das entsprechende HTML-Angebot zu wählen.
1. Klicken Sie auf **Weiter**, um fortzufahren. Es wird eine Zuordnung für Zielgruppen zu Erlebnissen angezeigt.
1. Klicken Sie auf **Weiter**, um zu „Ziele und Einstellungen“ zu wechseln.
1. Wählen Sie Ihre Reporting-Quelle aus und bestimmen Sie ein Hauptziel für Ihre Aktivität. Wählen Sie für unser Szenario **Adobe Target** als Reporting-Quelle, **Konversion** als Messaktivität, „Seite angezeigt“ als Aktion und eine URL, die auf die WKND SkateFest-Detailseite verweist.

   ![Ziel und Targeting – Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Sie können auch Adobe Analytics als Reporting-Quelle auswählen.

1. Bewegen Sie den Mauszeiger über den aktuellen Aktivitätsnamen und benennen Sie ihn um in **WKND SkateFest – USA**. **Speichern und schließen** Sie Ihre Änderungen.
1. Stellen Sie im Bildschirm mit den Aktivitätsdetails sicher, dass Sie Ihre Aktivität **aktivieren**.

   ![Aktivieren der Aktivität](assets/personalization-use-case-1/activate-activity.png)

1. Ihre WKND SkateFest-Kampagne ist jetzt für alle Besucherinnen und Besucher der WKND-Site live.
1. Navigieren Sie zur [Homepage der WKND-Site](http://localhost:4503/content/wknd/en.html). Sie sollten das WKND SkateFest-Angebot basierend auf Ihrer Geolokation (*Bundesstaat: Kalifornien*) sehen können.

   ![Aktivitäts-QA](assets/personalization-use-case-1/wknd-california.png)

### Aktivitäts-QA in Target

1. Klicken Sie unter **Aktivitätsdetails > Überblick** auf die Schaltfläche **Aktivitäts-QA**. Auf diese Weise erhalten Sie den direkten QA-Link zu allen Ihren Erlebnissen.

   ![Aktivitäts-QA](assets/personalization-use-case-1/activity-qa.png)

1. Navigieren Sie zur [Homepage der WKND-Site](http://localhost:4503/content/wknd/en.html). Sie sollten das WKND SkateFest-Angebot basierend auf Ihrer Geolokation (Bundesstaat) sehen können.
1. Sehen Sie sich das folgende Video an, um zu verstehen, wie ein Angebot Ihrer Seite bereitgestellt wird und wie Sie Antwort-Token anpassen und eine Qualitätsprüfung durchführen können.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Zusammenfassung

In diesem Kapitel konnte eine Inhaltsbearbeiterin bzw. ein Inhaltsbearbeiter den gesamten Inhalt erstellen, um die WKND SkateFest-Kampagne in Adobe Experience Manager zu unterstützen, und ihn in Form von HTML-Angeboten nach Adobe Target exportieren, um Experience Targeting basierend auf der Geolokation der Benutzenden zu erstellen.
