---
title: Analysieren von Daten mit Analysis Workspace
description: Erfahren Sie, wie Sie die von einer Adobe Experience Manager-Site erfassten Daten den Metriken und Dimensionen in den Adobe Analytics Report Suites zuordnen können. Erfahren Sie, wie Sie mit der Analysis Workspace-Funktion von Adobe Analytics ein detailliertes Berichts-Dashboard erstellen.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Integration" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 596
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 100%

---

# Analysieren von Daten mit Analysis Workspace

Erfahren Sie, wie Sie die von einer Adobe Experience Manager-Site erfassten Daten den Metriken und Dimensionen in den Adobe Analytics Report Suites zuordnen können. Erfahren Sie, wie Sie mit der Analysis Workspace-Funktion von Adobe Analytics ein detailliertes Berichts-Dashboard erstellen.

## Was Sie erstellen werden {#what-build}

Das WKND-Marketing-Team möchte wissen, welche `Call to Action (CTA)`-Schaltflächen auf der Startseite am besten funktionieren. In diesem Tutorial erstellen Sie ein Projekt in **Analysis Workspace**, um die Leistung verschiedener CTA-Schaltflächen zu visualisieren und das Benutzerverhalten auf der Website zu verstehen. Die folgenden Informationen werden mit Adobe Analytics erfasst, wenn jemand auf der WKND-Startseite auf die Schaltfläche „Aktionsaufruf“ (CTA) klickt.

**Analytics-Variablen**

Nachfolgend finden Sie die Analytics-Variablen, die derzeit erfasst werden:

* `eVar5` – `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA-Klick in Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine Report Suite oder verwenden Sie eine vorhandene.
1. Konfigurieren Sie die [Konvertierungsvariablen (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=de) und [Erfolgsereignisse (Ereignisse)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html?lang=de) in der Report Suite.
1. Erstellen Sie ein [Analysis Workspace-Projekt](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=de), um Daten mithilfe von Tools zu analysieren, mit denen Sie schnell Einblicke erstellen, analysieren und freigeben können.
1. Geben Sie das Analysis Workspace-Projekt für andere Team-Mitglieder frei.

## Voraussetzungen

Dieses Tutorial ist eine Fortsetzung von [Nachverfolgen angeklickter Komponenten mit Adobe Analytics](./track-clicked-component.md) und setzt voraus, dass Sie über Folgendes verfügen:

* Eine **Tag-Eigenschaft** mit aktivierter [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=de)
* **Adobe Analytics** test/dev-Report Suite-ID und Tracking-Server. Weitere Informationen zum [Erstellen einer Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=de) finden Sie in der folgenden Dokumentation.
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=de)-Browser-Erweiterung, konfiguriert mit einer Tag-Eigenschaft, die auf der [WKND-Site](https://wknd.site/us/en.html) oder einer AEM-Site mit aktivierter Adobe Datenschicht geladen ist.

## Konversionsvariablen (eVars) und Erfolgsereignisse (Ereignisse)

Die Custom Insight-Konversionsvariable (oder eVar) wird auf den ausgewählten Web-Seiten Ihrer Site im Adobe-Code platziert. Ihr Hauptzweck besteht darin, Konversionserfolgsmetriken in benutzerspezifischen Marketing-Berichten zu segmentieren. Eine eVar kann besuchsbasiert sein und funktioniert ähnlich wie Cookies. Die Werte, die an eVar-Variablen übergeben werden, folgen den Benutzenden für einen vorgegebenen Zeitraum.

Wenn eine eVar auf den Wert einer Besucherin oder eines Besuchers gesetzt wird, speichert Adobe diesen Wert automatisch, bis er abläuft. Alle Erfolgsereignisse, die eine Besucherin oder ein Besucher aufruft, während die eVar aktiv ist, werden für die eVar gezählt.

eVars eignen sich am besten zur Messung von Ursache und Wirkung, z. B.:

* Welche internen Kampagnen haben den Umsatz beeinflusst?
* Welche Banneranzeigen führten letztendlich zu einer Registrierung?
* Die Häufigkeit, mit der eine interne Suche vor einer Bestellung verwendet wurde

Erfolgsereignisse sind Aktionen, die verfolgt werden können. Es liegt an Ihnen zu bestimmen, was ein Erfolgsereignis ist. Wenn eine Besucherin oder ein Besucher beispielsweise auf eine CTA-Schaltfläche klickt, kann das Klick-Ereignis als Erfolgsereignis betrachtet werden.

### Konfigurieren von eVars

1. Wählen Sie auf der Adobe Experience Cloud-Startseite Ihr Unternehmen aus und starten Sie Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Klicken Sie in der Analytics-Symbolleiste auf **Admin** > **Report Suites** und finden Sie Ihre Report Suite.

   ![Analytics Report Suite](assets/create-analytics-workspace/select-report-suite.png)

1. Wählen Sie die entsprechende Report Suite aus > **Einstellungen bearbeiten** > **Konversion** > **Konversionsvariablen**

   ![Analytics-Konversionsvariablen](assets/create-analytics-workspace/conversion-variables.png)

1. Verwenden Sie die Option **Neu hinzufügen** und erstellen Sie Konversionsvariablen, um das Schema wie unten dargestellt abzubilden:

   * `eVar5` – `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Neue eVars hinzufügen](assets/create-analytics-workspace/add-new-evars.png)

