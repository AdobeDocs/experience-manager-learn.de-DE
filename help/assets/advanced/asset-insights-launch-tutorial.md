---
title: Einrichten von Asset Insights mit AEM Assets und Adobe Launch
description: In dieser 5-teiligen Videoserie führen wir durch die Einrichtung und Konfiguration von Asset Insights für Experience Manager, der über Launch by Adobe bereitgestellt wird.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# Einrichten von Asset Insights mit AEM Assets und Adobe Experience Platform Launch

In dieser 5-teiligen Videoreihe werden wir durch die Einrichtung und Konfiguration von Asset Insights für Experience Manager, der über Adobe Launch bereitgestellt wird, geleitet.

## Teil 1: Überblick über Asset Insights {#overview}

Asset Insights - Überblick. Installieren Sie Kernkomponenten, Beispielbildkomponente und andere Inhaltspakete, um Ihre Umgebung fertig zu machen.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Architekturdiagramm {#architecture-diagram}

![Architekturdiagramm](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Laden Sie die [aktuelle Version der Kernkomponenten](https://github.com/adobe/aem-core-wcm-components) für Ihre Implementierung.

Das Video verwendet Kernkomponenten v2.2.2, die nicht mehr die neueste Version sind. Stellen Sie sicher, dass Sie die neueste Version verwenden, bevor Sie mit dem nächsten Abschnitt fortfahren.

* Download [Asset Insights-Beispielbildinhalt](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Download [Die neuesten AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases)

## Teil 2: Aktivieren der Asset Insights-Verfolgung für die Beispielbildkomponente {#sample-image-component-asset-insights}

Verbesserungen an Kernkomponenten und Verwendung der Proxy-Komponente (Beispielbildkomponente) für Asset Insights. Bearbeiten der Vorlagenrichtlinien für die Inhaltsseite, um die Beispielbildkomponente für die Referenz-Site zu aktivieren.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>Die Bild-Core-Komponente bietet die Möglichkeit, die UUID-Verfolgung zu deaktivieren, indem das Tracking der UUID des Assets deaktiviert wird (eindeutiger Identifikationswert für einen innerhalb von JCR erstellten Knoten).

Kernbildkomponenten verwenden ***data-asset-id*** -Attribut innerhalb des übergeordneten &lt;div> eines Bild-Tags, um diese Funktion zu aktivieren/deaktivieren. Die Proxy-Komponente überschreibt die Kernkomponente mit den folgenden Änderungen.

* Entfernt die ***data-asset-id*** aus dem übergeordneten div eines  &lt;img> Elements innerhalb der image.html
* Hinzufügungen ***data-aem-asset-id*** direkt zum  &lt;img> Element innerhalb der image.html
* Hinzufügungen ***data-trackable=&#39;true&#39;*** Wert zum  &lt;img> Element in image.html
* ***data-aem-asset-id*** und ***data-trackable=&#39;true&#39;*** werden auf derselben Knotenebene beibehalten

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* und *data-trackable=&#39;true&#39;* sind die Schlüsselattribute, die für Asset-Impressionen vorhanden sein müssen. Für Asset Click Insights muss das übergeordnete Tag zusätzlich zu den oben genannten Datenattributen, die im - &lt;img> Tag vorhanden sind, über einen gültigen href-Wert verfügen.

## Teil 3: Adobe Analytics - Erstellen von Report Suites, Aktivieren der Echtzeit-Datenerfassung und AEM Assets Reporting {#adobe-analytics-asset-insights}

Für die Asset-Verfolgung wird eine Report Suite mit Echtzeit-Datenerfassung erstellt. Die AEM Assets Insights-Konfiguration wird mit Adobe Analytics-Anmeldeinformationen eingerichtet.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
Die Echtzeit-Datenerfassung und AEM Asset-Berichterstellung müssen für Ihre Adobe Analytics Report Suite aktiviert sein. Durch Aktivierung AEM Asset Reporting werden Analysevariablen zur Verfolgung von Asset-Einblicken reserviert.

Für die AEM Assets Insights-Konfiguration benötigen Sie die folgenden Anmeldedaten

* Rechenzentrum
* Analytics-Unternehmensname
* Analytics-Benutzername
* Gemeinsamer geheimer Schlüssel (kann abgerufen werden von *Adobe Analytics > Admin > Unternehmenseinstellungen > Webdienst*).
* Report Suite (Stellen Sie sicher, dass Sie die richtige Report Suite auswählen, die für die Asset-Berichterstellung verwendet wird)

## Teil 4: Verwenden von Adobe Experience Platform Launch zum Hinzufügen der Adobe Analytics-Erweiterung {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Hinzufügen der Adobe Analytics-Erweiterung, Erstellen von Seitenladeregeln und Integrieren von AEM in Launch mit dem technischen Adobe IMS-Konto.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Stellen Sie sicher, dass Sie alle Ihre Änderungen von der Autoreninstanz zur Veröffentlichungsinstanz replizieren.

### Regel 1: Seitenverfolgung (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Seitenverfolgung implementiert zwei Rückrufe (registriert in Asset-Einbettungscode)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : aufgerufen, wenn das Ereignis &quot;load&quot;für das Asset-DOM-Element gesendet wird.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : aufgerufen wird, wenn das &quot;click&quot;-Ereignis für das Asset-DOM-Element gesendet wird, ist dies nur relevant, wenn das Asset-DOM-Element über ein Anker-Tag verfügt, das mit einem gültigen, externen &quot;href&quot;-Attribut übergeordnet ist.

Schließlich implementiert Pagetracker eine Initialisierungsfunktion als .

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : aufgerufen, um die Seitentracker-Komponente zu initialisieren. Diese MUSS aufgerufen werden, bevor eines der Asset-Insights-Ereignisse (Impressionen und/oder Klicks) von der Webseite generiert wird.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : Akzeptiert optional ein AppMeasurement-Objekt - sofern angegeben, wird nicht versucht, eine neue Instanz des AppMeasurement-Objekts zu erstellen.

### Regel 2: Bildverfolgung - Aktion 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Regel 2: Bildverfolgung - Aktion 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : wird beim Abschluss des Seitenladevorgangs aufgerufen und ist Trigger Asset Impressions für alle nachverfolgten Bilder
* Analytics-Variable, die die geladene Asset-Liste enthält: **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() : wird aufgerufen, wenn das Asset-DOM-Element über ein Anker-Tag mit einem gültigen href-Wert verfügt. Wenn auf ein Asset geklickt wird, wird ein Cookie mit der angeklickten Asset-ID als Wert erstellt.**(Cookie-Name: a.assets.clickedid)**
* Analytics-Variable, die die geladene Asset-Liste enthält: **contextData[&quot;c.a.assets.clickedid&quot;]**
* Ursprungsquelle : **contextData[&quot;c.a.assets.source&quot;]**

### Debug-Anweisungen für Konsolen {#console-debug-statements}

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

Im Video werden zwei Google Chrome-Browsererweiterungen als Möglichkeiten zum Debugging von Analytics referenziert. Ähnliche Erweiterungen sind auch für andere Browser verfügbar.

* [Chrome-Erweiterung Launch Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

Mit der folgenden Chrome-Erweiterung ist es auch möglich, DTM in den Debug-Modus zu wechseln: [Launch und DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Dies erleichtert die Überprüfung von Fehlern bei der DTM-Bereitstellung. Darüber hinaus können Sie DTM über beliebige Browser manuell in den Debug-Modus wechseln *Entwicklertools -> JS-Konsole* durch Hinzufügen des folgenden Snippets:

## Teil 5: Testen von Analytics-Tracking und Synchronisierung von Insight-Daten{#analytics-tracking-asset-insights}

Konfigurieren AEM Asset Reporting Sync Job Scheduler und Assets Insights Report

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
