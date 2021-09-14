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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2375'
ht-degree: 4%

---

# Erfassen von Seitendaten mit Adobe Analytics

Erfahren Sie, wie Sie die integrierten Funktionen der [Adobe Client-Datenschicht mit AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de) verwenden können, um Daten zu einer Seite in Adobe Experience Manager Sites zu erfassen. [Experience Platform Launch und die Adobe Analytics-Erweiterung werden verwendet, um Regeln zum Senden von Seitendaten an Adobe Analytics zu erstellen.](https://www.adobe.com/experience-platform/launch.html)[](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)

## Was Sie erstellen werden

![Seitendatenverfolgung](assets/collect-data-analytics/analytics-page-data-tracking.png)

In diesem Tutorial erstellen Sie eine Launch-Regel basierend auf einem Ereignis aus der Adobe Client-Datenschicht, fügen Bedingungen für den Zeitpunkt hinzu, zu dem die Regel ausgelöst werden soll, und senden die **Seitenname** und die **Seitenvorlage** einer AEM an Adobe Analytics.

### Ziele {#objective}

1. Erstellen einer ereignisgesteuerten Regel in Launch basierend auf Änderungen an der Datenschicht
1. Seitendatenschichteigenschaften Datenelementen in Launch zuordnen
1. Erfassen von Seitendaten und Senden an Adobe Analytics mit dem Seitenansichts-Beacon

## Voraussetzungen

Folgendes ist erforderlich:

* **Experience Platform** LaunchProperty
* **Report Suite-ID und Tracking-Server für Adobe** Analytics-Tests/Entwicklung. Weitere Informationen finden Sie in der folgenden Dokumentation für [Erstellen einer neuen Report Suite](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debugger-Browsererweiterung. Screenshots in diesem Tutorial, die aus dem Chrome-Browser erfasst wurden.
* (Optional) AEM Site mit der aktivierten Adobe Client-Datenschicht ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). [ In diesem Tutorial wird die öffentliche Site [https://wknd.site/us/en.html](https://wknd.site/us/en.html) verwendet, Sie können jedoch Ihre eigene Website verwenden.

>[!NOTE]
>
> Benötigen Sie Hilfe bei der Integration von Launch und Ihrer AEM Site? [Siehe diese Videoreihe](../experience-platform-launch/overview.md).

## Wechseln von Launch-Umgebungen für WKND-Site

[https://wknd.](https://wknd.site) siteis ist eine öffentliche Website, die auf  [einem Open-Source-](https://github.com/adobe/aem-guides-wknd) Projekt basiert und als Referenz und  [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) Tutorial für AEM Implementierungen konzipiert ist.

Anstatt eine AEM Umgebung einzurichten und die WKND-Codebasis zu installieren, können Sie den Experience Platform-Debugger zu **switch** the live [https://wknd.site/](https://wknd.site/) zu *your* Launch-Eigenschaft verwenden. Natürlich können Sie Ihre eigene AEM-Site verwenden, wenn die [Adobe Client-Datenschicht bereits aktiviert ist](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Melden Sie sich bei Experience Platform Launch an und erstellen Sie [eine Launch-Eigenschaft](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (falls noch nicht geschehen).
1. Stellen Sie sicher, dass eine erste Launch [Bibliothek](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) erstellt und in eine Launch [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) weitergeleitet wurde.
1. Kopieren Sie den Launch-Einbettungscode aus der Umgebung, in der Ihre Bibliothek veröffentlicht wurde.

   ![Kopieren des eingebetteten Launch-Codes](assets/collect-data-analytics/launch-environment-copy.png)

1. Öffnen Sie in Ihrem Browser eine neue Registerkarte und navigieren Sie zu [https://wknd.site/](https://wknd.site/)
1. Öffnen Sie die Browsererweiterung Experience Platform Debugger .

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigieren Sie zu **Launch** > **Konfiguration** und ersetzen Sie unter **Eingefügte Einbettungscodes** den vorhandenen Launch-Einbettungscode durch *Ihren* -Einbettungscode, der aus Schritt 3 kopiert wurde.

   ![Eingebetteten Code ersetzen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Aktivieren Sie **Konsolenprotokollierung** und **Sperren** den Debugger auf der Registerkarte WKND .

   ![Konsolenprotokollierung](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Überprüfen der Adobe Client-Datenschicht auf der WKND-Site

Das [WKND-Referenzprojekt](https://github.com/adobe/aem-guides-wknd) wurde mit AEM Kernkomponenten erstellt und verfügt standardmäßig über die [Adobe Client-Datenschicht](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Überprüfen Sie als Nächstes, ob die Adobe Client-Datenschicht aktiviert ist.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site).
1. Öffnen Sie die Entwicklertools des Browsers und navigieren Sie zur **Konsole**. Führen Sie den folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

   ![Status der Adobe-Datenschicht](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Erweitern Sie die Antwort und überprüfen Sie den Eintrag `page`. Es sollte ein Datenschema wie das folgende angezeigt werden:

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

   Wir verwenden Standardeigenschaften, die aus dem [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` und `xdm:template` der Datenschicht abgeleitet wurden, um Seitendaten an Adobe Analytics zu senden.

   >[!NOTE]
   >
   > Das JavaScript-Objekt `adobeDataLayer` wird nicht angezeigt? Stellen Sie sicher, dass die Adobe Client Data Layer](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) auf Ihrer Site aktiviert wurde.[

## Erstellen einer Regel &quot;Seite geladen&quot;

Die Adobe Client-Datenschicht ist eine **event**-gesteuerte Datenschicht. Wenn die AEM **Seite**-Datenschicht geladen wird, wird ein -Ereignis `cmp:show` Trigger. Erstellen Sie eine Regel, die basierend auf dem `cmp:show` -Ereignis ausgelöst wird.

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie in der Launch-Benutzeroberfläche zum Abschnitt **Regeln** und klicken Sie dann auf **Neue Regel erstellen**.

   ![Regel erstellen](assets/collect-data-analytics/analytics-create-rule.png)

1. Nennen Sie die Regel **Seite geladen**.
1. Klicken Sie auf **Ereignisse** **Hinzufügen** , um den Assistenten **Ereigniskonfiguration** zu öffnen.
1. Wählen Sie unter **Ereignistyp** **Benutzerspezifischer Code** aus.

   ![Benennen Sie die Regel und fügen Sie das Ereignis für benutzerdefinierten Code hinzu.](assets/collect-data-analytics/custom-code-event.png)

1. Klicken Sie im Hauptbereich auf **Editor** öffnen und geben Sie das folgende Codefragment ein:

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

   Das obige Codefragment fügt einen Ereignis-Listener hinzu, indem [eine Funktion](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) in die Datenschicht gepusht wird. Wenn das `cmp:show`-Ereignis ausgelöst wird, wird die Funktion `pageShownEventHandler` aufgerufen. In dieser Funktion werden einige Integritätsprüfungen hinzugefügt und ein neuer `event` wird mit dem neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente erstellt, die das Ereignis ausgelöst hat.

   Danach wird `trigger(event)` aufgerufen. `trigger()` ist ein reservierter Name in Launch und &quot;Trigger&quot;für die Launch-Regel. Wir übergeben das `event` -Objekt als Parameter, der wiederum durch einen anderen reservierten Namen in Launch namens `event` verfügbar gemacht wird. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen, z. B.: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Klicken Sie dann unter **Aktionen** auf **Hinzufügen** , um den Assistenten **Aktionskonfiguration** zu öffnen.
1. Wählen Sie unter **Aktionstyp** **Benutzerspezifischer Code** aus.

   ![Aktionstyp für benutzerspezifischen Code](assets/collect-data-analytics/action-custom-code.png)

1. Klicken Sie im Hauptbereich auf **Editor** öffnen und geben Sie das folgende Codefragment ein:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   Das `event`-Objekt wird von der `trigger()`-Methode übergeben, die im benutzerdefinierten Ereignis aufgerufen wird. `component` ist die aktuelle Seite, die von der Datenschicht  `getState` im benutzerdefinierten Ereignis abgeleitet wurde. Erinnern Sie sich an das frühere von der Datenschicht angezeigte [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), um die verschiedenen standardmäßig angezeigten Schlüssel anzuzeigen.

1. Speichern Sie die Änderungen und führen Sie einen [Build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) in Launch aus, um den Code in die auf Ihrer AEM Site verwendete [Umgebung](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) weiterzuleiten.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, den [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) zu verwenden, um den Einbettungscode in eine **Entwicklungsumgebung** zu wechseln.

1. Navigieren Sie zu Ihrer AEM-Site und öffnen Sie die Entwickler-Tools, um die Konsole anzuzeigen. Aktualisieren Sie die Seite. Sie sollten sehen, dass die Konsolenmeldungen protokolliert wurden:

   ![Seitengeladene Konsolenmeldungen](assets/collect-data-analytics/page-show-event-console.png)

## Erstellen von Datenelementen

Erstellen Sie anschließend mehrere Datenelemente , um andere Werte als die Adobe Client-Datenschicht zu erfassen. Wie wir in der vorherigen Übung gesehen haben, ist es möglich, direkt über benutzerdefinierten Code auf die Eigenschaften der Datenschicht zuzugreifen. Der Vorteil der Verwendung von Datenelementen besteht darin, dass sie über Launch-Regeln hinweg wiederverwendet werden können.

Erinnern Sie sich an das frühere [Seitenschema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), das von der Datenschicht verfügbar gemacht wird:

Datenelemente werden den Eigenschaften `@type`, `dc:title` und `xdm:template` zugeordnet.

### Komponenten-Ressourcentyp

1. Navigieren Sie zu Experience Platform Launch und zur Webeigenschaft, die mit der AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf **Neues Datenelement erstellen**.
1. Geben Sie für **Name** **Component Resource Type** ein.
1. Wählen Sie für **Datenelementtyp** **Benutzerspezifischer Code** aus.

   ![Komponenten-Ressourcentyp](assets/collect-data-analytics/component-resource-type-form.png)

1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Beachten Sie, dass das `event`-Objekt basierend auf dem Ereignis, das die **Regel** in Launch ausgelöst hat, verfügbar gemacht und in den Gültigkeitsbereich gesetzt wird. Der Wert eines Datenelements wird erst festgelegt, wenn das Datenelement innerhalb einer Regel *referenced* ist. Daher ist es sicher, dieses Datenelement innerhalb einer Regel zu verwenden, wie die Regel **Seite geladen** , die im vorherigen Schritt *erstellt wurde, aber* in anderen Kontexten nicht sicher verwendet werden kann.

### Seitenname

1. Klicken Sie auf **Datenelement** hinzufügen.
1. Geben Sie für **Name** **Seitenname** ein.
1. Wählen Sie für **Datenelementtyp** **Benutzerspezifischer Code** aus.
1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

### Seitenvorlage

1. Klicken Sie auf **Datenelement** hinzufügen.
1. Geben Sie für **Name** **Seitenvorlage** ein.
1. Wählen Sie für **Datenelementtyp** **Benutzerspezifischer Code** aus.
1. Klicken Sie auf **Editor** öffnen und geben Sie Folgendes im Editor für benutzerdefinierten Code ein:

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
1. Gehen Sie zu **Erweiterungen** > **Katalog**
1. Suchen Sie die Erweiterung **Adobe Analytics** und klicken Sie auf **Installieren**

   ![Adobe Analytics-Erweiterung](assets/collect-data-analytics/analytics-catalog-install.png)

1. Geben Sie unter **Bibliotheksverwaltung** > **Report Suites** die Report Suite-IDs ein, die Sie für jede Launch-Umgebung verwenden möchten.

   ![Report Suite-IDs eingeben](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In diesem Tutorial können Sie eine Report Suite für alle Umgebungen verwenden. In der Praxis sollten Sie jedoch separate Report Suites verwenden, wie in der Abbildung unten dargestellt

   >[!TIP]
   >
   >Es wird empfohlen, die Option *Bibliothek für mich verwalten* als Einstellung für die Bibliotheksverwaltung zu verwenden, da es wesentlich einfacher ist, die Bibliothek `AppMeasurement.js` auf dem neuesten Stand zu halten.

1. Aktivieren Sie das Kontrollkästchen, um **Activity Map** zu aktivieren.

   ![Verwenden von Activity Map aktivieren](assets/track-clicked-component/analytic-track-click.png)

1. Geben Sie unter **Allgemein** > **Tracking-Server** Ihren Tracking-Server ein, z. B. `tmd.sc.omtrdc.net`. Geben Sie Ihren SSL-Tracking-Server ein, wenn Ihre Site `https://` unterstützt.

   ![Trackingserver eingeben](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## Hinzufügen einer Bedingung zur Regel &quot;Seite geladen&quot;

Aktualisieren Sie anschließend die Regel **Seite geladen** , um das Datenelement **Komponentenressourcentyp** zu verwenden, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn das `cmp:show`-Ereignis für **Seite** gilt. Andere Komponenten können das `cmp:show`-Ereignis auslösen, z. B. löst die Karussellkomponente es aus, wenn sich die Folien ändern. Daher ist es wichtig, eine Bedingung für diese Regel hinzuzufügen.

1. Navigieren Sie in der Launch-Benutzeroberfläche zur Regel **Seite geladen** , die Sie zuvor erstellt haben.
1. Klicken Sie unter **Conditions** auf **Hinzufügen** , um den Assistenten **Bedingungskonfiguration** zu öffnen.
1. Wählen Sie für **Bedingungstyp** **Wertvergleich** aus.
1. Setzen Sie den ersten Wert im Formularfeld auf `%Component Resource Type%`. Sie können das Datenelementsymbol ![Datenelementsymbol](assets/collect-data-analytics/cylinder-icon.png) verwenden, um das Datenelement **Komponentenressourcentyp** auszuwählen. Belassen Sie den Vergleichssatz auf `Equals`.
1. Setzen Sie den zweiten Wert auf `wknd/components/page`.

   ![Bedingungskonfiguration für Seitenladeregel](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Sie können diese Bedingung innerhalb der benutzerdefinierten Code-Funktion hinzufügen, die auf das zuvor im Tutorial erstellte `cmp:show`-Ereignis wartet. Das Hinzufügen der Regel innerhalb der Benutzeroberfläche bietet zusätzlichen Benutzern mehr Sichtbarkeit, die möglicherweise Änderungen an der Regel vornehmen müssen. Außerdem können wir unser Datenelement verwenden!

1. Speichern Sie die Änderungen.

## Analytics-Variablen und Trigger-Seitenansichts-Beacon festlegen

Derzeit gibt die Regel **Seite geladen** einfach eine Konsolenanweisung aus. Verwenden Sie als Nächstes die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **action** in der Regel **Seite geladen** festzulegen. Wir werden außerdem eine zusätzliche Aktion einrichten, um den **Seitenansichts-Beacon** Trigger und die erfassten Daten an Adobe Analytics zu senden.

1. In der Regel **Seite geladen** **Entfernen** die Aktion **Core - Benutzerspezifischer Code** (die Konsolenanweisungen):

   ![Aktion für benutzerdefinierten Code entfernen](assets/collect-data-analytics/remove-console-statements.png)

1. Klicken Sie unter &quot;Aktionen&quot;auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Setzen Sie den Typ **Erweiterung** auf **Adobe Analytics** und setzen Sie **Aktionstyp** auf **Variablen festlegen**

   ![Aktionserweiterung auf Analytics-Variablen festlegen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Wählen Sie im Hauptbereich ein verfügbares **eVar** aus und legen Sie als Wert des Datenelements **Seitenvorlage** fest. Verwenden Sie das Symbol Datenelemente ![Symbol Datenelemente](assets/collect-data-analytics/cylinder-icon.png), um das Element **Seitenvorlage** auszuwählen.

   ![Als eVar-Seitenvorlage festlegen](assets/collect-data-analytics/set-evar-page-template.png)

1. Scrollen Sie nach unten unter **Weitere Einstellungen** **Seitenname** auf das Datenelement **Seitenname**:

   ![Umgebungsvariablensatz für Seitenname](assets/collect-data-analytics/page-name-env-variable-set.png)

   Speichern Sie die Änderungen.

1. Fügen Sie anschließend eine zusätzliche Aktion rechts neben **Adobe Analytics - Variablen festlegen** hinzu, indem Sie auf das Symbol **plus** tippen:

   ![Hinzufügen einer zusätzlichen Launch-Aktion](assets/collect-data-analytics/add-additional-launch-action.png)

1. Setzen Sie den Typ **Erweiterung** auf **Adobe Analytics** und setzen Sie **Aktionstyp** auf **Beacon senden**. Da dies als Seitenansicht gilt, lassen Sie den standardmäßigen Tracking-Satz auf **`s.t()`**.

   ![Aktion &quot;Beacon Adobe Analytics senden&quot;](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Speichern Sie die Änderungen. Die Regel **Seite geladen** sollte jetzt die folgende Konfiguration aufweisen:

   ![Abschließende Launch-Konfiguration](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Suchen Sie nach dem  `cmp:show` Ereignis.
   * **2.** Überprüfen Sie, ob das Ereignis von einer Seite ausgelöst wurde.
   * **3.** Analytics-Variablen für  **Seitennamen** und  **Seitenvorlage festlegen**
   * **4.** Senden des Analytics-Beacons für die Seitenansicht
1. Speichern Sie alle Änderungen und erstellen Sie Ihre Launch-Bibliothek, indem Sie sie in die entsprechende Umgebung weiterleiten.

## Überprüfen des Seitenansichts-Beacons- und Analytics-Aufrufs

Nachdem die Regel **Seite geladen** das Analytics-Beacon sendet, sollten Sie die Analytics-Tracking-Variablen mit dem Experience Platform Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Debugger-Symbol ![Experience Platform Debugger-Symbol](assets/collect-data-analytics/experience-cloud-debugger.png), um den Experience Platform Debugger zu öffnen.
1. Stellen Sie sicher, dass der Debugger die Launch-Eigenschaft *Ihrer* Entwicklungsumgebung zuordnet, wie zuvor beschrieben, und **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Menü Analytics und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite eingestellt ist. Der Seitenname sollte ebenfalls angegeben werden:

   ![Analytics-Tab-Debugger](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Scrollen Sie nach unten und erweitern Sie **Netzwerkanforderungen**. Sie sollten die **evar**-Einstellung für **Seitenvorlage** finden:

   ![eVar- und Seitenname-Satz](assets/collect-data-analytics/evar-page-name-set.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Entwicklerkonsole. Klicken Sie oben auf der Seite durch das **Karussell**.

   ![Karussellseite durchklicken](assets/collect-data-analytics/click-carousel-page.png)

1. Beobachten Sie in der Browser-Konsole die Konsolenanweisung:

   ![Bedingung nicht erfüllt](assets/collect-data-analytics/condition-not-met.png)

   Dies liegt daran, dass das Karussell ein `cmp:show`-Ereignis *aber* Trigger, da wir den **Komponenten-Ressourcentyp** überprüfen, wird kein Ereignis ausgelöst.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass **Konsolenprotokollierung** im Experience Platform Debugger unter **Launch** aktiviert ist.

1. Navigieren Sie zu einer Artikelseite wie [Westaustralien](https://wknd.site/us/en/magazine/western-australia.html). Beachten Sie, dass sich Seitenname und Vorlagentyp ändern.

## Herzlichen Glückwunsch!

Sie haben soeben die ereignisbasierte Adobe Client Data Layer and Experience Platform Launch verwendet, um Daten von einer AEM Site zu erfassen und an Adobe Analytics zu senden.

### Nächste Schritte

Sehen Sie sich das folgende Tutorial an, um zu erfahren, wie Sie die ereignisgesteuerte Adobe Client Data-Schicht verwenden können, um Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site](track-clicked-component.md) zu verfolgen.[
