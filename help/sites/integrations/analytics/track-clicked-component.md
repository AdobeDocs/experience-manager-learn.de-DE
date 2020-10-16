---
title: Klickende Komponente mit Adobe Analytics verfolgen
description: Verwenden Sie die Ereignis-gesteuerte Adobe Client Data-Ebene, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie in Experience Platform Launch Regeln verwenden, um auf diese Ereignis zu hören und Daten mit einem Tracking-Link-Beacon an ein Adobe Analytics zu senden.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 4%

---


# Klickende Komponente mit Adobe Analytics verfolgen

Use the event-driven [Adobe Client Data Layer with AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html) to track clicks of specific components on an Adobe Experience Manager site. Erfahren Sie, wie Sie Regeln in Experience Platform Launch verwenden, um Klick-Ereignisse zu überwachen, nach Komponenten zu filtern und die Daten mit einem Verfolgungs-Linkbeacon an Adobe Analytics zu senden.

## Was Sie erstellen

Das WKND-Marketingteam möchte wissen, welche Aktionsaufruf-Schaltflächen auf der Startseite die beste Leistung erbringen. In diesem Lernprogramm fügen wir eine neue Regel in Experience Platform Launch hinzu, die auf `cmp:click` Ereignis aus **Teaser** - und **Button** -Komponenten überwacht und die Komponenten-ID und ein neues Ereignis neben dem Tracking-Link-Beacon an Adobe Analytics sendet.

