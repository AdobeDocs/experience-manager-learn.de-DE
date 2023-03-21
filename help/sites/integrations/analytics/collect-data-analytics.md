---
title: Erfassen von Seitendaten mit Adobe Analytics
description: Verwenden Sie die ereignisbasierte Adobe Client Data-Ebene, um Daten zur Benutzeraktivität auf einer mit Adobe Experience Manager erstellten Website zu erfassen. Erfahren Sie, wie Sie in Experience Platform Launch mithilfe von Regeln auf diese Ereignisse warten und Daten an eine Adobe Analytics Report Suite senden können.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: ef1fe712921bd5516cb389862cacf226a71aa193
workflow-type: tm+mt
source-wordcount: '2371'
ht-degree: 4%

---

# Erfassen von Seitendaten mit Adobe Analytics

Erfahren Sie, wie Sie die integrierten Funktionen der [Adobe der Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de) , um Daten zu einer Seite in Adobe Experience Manager Sites zu erfassen. [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) und [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html?lang=de) werden zum Erstellen von Regeln verwendet, um Seitendaten an Adobe Analytics zu senden.

## Was Sie erstellen werden

![Seitendatenverfolgung](assets/collect-data-analytics/analytics-page-data-tracking.png)

In diesem Tutorial erstellen Sie eine Launch-Regel basierend auf einem Ereignis aus der Client-Datenschicht der Adobe, fügen Bedingungen für den Zeitpunkt hinzu, zu dem die Regel ausgelöst werden soll, und senden die **Seitenname** und **Seitenvorlage** einer AEM Seite in Adobe Analytics.

### Ziele {#objective}

1. Erstellen einer ereignisgesteuerten Regel in Launch basierend auf Änderungen an der Datenschicht
1. Seitendatenschichteigenschaften Datenelementen in Launch zuordnen
1. Erfassen von Seitendaten und Senden an Adobe Analytics mit dem Seitenansichts-Beacon

## Voraussetzungen

Folgendes ist erforderlich:

