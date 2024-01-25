---
title: Einrichten von Asset Insights mit AEM Assets und Adobe Launch
description: In dieser fünfteiligen Videoreihe führen wir Sie durch die Einrichtung und Konfiguration von Asset Insights für mit Launch By Adobe durchgeführte Experience Manager-Bereitstellungen.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Assets as a Cloud Service, AEM Assets 6.5" before-title="false"
doc-type: Tutorial
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
duration: 2056
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 100%

---

# Einrichten von Asset Insights mit AEM Assets und Adobe Experience Platform Launch

In dieser fünfteiligen Videoreihe führen wir Sie durch die Einrichtung und Konfiguration von Asset Insights für mit Adobe Launch durchgeführte Experience Manager-Bereitstellungen.

## Teil 1: Überblick über Asset Insights {#overview}

Überblick über Asset Insights Installieren Sie die Kernkomponenten, die Beispielbildkomponente und andere Inhaltspakete, um Ihre Umgebung einsatzbereit zu machen.

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### Architekturdiagramm {#architecture-diagram}

![Architekturdiagramm](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Laden Sie die [aktuelle Version der Kernkomponenten](https://github.com/adobe/aem-core-wcm-components) für Ihre Implementierung herunter.

In dem Video werden die Kernkomponenten v2.2.2 verwendet, die nicht mehr der neuesten Version entsprechen. Stellen Sie sicher, dass Sie die neueste Version nutzen, bevor Sie zum nächsten Abschnitt gehen.

* Download der [Asset Insights-Beispielbildinhalte](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Download [der neuesten AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases)

## Teil 2: Aktivieren von Asset Insights-Tracking für die Beispielbildkomponente {#sample-image-component-asset-insights}

Verbesserungen an Kernkomponenten und Verwendung der Proxy-Komponente (Beispielbildkomponente) für Asset Insights. Bearbeiten der Vorlagenrichtlinien für die Inhaltsseite, um die Beispielbildkomponente für die Referenz-Site zu aktivieren.

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>Die bildbezogene Kernkomponente bietet die Möglichkeit, das UUID-Verfolgung zu deaktivieren, indem das Tracking der Asset-UUID (eindeutiger Kennungswert für einen innerhalb von JCR erstellten Knoten) deaktiviert wird.

Die bildbezogene Kernkomponente verwendet das Attribut ***data-asset-id*** innerhalb des übergeordneten &lt;div>-Elements eines Bild-Tags, um diese Funktion zu aktivieren/deaktivieren. Die Proxy-Komponente überschreibt die Kernkomponente mit den folgenden Änderungen:

* ***data-asset-id*** wird aus dem übergeordneten div eines &lt;img>-Elements innerhalb der Datei „image.html“ entfernt.
* ***data-aem-asset-id*** wird direkt zum &lt;img>-Element innerhalb der Datei „image.html“ hinzugefügt.
* Der ***data-trackable=&#39;true&#39;***-Wert wird zum &lt;img>-Element innerhalb der Datei „image.html“ hinzugefügt.
* ***data-aem-asset-id*** und ***data-trackable=&#39;true&#39;*** werden auf derselben Knotenebene beibehalten.

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* und *data-trackable=&#39;true&#39;* sind die Schlüsselattribute, die für Asset-Impressions vorhanden sein müssen. Für Asset Click Insights muss das übergeordnete &lt;a>-Tag zusätzlich zu den oben genannten Datenattributen, die im &lt;img>-Tag vorhanden sind, über einen gültigen href-Wert verfügen.

## Teil 3: Adobe Analytics – Erstellen einer Report Suite, Aktivieren der Echtzeit-Datenerfassung und von AEM Assets-Reporting {#adobe-analytics-asset-insights}

Zum Asset-Tracking wird eine Report Suite mit Echtzeit-Datenerfassung erstellt. Die AEM Assets Insights-Konfiguration wird mit Adobe Analytics-Anmeldeinformationen eingerichtet.

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
>Echtzeit-Datenerfassung und AEM Asset-Reporting müssen für Ihre Adobe Analytics Report Suite aktiviert sein. Durch Aktivierung der AEM Asset Reporting-Funktion werden Analytics-Variablen zum Tracking von Asset-Erkenntnissen reserviert.

Für die AEM Assets Insights-Konfiguration sind die folgenden Anmeldeinformationen erforderlich:

* Rechenzentrum
* Analytics-Unternehmensname
* Analytics-Benutzername
* Gemeinsamer geheimer Schlüssel (abrufbar über *Adobe Analytics > Admin > Unternehmenseinstellungen > Webservice*).
* Report Suite (die richtige, für das Asset-Reporting verwendete Report Suite auswählen!)

## Teil 4: Verwenden von Adobe Experience Platform Launch zum Hinzufügen der Adobe Analytics-Erweiterung {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Hinzufügen der Adobe Analytics-Erweiterung, Erstellen von Seitenladeregeln und Integrieren von AEM mit Adobe Experience Platform Launch in das technischen Adobe IMS-Konto.

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
>Stellen Sie sicher, dass Sie alle Ihre Änderungen von der Autoreninstanz in der Veröffentlichungsinstanz replizieren.

### Regel 1: Seitenverfolgung (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Mit der Seitenverfolgung werden zwei Rückrufe implementiert (registriert in asset-embed-code).

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>**: Wird aufgerufen, wenn das Ladeereignis für das asset-DOM-Element gesendet wird.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>**: Wird aufgerufen, wenn das Klickereignis für das asset-DOM-Element gesendet wird. Dies ist nur relevant, wenn das asset-DOM-Element über ein Anker-Tag als übergeordnetes Element mit einem gültigen, externen href-Attribut verfügt.

Schließlich wird mit der Seitenverfolgung eine Initialisierungsfunktion implementiert.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>**: Wird aufgerufen, um die Seitenverfolgungskomponente zu initialisieren. Der Aufruf muss erfolgen, bevor eines der asset-insights-Ereignisse (Impressions und/oder Klicks) von der Web-Seite generiert wird.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>**: Akzeptiert optional ein AppMeasurement-Objekt. Sofern angegeben, wird nicht versucht, eine neue Instanz des AppMeasurement-Objekts zu erstellen.

### Regel 2: Bildverfolgung – Aktion 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### Regel 2: Bildverfolgung – Aktion 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded(): Wird beim Abschluss des Seitenladevorgangs aufgerufen und löst Asset-Impressions für alle nachverfolgbaren Bilder aus.
* Analytics-Variable, die die geladene Asset-Liste enthält: **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked(): Wird aufgerufen, wenn das asset-DOM-Element über ein Anker-Tag mit einem gültigen href-Wert verfügt. Wenn auf ein Asset geklickt wird, wird ein Cookie mit der angeklickten Asset-ID als Wert erstellt.**(Cookie-Name: a.assets.clickedid)**
* Analytics-Variable, die die geladene Asset-Liste enthält: **contextData[&#39;c.a.assets.clickedid&#39;]**
* Ursprungsquelle: **contextData[&#39;c.a.assets.source&#39;]**

### Konsolen-Debug-Anweisungen {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Im Video wird auf zwei Google Chrome-Browser-Erweiterungen als Möglichkeit zum Debugging von Analytics verwiesen. Ähnliche Erweiterungen sind auch für andere Browser verfügbar.

* [Chrome-Erweiterung „Launch and DTM Switch“](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=de)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

Mit der folgenden Chrome-Erweiterung ist es auch möglich, DTM in den Debug-Modus wechseln zu lassen: [Launch and DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=de). Dies vereinfacht die Überprüfung auf Fehler im Zusammenhang mit der DTM-Bereitstellung. Darüber hinaus können Sie DTM in einem beliebigen Browser über *Entwickler-Tools > JS-Konsole* manuell in den Debug-Modus wechseln lassen, indem Sie folgendes Snippet hinzufügen:

## Teil 5: Testen der Analytics-Verfolgung und Synchronisieren von Insights-Daten{#analytics-tracking-asset-insights}

Konfigurieren des AEM Asset Reporting-Synchronisationsauftragsplaners und des Assets Insights-Berichts

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
