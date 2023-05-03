---
title: Nachverfolgen angeklickter Komponenten mit Adobe Analytics
description: Verwenden Sie die ereignisbasierte Adobe Client Data-Schicht, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie mit Tag-Regeln diese Ereignisse überwachen und Daten mithilfe eines Verfolgungslink-Beacons an eine Adobe Analytics Report Suite senden können.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 3%

---

# Nachverfolgen angeklickter Komponenten mit Adobe Analytics

>[!NOTE]
>
>Adobe Experience Platform Launch wurde als eine Suite von Datenerfassungstechnologien in Adobe Experience Platform umbenannt. Infolgedessen wurden in der gesamten Produktdokumentation mehrere terminologische Änderungen eingeführt. Siehe Folgendes [Dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) für eine konsolidierte Übersicht über die terminologischen Änderungen.

Ereignisgesteuert verwenden [Adobe der Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de) , um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie in der Tag-Eigenschaft Regeln verwenden, um auf Klick-Ereignisse zu überwachen, nach Komponenten zu filtern und die Daten mit einem Verfolgungslink-Beacon an eine Adobe Analytics zu senden.

## Was Sie erstellen werden {#what-build}

Das WKND-Marketing-Team möchte wissen, welches `Call to Action (CTA)` -Schaltflächen funktionieren am besten auf der Startseite. In diesem Tutorial fügen wir eine Regel zur Tag-Eigenschaft hinzu, die auf die `cmp:click` Ereignisse aus **Teaser** und **Schaltfläche** Komponenten. Senden Sie dann die Komponenten-ID und ein neues Ereignis zusammen mit dem Verfolgungslink-Beacon an Adobe Analytics.

