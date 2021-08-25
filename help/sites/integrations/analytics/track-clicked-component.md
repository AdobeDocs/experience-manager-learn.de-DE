---
title: Verfolgen geklickter Komponenten mit Adobe Analytics
description: Verwenden Sie die ereignisbasierte Adobe Client Data-Schicht, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie in Experience Platform Launch mithilfe von Regeln auf diese Ereignisse warten und Daten mit einem Verfolgungslink-Beacon an eine Adobe Analytics senden können.
version: cloud-service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1810'
ht-degree: 3%

---


# Verfolgen geklickter Komponenten mit Adobe Analytics

Verwenden Sie die ereignisbasierte [Adobe Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de), um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen. Erfahren Sie, wie Sie Regeln in Experience Platform Launch verwenden, um Klick-Ereignisse zu überwachen, nach Komponenten zu filtern und die Daten mit einem Verfolgungs-Linkbeacon an Adobe Analytics zu senden.

## Was Sie erstellen werden

Das WKND-Marketing-Team möchte wissen, welche Aktionsaufruf-Schaltflächen (CTA) die beste Leistung auf der Startseite erzielen. In diesem Tutorial fügen wir eine neue Regel in Experience Platform Launch hinzu, die auf `cmp:click`-Ereignisse aus **Teaser**- und **Button**-Komponenten wartet und die Komponenten-ID und ein neues Ereignis neben dem Verfolgungslink-Beacon an Adobe Analytics sendet.