1. Geben Sie einen geeigneten Namen und eine Beschreibung für jede eVar ein und **speichern** Sie Ihre Änderungen. Im Analysis Workspace-Projekt werden die eVars mit dem entsprechenden Namen verwendet, sodass die Variablen durch einen benutzerfreundlichen Namen leicht gefunden werden können.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Konfigurieren von Erfolgsereignissen

Als Nächstes erstellen wir ein Ereignis, um den Klick auf die CTA-Schaltfläche zu verfolgen.

1. Wählen Sie im Fenster **Report Suite Manager** die **Report Suite-ID** und klicken Sie auf **Einstellungen bearbeiten**.
1. Klicken Sie auf **Konversion** > **Erfolgsereignisse**
1. Erstellen Sie mit der Option **Neu hinzufügen** ein benutzerdefiniertes Erfolgsereignis, um den Klick auf die CTA-Schaltfläche zu verfolgen, und **speichern** Sie dann Ihre Änderungen.
   * `Event`: `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Erstellen eines Projekts in Analysis Workspace {#workspace-project}

Analysis Workspace ist ein flexibles Browser-Tool, mit dem Sie schnell Analysen erstellen und Einblicke freigeben können. Mithilfe der Drag-and-Drop-Benutzeroberfläche können Sie Ihre Analyse gestalten, Visualisierungen hinzufügen, um Daten zum Leben zu erwecken, einen Datensatz kuratieren, Projekte für andere in Ihrer Organisation freigeben und planen.

Erstellen Sie als Nächstes ein [Projekt](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html?lang=de#analysis-workspace), um ein Dashboard zu erstellen, um die Leistung der CTA-Schaltflächen auf der gesamten Site zu analysieren.

1. Wählen Sie in der Analytics-Symbolleiste **Arbeitsbereich** und klicken Sie auf **Neues Projekt erstellen**.

   ![Arbeitsbereich](assets/create-analytics-workspace/create-workspace.png)

1. Wählen Sie als Ausgangspunkt ein **leeres Projekt** oder wählen Sie eine der vordefinierten Vorlagen aus, die entweder von Adobe zur Verfügung gestellt oder von Ihrem Unternehmen erstellt wurden. Je nach Analyse oder Anwendungsfall stehen verschiedene Vorlagen zur Verfügung. [Erfahren Sie mehr](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html?lang=de) über die verschiedenen verfügbaren Vorlagenoptionen.

   In Ihrem Workspace-Projekt können Sie über die linke Leiste auf Bedienfelder, Tabellen, Visualisierungen und Komponenten zugreifen. Sie bilden die Bausteine für Ihr Projekt.

   * **[Komponenten](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html?lang=de)**: Komponenten sind Dimensionen, Metriken, Segmente oder Datumsbereiche, die alle in einer Freiformtabelle kombiniert werden können, um Ihre Geschäftsfragen zu beantworten. Machen Sie sich mit jedem Komponententyp vertraut, bevor Sie sich mit Ihrer Analyse befassen. Sobald Sie die Terminologie der Komponenten kennen, können Sie mit dem Ziehen und Ablegen beginnen, um Ihre Analyse in einer Freiformtabelle zu erstellen.
   * **[Visualisierung](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html?lang=de)**: Visualisierungen wie Balken- oder Liniendiagramme werden dann über den Daten hinzugefügt, damit sie visuell zum Tragen kommen. Wählen Sie in der linken Leiste das mittlere Symbol „Visualisierungen“ aus, um die vollständige Liste der verfügbaren Visualisierungen anzuzeigen.
   * **[Bedienfelder](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html?lang=de)**: Ein Bedienfeld ist eine Sammlung von Tabellen und Visualisierungen. Sie können auf Bedienfelder über das Symbol oben links in Workspace zugreifen. Bedienfelder sind hilfreich, wenn Sie Ihre Projekte nach Zeiträumen, Report Suites oder Anwendungsfällen für Analysen organisieren möchten. Die folgenden Bedienfeldtypen sind in Analysis Workspace verfügbar:

   ![Vorlagenauswahl](assets/create-analytics-workspace/workspace-tools.png)

### Hinzufügen von Datenvisualisierungen mit Analysis Workspace

Erstellen Sie als Nächstes eine Tabelle, um eine visuelle Darstellung der Interaktion der Benutzenden mit den `Call to Action (CTA)`-Schaltflächen auf der Homepage der WKND-Site zu erstellen. Um eine solche Darstellung zu erstellen, verwenden wir die Daten, die im Abschnitt [Nachverfolgen angeklickter Komponenten mit Adobe Analytics](./track-clicked-component.md) gesammelt wurden. Im Folgenden finden Sie eine kurze Zusammenfassung der Daten, die für die Benutzerinteraktionen mit den Schaltflächen für den Aktionsaufruf auf der WKND-Website erfasst wurden.

* `eVar5` – `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Ziehen Sie die Dimensionskomponente **Seite** auf die Freiformtabelle. Sie sollten jetzt in der Lage sein, eine Visualisierung anzuzeigen, die den Seitennamen (eVar9) und die entsprechenden Seitenansichten (Vorkommen) anzeigt, die in der Tabelle angezeigt werden.

   ![Seitendimension](assets/create-analytics-workspace/evar9-dimension.png)