![Was Sie für Tracking-Klicks erstellen werden](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine ereignisgesteuerte Regel in der Tag-Eigenschaft, die die `cmp:click` -Ereignis.
1. Filtern Sie die verschiedenen Ereignisse nach Komponenten-Ressourcentyp.
1. Legen Sie die Komponenten-ID fest und senden Sie ein Ereignis mit dem Verfolgungslink-Beacon an Adobe Analytics.

## Voraussetzungen

Dieses Tutorial stellt eine Fortsetzung von [Erfassen von Seitendaten mit Adobe Analytics](./collect-data-analytics.md) und geht davon aus, dass Sie über Folgendes verfügen:

* A **Tag-Eigenschaft** mit dem [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=de) enabled
* **Adobe Analytics** Report Suite-ID und Tracking-Server testen/entwickeln. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) Browsererweiterung , die mit Ihrer Tag-Eigenschaft konfiguriert wurde, die auf geladen wird [WKND-Site](https://wknd.site/us/en.html) oder einer AEM Site mit aktivierter Adobe-Datenschicht.

## Inspect des Schaltflächen- und Teaser-Schemas

Bevor Sie Regeln in der Tag-Eigenschaft erstellen, sollten Sie die [Schema für Schaltfläche und Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) und überprüfen Sie sie in der Datenschichtimplementierung.

1. Navigieren Sie zu [WKND-Homepage](https://wknd.site/us/en.html)
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zum **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Der obige Code gibt den aktuellen Status der Adobe Client-Datenschicht zurück.

   ![Status der Datenschicht über die Browser-Konsole](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Erweitern Sie die Antwort und suchen Sie Einträge mit dem Präfix `button-` und  `teaser-xyz-cta` eingeben. Es sollte ein Datenschema wie das folgende angezeigt werden:

   Schaltflächenschema:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaser-Schema:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Die obigen Datendetails basieren auf der Variablen [Komponenten-/Container-Element-Schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). Die neue Tag-Regel verwendet dieses Schema.

## Erstellen einer auf CTA geklickten Regel

Die Adobe Client-Datenschicht ist eine **event** gesteuerte Datenschicht. Wenn auf eine Kernkomponente geklickt wird, wird ein `cmp:click` -Ereignis über die Datenschicht gesendet. So lauschen Sie auf `cmp:click` -Ereignis erstellen, lassen Sie uns eine Regel erstellen.

1. Navigieren Sie zur Experience Platform und zur Tag-Eigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Regeln** in der Benutzeroberfläche &quot;Tag-Eigenschaft&quot;auf **Regel hinzufügen**.
1. Benennen Sie die Regel. **CTA angeklickt**.
1. Klicken **Veranstaltungen** > **Hinzufügen** , um **Ereigniskonfiguration** Assistent.
1. Für **Ereignistyp** Feld, wählen Sie **Benutzerspezifischer Code**.

   ![Benennen Sie die Regel CTA Angeklickt und fügen Sie das benutzerspezifische Code-Ereignis hinzu.](assets/track-clicked-component/custom-code-event.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   Das obige Codefragment fügt einen Ereignis-Listener hinzu von [Pushen einer Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht ein. Immer `cmp:click` -Ereignis ausgelöst wird, wird die `componentClickedHandler` aufgerufen. In dieser Funktion werden einige Integritätsprüfungen hinzugefügt und eine neue `event` -Objekt wird mit der neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente, die das Ereignis ausgelöst hat.

   Schließlich `trigger(event)` aufgerufen. Die `trigger()` -Funktion ist ein reservierter Name in der Tag-Eigenschaft und **Trigger** die Regel. Die `event` -Objekt wird als Parameter übergeben, der wiederum durch einen anderen reservierten Namen in der Tag-Eigenschaft verfügbar gemacht wird. Datenelemente in der Tag-Eigenschaft können jetzt mithilfe eines Code-Snippets wie `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Nächste unter **Aktionen** click **Hinzufügen** , um **Aktionskonfiguration** Assistent.
1. Für **Aktionstyp** Feld, wählen Sie **Benutzerspezifischer Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/track-clicked-component/action-custom-code.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Die `event` -Objekt wird von der `trigger()` -Methode, die im benutzerspezifischen Ereignis aufgerufen wird. Die `component` -Objekt ist der aktuelle Status der aus der Datenschicht abgeleiteten Komponente `getState()` -Methode und ist das Element, das den Klick ausgelöst hat.

1. Speichern Sie die Änderungen und führen Sie eine [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in der Tag-Eigenschaft, um den Code für die [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=de) verwendet auf Ihrer AEM Site.

   >[!NOTE]
   >
   > Es kann nützlich sein, die [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) , um den Einbettungscode in einen **Entwicklung** Umgebung.

1. Navigieren Sie zum [WKND-Site](https://wknd.site/us/en.html) und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Wählen Sie außerdem die **Protokoll beibehalten** aktivieren.

1. Klicken Sie auf einen der **Teaser** oder **Schaltfläche** CTA-Schaltflächen zum Navigieren zu einer anderen Seite.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Beachten Sie in der Entwicklerkonsole, dass die **CTA angeklickt** -Regel wurde ausgelöst:

   ![CTA-Schaltfläche angeklickt](assets/track-clicked-component/cta-button-clicked-log.png)

## Datenelemente erstellen

Erstellen Sie anschließend ein Datenelement , um die Komponenten-ID und den Titel zu erfassen, auf die geklickt wurde. Erinnern Sie sich in der vorherigen Übung an die Ergebnisse von `event.path` etwas Ähnliches war `component.button-b6562c963d` und der Wert von `event.component['dc:title']` war so etwas wie &quot;Ausflüge ansehen&quot;.

### Komponenten-ID

1. Navigieren Sie zur Experience Platform und zur Tag-Eigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Für **Name** Feld, eingeben **Komponenten-ID**.
1. Für **Datenelementtyp** Feld, wählen Sie **Benutzerspezifischer Code**.

   ![Komponenten-ID-Datenelementformular](assets/track-clicked-component/component-id-data-element.png)

1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Erinnern Sie sich daran, dass die `event` -Objekt wird basierend auf dem Ereignis, das die **Regel** in der Tag-Eigenschaft. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement *referenziert* innerhalb einer Regel. Daher ist es sicher, dieses Datenelement innerhalb einer Regel wie der **Seite geladen** im vorherigen Schritt erstellte Regel *but* in anderen Kontexten nicht sicher verwendet werden.


### Komponententitel

1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Für **Name** Feld, eingeben **Komponententitel**.
1. Für **Datenelementtyp** Feld, wählen Sie **Benutzerspezifischer Code**.
1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Speichern Sie die Änderungen.

## Hinzufügen einer Bedingung zur Regel &quot;CTA angeklickt&quot;

Aktualisieren Sie anschließend die **CTA angeklickt** -Regel, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn die `cmp:click` -Ereignis ausgelöst wird für eine **Teaser** oder **Schaltfläche**. Da der CTA des Teasers als separates Objekt in der Datenschicht betrachtet wird, ist es wichtig, das übergeordnete Element zu überprüfen, um sicherzustellen, dass es von einem Teaser stammt.

1. Navigieren Sie in der Benutzeroberfläche der Tag-Eigenschaft zum **CTA angeklickt** Regel, die zuvor erstellt wurde.
1. under **Bedingungen** click **Hinzufügen** , um **Bedingungskonfiguration** Assistent.
1. Für **Bedingungstyp** Feld, wählen Sie **Benutzerspezifischer Code**.

   ![CTA angeklickter benutzerdefinierter Code für Bedingung](assets/track-clicked-component/custom-code-condition.png)

1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   Der obige Code prüft zunächst, ob der Ressourcentyp von einer **Schaltfläche** oder wenn der Ressourcentyp aus einem CTA in einer **Teaser**.

1. Speichern Sie die Änderungen.

## Festlegen von Analytics-Variablen und Trigger-Tracking-Link-Beacon

Derzeit ist das **CTA angeklickt** -Regel gibt einfach eine Konsolenanweisung aus. Verwenden Sie als Nächstes die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **action**. Legen wir außerdem eine zusätzliche Aktion fest, um den Trigger **Link verfolgen** und senden Sie die erfassten Daten an Adobe Analytics.

1. Im **CTA angeklickt** Regel, **remove** die **Core - benutzerspezifischer Code** action (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/track-clicked-component/remove-console-statements.png)

1. Klicken Sie unter &quot;Aktionen&quot;auf **Hinzufügen** , um eine Aktion zu erstellen.
1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Variablen festlegen**.

1. Legen Sie die folgenden Werte für **eVars**, **Props** und **Veranstaltungen**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Festlegen von eVar-Prop- und -Ereignissen](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Hier `%Component ID%` wird verwendet, da sie eine eindeutige Kennung für den CTA garantiert, auf den geklickt wurde. Ein potenzieller Nachteil bei der Verwendung von `%Component ID%` ist, dass der Analytics-Bericht Werte wie `button-2e6d32893a`. Verwenden der `%Component Title%` würde einen benutzerfreundlicheren Namen geben, aber der Wert ist möglicherweise nicht eindeutig.

1. Als Nächstes fügen Sie rechts neben dem **Adobe Analytics - Variablen festlegen** durch Tippen auf **plus** Symbol:

   ![Hinzufügen einer zusätzlichen Aktion zur Tag-Regel](assets/track-clicked-component/add-additional-launch-action.png)

1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Signal senden**.
1. under **Tracking** Setzen Sie das Optionsfeld auf **`s.tl()`**.
1. Für **Link-Typ** Feld, wählen Sie **Benutzerspezifischer Link** und **Linkname** Setzen Sie den Wert auf: **`%Component Title%: CTA Clicked`**:

   ![Konfiguration für das Signal &quot;Link senden&quot;](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Die obige Konfiguration kombiniert die dynamische Variable aus dem Datenelement **Komponententitel** und die statische Zeichenfolge **CTA angeklickt**.

1. Speichern Sie die Änderungen. Die **CTA angeklickt** -Regel sollte jetzt die folgende Konfiguration aufweisen:

   ![Konfiguration der endgültigen Tag-Regel](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Suchen Sie nach `cmp:click` -Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis durch eine **Schaltfläche** oder **Teaser**.
   * **3.** Legen Sie Analytics-Variablen zur Verfolgung der **Komponenten-ID** als **eVar**, **prop** und ein **event**.
   * **4.** Senden des Analytics-Beacons &quot;Link verfolgen&quot;(und führen Sie **not** behandelt es als Seitenansicht).

1. Speichern Sie alle Änderungen und erstellen Sie Ihre Tag-Bibliothek, indem Sie sie an die entsprechende Umgebung weiterleiten.

## Validieren des Verfolgungslink-Beacon- und Analytics-Aufrufs

Nun, dass **CTA angeklickt** sendet das Analytics-Beacon. Sie sollten die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/track-clicked-component/experience-cloud-debugger.png) , um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Tag-Eigenschaft dem *Ihre* Entwicklungsumgebung, wie zuvor beschrieben, und die **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite.

   ![Analytics-Tab-Debugger](assets/track-clicked-component/analytics-tab-debugger.png)

1. Klicken Sie im Browser auf einen der **Teaser** oder **Schaltfläche** CTA-Schaltflächen zum Navigieren zu einer anderen Seite.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Kehren Sie zum Experience Platform Debugger zurück, scrollen Sie nach unten und erweitern Sie **Netzwerkanforderungen** > *Ihre Report Suite*. Sie sollten die **eVar**, **prop** und **event** gesetzt.

   ![Analytics-Ereignisse, eVar und beim Klick verfolgte Eigenschaften](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Navigieren Sie zur Fußzeile der Site und klicken Sie auf einen der Navigationslinks:

   ![Klicken Sie in der Fußzeile auf den Link Navigation .](assets/track-clicked-component/click-navigation-link-footer.png)

1. In der Browser-Konsole die Nachricht beobachten *&quot;Benutzerspezifischer Code&quot;für Regel &quot;CTA angeklickt&quot;wurde nicht erfüllt*.

   Die obige Meldung liegt darin, dass die Navigationskomponente einen Trigger `cmp:click` event *but* aufgrund von [Bedingung für die Regel](#add-a-condition-to-the-cta-clicked-rule) überprüft, dass der Ressourcentyp nicht ausgeführt wird.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** wird unter **Experience Platform Tags** im Experience Platform Debugger.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisbasierte Adobe Client Data Layer und Tag in Experience Platform verwendet, um die Klicks auf bestimmte Komponenten auf einer AEM Site zu verfolgen.
