---
title: Seitendaten mit Adobe Analytics erfassen
description: Verwenden Sie die Ereignis-basierte Adobe Client Data-Ebene, um Daten zur Aktivität von Benutzern auf einer mit Adobe Experience Manager erstellten Website zu erfassen. Erfahren Sie, wie Sie in Experience Platform Launch Regeln verwenden, um auf diese Ereignis zu hören und Daten an eine Adobe Analytics Report Suite zu senden.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 97fe98c8c62f5472f7771bbc803b2a47dc97044d
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 4%

---


# Seitendaten mit Adobe Analytics erfassen

Erfahren Sie, wie Sie die integrierten Funktionen der [Adobe Client Data Layer mit AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html) verwenden können, um Daten zu einer Seite in Adobe Experience Manager Sites zu erfassen. [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) und die [Adobe Analytics-Erweiterung](https://docs.adobe.com/content/help/de-DE/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) werden verwendet, um Regeln zum Senden von Seitendaten an Adobe Analytics zu erstellen.

## Was Sie erstellen

![Seitendatenverfolgung](assets/collect-data-analytics/analytics-page-data-tracking.png)

In diesem Lernprogramm lösen Sie eine auf einem Ereignis aus der Adobe Client Data Layer basierende Startregel aus, fügen Bedingungen für den Zeitpunkt hinzu, zu dem die Regel ausgelöst werden soll, und senden den **Seitennamen** und die **Seitenvorlage** einer AEM an Adobe Analytics.

### Ziele {#objective}

1. Erstellen einer Ereignis-basierten Regel beim Start basierend auf Änderungen an der Datenschicht
1. Seitendatenschichteigenschaften Datenelementen beim Start zuordnen
1. Seitendaten erfassen und mit dem Beacon für die Ansicht an Adobe Analytics senden

## Voraussetzungen

Folgendes ist erforderlich:

* **Experience Platform Launch** -Eigenschaft
* **Adobe Analytics** -Test-/Entwicklungs-Report Suite-ID und Tracking-Server. Informationen zum [Erstellen einer neuen Report Suite](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)finden Sie in der folgenden Dokumentation.
* [Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Browsererweiterung. Screenshots in diesem Tutorial, die aus dem Chrome-Browser erfasst wurden.
* (Optional) AEM Site mit aktivierter [Adobe Client Data Layer](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Dieses Tutorial wird die öffentlich zugängliche Website [https://wknd.site/us/en.html](https://wknd.site/us/en.html) verwenden, aber Sie können gerne Ihre eigene Website verwenden.

>[!NOTE]
>
> Benötigen Sie Hilfe bei der Integration von Launch und Ihrer AEM Site? [Siehe diese Videoreihe](../experience-platform-launch/overview.md).

## Umgebung für den Start der WKND-Site wechseln

[https://wknd.site](https://wknd.site) ist eine öffentlich zugängliche Website, die auf [einem Open-Source-Projekt](https://github.com/adobe/aem-guides-wknd) basiert und als Referenz und [Lernprogramm](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) für AEM Implementierungen konzipiert wurde.

Anstatt eine AEM Umgebung einzurichten und die WKND-Codebasis zu installieren, können Sie mit dem Experience Platform-Debugger die Live- **https://wknd.site/** zu [Ihrer](https://wknd.site/) Launch-Eigenschaft *wechseln* . Natürlich können Sie Ihre eigene AEM verwenden, wenn die [Adobe Client Data Layer bereits aktiviert ist](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Melden Sie sich bei Experience Platform Launch an und [erstellen Sie eine Starteigenschaft](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (falls noch nicht geschehen).
1. Stellen Sie sicher, dass eine anfängliche [Startbibliothek erstellt](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) und in eine Launch- [Umgebung](https://docs.adobe.com/content/help/de-DE/launch/using/reference/publish/environments.html)umgewandelt wurde.
1. Kopieren Sie den Einbettungscode &quot;Start&quot;aus der Umgebung, in der Ihre Bibliothek veröffentlicht wurde.

   ![Einbettungscode kopieren](assets/collect-data-analytics/launch-environment-copy.png)

1. Öffnen Sie im Browser eine neue Registerkarte und navigieren Sie zu [https://wknd.site/](https://wknd.site/)
1. Öffnen Sie die Browsererweiterung &quot;Experience Platform Debugger&quot;

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navigieren Sie zu **Start** > **Konfiguration** und ersetzen Sie unter **Einbettungscodes** den vorhandenen Einbettungscode starten durch *Ihren* Einbettungscode, der aus Schritt 3 kopiert wurde.

   ![Einbettungscode ersetzen](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Aktivieren Sie **Konsolenprotokollierung** und **sperren** Sie den Debugger auf der Registerkarte WKND.

   ![Konsolenprotokollierung](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Adobe Client-Datenschicht auf WKND-Site überprüfen

Das [WKND-Referenzprojekt](https://github.com/adobe/aem-guides-wknd) wurde mit AEM Kernkomponenten erstellt und die [Adobe Client Data Layer ist standardmäßig aktiviert](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) . Überprüfen Sie anschließend, ob die Adobe Client Data Layer aktiviert ist.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site).
1. Öffnen Sie die Entwicklerwerkzeuge des Browsers und navigieren Sie zur **Konsole**. Führen Sie folgenden Befehl aus:

   ```js
   adobeDataLayer.getState();
   ```

   Dadurch wird der aktuelle Status der Client-Datenschicht der Adobe zurückgegeben.

   ![Status der Adobe-Datenschicht](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Erweitern Sie die Antwort und überprüfen Sie den `page` Eintrag. Es sollte ein Schema wie das folgende angezeigt werden:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Wir verwenden Standardeigenschaften, die vom [Seitenelement](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title``xdm:language` `xdm:template` undder Datenschicht abgeleitet wurden, um Seitendaten an Adobe Analytics zu senden.

   >[!NOTE]
   >
   > Sehen Sie nicht das `adobeDataLayer` JavaScript-Objekt? Stellen Sie sicher, dass die [Adobe Client-Datenschicht auf Ihrer Site aktiviert](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) wurde.

## Eine Regel zum Laden einer Seite erstellen

Die Adobe Client Data Layer ist eine vom **Ereignis** gesteuerte Datenschicht. Wenn die AEM **Seitendatenschicht** geladen wird, löst sie ein Ereignis aus `cmp:show`. Erstellen Sie eine Regel, die basierend auf dem `cmp:show` Ereignis ausgelöst wird.

1. Navigieren Sie zum Experience Platform Launch und zur Webeigenschaft, die in die AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Regeln** in der Benutzeroberfläche &quot;Start&quot;und klicken Sie dann auf Neue Regel **erstellen**.

   ![Regel erstellen](assets/collect-data-analytics/analytics-create-rule.png)

1. Benennen Sie die Regel **Seite geladen**.
1. Klicken Sie auf **Ereignisse** **Hinzufügen** , um den Assistenten zur **Ereignis-Konfiguration** zu öffnen.
1. Wählen Sie unter **Ereignistyp** die Option **Benutzerdefinierter Code**.

   ![Benennen Sie die Regel und fügen Sie das benutzerdefinierte Code-Ereignis hinzu](assets/collect-data-analytics/custom-code-event.png)

1. Klicken Sie im Hauptbereich auf &quot;Editor **öffnen** &quot;und geben Sie das folgende Codefragment ein:

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

   Das obige Codefragment fügt einen Ereignis-Listener hinzu, indem eine Funktion [in die Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) gedrängt wird. Wenn das `cmp:show` Ereignis ausgelöst wird, wird die `pageShownEventHandler` Funktion aufgerufen. In dieser Funktion werden einige Sanitätsprüfungen hinzugefügt und ein neuer `event` wird mit dem neuesten [Status der Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) für die Komponente erstellt, die das Ereignis ausgelöst hat.

   Danach `trigger(event)` wird gerufen. `trigger()` ist ein reservierter Name in Launch und löst die Launch-Regel aus. Wir übergeben das `event` Objekt als Parameter, der wiederum durch einen anderen reservierten Namen in Launch namens `event`. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen: `event.component['someKey']`.

1. Speichern Sie die Änderungen.
1. Klicken Sie anschließend unter **Aktionen** auf **Hinzufügen** , um den Assistenten zur **Aktionskonfiguration** zu öffnen.
1. Wählen Sie unter **Aktionstyp** die Option **Benutzerdefinierter Code**.

   ![Aktionstyp für benutzerspezifischen Code](assets/collect-data-analytics/action-custom-code.png)

1. Klicken Sie im Hauptbereich auf &quot;Editor **öffnen** &quot;und geben Sie das folgende Codefragment ein:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   Das `event` Objekt wird von der `trigger()` Methode übergeben, die im benutzerdefinierten Ereignis aufgerufen wird. `component` ist die aktuelle Seite, die von der Datenschicht `getState` im benutzerdefinierten Ereignis abgeleitet wird. Rufen Sie das von der Datenschicht offen gelegte [Seitenelement](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) von vornherein auf, um die verschiedenen Tasten standardmäßig anzuzeigen.

1. Speichern Sie die Änderungen und führen Sie einen [Build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) in Launch aus, um den Code für die auf Ihrer AEM Site verwendete [Umgebung](https://docs.adobe.com/content/help/de-DE/launch/using/reference/publish/environments.html) zu bewerben.

   >[!NOTE]
   >
   > Es kann sehr nützlich sein, den [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) zu verwenden, um den Einbettungscode in eine **Entwicklungs** -Umgebung umzuschalten.

1. Navigieren Sie zu Ihrer AEM Website und öffnen Sie die Entwicklerwerkzeuge, um die Konsole Ansicht. Aktualisieren Sie die Seite und Sie sollten sehen, dass die Konsolenmeldungen protokolliert wurden:

   ![Seitenladeseiten - Konsolenmeldungen](assets/collect-data-analytics/page-show-event-console.png)

## Datenelemente erstellen

Anschließend erstellen Sie mehrere Datenelemente, um verschiedene Werte aus der Adobe Client Data Layer zu erfassen. Wie in der vorherigen Übung gesehen haben wir gesehen, ist es möglich, direkt über benutzerspezifischen Code auf die Eigenschaften der Datenschicht zuzugreifen. Der Vorteil bei der Verwendung von Datenelementen besteht darin, dass sie in allen Startregeln wiederverwendet werden können.

Erinnern Sie sich an das von der Datenschicht offen gelegte [Seitenelement](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) :

Datenelemente werden den Eigenschaften `@type`, `dc:title`und `xdm:template` zugeordnet.

### Komponentenressourcentyp

1. Navigieren Sie zum Experience Platform Launch und zur Webeigenschaft, die in die AEM Site integriert ist.
1. Navigieren Sie zum Abschnitt **Datenelemente** und klicken Sie auf Neues Datenelement **erstellen**.
1. Geben Sie als **Name** den **Komponentenressource-Typ** ein.
1. Wählen Sie für **Datenelementtyp** &quot; **Benutzerdefinierter Code**&quot;aus.

   ![Komponentenressourcentyp](assets/collect-data-analytics/component-resource-type-form.png)

1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Speichern Sie die Änderungen.

   >[!NOTE]
   >
   > Denken Sie daran, dass das `event` Objekt auf der Grundlage des Ereignisses, das die **Regel** beim Start ausgelöst hat, verfügbar gemacht und in Scopes dargestellt wird. Der Wert eines Datenelements wird erst festgelegt, wenn innerhalb einer Regel auf das Datenelement *verwiesen* wird. Daher ist es sicher, dieses Datenelement innerhalb einer Regel wie die im vorherigen Schritt erstellte Regel &quot; **Seitenladevorgang** &quot;zu verwenden, *aber* in anderen Kontexten nicht sicher zu verwenden.

### Seitenname

1. Klicken Sie auf **Hinzufügen Datenelement**.
1. Geben Sie als **Name** den **Seitennamen** ein.
1. Wählen Sie für **Datenelementtyp** &quot; **Benutzerdefinierter Code**&quot;aus.
1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Speichern Sie die Änderungen.

### Seitenvorlage

1. Klicken Sie auf **Hinzufügen Datenelement**.
1. Geben Sie als **Name** den **Seitennamen** ein.
1. Wählen Sie für **Datenelementtyp** &quot; **Benutzerdefinierter Code**&quot;aus.
1. Klicken Sie auf Editor **öffnen** und geben Sie Folgendes im Editor für benutzerspezifischen Code ein:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Speichern Sie die Änderungen.

1. Sie sollten jetzt drei Datenelemente als Teil Ihrer Regel haben:

   ![Datenelemente in der Regel](assets/collect-data-analytics/data-elements-page-rule.png)

## hinzufügen der Analytics-Erweiterung

Fügen Sie dann der Eigenschaft &quot;Start&quot;die Analytics-Erweiterung hinzu. Wir müssen diese Daten irgendwo senden!

1. Navigieren Sie zum Experience Platform Launch und zur Webeigenschaft, die in die AEM Site integriert ist.
1. Zu **Erweiterungen** > **Katalog**
1. Suchen Sie die **Adobe Analytics** -Erweiterung und klicken Sie auf **Installieren**

   ![Adobe Analytics Extension](assets/collect-data-analytics/analytics-catalog-install.png)

1. Geben Sie unter **Bibliotheksverwaltung** > **Report Suites** die Report Suite-IDs ein, die Sie für jede Launch-Umgebung verwenden möchten.

   ![Report Suite-IDs eingeben](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > In diesem Lernprogramm können Sie eine Report Suite für alle Umgebung verwenden. In Wirklichkeit sollten Sie jedoch separate Report Suites verwenden, wie in der folgenden Abbildung dargestellt

   >[!TIP]
   >
   >Es wird empfohlen, die Option *&quot;Bibliothek für mich* verwalten&quot;als Einstellung &quot;Bibliotheksverwaltung&quot;zu verwenden, da die `AppMeasurement.js` Bibliothek dadurch leichter auf dem neuesten Stand gehalten werden kann.

1. Geben Sie unter **Allgemein** > **Tracking-Server** Ihren Tracking-Server ein, z. `tmd.sc.omtrdc.net`. Geben Sie den SSL-Tracking-Server ein, wenn Ihre Site `https://`

   ![Tracking-Server eingeben](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## hinzufügen einer Bedingung für die Regel &quot;Seite geladen&quot;

Aktualisieren Sie anschließend die **Seitenladeregel** , um das Datenelement &quot; **Komponentenressource-Typ** &quot;zu verwenden, um sicherzustellen, dass die Regel nur ausgelöst wird, wenn das `cmp:show` Ereignis für die **Seite** gilt. Andere Komponenten können das `cmp:show` Ereignis auslösen, beispielsweise wird es von der Karussell-Komponente ausgelöst, wenn sich die Folien ändern. Daher ist es wichtig, eine Bedingung für diese Regel hinzuzufügen.

1. Navigieren Sie in der Benutzeroberfläche &quot;Starten&quot;zu der zuvor erstellten Regel &quot; **Seite geladen** &quot;.
1. Klicken Sie unter **Bedingungen** auf **Hinzufügen** , um den Assistenten zur **Bedingungskonfiguration** zu öffnen.
1. Wählen Sie unter &quot; **Bedingungstyp** &quot; **Wertvergleich**.
1. Legen Sie als ersten Wert im Formularfeld `%Component Resource Type%`fest. Sie können das Datenelementsymbol ![Datenelement-Symbol](assets/collect-data-analytics/cylinder-icon.png) verwenden, um das Datenelement **Komponentenressource-Typ** auszuwählen. Lassen Sie den Komparator auf `Equals`.
1. Setzen Sie den zweiten Wert auf `wknd/components/page`.

   ![Bedingungskonfiguration für Seitenladeregel](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > Sie können diese Bedingung in die benutzerdefinierte Codefunktion einfügen, die auf das zuvor im Lernprogramm erstellte `cmp:show` Ereignis überwacht. Das Hinzufügen dieser Funktion in der Benutzeroberfläche ermöglicht jedoch zusätzliche Sichtbarkeit für weitere Benutzer, die möglicherweise Änderungen an der Regel vornehmen müssen. Außerdem haben wir unser Datenelement benutzt!

1. Speichern Sie die Änderungen.

## Festlegen von Analytics-Variablen und Auslösen des Beacons für die Ansicht von Seiten

Derzeit gibt die Regel **Seitenladevorgang** einfach eine Konsolenanweisung aus. Als Nächstes verwenden Sie die Datenelemente und die Analytics-Erweiterung, um Analytics-Variablen als **Aktion** in der Regel &quot; **Seite geladen** &quot;festzulegen. Außerdem werden wir eine zusätzliche Aktion einrichten, um den Beacon für die **SeitenAnsicht** auszulösen und die erfassten Daten an Adobe Analytics zu senden.

1. Entfernen Sie in der Regel **Seitenladevorgang** die Aktion **Core - Benutzerdefinierter Code** (die Konsolenanweisungen) **aus** :

   ![Aktion für benutzerdefinierten Code entfernen](assets/collect-data-analytics/remove-console-statements.png)

1. Klicken Sie unter Aktionen auf **Hinzufügen** , um eine neue Aktion hinzuzufügen.
1. Legen Sie den **Erweiterungstyp** auf **Adobe Analytics** fest und legen Sie als **Aktionstyp** Variablen **festlegen fest.**

   ![Aktionserweiterung auf Analytics-Set-Variablen festlegen](assets/collect-data-analytics/analytics-set-variables-action.png)

1. Wählen Sie im Hauptbereich eine verfügbare **eVar** aus und legen Sie als Wert der **Seitenvorlage** des Datenelements fest. Verwenden Sie das Symbol ![Datenelemente](assets/collect-data-analytics/cylinder-icon.png) , um das Element **Seitenvorlage** auszuwählen.

   ![Als eVar-Seitenvorlage festlegen](assets/collect-data-analytics/set-evar-page-template.png)

1. Blättern Sie nach unten und legen Sie unter **Zusätzliche Einstellungen** den **Seitennamen** auf den **Seitennamen** des Datenelements fest:

   ![Umgebung der Seitennamen](assets/collect-data-analytics/page-name-env-variable-set.png)

   Speichern Sie die Änderungen.

1. Fügen Sie anschließend rechts neben dem **Adobe Analytics - Variablen** festlegen eine weitere Aktion hinzu, indem Sie auf das **Pluszeichen** tippen:

   ![hinzufügen einer zusätzlichen Startaktion](assets/collect-data-analytics/add-additional-launch-action.png)

1. Legen Sie als **Erweiterungstyp** **Adobe Analytics** fest und legen Sie als **Aktionstyp** den **Beacon**-Senden-Typ fest. Da dies als Ansicht der Seite gilt, belassen Sie den Standard-Verfolgungssatz auf **`s.t()`**.

   ![Beacon Adobe Analytics-Aktion senden](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Speichern Sie die Änderungen. Die Regel &quot; **Seite geladen** &quot;sollte jetzt die folgende Konfiguration aufweisen:

   ![Konfiguration des endgültigen Starts](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Hör auf das `cmp:show` Ereignis!
   * **2.** Überprüfen Sie, ob das Ereignis durch eine Seite ausgelöst wurde.
   * **3.** Festlegen von Analytics-Variablen für **Seitennamen** und **Seitenvorlage**
   * **4.** Beacon für die Ansicht der Analytics-Seite senden
1. Speichern Sie alle Änderungen und erstellen Sie Ihre Startbibliothek, indem Sie die entsprechende Umgebung verwenden.

## Validieren des Beacon- und Analytics-Aufrufs für die Ansicht der Seite

Nachdem die **Seitenladeregel** nun den Analytics-Beacon sendet, sollten Sie die Analytics-Verfolgungsvariablen mit dem Experience Platform-Debugger sehen können.

1. Öffnen Sie die [WKND-Site](https://wknd.site/us/en.html) in Ihrem Browser.
1. Klicken Sie auf das Symbol Debugger ![Experience Platform Debugger, um den Experience Platform Debugger zu öffnen](assets/collect-data-analytics/experience-cloud-debugger.png) .
1. Vergewissern Sie sich, dass der Debugger die Eigenschaft Launch *Ihrer* Development-Umgebung zuordnet, wie zuvor beschrieben und die **Konsolenprotokollierung** aktiviert ist.
1. Öffnen Sie das Analytics-Menü und überprüfen Sie, ob die Report Suite auf *Ihre* Report Suite eingestellt ist. Der Seitenname sollte ebenfalls gefüllt werden:

   ![Analytics-Registerkarten-Debugger](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Blättern Sie nach unten und erweitern Sie **Netzwerkanforderungen**. Sie sollten den **evar** -Satz für die **Seitenvorlage** suchen können:

   ![eVar- und Seitenname-Satz](assets/collect-data-analytics/evar-page-name-set.png)

1. Kehren Sie zum Browser zurück und öffnen Sie die Developer Console. Klicken Sie oben auf der Seite durch das **Karussell** .

   ![Durchklicken der Karussellseite](assets/collect-data-analytics/click-carousel-page.png)

1. Beobachten Sie in der Browser-Konsole die Konsolenanweisung:

   ![Bedingung nicht erfüllt](assets/collect-data-analytics/condition-not-met.png)

   Das liegt daran, dass das Karussell ein `cmp:show` Ereignis auslöst, *aber* aufgrund der Überprüfung des **Komponentenressource-Typs** kein Ereignis ausgelöst wird.

   >[!NOTE]
   >
   > Wenn keine Konsolenprotokolle angezeigt werden, stellen Sie sicher, dass im Experience Platform-Debugger unter &quot; **Starten** &quot;die Option &quot; **Konsolenprotokollierung** &quot;aktiviert ist.

1. Navigieren Sie zu einer Artikelseite wie [Westaustralien](https://wknd.site/us/en/magazine/western-australia.html). Achten Sie auf die Änderung von Seitenname und Vorlagentyp.

## Herzlichen Glückwunsch!

Sie haben soeben die Ereignis-basierte Adobe Client Data Layer und den Experience Platform Launch verwendet, um Daten von einer AEM Site zu erfassen und an Adobe Analytics zu senden.

### Nächste Schritte

Sehen Sie sich das folgende Lernprogramm an, um zu erfahren, wie Sie mit der Ereignis-basierten Adobe-Client-Datenschicht Klicks auf bestimmte Komponenten auf einer Adobe Experience Manager-Site [verfolgen können](track-clicked-component.md).