1. Ziehen Sie die Metrik **CTA-Klick** (Ereignis8) auf die Metrik „Vorkommen“ und ersetzen Sie sie dadurch. Sie können jetzt eine Visualisierung anzeigen, in der der Seitenname (eVar9) und die zugehörige Anzahl der CTA-Klick-Ereignisse auf einer Seite angezeigt werden.

   ![Seitenmetrik – CTA-Klick](assets/create-analytics-workspace/evar8-cta-click.png)

1. Teilen wir die Seite nach Vorlagentyp auf. Wählen Sie aus den Komponenten die Metrik „Seitenvorlage“ aus und ziehen Sie sie auf die Dimension „Seitenname“. Jetzt können Sie den Seitennamen, aufgeschlüsselt nach Vorlagentyp, anzeigen.

   * **Vorher**
     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Nachher**
     ![eVar5-Metriken](assets/create-analytics-workspace/evar5-metrics.png)

1. Um zu verstehen, wie Benutzende mit CTA-Schaltflächen interagieren, wenn sie sich auf den WKND Site-Seiten befinden, ist eine weitere Aufschlüsselung erforderlich, indem die Metrik „Schaltflächen-ID“ (eVar8) hinzugefügt wird.

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Unten sehen Sie eine visuelle Darstellung der WKND-Site, aufgeschlüsselt nach der zugehörigen Seitenvorlage und weiter aufgeschlüsselt nach Benutzerinteraktion mit den Schaltflächen „Aktionsaufruf“ (CTA) der WKND-Site.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Sie können den Wert der Schaltflächen-ID durch einen benutzerfreundlicheren Namen ersetzen, indem Sie die Adobe Analytics-Klassifizierungen verwenden. Weitere Informationen zum Erstellen einer Klassifizierung für eine bestimmte Metrik finden Sie [hier](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html?lang=de). In diesem Fall haben wir eine Klassifizierungsmetrik `Button Section (Button ID)` für `eVar8` eingerichtet, die der Schaltflächen-ID einen benutzerfreundlichen Namen zuordnet.

   ![Schaltflächenabschnitt](assets/create-analytics-workspace/button-section.png)

## Hinzufügen von Klassifizierungen zu einer Analytics-Variablen

### Konversionsklassifizierungen

