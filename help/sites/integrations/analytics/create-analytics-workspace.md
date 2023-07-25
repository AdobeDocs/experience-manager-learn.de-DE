---
title: Analysieren von Daten mit Analysis Workspace
description: Erfahren Sie, wie Sie von einer Adobe Experience Manager-Site erfasste Daten Metriken und Dimensionen in Adobe Analytics Report Suites zuordnen. Erfahren Sie, wie Sie mit der Analysis Workspace-Funktion von Adobe Analytics ein detailliertes Berichts-Dashboard erstellen.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
kt: 6409
thumbnail: KT-6296.jpg
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Integration" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '2162'
ht-degree: 1%

---

# Analysieren von Daten mit Analysis Workspace

Erfahren Sie, wie Sie von einer Adobe Experience Manager-Site erfasste Daten Metriken und Dimensionen in Adobe Analytics Report Suites zuordnen. Erfahren Sie, wie Sie mit der Analysis Workspace-Funktion von Adobe Analytics ein detailliertes Berichts-Dashboard erstellen.

## Was Sie erstellen werden {#what-build}

Das WKND-Marketing-Team möchte wissen, welches `Call to Action (CTA)` -Schaltflächen funktionieren am besten auf der Startseite. Erstellen Sie in diesem Tutorial ein Projekt im **Analysis Workspace** , um die Leistung verschiedener CTA-Schaltflächen zu visualisieren und das Benutzerverhalten auf der Site zu verstehen. Die folgenden Informationen werden mit Adobe Analytics erfasst, wenn ein Benutzer auf der WKND-Startseite auf die Schaltfläche Aktionsaufruf (CTA) klickt.

**Analytics-Variablen**