![Was Sie für Tracking-Klicks erstellen werden](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Ziele {#objective}

1. Erstellen Sie eine ereignisbasierte Regel in Launch basierend auf dem `cmp:click` -Ereignis.
1. Filtern Sie die verschiedenen Ereignisse nach Komponenten-Ressourcentyp.
1. Legen Sie die angeklickte Komponenten-ID fest und senden Sie das Ereignis Adobe Analytics mit dem Verfolgungslink-Beacon.

## Voraussetzungen

Dieses Tutorial stellt eine Fortsetzung von [Erfassen von Seitendaten mit Adobe Analytics](./collect-data-analytics.md) dar und geht davon aus, dass Sie über Folgendes verfügen:

* Eine **Launch-Eigenschaft** mit der aktivierten [Adobe Analytics-Erweiterung](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)
* **Report Suite-ID und Tracking-Server für Adobe** Analytics-Tests/Entwicklung. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer neuen Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) DebuggerBrowser-Erweiterung, die mit Ihrer Launch-Eigenschaft konfiguriert wurde, die auf  [https://wknd.site/us/en.](https://wknd.site/us/en.html) html oder einer AEM Site geladen wurde, auf der die Adobe-Datenschicht aktiviert ist.

## Inspect des Schaltflächen- und Teaser-Schemas

Bevor Sie Regeln in Launch erstellen, sollten Sie das [Schema für die Schaltfläche und den Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) überprüfen und in der Datenschichtimplementierung überprüfen.

1. Navigieren Sie zu [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zur **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

   ![Status der Datenschicht über die Browser-Konsole](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Erweitern Sie die Antwort und suchen Sie Einträge mit dem Präfix `button-` und `teaser-xyz-cta` . Es sollte ein Datenschema wie das folgende angezeigt werden:

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

   Diese basieren auf dem [Komponenten-/Container-Element-Schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). Die Regel, die wir in Launch erstellen, verwendet dieses Schema.

## Erstellen einer auf CTA geklickten Regel

Die Adobe Client-Datenschicht ist eine **event**-gesteuerte Datenschicht. Wenn auf eine Kernkomponente geklickt wird, wird ein `cmp:click` -Ereignis über die Datenschicht gesendet. Als Nächstes erstellen Sie eine Regel, die auf das `cmp:click` -Ereignis überwacht.

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie in der Launch-Benutzeroberfläche zum Abschnitt **Regeln** und klicken Sie dann auf **Regel hinzufügen**.
1. Nennen Sie die Regel **CTA Clicked**.
1. Klicken Sie auf **Ereignisse** > **Hinzufügen** , um den Assistenten **Ereigniskonfiguration** zu öffnen.
1. Wählen Sie unter **Ereignistyp** **Benutzerspezifischer Code** aus.

   ![Benennen Sie die Regel CTA Angeklickt und fügen Sie das benutzerspezifische Code-Ereignis hinzu.](assets/track-clicked-component/custom-code-event.png)

1. Klicken Sie im Hauptbereich auf **Editor** öffnen und geben Sie das folgende Codefragment ein:

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

   Das obige Codefragment fügt einen Ereignis-Listener hinzu, indem [eine Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht gepusht wird. Wenn das `cmp:click`-Ereignis ausgelöst wird, wird die Funktion `componentClickedHandler` aufgerufen. In dieser Funktion werden einige Integritätsprüfungen hinzugefügt und ein neues `event`-Objekt wird mit dem neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente erstellt, die das Ereignis ausgelöst hat.

   Danach wird `trigger(event)` aufgerufen. `trigger()` ist ein reservierter Name in Launch und &quot;Trigger&quot;für die Launch-Regel. Wir übergeben das `event` -Objekt als Parameter, der wiederum durch einen anderen reservierten Namen in Launch namens `event` verfügbar gemacht wird. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen, z. B.: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Klicken Sie dann unter **Aktionen** auf **Hinzufügen** , um den Assistenten **Aktionskonfiguration** zu öffnen.
1. Wählen Sie unter **Aktionstyp** **Benutzerspezifischer Code** aus.

   ![Aktionstyp für benutzerspezifischen Code](assets/track-clicked-component/action-custom-code.png)

1. Klicken Sie im Hauptbereich auf **Editor** öffnen und geben Sie das folgende Codefragment ein:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   Das `event`-Objekt wird von der `trigger()`-Methode übergeben, die im benutzerdefinierten Ereignis aufgerufen wird. `component` ist der aktuelle Status der Komponente, der von der Datenschicht abgeleitet wurde,  `getState` die den Klick ausgelöst hat.

1. Speichern Sie die Änderungen und führen Sie einen [Build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch aus, um den Code in die auf Ihrer AEM Site verwendete [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) weiterzuleiten.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, den [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) zu verwenden, um den Einbettungscode in eine **Entwicklungsumgebung** zu wechseln.

1. Navigieren Sie zur [WKND-Site](https://wknd.site/us/en.html) und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Wählen Sie **Protokoll beibehalten** aus.

1. Klicken Sie auf eine der Schaltflächen **Teaser** oder **Schaltfläche** CTA , um zu einer anderen Seite zu navigieren.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Beachten Sie in der Entwicklerkonsole, dass die Regel **CTA Clicked** ausgelöst wurde:

   ![CTA-Schaltfläche angeklickt](assets/track-clicked-component/cta-button-clicked-log.png)

## Erstellen von Datenelementen

Erstellen Sie anschließend ein Datenelement , um die Komponenten-ID und den Titel zu erfassen, auf die geklickt wurde. In der vorherigen Übung war die Ausgabe von `event.path` ungefähr `component.button-b6562c963d` und der Wert von `event.component['dc:title']` war ungefähr &quot;Trips anzeigen&quot;.

### Komponenten-ID

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Geben Sie für **Name** **Komponenten-ID** ein.
1. Wählen Sie für **Datenelementtyp** **Benutzerspezifischer Code** aus.

   ![Komponenten-ID-Datenelementformular](assets/track-clicked-component/component-id-data-element.png)

1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Beachten Sie, dass das `event`-Objekt basierend auf dem Ereignis, das die **Regel** in Launch ausgelöst hat, verfügbar gemacht und in den Gültigkeitsbereich gesetzt wird. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement innerhalb einer Regel *referenced* ist. Daher ist es sicher, dieses Datenelement innerhalb einer Regel zu verwenden, wie die Regel **CTA Clicked** , die in der vorherigen Übung erstellt wurde *aber* wäre für die Verwendung in anderen Kontexten nicht sicher.

### Komponentenname

1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf **Neues Datenelement hinzufügen**.
1. Geben Sie für **Name** **Komponententitel** ein.
1. Wählen Sie für **Datenelementtyp** **Benutzerspezifischer Code** aus.
1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

## Hinzufügen einer Bedingung zur Regel &quot;CTA angeklickt&quot;

Aktualisieren Sie anschließend die Regel **CTA Clicked** , um sicherzustellen, dass die Regel nur ausgelöst wird, wenn das `cmp:click`-Ereignis für einen **Teaser** oder eine **Schaltfläche** ausgelöst wird. Da der CTA des Teasers als separates Objekt in der Datenschicht betrachtet wird, ist es wichtig, das übergeordnete Element zu überprüfen, um sicherzustellen, dass es von einem Teaser stammt.

1. Navigieren Sie in der Launch-Benutzeroberfläche zur Regel **CTA Clicked** , die Sie zuvor erstellt haben.
1. Klicken Sie unter **Conditions** auf **Hinzufügen** , um den Assistenten **Bedingungskonfiguration** zu öffnen.
1. Wählen Sie für **Bedingungstyp** **Benutzerspezifischer Code** aus.

   ![CTA angeklickter benutzerdefinierter Code für Bedingung](assets/track-clicked-component/custom-code-condition.png)

1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

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

   Der obige Code prüft zunächst, ob der Ressourcentyp von einer **Schaltfläche** stammt, und prüft dann, ob der Ressourcentyp von einem CTA innerhalb eines **Teasers** stammt.

1. Speichern Sie die Änderungen.

## Festlegen von Analytics-Variablen und Trigger-Tracking-Link-Beacon

Derzeit gibt die Regel **CTA Clicked** einfach eine Konsolenanweisung aus. Verwenden Sie als Nächstes die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **Aktion** festzulegen. Wir werden außerdem eine zusätzliche Aktion einrichten, um den **Link verfolgen** Trigger und die erfassten Daten an Adobe Analytics zu senden.

1. In der Regel **CTA angeklickt** **Entfernen** die Aktion **Core - Benutzerspezifischer Code** (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/track-clicked-component/remove-console-statements.png)

1. Klicken Sie unter &quot;Aktionen&quot;auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Setzen Sie den Typ **Erweiterung** auf **Adobe Analytics** und setzen Sie **Aktionstyp** auf **Variablen festlegen**.

1. Legen Sie die folgenden Werte für **eVars**, **Props** und **Ereignisse** fest:

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Festlegen von eVar-Prop- und -Ereignissen](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Hier wird `%Component ID%` verwendet, da dadurch eine eindeutige Kennung für den angeklickten CTA sichergestellt wird. Ein möglicher Nachteil von `%Component ID%` ist, dass der Analytics-Bericht Werte wie `button-2e6d32893a` enthält. Die Verwendung von `%Component Title%` würde einen benutzerfreundlicheren Namen geben, der Wert ist jedoch möglicherweise nicht eindeutig.

1. Fügen Sie anschließend eine zusätzliche Aktion rechts neben **Adobe Analytics - Variablen festlegen** hinzu, indem Sie auf das Symbol **plus** tippen:

   ![Hinzufügen einer zusätzlichen Launch-Aktion](assets/track-clicked-component/add-additional-launch-action.png)

1. Setzen Sie den Typ **Erweiterung** auf **Adobe Analytics** und setzen Sie **Aktionstyp** auf **Beacon senden**.
1. Legen Sie unter **Tracking** die Optionsschaltfläche auf **`s.tl()`** fest.
1. Wählen Sie für **Link-Typ** **Benutzerspezifischer Link** und für **Linkname** den Wert auf: **`%Component Title%: CTA Clicked`**:

   ![Konfiguration für das Signal &quot;Link senden&quot;](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Dadurch wird die dynamische Variable aus dem Datenelement **Komponententitel** und der statischen Zeichenfolge **CTA angeklickt** kombiniert.

1. Speichern Sie die Änderungen. Die Regel **CTA Clicked** sollte jetzt die folgende Konfiguration aufweisen:

   ![Abschließende Launch-Konfiguration](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Suchen Sie nach dem  `cmp:click` Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis von einem  **** Buttonor- **Teaser** ausgelöst wurde.
   * **3.** Legen Sie Analytics-Variablen für fest, um die  **Komponenten-** ID als  **eVar**,  **Eigenschaft** und  **Ereignis** zu verfolgen.
   * **4.** Senden Sie das Analytics-Beacon &quot;Link verfolgen&quot;(und behandeln Sie es  **** nicht als Seitenansicht).

1. Speichern Sie alle Änderungen und erstellen Sie Ihre Launch-Bibliothek, indem Sie sie in die entsprechende Umgebung weiterleiten.

## Validieren des Verfolgungslink-Beacon- und Analytics-Aufrufs

Nachdem die Regel **CTA angeklickt** das Analytics-Beacon sendet, sollten Sie die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/track-clicked-component/experience-cloud-debugger.png), um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Launch-Eigenschaft *Ihrer* Entwicklungsumgebung zuordnet, wie zuvor beschrieben, und **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Menü Analytics und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite eingestellt ist.

   ![Analytics-Tab-Debugger](assets/track-clicked-component/analytics-tab-debugger.png)

1. Klicken Sie im Browser auf eine der Schaltflächen **Teaser** oder **Button** CTA , um zu einer anderen Seite zu navigieren.

   ![CTA-Schaltfläche zum Klicken](assets/track-clicked-component/cta-button-to-click.png)

1. Kehren Sie zum Experience Platform Debugger zurück, blättern Sie nach unten und erweitern Sie **Netzwerkanfragen** > *Ihre Report Suite*. Sie sollten die Einstellungen **eVar**, **prop** und **event** finden.

   ![Analytics-Ereignisse, eVar und beim Klick verfolgte Eigenschaften](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Navigieren Sie zur Fußzeile der Site und klicken Sie auf einen der Navigationslinks:

   ![Klicken Sie in der Fußzeile auf den Link Navigation .](assets/track-clicked-component/click-navigation-link-footer.png)

1. In der Browser-Konsole wurde die Meldung *&quot;Benutzerdefinierter Code&quot;für Regel &quot;CTA angeklickt&quot;nicht erfüllt*.

   Dies liegt daran, dass die Navigationskomponente einen Trigger mit dem Ereignis `cmp:click` *aber* ausführt, da wir die mit dem Ressourcentyp verknüpfen, wird keine Aktion ausgeführt.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** im Experience Platform Debugger unter **Launch** aktiviert ist.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisgesteuerte Adobe Client Data Layer und den Experience Platform Launch verwendet, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site zu verfolgen.