Analytics-Klassifizierung ist eine Möglichkeit, Analytics-Variablendaten zu kategorisieren und diese Daten bei der Berichterstellung auf unterschiedliche Weise anzuzeigen. Um die Anzeige der Schaltflächen-ID im Analytics Workspace-Bericht zu verbessern, erstellen wir eine Klassifizierungs-Variable für die Schaltflächen-ID (eVar8). Bei der Klassifizierung stellen Sie eine Beziehung zwischen der Variable und den Metadaten zu dieser Variable her.

Als Nächstes erstellen wir eine Klassifizierung für eine Analytics-Variable.

1. Wählen Sie im **Admin**-Symbolleistenmenü **Report Suites**.
1. Wählen Sie die **Report Suite-ID** im Fenster **Report Suite Manager** aus und klicken Sie auf **Einstellungen bearbeiten** > **Konversion** > **Konversionsklassifizierung**

   ![Konversionsklassifizierung](assets/create-analytics-workspace/conversion-classification.png)

1. Wählen Sie aus der Dropdown-Liste **Klassifizierungstyp** die Variable (eVar8-Schaltflächen-ID), um eine Klassifizierung hinzuzufügen.
1. Klicken Sie auf den Pfeil rechts neben der Klassifizierungs-Variablen, die im Abschnitt „Klassifizierungen“ aufgeführt ist, um eine neue Klassifizierung hinzuzufügen.

   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/select-classification-variable.png)

1. Geben Sie im Dialogfeld **Klassifizierung bearbeiten** einen geeigneten Namen für die Textklassifizierung ein. Eine Dimensionskomponente mit dem Namen „Textklassifizierung“ wird erstellt.

   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/new-classification.png)

1. **Speichern** Sie Ihre Änderungen.

### Classification Importer

Verwenden Sie das Import-Tool, um Klassifizierungen in Adobe Analytics hochzuladen. Sie können die Daten auch exportieren, um sie vor einem Import zu aktualisieren. Die mit dem Import-Tool importierten Daten müssen ein bestimmtes Format aufweisen. Adobe bietet Ihnen die Möglichkeit, eine Datenvorlage mit allen korrekten Kopfzeilen-Details in einer tabulatorgetrennten Datendatei herunterzuladen. Sie können Ihre neuen Daten zu dieser Vorlage hinzufügen und dann die Datendatei per FTP in den Browser importieren.

#### Klassifizierungsvorlage

Bevor Sie Klassifizierungen in Marketing-Berichte importieren, können Sie eine Vorlage herunterladen, mit der Sie eine Klassifizierungsdatendatei erstellen können. Die Datendatei verwendet Ihre gewünschten Klassifizierungen als Spaltenüberschriften und organisiert dann den Berichtdatensatz unter den entsprechenden Klassifizierungsüberschriften.

Als Nächstes laden wir die Klassifizierungs-Vorlage für die Schaltflächen-ID-Variable (eVar8) herunter:

1. Navigieren Sie zu **Admin** > **Classification Importer**
1. Laden Sie eine Klassifizierungsvorlage für die Konversionsvariable von der Registerkarte **Vorlage herunterladen** herunter.
   ![Konversionsklassifizierungstyp](assets/create-analytics-workspace/classification-importer.png)

1. Geben Sie auf der Registerkarte zum Herunterladen der Vorlage die Konfiguration der Datenvorlage an.
   * **Report-Suite auswählen**: Wählen Sie die Report Suite aus, die in der Vorlage verwendet werden soll. Die Report Suite und der Datensatz müssen sich entsprechen.
   * **Datensatz, der klassifiziert werden soll**: Wählen Sie den Datentyp für die Datendatei aus. Das Menü enthält alle Berichte in Ihren Report Suites, die für Klassifizierungen konfiguriert sind.
   * **Codierung**: Wählen Sie die Zeichencodierung für die Datendatei aus. Das Standard-Codierungsformat ist UTF-8.

1. Klicken Sie auf **Herunterladen** und speichern Sie die Vorlagendatei auf Ihrem lokalen System. Die Vorlagendatei ist eine tabulatorgetrennte Datendatei (Dateinamenerweiterung .tab), die von den meisten Tabellenkalkulationsprogrammen unterstützt wird.
1. Öffnen Sie die tabulatorgetrennte Datendatei mit einem Editor Ihrer Wahl.
1. Fügen Sie für jeden eVar9-Wert aus Schritt 9 im Abschnitt die Schaltflächen-ID (eVar9) und einen entsprechenden Schaltflächennamen zur tabulatorgetrennten Datei hinzu.

   ![Schlüsselwert](assets/create-analytics-workspace/key-value.png)