* **Experience Platform Launch** Eigenschaft
* **Adobe Analytics** Report Suite-ID und Tracking-Server testen/entwickeln. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer neuen Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Browsererweiterung. Screenshots in diesem Tutorial, die aus dem Chrome-Browser erfasst wurden.
* (Optional) AEM Site mit der [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). In diesem Tutorial wird die öffentliche Website verwendet [https://wknd.site/us/en.html](https://wknd.site/us/en.html) Sie können jedoch Ihre eigene Website nutzen.

>[!NOTE]
>
> Benötigen Sie Hilfe bei der Integration von Launch und Ihrer AEM Site? [Siehe diese Videoreihe](../experience-platform/data-collection/tags/overview.md).

## Wechseln von Launch-Umgebungen für WKND-Site

[https://wknd.site](https://wknd.site) ist eine öffentlich zugängliche Website, die auf [ein Open-Source-Projekt](https://github.com/adobe/aem-guides-wknd) als Referenz und [Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de) für AEM Implementierungen.

Anstatt eine AEM Umgebung einzurichten und die WKND-Codebasis zu installieren, können Sie den Experience Platform-Debugger verwenden, um **switch** live [https://wknd.site/](https://wknd.site/) nach *Ihre* Launch-Eigenschaft. Natürlich können Sie Ihre eigene AEM-Website verwenden, wenn sie bereits über [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Melden Sie sich beim Experience Platform Launch an und [Erstellen einer Launch-Eigenschaft](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (falls noch nicht geschehen).
1. Stellen Sie sicher, dass ein erster Launch [Bibliothek wurde erstellt](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) und zu einem Launch weitergeleitet werden [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=de).
1. Kopieren Sie den Launch-Einbettungscode aus der Umgebung, in der Ihre Bibliothek veröffentlicht wurde.

   ![Kopieren des eingebetteten Launch-Codes](assets/collect-data-analytics/launch-environment-copy.png)

1. Öffnen Sie in Ihrem Browser eine neue Registerkarte und navigieren Sie zu [https://wknd.site/](https://wknd.site/)
1. Öffnen Sie die Browsererweiterung Experience Platform Debugger .

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigieren Sie zu **Launch** > **Konfiguration** und **Eingebettete Einbettungscodes** Ersetzen Sie den vorhandenen Launch-Einbettungscode durch *Ihre* Einbettungscode, der aus Schritt 3 kopiert wurde.

   ![Eingebetteten Code ersetzen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Aktivieren **Konsolenprotokollierung** und **Sperren** den Debugger auf der Registerkarte WKND .

   ![Konsolenprotokollierung](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Überprüfen der Adobe Client-Datenschicht auf der WKND-Site

Die [WKND-Referenzprojekt](https://github.com/adobe/aem-guides-wknd) ist mit AEM Kernkomponenten erstellt und verfügt über die [Adobe Client-Datenschicht aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) Standardmäßig. Überprüfen Sie als Nächstes, ob die Adobe Client-Datenschicht aktiviert ist.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site).
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zum **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

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

   Wir verwenden Standardeigenschaften, die von der Variablen [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page),  `dc:title`, `xdm:language` und `xdm:template` der Datenschicht zum Senden von Seitendaten an Adobe Analytics.

   >[!NOTE]
   >
   > Sehen Sie nicht die `adobeDataLayer` JavaScript-Objekt? Stellen Sie sicher, dass [Adobe Client-Datenschicht wurde aktiviert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) auf Ihrer Site.

## Erstellen einer Regel &quot;Seite geladen&quot;

Die Adobe Client-Datenschicht ist eine **event** gesteuerte Datenschicht. Wenn die AEM **Seite** Datenschicht geladen wird, wird ein Ereignis Trigger `cmp:show`. Erstellen Sie eine Regel, die basierend auf der `cmp:show` -Ereignis.

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Regeln** in der Launch-Benutzeroberfläche und klicken Sie dann auf **Neue Regel erstellen**.

   ![Regel erstellen](assets/collect-data-analytics/analytics-create-rule.png)

1. Benennen Sie die Regel. **Seite geladen**.
1. Klicken **Veranstaltungen** **Hinzufügen** , um **Ereigniskonfiguration** Assistent.
1. under **Ereignistyp** select **Benutzerspezifischer Code**.

   ![Benennen Sie die Regel und fügen Sie das Ereignis für benutzerdefinierten Code hinzu.](assets/collect-data-analytics/custom-code-event.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
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

   Das obige Codefragment fügt einen Ereignis-Listener hinzu durch [Pushen einer Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht ein. Wenn die `cmp:show` -Ereignis ausgelöst wird, wird die `pageShownEventHandler` aufgerufen. In dieser Funktion werden einige Sanitätsprüfungen hinzugefügt und ein neuer `event` mit der neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente, die das Ereignis ausgelöst hat.

   Danach `trigger(event)` aufgerufen wird. `trigger()` ist ein reservierter Name in Launch und &quot;Trigger&quot;für die Launch-Regel. Wir überholen die `event` -Objekt als Parameter, der wiederum von einem anderen reservierten Namen in Launch mit dem Namen `event`. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen, z. B.: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Nächste unter **Aktionen** click **Hinzufügen** , um **Aktionskonfiguration** Assistent.
1. under **Aktionstyp** Auswählen **Benutzerspezifischer Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/collect-data-analytics/action-custom-code.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   Die `event` -Objekt wird von der `trigger()` -Methode, die im benutzerspezifischen Ereignis aufgerufen wird. `component` ist die aktuelle von der Datenschicht abgeleitete Seite `getState` im benutzerspezifischen Ereignis. Erinnern Sie sich an frühere [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) durch die Datenschicht verfügbar gemacht werden, um die verschiedenen standardmäßig angezeigten Schlüssel zu sehen.

1. Speichern Sie die Änderungen und führen Sie eine [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch , um den Code für die [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=de) verwendet auf Ihrer AEM Site.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, die [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) , um den Einbettungscode in einen **Entwicklung** Umgebung.

1. Navigieren Sie zu Ihrer AEM-Site und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Aktualisieren Sie die Seite. Sie sollten sehen, dass die Konsolenmeldungen protokolliert wurden:

   ![Seitengeladene Konsolenmeldungen](assets/collect-data-analytics/page-show-event-console.png)

## Datenelemente erstellen

Erstellen Sie anschließend mehrere Datenelemente , um andere Werte als die Adobe Client-Datenschicht zu erfassen. Wie wir in der vorherigen Übung gesehen haben, ist es möglich, direkt über benutzerdefinierten Code auf die Eigenschaften der Datenschicht zuzugreifen. Der Vorteil der Verwendung von Datenelementen besteht darin, dass sie über Launch-Regeln hinweg wiederverwendet werden können.

Erinnern Sie sich an frühere [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) durch die Datenschicht verfügbar gemacht werden:

Datenelemente werden dem `@type`, `dc:title`und `xdm:template` Eigenschaften.

### Komponenten-Ressourcentyp

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement erstellen**.
1. Für **Name** enter **Komponenten-Ressourcentyp**.
1. Für **Datenelementtyp** select **Benutzerspezifischer Code**.

   ![Komponenten-Ressourcentyp](assets/collect-data-analytics/component-resource-type-form.png)

1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Erinnern Sie sich daran, dass die `event` -Objekt wird basierend auf dem Ereignis, das die **Regel** in Launch. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement *referenziert* innerhalb einer Regel. Daher ist es sicher, dieses Datenelement innerhalb einer Regel wie die **Seite geladen** im vorherigen Schritt erstellte Regel *but* in anderen Kontexten nicht sicher verwendet werden.

### Seitenname

1. Klicken **Datenelement hinzufügen**.
1. Für **Name** enter **Seitenname**.
1. Für **Datenelementtyp** select **Benutzerspezifischer Code**.
1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

### Seitenvorlage

1. Klicken **Datenelement hinzufügen**.
1. Für **Name** enter **Seitenvorlage**.
1. Für **Datenelementtyp** select **Benutzerspezifischer Code**.
1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Speichern Sie die Änderungen.

1. Sie sollten jetzt drei Datenelemente als Teil Ihrer Regel haben:

   ![Datenelemente in der Regel](assets/collect-data-analytics/data-elements-page-rule.png)

## Hinzufügen der Analytics-Erweiterung

Fügen Sie als Nächstes die Analytics-Erweiterung Ihrer Launch-Eigenschaft hinzu. Wir müssen diese Daten irgendwo senden!

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zu **Erweiterungen** > **Katalog**
1. Suchen Sie die **Adobe Analytics** Erweiterung und Klicken **Installieren**

   ![Adobe Analytics-Erweiterung](assets/collect-data-analytics/analytics-catalog-install.png)

1. under **Bibliotheksverwaltung** > **Report Suites** Geben Sie die Report Suite-IDs ein, die Sie für jede Launch-Umgebung verwenden möchten.

   ![Report Suite-IDs eingeben](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In diesem Tutorial können Sie eine Report Suite für alle Umgebungen verwenden. In der Praxis sollten Sie jedoch separate Report Suites verwenden, wie in der Abbildung unten dargestellt

   >[!TIP]
   >
   >Es wird empfohlen, die *Option &quot;Bibliothek für mich verwalten&quot;* als Einstellung für die Bibliotheksverwaltung, da dies die `AppMeasurement.js` -Bibliothek auf dem neuesten Stand.

1. Aktivieren Sie das Kontrollkästchen, um **Activity Map verwenden**.

   ![Verwenden von Activity Map aktivieren](assets/track-clicked-component/analytic-track-click.png)

1. under **Allgemein** > **Tracking-Server**, geben Sie Ihren Trackingserver ein, z. B. `tmd.sc.omtrdc.net`. Geben Sie Ihren SSL-Tracking-Server ein, wenn Ihre Site `https://`

   ![Trackingserver eingeben](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## Hinzufügen einer Bedingung zur Regel &quot;Seite geladen&quot;

Aktualisieren Sie anschließend die **Seite geladen** -Regel verwenden, um **Komponenten-Ressourcentyp** -Datenelement, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn die `cmp:show` -Ereignis für **Seite**. Andere Komponenten können die `cmp:show` -Ereignis, z. B. löst die Karussellkomponente diese aus, wenn sich die Folien ändern. Daher ist es wichtig, eine Bedingung für diese Regel hinzuzufügen.

1. Navigieren Sie in der Launch-Benutzeroberfläche zum **Seite geladen** Regel, die zuvor erstellt wurde.
1. under **Bedingungen** click **Hinzufügen** , um **Bedingungskonfiguration** Assistent.
1. Für **Bedingungstyp** select **Wertvergleich**.
1. Setzen Sie den ersten Wert im Formularfeld auf `%Component Resource Type%`. Sie können das Datenelement-Symbol verwenden ![Datenelementsymbol](assets/collect-data-analytics/cylinder-icon.png) zur Auswahl der **Komponenten-Ressourcentyp** Datenelement. Belassen Sie den Vergleichssatz auf `Equals`.
1. Setzen Sie den zweiten Wert auf `wknd/components/page`.

   ![Bedingungskonfiguration für Seitenladeregel](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Diese Bedingung kann innerhalb der benutzerdefinierten Code-Funktion hinzugefügt werden, die auf die `cmp:show` -Ereignis, das zuvor im Tutorial erstellt wurde. Das Hinzufügen der Regel innerhalb der Benutzeroberfläche bietet zusätzlichen Benutzern mehr Sichtbarkeit, die möglicherweise Änderungen an der Regel vornehmen müssen. Außerdem können wir unser Datenelement verwenden!

1. Speichern Sie die Änderungen.

## Analytics-Variablen und Trigger-Seitenansichts-Beacon festlegen

Derzeit ist das **Seite geladen** -Regel gibt einfach eine Konsolenanweisung aus. Verwenden Sie als Nächstes die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **action** im **Seite geladen** Regel. Wir werden auch zusätzliche Maßnahmen zum Trigger der **Seitenansichts-Beacon** und senden Sie die erfassten Daten an Adobe Analytics.

1. Im **Seite geladen** Regel **remove** die **Core - benutzerspezifischer Code** action (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/collect-data-analytics/remove-console-statements.png)

1. Klicken Sie unter &quot;Aktionen&quot;auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Variablen festlegen**

   ![Aktionserweiterung auf Analytics-Variablen festlegen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Wählen Sie im Hauptbereich eine verfügbare **eVar** und als Wert des Datenelements festlegen **Seitenvorlage**. Verwenden des Symbols Datenelemente ![Symbol &quot;Datenelemente&quot;](assets/collect-data-analytics/cylinder-icon.png) zur Auswahl der **Seitenvorlage** -Element.

   ![Als eVar-Seitenvorlage festlegen](assets/collect-data-analytics/set-evar-page-template.png)

1. Hinunter scrollen **Zusätzliche Einstellungen** set **Seitenname** zum Datenelement **Seitenname**:

   ![Umgebungsvariablensatz für Seitenname](assets/collect-data-analytics/page-name-env-variable-set.png)

   Speichern Sie die Änderungen.

1. Fügen Sie anschließend rechts neben dem **Adobe Analytics - Variablen festlegen** durch Tippen auf **plus** Symbol:

   ![Hinzufügen einer zusätzlichen Launch-Aktion](assets/collect-data-analytics/add-additional-launch-action.png)

1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Signal senden**. Da dies als Seitenansicht gilt, lassen Sie die standardmäßige Tracking-Einstellung auf **`s.t()`**.

   ![Aktion &quot;Beacon Adobe Analytics senden&quot;](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Speichern Sie die Änderungen. Die **Seite geladen** -Regel sollte jetzt die folgende Konfiguration aufweisen:

   ![Abschließende Launch-Konfiguration](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Suchen Sie nach `cmp:show` -Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis von einer Seite ausgelöst wurde.
   * **3.** Festlegen von Analytics-Variablen für **Seitenname** und **Seitenvorlage**
   * **4.** Senden des Analytics-Beacons für die Seitenansicht
1. Speichern Sie alle Änderungen und erstellen Sie Ihre Launch-Bibliothek, indem Sie sie in die entsprechende Umgebung weiterleiten.

## Überprüfen des Seitenansichts-Beacons- und Analytics-Aufrufs

Nun, dass **Seite geladen** sendet das Analytics-Beacon. Sie sollten die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/collect-data-analytics/experience-cloud-debugger.png) , um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Launch-Eigenschaft zu *Ihre* Entwicklungsumgebung, wie zuvor beschrieben und **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite. Der Seitenname sollte ebenfalls angegeben werden:

   ![Analytics-Tab-Debugger](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Hinunter scrollen und erweitern **Netzwerkanforderungen**. Sie sollten die **evar** für **Seitenvorlage**:

   ![eVar- und Seitenname-Satz](assets/collect-data-analytics/evar-page-name-set.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Klicken Sie durch die **Karussell** oben auf der Seite.

   ![Karussellseite durchklicken](assets/collect-data-analytics/click-carousel-page.png)

1. Beobachten Sie in der Browser-Konsole die Konsolenanweisung:

   ![Bedingung nicht erfüllt](assets/collect-data-analytics/condition-not-met.png)

   Dies liegt daran, dass das Karussell einen Trigger `cmp:show` event *but* aufgrund unserer Kontrolle der **Komponenten-Ressourcentyp**, wird kein Ereignis ausgelöst.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** wird unter **Launch** im Experience Platform Debugger.

1. Navigieren Sie zu einer Artikelseite wie [Westaustralien](https://wknd.site/us/en/magazine/western-australia.html). Beachten Sie, dass sich Seitenname und Vorlagentyp ändern.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisbasierte Adobe Client Data Layer and Experience Platform Launch verwendet, um Daten von einer AEM Site zu erfassen und an Adobe Analytics zu senden.

### Nächste Schritte

Sehen Sie sich das folgende Tutorial an, um zu erfahren, wie Sie mit der ereignisgesteuerten Adobe Client Data-Schicht [Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site verfolgen](track-clicked-component.md).
