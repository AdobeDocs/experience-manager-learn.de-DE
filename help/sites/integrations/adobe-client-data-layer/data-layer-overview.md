---
title: Verwenden der Adobe Client-Datenschicht in Verbindung mit AEM Kernkomponenten
description: Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über ein Besuchererlebnis auf einer Webseite zu erfassen und zu speichern und so den Zugriff auf diese Daten zu erleichtern. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.
topic: Integrationen
feature: Adobe Client-Datenschicht, Kernkomponenten
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 8%

---


# Verwenden der Adobe Client-Datenschicht in Verbindung mit AEM Kernkomponenten {#overview}

Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über ein Besuchererlebnis auf einer Webseite zu erfassen und zu speichern und so den Zugriff auf diese Daten zu erleichtern. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Möchten Sie die Adobe Client-Datenschicht auf Ihrer AEM aktivieren? [Die Anweisungen finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Datenschicht durchsuchen

Sie können die integrierte Funktionalität der Adobe Client-Datenschicht einfach mithilfe der Entwicklertools Ihres Browsers und der Live-Referenz-Site [WKND](https://wknd.site/) ermitteln.

>[!NOTE]
>
> Die folgenden Screenshots stammen aus dem Chrome-Browser.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site)
1. Öffnen Sie Ihre Entwicklertools und geben Sie den folgenden Befehl in die **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect die Antwort, um den aktuellen Status der Datenschicht auf einer AEM Site anzuzeigen. Sie sollten Informationen über die Seite und einzelne Komponenten sehen.

   ![Adobe Data Layer Response](assets/data-layer-state-response.png)

1. Pushen Sie ein Datenobjekt in die Datenschicht, indem Sie Folgendes in die Konsole eingeben:

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Führen Sie den Befehl `adobeDataLayer.getState()` erneut aus und suchen Sie nach dem Eintrag für `training-data`.
1. Fügen Sie anschließend einen Pfadparameter hinzu, um nur den spezifischen Status einer Komponente zurückzugeben:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Nur einen einzelnen Komponentendaten-Eintrag zurückgeben](assets/return-just-single-component.png)

## Arbeiten mit Ereignissen

Es empfiehlt sich, benutzerspezifischen Code basierend auf einem Ereignis aus der Datenschicht Trigger. Erkunden Sie als Nächstes die Registrierung und das Listening verschiedener Ereignisse.

1. Geben Sie die folgende Hilfsmethode in Ihre Konsole ein:

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   Der obige Code überprüft das `event`-Objekt und ruft mithilfe der `adobeDataLayer.getState`-Methode den aktuellen Status des Objekts ab, das das Ereignis ausgelöst hat. Die Hilfsmethode prüft dann die `filter`-Kriterien und wird nur zurückgegeben, wenn die aktuelle `dataObject` den Filter erfüllt.

   >[!CAUTION]
   >
   > Es ist wichtig **nicht**, den Browser während dieser Übung zu aktualisieren. Andernfalls geht das Konsolen-JavaScript verloren.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn eine **Teaser**-Komponente in einer **Karussell** angezeigt wird.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Der `teaserShownHandler` ruft die `getDataObjectHelper`-Methode auf und übergibt einen Filter von `wknd/components/teaser` als `@type`, um Ereignisse herauszufiltern, die von anderen Komponenten ausgelöst werden.

1. Als Nächstes pushen Sie einen Ereignis-Listener auf die Datenschicht, um auf das `cmp:show` -Ereignis zu warten.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Das `cmp:show`-Ereignis wird von vielen verschiedenen Komponenten ausgelöst, z. B. wenn eine neue Folie in **Karussell** angezeigt wird oder wenn in der Komponente **Tab** eine neue Registerkarte ausgewählt ist.

1. Auf der Seite können Sie die Karussellfolien umschalten und die Konsolenanweisungen beachten:

   ![Karussell umschalten und Ereignis-Listener anzeigen](assets/teaser-console-slides.png)

1. Entfernen Sie den Ereignis-Listener aus der Datenschicht, um die Überwachung auf das `cmp:show` -Ereignis zu beenden:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Kehren Sie zur Seite zurück und schalten Sie die Karussellfolien um. Beachten Sie, dass keine weiteren Anweisungen protokolliert werden und dass das Ereignis nicht überwacht wird.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn das angezeigte Seitenereignis ausgelöst wird:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Beachten Sie, dass der Ressourcentyp `wknd/components/page` zum Filtern des Ereignisses verwendet wird.

1. Als Nächstes pushen Sie einen Ereignis-Listener auf die Datenschicht, um auf das `cmp:show` -Ereignis zu warten, und rufen Sie `pageShownHandler` auf.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Sie sollten sofort eine Konsolenanweisung sehen, die mit den Seitendaten ausgelöst wird:

   ![Seitenanzeigedaten](assets/page-show-console-data.png)

   Das `cmp:show`-Ereignis für die Seite wird bei jedem Laden der Seite ganz oben auf der Seite ausgelöst. Sie fragen sich vielleicht, warum der Ereignishandler ausgelöst wurde, wenn die Seite eindeutig bereits geladen wurde.

   Dies ist eine der einzigartigen Funktionen der Adobe Client-Datenschicht, da Sie Ereignis-Listener **vor** oder **registrieren können, nachdem** die Datenschicht initialisiert wurde. Dies ist ein wichtiges Merkmal, um Wettlaufsituationen zu vermeiden.

   Die Datenschicht verwaltet ein Warteschlangenarray aller Ereignisse, die nacheinander aufgetreten sind. Die Datenschicht Trigger standardmäßig Ereignisrückrufe für Ereignisse, die in **after** aufgetreten sind, sowie Ereignisse in **future**. Es ist möglich, die Ereignisse nach Vergangenheit oder Zukunft zu filtern. [Weitere Informationen finden Sie in der Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Nächste Schritte

Sehen Sie sich das folgende Tutorial an, um zu erfahren, wie Sie die ereignisgesteuerte Adobe Client Data Layer verwenden, um [Seitendaten zu erfassen und an Adobe Analytics](../analytics/collect-data-analytics.md) zu senden.

Oder erfahren Sie, wie Sie die Adobe Client-Datenschicht mit AEM Komponenten ](./data-layer-customize.md) anpassen.[


## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