1. **Speichern** Sie die tabulatorgetrennte Datei.
1. Navigieren Sie zur Registerkarte **Datei importieren**.
1. Konfigurieren Sie das Ziel für den Dateiimport.
   * **Report-Suite auswählen**: WKND Site AEM (Report Suite)
   * **Datensatz, der klassifiziert werden soll**: Schaltflächen-ID (Konversionsvariable eVar8)
1. Klicken Sie auf die Option **Datei auswählen**, um die tabulatorgetrennte Datei von Ihrem System hochzuladen, und klicken Sie dann auf **Datei importieren**.

   ![Datei-Import-Tool](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Bei einem erfolgreichen Import werden die entsprechenden Änderungen sofort in einem Export angezeigt. Datenänderungen in Berichten dauern jedoch bis zu vier Stunden, wenn ein Browser-Import verwendet wird, und bis zu 24 Stunden, wenn ein FTP-Import verwendet wird.

#### Ersetzen der Konversionsvariable durch eine Klassifizierungsvariable

1. Wählen Sie in der Analytics-Symbolleiste die Option **Arbeitsbereich** und öffnen Sie den im Abschnitt [Erstellen eines Projekts in Analysis Workspace](#create-a-project-in-analysis-workspace) dieses im Tutorial erstellten Arbeitsbereichs.

   ![Arbeitsbereich-Schaltflächen-ID](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Ersetzen Sie als Nächstes die **Schaltflächen-ID**-Metrik in Ihrem Arbeitsbereich, die die ID der Schaltfläche „Aktionsaufruf“ (CTA) anzeigt, durch den Klassifizierungs-Namen, der im vorherigen Schritt erstellt wurde.

1. Suchen Sie in der Komponentensuche nach **WKND-CTA-Schaltflächen** und ziehen Sie die Dimension **WKND-CTA-Schaltflächen (Schaltflächen-ID)** per Drag-and-Drop auf die Schaltflächen-ID-Metrik, um sie zu ersetzen.

   * **Vorher**
     ![Arbeitsbereich-Schaltfläche vorher](assets/create-analytics-workspace/wknd-button-before.png)
   * **Nachher**
     ![Workspace-Schaltfläche nachher](assets/create-analytics-workspace/wknd-button-after.png)

1. Die Schaltflächen-ID-Metrik, die die Schaltflächen-ID einer Aktionsaufruf-Schaltfläche (CTA) enthielt, wurde jetzt durch einen entsprechenden Namen ersetzt, der in der Klassifizierungsvorlage angegeben ist.
1. Vergleichen wir nun die Analytics Workspace-Tabelle mit der WKND-Startseite und betrachten die Anzahl der CTA-Schaltflächen-Klicks und deren Analyse. Aus den Daten der Freiformtabelle für den Arbeitsbereich geht hervor, dass die Benutzenden 22 Mal auf die Schaltfläche **SKI NOW** und viermal auf die Schaltfläche **Read More** für die WKND-Homepage „Camping in Western Australia“ geklickt haben.

   ![CTA-Bericht](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Achten Sie darauf, Ihr Adobe Analytics Workspace-Projekt zu speichern und einen geeigneten Namen und eine Beschreibung anzugeben. Optional können Sie Tags zu einem Workspace-Projekt hinzufügen.

   ![Speichern des Projekts](assets/create-analytics-workspace/save-project.png)

1. Nach dem erfolgreichen Speichern Ihres Projekts können Sie Ihr Workspace-Projekt mit anderen Team-Mitgliedern teilen, indem Sie die Option „Freigeben“ verwenden.

   ![Freigeben eines Projekts](assets/create-analytics-workspace/share.png)

## Herzlichen Glückwunsch!

Sie haben soeben gelernt, wie man Daten, die von einer Adobe Experience Manager-Site erfasst wurden, auf Metriken und Dimensionen in den Report Suites von Adobe Analytics zuordnet. Außerdem haben Sie eine Klassifizierung für die Metriken durchgeführt und ein detailliertes Berichts-Dashboard mit der Analysis Workspace-Funktion von Adobe Analytics erstellt.