Nachfolgend finden Sie die Analytics-Variablen, die derzeit verfolgt werden:

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA Click Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine Report Suite oder verwenden Sie eine vorhandene.
1. Konfigurieren [Konversionsvariablen (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html) und [Erfolgsereignisse (Ereignisse)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html) in der Report Suite.
1. Erstellen Sie eine [Analysis Workspace-Projekt](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html) um Daten mithilfe von Tools zu analysieren, mit denen Sie schnell Einblicke erstellen, analysieren und teilen können.
1. Geben Sie das Analysis Workspace-Projekt für andere Teammitglieder frei.

## Voraussetzungen

Dieses Tutorial ist eine Fortsetzung der [Verfolgen geklickter Komponenten mit Adobe Analytics](./track-clicked-component.md) und geht davon aus, dass Sie über Folgendes verfügen:

* A **Tag-Eigenschaft** mit dem [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=de) enabled
* **Adobe Analytics** Report Suite-ID und Tracking-Server testen/entwickeln. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) Browsererweiterung , die mit einer Tag-Eigenschaft konfiguriert wurde, die auf der [WKND-Site](https://wknd.site/us/en.html) oder einer AEM Site mit aktivierter Adobe-Datenschicht.

## Konversionsvariablen (eVars) und Erfolgsereignisse (Ereignis)

Die Custom Insight-Konversionsvariable (oder -eVar) wird auf den ausgewählten Webseiten Ihrer Site im Adobe-Code platziert. Ihr Hauptzweck besteht darin, Konversionserfolgsmetriken in benutzerspezifischen Marketing-Berichten zu segmentieren. Ein eVar kann besuchsbasiert sein und ähnelt Cookies. Die Werte, die an eVar-Variablen übergeben werden, folgen dem Benutzer für einen bestimmten Zeitraum.

Wenn ein eVar auf den Wert eines Besuchers gesetzt wird, speichert Adobe diesen Wert automatisch, bis er abläuft. Alle Erfolgsereignisse, die ein Besucher aufruft, während der eVar aktiv ist, werden für den eVar gezählt.

eVars eignen sich am besten zur Messung von Ursache und Wirkung, z. B.:

* Welche internen Kampagnen haben den Umsatz beeinflusst?
* Welche Banneranzeigen führten letztendlich zu einer Registrierung?
* Die Häufigkeit, mit der eine interne Suche verwendet wurde, bevor eine Bestellung aufgegeben wurde

Erfolgsereignisse sind Aktionen, die verfolgt werden können. Sie bestimmen, was ein Erfolgsereignis ist. Wenn ein Besucher beispielsweise auf eine CTA-Schaltfläche klickt, kann das Klickereignis als Erfolgsereignis betrachtet werden.

### eVars konfigurieren

1. Wählen Sie auf der Adobe Experience Cloud-Startseite Ihr Unternehmen aus und starten Sie Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Klicken Sie in der Analytics-Symbolleiste auf **Admin** > **Report Suites** und finden Sie Ihre Report Suite.

   ![Analytics Report Suite](assets/create-analytics-workspace/select-report-suite.png)

1. Wählen Sie die Report Suite aus > **Einstellungen bearbeiten** > **Konversion** > **Konversionsvariablen**

   ![Analytics-Konversionsvariablen](assets/create-analytics-workspace/conversion-variables.png)

1. Verwenden der **Neu hinzufügen** erstellen, erstellen wir Konversionsvariablen, um das Schema wie folgt zuzuordnen:

   * `eVar5` -  `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Neue eVars hinzufügen](assets/create-analytics-workspace/add-new-evars.png)

1. Geben Sie einen geeigneten Namen und eine Beschreibung für jede eVar ein und **Speichern** Ihre Änderungen. Im Analysis Workspace-Projekt werden die eVars mit dem entsprechenden Namen verwendet, sodass die Variablen durch einen benutzerfreundlichen Namen leicht gefunden werden können.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Erfolgsereignisse konfigurieren

Als Nächstes erstellen wir ein Ereignis, um den Klick auf die CTA-Schaltfläche zu verfolgen.

1. Aus dem **Report Suite Manager** auswählen, wählen Sie die **Report Suite-ID** und klicken Sie auf **Einstellungen bearbeiten**.
1. Klicken **Konversion** > **Erfolgsereignisse**
1. Verwenden der **Neu hinzufügen** erstellen Sie ein benutzerdefiniertes Erfolgsereignis, um die CTA-Schaltfläche zu verfolgen, auf die Sie klicken, und dann **Speichern** Ihre Änderungen.
   * `Event`: `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Erstellen eines Projekts in Analysis Workspace {#workspace-project}

Analysis Workspace ist ein flexibles Browser-Tool, mit dem Sie schnell Analysen erstellen und Erkenntnisse austauschen können. Mithilfe der Drag &amp; Drop-Benutzeroberfläche können Sie Ihre Analyse gestalten, Visualisierungen hinzufügen, um Daten zum Leben zu erwecken, einen Datensatz zu kuratieren, Projekte für andere in Ihrer Organisation freizugeben und zu planen.

Erstellen Sie anschließend eine [Projekt](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace) , um ein Dashboard zu erstellen, um die Leistung der CTA-Schaltflächen auf der gesamten Site zu analysieren.

1. Wählen Sie in der Analytics-Symbolleiste die Option **Arbeitsbereich** und klicken Sie auf **Neues Projekt erstellen**.

   ![Arbeitsbereich](assets/create-analytics-workspace/create-workspace.png)

1. Wählen Sie als Ausgangspunkt eine **leeres Projekt** oder wählen Sie eine der vordefinierten Vorlagen aus, die entweder von der Adobe bereitgestellt werden oder von Ihrem Unternehmen erstellte benutzerdefinierte Vorlagen. Je nach Analyse oder Anwendungsfall stehen verschiedene Vorlagen zur Verfügung. [Weitere Infos](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) über die verschiedenen verfügbaren Vorlagenoptionen.

   In Ihrem Workspace-Projekt können Sie über die linke Leiste auf Bedienfelder, Tabellen, Visualisierungen und Komponenten zugreifen. Sie stellen Bausteine für Ihr Projekt her.

   * **[Komponenten](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** - Komponenten sind Dimensionen, Metriken, Segmente oder Datumsbereiche, die alle in einer Freiformtabelle kombiniert werden können, um Ihre Geschäftsfrage zu beantworten. Machen Sie sich mit jedem Komponententyp vertraut, bevor Sie sich mit Ihrer Analyse befassen. Sobald Sie die Terminologie der Komponenten kennen, können Sie mit dem Ziehen und Ablegen beginnen, um Ihre Analyse in einer Freiformtabelle zu erstellen.
   * **[Visualisierungen](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** - Visualisierungen wie Balken- oder Liniendiagramme werden dann über den Daten hinzugefügt, damit sie visuell zum Tragen kommen. Wählen Sie in der linken Leiste das mittlere Symbol Visualisierungen aus, um die vollständige Liste der verfügbaren Visualisierungen anzuzeigen.
   * **[Bedienfelder](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)** - Ein Bedienfeld ist eine Sammlung von Tabellen und Visualisierungen. Sie können auf Bedienfelder über das Symbol oben links in Workspace zugreifen. Bedienfelder sind hilfreich, wenn Sie Ihre Projekte nach Zeiträumen, Report Suites oder Anwendungsfällen für Analysen organisieren möchten. Die folgenden Bedienfeldtypen sind in Analysis Workspace verfügbar:

   ![Vorlagenauswahl](assets/create-analytics-workspace/workspace-tools.png)

### Datenvisualisierung mit Analysis Workspace hinzufügen

Erstellen Sie anschließend eine Tabelle, um eine visuelle Darstellung der Interaktion der Benutzer mit `Call to Action (CTA)` auf der WKND-Site-Startseite. Um eine solche Darstellung zu erstellen, verwenden wir die in der [Verfolgen geklickter Komponenten mit Adobe Analytics](./track-clicked-component.md). Nachfolgend finden Sie eine kurze Zusammenfassung der für Benutzerinteraktionen mit den Aktionsaufrufen -Schaltflächen für die WKND-Site verfolgten Daten.

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Ziehen Sie die **Seite** Dimension in die Freiformtabelle. Sie sollten jetzt in der Lage sein, eine Visualisierung anzuzeigen, die den Seitennamen (eVar 9) und die entsprechenden Seitenansichten (Vorfälle) anzeigt, die in der Tabelle angezeigt werden.

   ![Dimension der Seite](assets/create-analytics-workspace/evar9-dimension.png)

1. Ziehen Sie die **CTA-Klick** (event8) auf die Metrik &quot;Vorkommen&quot;gesetzt und ersetzt sie. Sie können jetzt eine Visualisierung anzeigen, die den Seitennamen (eVar9) und die zugehörige Anzahl der CTA-Klickereignisse auf einer Seite anzeigt.

   ![Seitenmetrik - CTA-Klick](assets/create-analytics-workspace/evar8-cta-click.png)

1. Teilen wir die Seite nach Vorlagentyp auf. Wählen Sie aus den Komponenten die Metrik Seitenvorlage aus und ziehen Sie die Metrik Seitenvorlage auf die Dimension Seitenname . Jetzt können Sie den Seitennamen, aufgeschlüsselt nach Vorlagentyp, anzeigen.

   * **Vorher**
     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Nachher**
     ![eVar5-Metriken](assets/create-analytics-workspace/evar5-metrics.png)

1. Um zu verstehen, wie Benutzer mit CTA-Schaltflächen interagieren, wenn sie sich auf den WKND Site-Seiten befinden, ist eine weitere Aufschlüsselung erforderlich, indem die Metrik Button-ID (eVar8) hinzugefügt wird.

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Unten sehen Sie eine visuelle Darstellung der WKND-Site, aufgeschlüsselt nach der zugehörigen Seitenvorlage und weiter aufgeschlüsselt nach Benutzerinteraktion mit den Schaltflächen &quot;Klick zur Aktion&quot;(CTA) der WKND-Site.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Sie können den Wert der Schaltfläche-ID mit einem benutzerfreundlicheren Namen mithilfe der Adobe Analytics Classifications ersetzen. Weitere Informationen zum Erstellen einer Classification für eine bestimmte Metrik finden Sie unter [here](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html). In diesem Fall haben wir eine Classification-Metrik `Button Section (Button ID)` für `eVar8` , der die Schaltflächen-ID einem benutzerfreundlichen Namen zuordnet.

   ![Schaltflächenabschnitt](assets/create-analytics-workspace/button-section.png)

## Hinzufügen von Classifications zu einer Analytics-Variablen

### Konversionsklassifizierungen

Analytics Classification ist eine Möglichkeit, Analytics-Variablendaten zu kategorisieren und diese Daten bei der Berichterstellung auf unterschiedliche Weise anzuzeigen. Um die Anzeige der Schaltflächen-ID im Analytics Workspace-Bericht zu verbessern, erstellen wir eine Classification-Variable für die Schaltflächen-ID (eVar 8). Beim Klassifizieren erstellen Sie eine Beziehung zwischen der Variablen und den Metadaten, die sich auf diese Variable beziehen.

Als Nächstes erstellen wir eine Classification für Analytics-Variable.

1. Aus dem **Admin** Symbolleistenmenü auswählen **Report Suites**
1. Wählen Sie die **Report Suite-ID** von **Report Suite Manager** Fenster und klicken Sie auf **Einstellungen bearbeiten** > **Konversion** > **Konversionsklassifizierungen**

   ![Konversionsklassifizierung](assets/create-analytics-workspace/conversion-classification.png)

1. Aus dem **Einen Klassifizierungstyp auswählen** in der Dropdownliste die Variable (eVar8-Schaltfläche-ID) auswählen, um eine Classification hinzuzufügen.
1. Klicken Sie auf den Pfeil rechts neben der Variablen Klassifizierung , die im Abschnitt Klassifizierungen aufgeführt ist, um eine neue Klassifizierung hinzuzufügen.

   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/select-classification-variable.png)

1. Im **Bearbeiten einer Klassifizierung** Geben Sie einen geeigneten Namen für die Textklassifizierung ein. Eine Dimensionskomponente mit dem Namen Textklassifizierung wird erstellt.

   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/new-classification.png)

1. **Speichern Sie Ihre Änderungen.**

### Classification Importer

Verwenden Sie das Importtool, um Classifications in Adobe Analytics hochzuladen. Sie können die Daten auch exportieren, um sie vor einem Import zu aktualisieren. Die mit dem Import-Tool importierten Daten müssen in einem bestimmten Format vorliegen. Adobe bietet Ihnen die Möglichkeit, eine Datenvorlage mit allen korrekten Kopfzeilendetails in einer tabulatorgetrennten Datendatei herunterzuladen. Sie können Ihre neuen Daten zu dieser Vorlage hinzufügen und dann die Datendatei per FTP in den Browser importieren.

#### Klassifizierungsvorlage

Bevor Sie Klassifizierungen in Marketingberichte importieren, können Sie eine Vorlage herunterladen, mit der Sie eine Klassifizierungsdatendatei erstellen können. Die Datendatei verwendet Ihre gewünschten Klassifizierungen als Spaltenüberschriften und organisiert dann den Berichtdatensatz unter den entsprechenden Klassifizierungsüberschriften.

Als Nächstes laden wir die Classification-Vorlage für die Variable Button-ID (eVar 8) herunter

1. Navigieren Sie zu **Admin** > **Classification Importer**
1. Laden wir eine Classification-Vorlage für die Konversionsvariable aus der **Vorlage herunterladen** Registerkarte.
   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/classification-importer.png)

1. Geben Sie auf der Registerkarte Vorlage herunterladen die Konfiguration der Datenvorlage an.
   * **Report Suite auswählen** : Wählen Sie die Report Suite aus, die in der Vorlage verwendet werden soll. Die Report Suite und der Datensatz müssen übereinstimmen.
   * **Zu klassifizierender Datensatz** : Wählen Sie den Datentyp für die Datendatei aus. Das Menü enthält alle Berichte in Ihren Report Suites, die für Klassifizierungen konfiguriert sind.
   * **Kodierung** : Wählen Sie die Zeichenkodierung für die Datendatei aus. Das Standard-Kodierungsformat ist UTF-8.

1. Klicken **Download** und speichern Sie die Vorlagendatei auf Ihrem lokalen System. Die Vorlagendatei ist eine tabulatorgetrennte Datendatei (Dateierweiterung .tab ), die von den meisten Tabellenkalkulationsprogrammen unterstützt wird.
1. Öffnen Sie die tabulatorgetrennte Datendatei mit einem Editor Ihrer Wahl.
1. Fügen Sie die Schaltflächen-ID (eVar9) und einen entsprechenden Schaltflächennamen zur tabulatorgetrennten Datei für jeden eVar9-Wert aus Schritt 9 im Abschnitt hinzu.

   ![Schlüsselwert](assets/create-analytics-workspace/key-value.png)

1. **Speichern** die tabulatorgetrennte Datei.
1. Navigieren Sie zum **Importdatei** Registerkarte.
1. Konfigurieren Sie das Ziel für den Dateiimport.
   * **Report Suite auswählen** : WKND Site AEM (Report Suite)
   * **Zu klassifizierender Datensatz** : Schaltflächen-ID (Konversionsvariable eVar 8)
1. Klicken Sie auf **Datei auswählen** Option zum Hochladen der tabulatorgetrennten Datei von Ihrem System aus und klicken Sie auf **Importdatei**

   ![Datei-Importtool](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Bei einem erfolgreichen Import werden die entsprechenden Änderungen sofort in einem Export angezeigt. Datenänderungen in Berichten können jedoch bis zu vier Stunden bei Verwendung eines Browser-Imports und bis zu 24 Stunden bei Verwendung eines FTP-Imports dauern.

#### Konversionsvariable durch Klassifizierungsvariable ersetzen

1. Wählen Sie in der Analytics-Symbolleiste die Option **Arbeitsbereich** und öffnen Sie den Arbeitsbereich, der im [Erstellen eines Projekts in Analysis Workspace](#create-a-project-in-analysis-workspace) Abschnitt dieses Tutorials.

   ![Workspace-Schaltflächen-ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Ersetzen Sie als Nächstes das **Schaltflächen-ID** Metrik in Ihrem Arbeitsbereich, die die ID der Schaltfläche Aktionsaufruf (CTA) mit dem Classification-Namen anzeigt, der im vorherigen Schritt erstellt wurde.

1. Suchen Sie in der Komponentensuche nach **WKND-CTA-Schaltflächen** und ziehen Sie die **WKND-CTA-Schaltflächen (Schaltflächen-ID)** Dimension auf die Schaltfläche-ID-Metrik ein und ersetzen Sie sie.

   * **Vorher**
     ![Workspace-Schaltfläche vor](assets/create-analytics-workspace/wknd-button-before.png)
   * **Nachher**
     ![Workspace-Schaltfläche nach](assets/create-analytics-workspace/wknd-button-after.png)

1. Die Metrik &quot;Schaltflächen-ID&quot;, die die Schaltflächen-ID einer Aktionsaufruf-Schaltfläche (CTA) enthielt, wurde jetzt durch einen entsprechenden Namen ersetzt, der in der Klassifizierungsvorlage angegeben ist.
1. Vergleichen wir nun die Analytics Workspace-Tabelle mit der WKND-Startseite und betrachten die Anzahl der CTA-Schaltflächen-Klicks und deren Analyse. Basierend auf den Daten der Freiformtabelle im Arbeitsbereich ist klar, dass 22 Mal Benutzer auf die **SKI JETZT** Schaltfläche und viermal für den WKND Home Page Camping in Westaustralien **Mehr dazu** Schaltfläche.

   ![CTA-Bericht](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Achten Sie darauf, Ihr Adobe Analytics Workspace-Projekt zu speichern und einen geeigneten Namen und eine Beschreibung anzugeben. Optional können Sie Tags zu einem Workspace-Projekt hinzufügen.

   ![Projekt speichern](assets/create-analytics-workspace/save-project.png)

1. Nach dem erfolgreichen Speichern Ihres Projekts können Sie Ihr Workspace-Projekt mit anderen Kollegen oder Teammitgliedern teilen, indem Sie die Option Freigeben verwenden.

   ![Projekt freigeben](assets/create-analytics-workspace/share.png)

## Herzlichen Glückwunsch!

Sie haben soeben erfahren, wie Sie von einer Adobe Experience Manager-Site erfasste Daten Metriken und Dimensionen in Adobe Analytics Report Suites zuordnen. Führen Sie außerdem eine Klassifizierung für die Metriken durch und erstellen Sie ein detailliertes Berichts-Dashboard mit der Analysis Workspace-Funktion von Adobe Analytics.