![Was Sie zum Erstellen von Trackklicks benötigen](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine Ereignis-basierte Regel in Launch basierend auf dem `cmp:click` Ereignis.
1. Filtern Sie die verschiedenen Ereignis nach Komponentenressourcentyp.
1. Legen Sie die angeklickte Komponenten-ID fest und senden Sie Ereignis Adobe Analytics mit dem Tracking-Link-Beacon.

## Voraussetzungen

Diese Übung ist eine Fortsetzung der [Erfassung von Seitendaten mit Adobe Analytics](./collect-data-analytics.md) und setzt voraus, dass Sie über Folgendes verfügen:

* Eine **Starteigenschaft** mit aktivierter [Adobe Analytics Extension](https://docs.adobe.com/content/help/de-DE/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)
* **Adobe Analytics** -Test-/Entwicklungs-Report Suite-ID und Tracking-Server. Informationen zum [Erstellen einer neuen Report Suite](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)finden Sie in der folgenden Dokumentation.
* [Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) -Browsererweiterung, die mit der Launch-Eigenschaft konfiguriert wurde, die auf [https://wknd.site/us/en.html](https://wknd.site/us/en.html) geladen wurde, oder einer AEM Site, auf der die Adobe Data Layer aktiviert ist.

## Inspect the Button and Teaser Schema

Bevor Sie Regeln in Launch erstellen, sollten Sie das [Schema für Button und Teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) überprüfen und sie in der Datenschichtimplementierung überprüfen.

1. Navigieren Sie zu [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Öffnen Sie die Entwicklerwerkzeuge des Browsers und navigieren Sie zur **Konsole**. Führen Sie folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

   ![Status der Datenschicht über die Browser-Konsole](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Erweitern Sie die Antwort und suchen Sie Einträge mit dem Präfix `button-` und `teaser-xyz-cta` Eintrag. Es sollte ein Schema wie das folgende angezeigt werden:

   Schaltflächen-Schema:

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
       @type: "nt:unstructured"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Diese basieren auf dem Schema [&quot;Komponenten/Container-Element&quot;](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item). Die Regel, die wir in Launch erstellen, wird dieses Schema verwenden.

## Eine auf CTA geklickte Regel erstellen

Die Adobe Client Data Layer ist eine vom **Ereignis** gesteuerte Datenschicht. Wenn auf die Kernkomponente geklickt wird, wird ein `cmp:click` Ereignis über die Datenschicht gesendet. Anschließend erstellen Sie eine Regel, die auf das `cmp:click` Ereignis überwacht.

1. Navigieren Sie zum Experience Platform Launch und zur Webeigenschaft, die in die AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Regeln** in der Benutzeroberfläche &quot;Start&quot;und klicken Sie dann auf **Hinzufügen Regel**.
1. Benennen Sie die Regel **CTA angeklickt**.
1. Klicken Sie auf **Ereignisse** > **Hinzufügen** , um den Assistenten zur **Ereignis-Konfiguration** zu öffnen.
1. Wählen Sie unter **Ereignistyp** die Option **Benutzerdefinierter Code**.

   ![Benennen Sie die Regel CTA Click und fügen Sie das benutzerdefinierte Code-Ereignis hinzu](assets/track-clicked-component/custom-code-event.png)

1. Klicken Sie im Hauptbereich auf &quot;Editor **öffnen** &quot;und geben Sie das folgende Codefragment ein:

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

   Das obige Codefragment fügt einen Ereignis-Listener hinzu, indem eine Funktion [in die Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) gedrängt wird. Wenn das `cmp:click` Ereignis ausgelöst wird, wird die `componentClickedHandler` Funktion aufgerufen. In dieser Funktion werden einige Sanitätsprüfungen hinzugefügt und ein neues `event` Objekt mit dem neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente erstellt, die das Ereignis ausgelöst hat.

   Danach `trigger(event)` wird gerufen. `trigger()` ist ein reservierter Name in Launch und löst die Launch-Regel aus. Wir übergeben das `event` Objekt als Parameter, der wiederum durch einen anderen reservierten Namen in Launch namens `event`. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Klicken Sie anschließend unter **Aktionen** auf **Hinzufügen** , um den Assistenten zur **Aktionskonfiguration** zu öffnen.
1. Wählen Sie unter **Aktionstyp** die Option **Benutzerdefinierter Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/track-clicked-component/action-custom-code.png)

1. Klicken Sie im Hauptbereich auf &quot;Editor **öffnen** &quot;und geben Sie das folgende Codefragment ein:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Das `event` Objekt wird von der `trigger()` Methode übergeben, die im benutzerdefinierten Ereignis aufgerufen wird. `component` ist der aktuelle Status der Komponente, die von der Datenschicht abgeleitet wurde, die den Klick ausgelöst `getState` hat.

1. Speichern Sie die Änderungen und führen Sie einen [Build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) in Launch aus, um den Code für die auf Ihrer AEM Site verwendete [Umgebung](https://docs.adobe.com/content/help/de-DE/launch/using/reference/publish/environments.html) zu bewerben.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, den [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) zu verwenden, um den Einbettungscode in eine **Entwicklungs** -Umgebung umzuschalten.

1. Navigieren Sie zur [WKND-Site](https://wknd.site/us/en.html) und öffnen Sie die Entwicklerwerkzeuge, um die Konsole Ansicht. Wählen Sie Protokoll **beibehalten**.

1. Klicken Sie auf eine der **Teaser** - oder **Button** -CTA-Schaltflächen, um zu einer anderen Seite zu navigieren.

   ![CTA-Schaltfläche zum Klicken auf](assets/track-clicked-component/cta-button-to-click.png)

1. Beachten Sie in der Entwicklerkonsole, dass die **CTA-Regel, auf die geklickt** wurde, ausgelöst wurde:

   ![CTA-Schaltfläche angeklickt](assets/track-clicked-component/cta-button-clicked-log.png)

## Datenelemente erstellen

Erstellen Sie anschließend ein Datenelement, um die Komponenten-ID und den Titel zu erfassen, auf die geklickt wurde. Erinnern Sie sich in der vorherigen Übung das Ergebnis `event.path` war ähnlich `component.button-b6562c963d` und der Wert von `event.component['dc:title']` war etwas wie &quot;Ansicht Reisen&quot;.

### Komponenten-ID

1. Navigieren Sie zum Experience Platform Launch und zur Webeigenschaft, die in die AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf **Hinzufügen Neues Datenelement**.
1. Geben Sie als **Name** die **Komponenten-ID** ein.
1. Wählen Sie für **Datenelementtyp** &quot; **Benutzerdefinierter Code**&quot;aus.

   ![Element für Komponenten-ID](assets/track-clicked-component/component-id-data-element.png)

1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Denken Sie daran, dass das `event` Objekt auf der Grundlage des Ereignisses, das die **Regel** beim Start ausgelöst hat, verfügbar gemacht und in Scopes dargestellt wird. Der Wert eines Datenelements wird erst festgelegt, wenn innerhalb einer Regel auf das Datenelement *verwiesen* wird. Daher ist es sicher, dieses Datenelement innerhalb einer Regel zu verwenden, wie die im vorherigen Vorgang erstellte **CTA-Regel, auf die geklickt** wurde. Die Verwendung in anderen Kontexten ist *jedoch* nicht sicher.

### Komponentenname

1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf **Hinzufügen Neues Datenelement**.
1. Geben Sie als **Name** den **Komponententitel** ein.
1. Wählen Sie für **Datenelementtyp** &quot; **Benutzerdefinierter Code**&quot;aus.
1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

## hinzufügen einer Bedingung für die CTA-Regel

Aktualisieren Sie anschließend die **CTA-Regel** , um sicherzustellen, dass die Regel nur ausgelöst wird, wenn das `cmp:click` Ereignis für einen **Teaser** oder eine **Schaltfläche** ausgelöst wird. Da die CTA des Teasers als separates Objekt in der Datenschicht betrachtet wird, ist es wichtig, die übergeordnete Ebene zu überprüfen, um sicherzustellen, dass sie von einem Teaser stammt.

1. Navigieren Sie in der Benutzeroberfläche &quot;Starten&quot;zu der zuvor erstellten Regel &quot; **Seite geladen** &quot;.
1. Klicken Sie unter **Bedingungen** auf **Hinzufügen** , um den Assistenten zur **Bedingungskonfiguration** zu öffnen.
1. Wählen Sie unter **Bedingungstyp** die Option **Benutzerdefinierter Code**.

   ![Benutzerdefinierter Code für CTA-Klicks](assets/track-clicked-component/custom-code-condition.png)

1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       //Check for Button Type
       if(event.component['@type'] === 'wknd/components/button') {
           return true;
       } else if (event.component['@type'] == 'nt:unstructured') {
           // Check for CTA inside a Teaser
           var parentComponentId = event.component['parentId'];
           var parentComponent = window.adobeDataLayer.getState('component.' + parentComponentId);
   
           if(parentComponent['@type'] === 'wknd/components/teaser') {
               return true;
           }
       }
   }
   
   return false;
   ```

   Der obige Code prüft zunächst, ob der Ressourcentyp von einer **Schaltfläche** stammt, und prüft dann, ob der Ressourcentyp von einer CTA in einem **Teaser** stammt.

1. Speichern Sie die Änderungen.

## Festlegen von Analytics-Variablen und Auslösen des Beacons für die Verfolgung von Links

Derzeit gibt die **CTA Click** -Regel einfach eine Konsolenanweisung aus. Als Nächstes verwenden Sie die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **Aktion** festzulegen. Außerdem werden wir eine zusätzliche Aktion einrichten, um den **Link** &quot;Verfolgen&quot;auszulösen und die erfassten Daten an Adobe Analytics zu senden.

1. Entfernen Sie in der Regel **Seitenladevorgang** die Aktion **Core - Benutzerdefinierter Code** (die Konsolenanweisungen) **aus** :

   ![Aktion für benutzerdefinierten Code entfernen](assets/track-clicked-component/remove-console-statements.png)

1. Klicken Sie unter Aktionen auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Legen Sie als **Erweiterungstyp** **Adobe Analytics** fest und legen Sie als **Aktionstyp** Variablen **festlegen** fest.

1. Legen Sie die folgenden Werte für **eVars**, **Props** und **Ereignis** fest:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![EVar-Prop und Ereignis festlegen](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Hier `%Component ID%` wird verwendet, da es eine eindeutige Kennung für die CTA sicherstellt, auf die geklickt wurde. Eine mögliche Nebenwirkung bei der Verwendung `%Component ID%` ist, dass der Analytics-Bericht Werte wie `button-2e6d32893a`. Die Verwendung `%Component Title%` würde einen benutzerfreundlicheren Namen geben, aber der Wert könnte nicht eindeutig sein.

1. Fügen Sie anschließend rechts neben dem **Adobe Analytics - Variablen** festlegen eine weitere Aktion hinzu, indem Sie auf das **Pluszeichen** tippen:

   ![hinzufügen einer zusätzlichen Startaktion](assets/track-clicked-component/add-additional-launch-action.png)

1. Legen Sie als **Erweiterungstyp** **Adobe Analytics** fest und legen Sie als **Aktionstyp** den **Beacon**-Senden-Typ fest.
1. Stellen Sie unter **Verfolgung** das Optionsfeld auf **`s.tl()`**.
1. Wählen Sie für **Linktyp** **Benutzerspezifischen Link** und für **Linkname** den Wert: **`%Component Title%: CTA Clicked`**:

   ![Konfiguration für den Beacon &quot;Link senden&quot;](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Dadurch werden die dynamische Variable aus dem Datenelement **Komponententitel** und die statische Zeichenfolge **CTA angeklickt** kombiniert.

1. Speichern Sie die Änderungen. Die **CTA-Regel, auf die geklickt** wurde, sollte jetzt die folgende Konfiguration aufweisen:

   ![Konfiguration des endgültigen Starts](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Hör auf das `cmp:click` Ereignis!
   * **2.** Überprüfen Sie, ob das Ereignis durch einen **Button** oder einen **Teaser** ausgelöst wurde.
   * **3.** Legen Sie Analytics-Variablen für die Verfolgung der **Komponenten-ID** als **eVar**, **Eigenschaft** und **Ereignis** fest.
   * **4.** Senden Sie den Analytics-Beacon &quot;Link verfolgen&quot;(und behandeln Sie ihn **nicht** als Ansicht einer Seite).

1. Speichern Sie alle Änderungen und erstellen Sie Ihre Startbibliothek, indem Sie die entsprechende Umgebung verwenden.

## Validieren des Tracking-Link-Beacon- und Analytics-Aufrufs

Nachdem die **CTA-Regel, auf die geklickt** wurde, den Analytics-Beacon sendet, sollten Sie die Analytics-Verfolgungsvariablen mit dem Experience Platform-Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Symbol Debugger ![Experience Platform Debugger, um den Experience Platform Debugger zu öffnen](assets/track-clicked-component/experience-cloud-debugger.png) .
1. Vergewissern Sie sich, dass der Debugger die Eigenschaft Launch *Ihrer* Development-Umgebung zuordnet, wie zuvor beschrieben und die **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite eingestellt ist.

   ![Analytics-Registerkarten-Debugger](assets/track-clicked-component/analytics-tab-debugger.png)

1. Klicken Sie im Browser auf eine der CTA-Schaltflächen **Teaser** oder **Button** , um zu einer anderen Seite zu navigieren.

   ![CTA-Schaltfläche zum Klicken auf](assets/track-clicked-component/cta-button-to-click.png)

1. Kehren Sie zum Experience Platform-Debugger zurück, blättern Sie nach unten und erweitern Sie **Netzwerkanforderungen** > *Ihre Report Suite*. Sie sollten den **eVar**-, **Prop**- und **Ereignis** -Satz finden können.

   ![Analytics-Ereignis, eVar und prop, die beim Klick verfolgt werden](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Developer Console. Navigieren Sie zur Fußzeile der Site und klicken Sie auf einen der Navigationslinks:

   ![Klicken Sie auf den Link Navigation in der Fußzeile](assets/track-clicked-component/click-navigation-link-footer.png)

1. Beobachten Sie in der Browser-Konsole die Meldung *&quot;Benutzerspezifischer Code&quot;für Regel &quot;CTA angeklickt&quot;wurde nicht erfüllt*.

   Das liegt daran, dass die Navigationskomponente ein `cmp:click` Ereignis auslöst, *aber* wegen unseres Kontrollvorgangs gegen den Ressourcentyp keine Aktion durchgeführt wird.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass im Experience Platform-Debugger unter &quot; **Starten** &quot;die Option &quot; **Konsolenprotokollierung** &quot;aktiviert ist.

## Herzlichen Glückwunsch!

Sie haben soeben die Ereignis-basierte Adobe Client Data Layer und den Experience Platform Launch verwendet, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen.