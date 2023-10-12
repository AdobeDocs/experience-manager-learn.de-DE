---
title: Integrieren von AEM Sites mit Adobe Analytics mit der Adobe Analytics-Tag-Erweiterung
description: Integrieren Sie AEM Sites mit Adobe Analytics mithilfe der ereignisgesteuerten Adobe Client-Datenschicht, um Daten zur Benutzeraktivität auf einer mit Adobe Experience Manager erstellten Website zu erfassen. Erfahren Sie, wie Sie mithilfe von Tag-Regeln auf diese Ereignisse warten und Daten an eine Adobe Analytics Report Suite senden können.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Integration" type="positive"
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 6%

---

# Integrieren von AEM Sites und Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch wurde als eine Suite von Datenerfassungstechnologien in Adobe Experience Platform umbenannt. Infolgedessen wurden in der gesamten Produktdokumentation mehrere terminologische Änderungen eingeführt. Siehe Folgendes [Dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) für eine konsolidierte Übersicht über die terminologischen Änderungen.


Erfahren Sie, wie Sie AEM Sites und Adobe Analytics mit der Adobe Analytics-Tag-Erweiterung integrieren, indem Sie die integrierten Funktionen der [Adobe Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de) , um Daten zu einer Seite in Adobe Experience Manager Sites zu erfassen. [Tags im Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de) und [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=de) werden zum Erstellen von Regeln verwendet, um Seitendaten an Adobe Analytics zu senden.

## Was Sie erstellen werden {#what-build}

![Seitendatenverfolgung](assets/collect-data-analytics/analytics-page-data-tracking.png)

In diesem Tutorial erstellen Sie eine Tag-Regel, die auf einem Ereignis aus der Adobe Client-Datenschicht basiert. Fügen Sie außerdem Bedingungen hinzu, wann die Regel ausgelöst werden soll, und senden Sie dann die **Seitenname** und **Seitenvorlage** Werte einer AEM Seite in Adobe Analytics.

### Ziele {#objective}

1. Erstellen Sie eine ereignisgesteuerte Regel in der Tag-Eigenschaft, die Änderungen aus der Datenschicht erfasst
1. Ordnen Sie die Seitendatenschichteigenschaften Datenelementen in der Tag-Eigenschaft zu.
1. Erfassen und Senden von Seitendaten an Adobe Analytics mithilfe des Seitenansichts-Beacons

## Voraussetzungen

Folgendes ist erforderlich:

* **Tag-Eigenschaft** in Experience Platform
* **Adobe Analytics** Report Suite-ID und Tracking-Server testen/entwickeln. Die folgende Dokumentation finden Sie für [Erstellen einer Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) Browsererweiterung. Screenshots in diesem Tutorial, die aus dem Chrome-Browser erfasst wurden.
* (Optional) AEM Site mit der [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de#installation-activation). In diesem Tutorial wird die öffentliche Meinung verwendet [WKND](https://wknd.site/us/en.html) Website, aber Sie können gerne Ihre eigene Website verwenden.

>[!NOTE]
>
> Benötigen Sie Hilfe bei der Integration von Tag-Eigenschaft und AEM Site? [Siehe diese Videoreihe](../experience-platform/data-collection/tags/overview.md).

## Tag-Umgebung für WKND-Site wechseln

Die [WKND](https://wknd.site/us/en.html) ist eine öffentlich zugängliche Website, die auf [ein Open-Source-Projekt](https://github.com/adobe/aem-guides-wknd) als Referenz konzipiert und [Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de) für eine AEM Implementierung.

Anstatt eine AEM Umgebung einzurichten und die WKND-Codebasis zu installieren, können Sie den Experience Platform-Debugger verwenden, um **switch** live [WKND-Site](https://wknd.site/us/en.html) nach *Ihre* Tag-Eigenschaft. Sie können jedoch Ihre eigene AEM-Site verwenden, wenn sie bereits über die [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de#installation-activation).

1. Melden Sie sich bei Experience Platform an und [Erstellen einer Tag-Eigenschaft](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (falls noch nicht geschehen).
1. Stellen Sie sicher, dass ein anfängliches Tag-JavaScript [Bibliothek wurde erstellt](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) und in das -Tag weitergeleitet werden [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=de).
1. Kopieren Sie den JavaScript-Einbettungscode aus der Tag-Umgebung, in der Ihre Bibliothek veröffentlicht wurde.

   ![Einbettungscode der Tag-Eigenschaft kopieren](assets/collect-data-analytics/launch-environment-copy.png)

1. Öffnen Sie in Ihrem Browser eine neue Registerkarte und navigieren Sie zu [WKND-Site](https://wknd.site/us/en.html)
1. Öffnen Sie die Browsererweiterung Experience Platform Debugger .

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigieren Sie zu **Experience Platform-Tags** > **Konfiguration** und **Eingebettete Einbettungscodes** Ersetzen Sie den vorhandenen Einbettungscode durch *Ihre* Einbettungscode, der aus Schritt 3 kopiert wurde.

   ![Eingebetteten Code ersetzen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Aktivieren **Konsolenprotokollierung** und **Sperren** den Debugger auf der Registerkarte WKND .

   ![Konsolenprotokollierung](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Überprüfen der Adobe Client-Datenschicht auf der WKND-Site

Die [WKND-Referenzprojekt](https://github.com/adobe/aem-guides-wknd) ist mit AEM Kernkomponenten erstellt und verfügt über die [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de#installation-activation) Standardmäßig. Überprüfen Sie als Nächstes, ob die Adobe Client-Datenschicht aktiviert ist.

1. Navigieren Sie zu [WKND-Site](https://wknd.site/us/en.html).
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zum **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Der obige Code gibt den aktuellen Status der Adobe Client-Datenschicht zurück.

   ![Status der Adobe-Datenschicht](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Erweitern Sie die Antwort und überprüfen Sie die `page` eingeben. Es sollte ein Datenschema wie das folgende angezeigt werden:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Verwenden Sie zum Senden von Seitendaten an Adobe Analytics die Standardeigenschaften wie `dc:title`, `xdm:language`, und `xdm:template` der Datenschicht.

   Weitere Informationen finden Sie unter [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) aus den Datenschemata der Kernkomponenten.

   >[!NOTE]
   >
   > Wenn Sie die `adobeDataLayer` JavaScript-Objekt? Stellen Sie sicher, dass [Adobe Die Client-Datenschicht wurde aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de#installation-activation) auf Ihrer Site.

## Erstellen einer Regel &quot;Seite geladen&quot;

Die Adobe Client-Datenschicht ist eine **ereignisgesteuert** Datenschicht. Beim Laden der AEM Seite wird eine `cmp:show` -Ereignis. Erstellen Sie eine Regel, die ausgelöst wird, wenn die `cmp:show` -Ereignis wird aus der Seitendatenschicht ausgelöst.

1. Navigieren Sie zu Experience Platform und in die Tag-Eigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Regeln** in der Benutzeroberfläche &quot;Tag-Eigenschaft&quot;auf und klicken Sie auf **Neue Regel erstellen**.

   ![Regel erstellen](assets/collect-data-analytics/analytics-create-rule.png)

1. Benennen Sie die Regel. **Seite geladen**.
1. Im **Veranstaltungen** Unterabschnitt klicken **Hinzufügen** , um die **Ereigniskonfiguration** Assistent.
1. Für **Ereignistyp** Feld auswählen **Benutzerspezifischer Code**.

   ![Benennen Sie die Regel und fügen Sie das Ereignis für benutzerdefinierten Code hinzu.](assets/collect-data-analytics/custom-code-event.png)

1. Klicks **Editor öffnen** Geben Sie im Hauptbereich folgendes Code-Snippet ein:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   Das obige Codefragment fügt einen Ereignis-Listener hinzu von [Pushen einer Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht ein. Wann `cmp:show` -Ereignis ausgelöst wird, wird die `pageShownEventHandler` -Funktion aufgerufen. In dieser Funktion werden einige Integritätsprüfungen hinzugefügt und eine neue `event` wird mit der neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente, die das Ereignis ausgelöst hat.

   Schließlich `trigger(event)` -Funktion aufgerufen. Die `trigger()` -Funktion ist ein reservierter Name in der Tag-Eigenschaft und **Trigger** die Regel. Die `event` -Objekt wird als Parameter übergeben, der wiederum durch einen anderen reservierten Namen in der Tag-Eigenschaft verfügbar gemacht wird. Datenelemente in der Tag-Eigenschaft können jetzt mithilfe eines Code-Snippets wie `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Nächste unter **Aktionen** click **Hinzufügen** , um die **Aktionskonfiguration** Assistent.
1. Für **Aktionstyp** Feld, wählen Sie **Benutzerspezifischer Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/collect-data-analytics/action-custom-code.png)

1. Klicks **Editor öffnen** Geben Sie im Hauptbereich folgendes Code-Snippet ein:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   Die `event` -Objekt wird von der `trigger()` -Methode, die im benutzerspezifischen Ereignis aufgerufen wird. Hier wird die `component` ist die aktuelle von der Datenschicht abgeleitete Seite `getState` im benutzerspezifischen Ereignis.

1. Speichern Sie die Änderungen und führen Sie eine [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in der Tag-Eigenschaft, um den Code für die [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=de) verwendet auf Ihrer AEM Site.

   >[!NOTE]
   >
   > Es kann nützlich sein, die [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) , um den Einbettungscode in einen **Entwicklung** Umgebung.

1. Navigieren Sie zu Ihrer AEM-Site und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Aktualisieren Sie die Seite. Sie sollten sehen, dass die Konsolenmeldungen protokolliert wurden:

![Seitengeladene Konsolenmeldungen](assets/collect-data-analytics/page-show-event-console.png)

## Datenelemente erstellen

Erstellen Sie anschließend mehrere Datenelemente, um andere Werte als die Adobe Client-Datenschicht zu erfassen. Wie in der vorherigen Übung gezeigt, ist es möglich, direkt über benutzerdefinierten Code auf die Eigenschaften der Datenschicht zuzugreifen. Der Vorteil der Verwendung von Datenelementen besteht darin, dass sie über Tag-Regeln hinweg wiederverwendet werden können.

Datenelemente werden dem `@type`, `dc:title`, und `xdm:template` Eigenschaften.

### Komponenten-Ressourcentyp

1. Navigieren Sie zu Experience Platform und in die Tag-Eigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement erstellen**.
1. Für **Name** und geben Sie die **Komponenten-Ressourcentyp**.
1. Für **Datenelementtyp** Feld auswählen **Benutzerspezifischer Code**.

   ![Komponenten-Ressourcentyp](assets/collect-data-analytics/component-resource-type-form.png)

1. Klicks **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Erinnern Sie sich daran, dass die `event` -Objekt wird basierend auf dem Ereignis, das die **Regel** in der Tag-Eigenschaft. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement *referenziert* innerhalb einer Regel. Daher ist es sicher, dieses Datenelement innerhalb einer Regel wie der **Seite geladen** im vorherigen Schritt erstellte Regel *but* in anderen Kontexten nicht sicher verwendet werden.

### Seitenname

1. Klicks **Datenelement hinzufügen** button
1. Für **Name** Feld, eingeben **Seitenname**.
1. Für **Datenelementtyp** Feld auswählen **Benutzerspezifischer Code**.
1. Klicks **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Speichern Sie die Änderungen.

### Seitenvorlage

1. Klicken Sie auf **Datenelement hinzufügen** button
1. Für **Name** Feld, eingeben **Seitenvorlage**.
1. Für **Datenelementtyp** Feld auswählen **Benutzerspezifischer Code**.
1. Klicks **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Speichern Sie die Änderungen.

1. Sie sollten jetzt drei Datenelemente als Teil Ihrer Regel haben:

   ![Datenelemente in der Regel](assets/collect-data-analytics/data-elements-page-rule.png)

## Hinzufügen der Analytics-Erweiterung

Fügen Sie anschließend die Analytics-Erweiterung zu Ihrer Tag-Eigenschaft hinzu, um Daten in eine Report Suite zu senden.

1. Navigieren Sie zu Experience Platform und in die Tag-Eigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zu **Erweiterungen** > **Katalog**
1. Suchen Sie die **Adobe Analytics** Erweiterung und Klicken **Installieren**

   ![Adobe Analytics-Erweiterung](assets/collect-data-analytics/analytics-catalog-install.png)

1. under **Bibliotheksverwaltung** > **Report Suites** Geben Sie die Report Suite-IDs ein, die Sie für jede Tag-Umgebung verwenden möchten.

   ![Report Suite-IDs eingeben](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In diesem Tutorial können Sie eine Report Suite für alle Umgebungen verwenden. In der Praxis sollten Sie jedoch separate Report Suites verwenden, wie in der Abbildung unten dargestellt

   >[!TIP]
   >
   >Es wird empfohlen, *Bibliothek für mich verwalten* als Einstellung für die Bibliotheksverwaltung, da dies die `AppMeasurement.js` -Bibliothek auf dem neuesten Stand.

1. Aktivieren Sie das Kontrollkästchen, um **Activity Map verwenden**.

   ![Verwenden von Activity Map aktivieren](assets/track-clicked-component/analytic-track-click.png)

1. under **Allgemein** > **Tracking-Server**, geben Sie Ihren Tracking-Server ein, z. B. `tmd.sc.omtrdc.net`. Geben Sie Ihren SSL-Tracking-Server ein, wenn Ihre Site `https://`

   ![Trackingserver eingeben](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## Hinzufügen einer Bedingung zur Regel &quot;Seite geladen&quot;

Aktualisieren Sie anschließend die **Seite geladen** -Regel verwenden, um **Komponenten-Ressourcentyp** -Datenelement, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn die `cmp:show` -Ereignis für **Seite**. Andere Komponenten können die `cmp:show` -Ereignis, beispielsweise löst die Karussellkomponente diese bei einer Änderung der Folien aus. Daher ist es wichtig, eine Bedingung für diese Regel hinzuzufügen.

1. Navigieren Sie in der Benutzeroberfläche der Tag-Eigenschaft zum **Seite geladen** Regel, die zuvor erstellt wurde.
1. under **Bedingungen** click **Hinzufügen** , um die **Bedingungskonfiguration** Assistent.
1. Für **Bedingungstyp** Feld auswählen **Wertvergleich** -Option.
1. Setzen Sie den ersten Wert im Formularfeld auf `%Component Resource Type%`. Sie können das Datenelement-Symbol verwenden ![Datenelementsymbol](assets/collect-data-analytics/cylinder-icon.png) zur Auswahl der **Komponenten-Ressourcentyp** Datenelement. Belassen Sie den Vergleichssatz auf `Equals`.
1. Setzen Sie den zweiten Wert auf `wknd/components/page`.

   ![Bedingungskonfiguration für Seitenladeregel](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Diese Bedingung kann innerhalb der benutzerdefinierten Code-Funktion hinzugefügt werden, die auf die `cmp:show` -Ereignis, das zuvor im Tutorial erstellt wurde. Das Hinzufügen der Regel innerhalb der Benutzeroberfläche bietet zusätzlichen Benutzern mehr Sichtbarkeit, die möglicherweise Änderungen an der Regel vornehmen müssen. Außerdem können wir unser Datenelement verwenden!

1. Speichern Sie die Änderungen.

## Analytics-Variablen und Trigger-Seitenansichts-Beacon festlegen

Derzeit ist das **Seite geladen** -Regel gibt einfach eine Konsolenanweisung aus. Verwenden Sie anschließend die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **action** im **Seite geladen** Regel. Wir haben auch eine zusätzliche Aktion zum Trigger der **Seitenansichts-Beacon** und senden Sie die erfassten Daten an Adobe Analytics.

1. In der Regel &quot;Seite geladen&quot; **remove** die **Core - benutzerspezifischer Code** action (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/collect-data-analytics/remove-console-statements.png)

1. Klicken Sie unter dem Unterabschnitt Aktionen auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.

1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Variablen festlegen**

   ![Aktionserweiterung auf Analytics-Variablen festlegen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Wählen Sie im Hauptbereich eine verfügbare **eVar** und als Wert des Datenelements festlegen **Seitenvorlage**. Verwenden des Symbols Datenelemente ![Symbol &quot;Datenelemente&quot;](assets/collect-data-analytics/cylinder-icon.png) zur Auswahl der **Seitenvorlage** -Element.

   ![Als eVar-Seitenvorlage festlegen](assets/collect-data-analytics/set-evar-page-template.png)

1. Hinunter scrollen **Zusätzliche Einstellungen** set **Seitenname** zum Datenelement **Seitenname**:

   ![Umgebungsvariable &quot;Seitenname&quot;](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Speichern Sie die Änderungen.

1. Als Nächstes fügen Sie rechts neben dem **Adobe Analytics - Variablen festlegen** durch Tippen auf die **plus** -Symbol:

   ![Hinzufügen einer zusätzlichen Tag-Regel-Aktion](assets/collect-data-analytics/add-additional-launch-action.png)

1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Beacon senden**. Da diese Aktion als Seitenansicht gilt, lassen Sie die standardmäßige Tracking-Einstellung auf **`s.t()`**.

   ![Aktion &quot;Beacon Adobe Analytics senden&quot;](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Speichern Sie die Änderungen. Die **Seite geladen** -Regel sollte jetzt die folgende Konfiguration aufweisen:

   ![Konfiguration der endgültigen Tag-Regel](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Suchen Sie nach `cmp:show` -Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis von einer Seite ausgelöst wurde.
   * **3.** Analytics-Variablen für **Seitenname** und **Seitenvorlage**
   * **4.** Senden des Analytics-Beacons für die Seitenansicht

1. Speichern Sie alle Änderungen und erstellen Sie Ihre Tag-Bibliothek, indem Sie sie an die entsprechende Umgebung weiterleiten.

## Überprüfen des Seitenansichts-Beacons- und Analytics-Aufrufs

Nun, dass **Seite geladen** sendet das Analytics-Beacon. Sie sollten die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/collect-data-analytics/experience-cloud-debugger.png) , um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Tag-Eigenschaft dem *Ihre* Entwicklungsumgebung, wie zuvor beschrieben und **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite. Der Seitenname sollte ebenfalls angegeben werden:

   ![Analytics-Tab-Debugger](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Hinunter scrollen und erweitern **Netzwerkanforderungen**. Sie sollten die **evar** für **Seitenvorlage**:

   ![eVar- und Seitenname-Satz](assets/collect-data-analytics/evar-page-name-set.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Klicken Sie durch **Karussell** oben auf der Seite.

   ![Karussellseite durchklicken](assets/collect-data-analytics/click-carousel-page.png)

1. Beobachten Sie in der Browser-Konsole die Konsolenanweisung:

   ![Bedingung nicht erfüllt](assets/collect-data-analytics/condition-not-met.png)

   Dies liegt daran, dass das Karussell einen Trigger `cmp:show` event *but* aufgrund unserer Kontrolle der **Komponenten-Ressourcentyp**, wird kein Ereignis ausgelöst.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** wird unter **Experience Platform-Tags** im Experience Platform Debugger.

1. Navigieren Sie zu einer Artikelseite wie [Westaustralien](https://wknd.site/us/en/magazine/western-australia.html). Beachten Sie, dass sich Seitenname und Vorlagentyp ändern.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisbasierte Adobe Client-Datenschicht und -Tags in Experience Platform verwendet, um Daten von einer AEM-Site zu erfassen und an Adobe Analytics zu senden.

### Nächste Schritte

Sehen Sie sich das folgende Tutorial an, um zu erfahren, wie Sie mit der ereignisgesteuerten Adobe Client-Datenschicht [Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site verfolgen](track-clicked-component.md).
