---
title: Verfolgen geklickter Komponenten mit Adobe Analytics
description: Verwenden Sie die ereignisbasierte Adobe Client Data-Schicht, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie in Experience Platform Launch mithilfe von Regeln auf diese Ereignisse warten und Daten mit einem Verfolgungslink-Beacon an eine Adobe Analytics senden können.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 4%

---

# Verfolgen geklickter Komponenten mit Adobe Analytics

Ereignisgesteuert verwenden [Adobe der Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de) , um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie Regeln in Experience Platform Launch verwenden, um Klick-Ereignisse zu überwachen, nach Komponenten zu filtern und die Daten mit einem Verfolgungs-Linkbeacon an Adobe Analytics zu senden.

## Was Sie erstellen werden

Das WKND-Marketing-Team möchte wissen, welche Aktionsaufruf-Schaltflächen (CTA) die beste Leistung auf der Startseite erzielen. In diesem Tutorial fügen wir eine neue Regel in Experience Platform Launch hinzu, die auf Folgendes überwacht: `cmp:click` Ereignisse aus **Teaser** und **Schaltfläche** Komponenten und senden Sie die Komponenten-ID und ein neues Ereignis zusammen mit dem Verfolgungslink-Beacon an Adobe Analytics.

![Was Sie für Tracking-Klicks erstellen werden](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine ereignisbasierte Regel in Launch basierend auf der `cmp:click` -Ereignis.
1. Filtern Sie die verschiedenen Ereignisse nach Komponenten-Ressourcentyp.
1. Legen Sie die angeklickte Komponenten-ID fest und senden Sie das Ereignis Adobe Analytics mit dem Verfolgungslink-Beacon.

## Voraussetzungen

Dieses Tutorial stellt eine Fortsetzung von [Erfassen von Seitendaten mit Adobe Analytics](./collect-data-analytics.md) und geht davon aus, dass Sie über Folgendes verfügen:

* A **Launch-Eigenschaft** mit dem [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html?lang=de) enabled
* **Adobe Analytics** Report Suite-ID und Tracking-Server testen/entwickeln. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer neuen Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Browsererweiterung , die mit Ihrer Launch-Eigenschaft konfiguriert ist, die in geladen wird [https://wknd.site/us/en.html](https://wknd.site/us/en.html) oder einer AEM Site mit aktivierter Adobe-Datenschicht.

## Inspect des Schaltflächen- und Teaser-Schemas

Bevor Sie Regeln in Launch erstellen, sollten Sie die [Schema für Schaltfläche und Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) und überprüfen Sie sie in der Datenschichtimplementierung.

1. Navigieren Sie zu [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zum **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

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

   Diese basieren auf der [Komponenten-/Container-Element-Schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). Die Regel, die wir in Launch erstellen, verwendet dieses Schema.

## Erstellen einer auf CTA geklickten Regel

Die Adobe Client-Datenschicht ist eine **event** gesteuerte Datenschicht. Wenn auf eine Kernkomponente geklickt wird, wird ein `cmp:click` -Ereignis über die Datenschicht gesendet. Erstellen Sie als Nächstes eine Regel, die auf die `cmp:click` -Ereignis.

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Regeln** in der Launch-Benutzeroberfläche und klicken Sie dann auf **Regel hinzufügen**.
1. Benennen Sie die Regel. **CTA angeklickt**.
1. Klicken **Veranstaltungen** > **Hinzufügen** , um **Ereigniskonfiguration** Assistent.
1. under **Ereignistyp** select **Benutzerspezifischer Code**.

   ![Benennen Sie die Regel CTA Angeklickt und fügen Sie das benutzerspezifische Code-Ereignis hinzu.](assets/track-clicked-component/custom-code-event.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   Das obige Codefragment fügt einen Ereignis-Listener hinzu von [Pushen einer Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht ein. Wenn die `cmp:click` -Ereignis ausgelöst wird, wird die `componentClickedHandler` aufgerufen. In dieser Funktion werden einige Sanitätsprüfungen hinzugefügt und ein neuer `event` -Objekt wird mit der neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente, die das Ereignis ausgelöst hat.

   Danach `trigger(event)` aufgerufen wird. `trigger()` ist ein reservierter Name in Launch und &quot;Trigger&quot;die Launch-Regel. Wir überholen die `event` -Objekt als Parameter, der wiederum von einem anderen reservierten Namen in Launch mit dem Namen `event`. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen, z. B.: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Nächste unter **Aktionen** click **Hinzufügen** , um **Aktionskonfiguration** Assistent.
1. under **Aktionstyp** Auswählen **Benutzerspezifischer Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/track-clicked-component/action-custom-code.png)

1. Klicken **Editor öffnen** Geben Sie im Hauptbereich das folgende Codefragment ein:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Die `event` -Objekt wird von der `trigger()` -Methode, die im benutzerspezifischen Ereignis aufgerufen wird. `component` ist der aktuelle Status der aus der Datenschicht abgeleiteten Komponente `getState` , der den Klick ausgelöst hat.

1. Speichern Sie die Änderungen und führen Sie eine [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch , um den Code für die [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) verwendet auf Ihrer AEM Site.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, die [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) , um den Einbettungscode in einen **Entwicklung** Umgebung.

1. Navigieren Sie zum [WKND-Site](https://wknd.site/us/en.html) und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Auswählen **Protokoll beibehalten**.

1. Klicken Sie auf einen der **Teaser** oder **Schaltfläche** CTA-Schaltflächen zum Navigieren zu einer anderen Seite.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Beachten Sie in der Entwicklerkonsole, dass die **CTA angeklickt** -Regel wurde ausgelöst:

   ![CTA-Schaltfläche angeklickt](assets/track-clicked-component/cta-button-clicked-log.png)

## Datenelemente erstellen

Erstellen Sie anschließend ein Datenelement , um die Komponenten-ID und den Titel zu erfassen, auf die geklickt wurde. Erinnern Sie sich in der vorherigen Übung an die Ergebnisse von `event.path` etwas Ähnliches war `component.button-b6562c963d` und der Wert von `event.component['dc:title']` war so etwas wie &quot;Ausflüge ansehen&quot;.

### Komponenten-ID

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Für **Name** enter **Komponenten-ID**.
1. Für **Datenelementtyp** select **Benutzerspezifischer Code**.

   ![Komponenten-ID-Datenelementformular](assets/track-clicked-component/component-id-data-element.png)

1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Erinnern Sie sich daran, dass die `event` -Objekt wird basierend auf dem Ereignis, das die **Regel** in Launch. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement *referenziert* innerhalb einer Regel. Daher ist es sicher, dieses Datenelement innerhalb einer Regel wie die **CTA angeklickt** in der vorherigen Übung erstellte Regel *but* in anderen Kontexten nicht sicher verwendet werden.

### Komponentenname

1. Navigieren Sie zum **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Für **Name** enter **Komponententitel**.
1. Für **Datenelementtyp** select **Benutzerspezifischer Code**.
1. Klicken **Editor öffnen** und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

## Hinzufügen einer Bedingung zur Regel &quot;CTA angeklickt&quot;

Aktualisieren Sie anschließend die **CTA angeklickt** -Regel, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn die `cmp:click` -Ereignis ausgelöst wird für eine **Teaser** oder **Schaltfläche**. Da der CTA des Teasers als separates Objekt in der Datenschicht betrachtet wird, ist es wichtig, das übergeordnete Element zu überprüfen, um sicherzustellen, dass es von einem Teaser stammt.

1. Navigieren Sie in der Launch-Benutzeroberfläche zum **CTA angeklickt** Regel, die zuvor erstellt wurde.
1. under **Bedingungen** click **Hinzufügen** , um **Bedingungskonfiguration** Assistent.
1. Für **Bedingungstyp** select **Benutzerspezifischer Code**.

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

   Der obige Code prüft zunächst, ob der Ressourcentyp von einer **Schaltfläche** und überprüft dann, ob der Ressourcentyp aus einem CTA in einer **Teaser**.

1. Speichern Sie die Änderungen.

## Festlegen von Analytics-Variablen und Trigger-Tracking-Link-Beacon

Derzeit ist das **CTA angeklickt** -Regel gibt einfach eine Konsolenanweisung aus. Verwenden Sie als Nächstes die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **action**. Wir werden auch zusätzliche Maßnahmen zum Trigger der **Link verfolgen** und senden Sie die erfassten Daten an Adobe Analytics.

1. Im **CTA angeklickt** Regel **remove** die **Core - benutzerspezifischer Code** action (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/track-clicked-component/remove-console-statements.png)

1. Klicken Sie unter &quot;Aktionen&quot;auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Variablen festlegen**.

1. Legen Sie die folgenden Werte für **eVars**, **Props** und **Veranstaltungen**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Festlegen von eVar-Prop- und -Ereignissen](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Hier `%Component ID%` verwendet wird, da dadurch eine eindeutige Kennung für den angeklickten CTA sichergestellt wird. Ein potenzieller Nachteil bei der Verwendung von `%Component ID%` ist, dass der Analytics-Bericht Werte wie `button-2e6d32893a`. Verwenden `%Component Title%` würde einen benutzerfreundlicheren Namen geben, aber der Wert ist möglicherweise nicht eindeutig.

1. Fügen Sie anschließend rechts neben dem **Adobe Analytics - Variablen festlegen** durch Tippen auf **plus** Symbol:

   ![Hinzufügen einer zusätzlichen Launch-Aktion](assets/track-clicked-component/add-additional-launch-action.png)

1. Legen Sie die **Erweiterung** type to **Adobe Analytics** und legen Sie die **Aktionstyp** nach  **Signal senden**.
1. under **Tracking** Setzen Sie das Optionsfeld auf **`s.tl()`**.
1. Für **Link-Typ** Auswählen **Benutzerspezifischer Link** und **Linkname** Setzen Sie den Wert auf: **`%Component Title%: CTA Clicked`**:

   ![Konfiguration für das Signal &quot;Link senden&quot;](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Dadurch wird die dynamische Variable aus dem Datenelement kombiniert. **Komponententitel** und die statische Zeichenfolge **CTA angeklickt**.

1. Speichern Sie die Änderungen. Die **CTA angeklickt** -Regel sollte jetzt die folgende Konfiguration aufweisen:

   ![Abschließende Launch-Konfiguration](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Suchen Sie nach `cmp:click` -Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis durch eine **Schaltfläche** oder **Teaser**.
   * **3.** Legen Sie Analytics-Variablen für fest, um die **Komponenten-ID** als **eVar**, **prop** und ein **event**.
   * **4.** Senden des Analytics-Beacons &quot;Link verfolgen&quot;(und führen Sie **not** behandelt es als Seitenansicht).

1. Speichern Sie alle Änderungen und erstellen Sie Ihre Launch-Bibliothek, indem Sie sie in die entsprechende Umgebung weiterleiten.

## Validieren des Verfolgungslink-Beacon- und Analytics-Aufrufs

Nun, dass **CTA angeklickt** sendet das Analytics-Beacon. Sie sollten die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/track-clicked-component/experience-cloud-debugger.png) , um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Launch-Eigenschaft zu *Ihre* Entwicklungsumgebung, wie zuvor beschrieben und **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite.

   ![Analytics-Tab-Debugger](assets/track-clicked-component/analytics-tab-debugger.png)

1. Klicken Sie im Browser auf einen der **Teaser** oder **Schaltfläche** CTA-Schaltflächen zum Navigieren zu einer anderen Seite.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Kehren Sie zum Experience Platform Debugger zurück, scrollen Sie nach unten und erweitern Sie **Netzwerkanforderungen** > *Ihre Report Suite*. Sie sollten die **eVar**, **prop** und **event** gesetzt.

   ![Analytics-Ereignisse, eVar und beim Klick verfolgte Eigenschaften](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Navigieren Sie zur Fußzeile der Site und klicken Sie auf einen der Navigationslinks:

   ![Klicken Sie in der Fußzeile auf den Link Navigation .](assets/track-clicked-component/click-navigation-link-footer.png)

1. In der Browser-Konsole die Nachricht beobachten *&quot;Benutzerspezifischer Code&quot;für Regel &quot;CTA angeklickt&quot;wurde nicht erfüllt*.

   Dies liegt daran, dass die Navigationskomponente den Trigger einer `cmp:click` event *but* weil wir den gegen den Ressourcentyp überprüfen, wird keine Aktion durchgeführt.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** wird unter **Launch** im Experience Platform Debugger.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisgesteuerte Adobe Client Data Layer und den Experience Platform Launch verwendet, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen.
