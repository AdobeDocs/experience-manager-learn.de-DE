---
title: Verwenden der Adobe Client-Datenschicht in Verbindung mit AEM Kernkomponenten
description: Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über ein Besuchererlebnis auf einer Webseite zu erfassen und zu speichern und so den Zugriff auf diese Daten zu erleichtern. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# Verwenden der Adobe Client-Datenschicht in Verbindung mit AEM Kernkomponenten {#overview}

Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über ein Besuchererlebnis auf einer Webseite zu erfassen und zu speichern und so den Zugriff auf diese Daten zu erleichtern. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Möchten Sie die Adobe Client-Datenschicht auf Ihrer AEM aktivieren? [Anweisungen finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Datenschicht durchsuchen

Die integrierte Funktionalität der Client-Datenschicht von Adobe lässt sich einfach mithilfe der Entwicklertools Ihres Browsers und der Live-Umgebung ermitteln. [WKND-Referenz-Site](https://wknd.site/).

>[!NOTE]
>
> Die folgenden Screenshots stammen aus dem Chrome-Browser.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site)
1. Öffnen Sie Ihre Entwickler-Tools und geben Sie den folgenden Befehl in das **Konsole**:

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

1. Führen Sie den Befehl aus `adobeDataLayer.getState()` erneut und suchen Sie nach dem Eintrag für `training-data`.
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

   Der obige Code prüft die `event` -Objekt und verwenden Sie die `adobeDataLayer.getState` -Methode, um den aktuellen Status des Objekts abzurufen, das das Ereignis ausgelöst hat. Die Hilfsmethode prüft dann die `filter` Kriterien und nur, wenn die aktuelle `dataObject` erfüllt den Filter wird zurückgegeben.

   >[!CAUTION]
   >
   > Es ist wichtig **not** , um den Browser während dieser Übung zu aktualisieren, da ansonsten das JavaScript-Konsolen-JavaScript verloren geht.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn eine **Teaser** -Komponente innerhalb einer **Karussell**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Die `teaserShownHandler` ruft die `getDataObjectHelper` -Methode verwenden und einen Filter von `wknd/components/teaser` als `@type` , um Ereignisse herauszufiltern, die von anderen Komponenten ausgelöst wurden.

1. Senden Sie als Nächstes einen Ereignis-Listener auf die Datenschicht, um auf die `cmp:show` -Ereignis.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Die `cmp:show` -Ereignis wird von vielen verschiedenen Komponenten ausgelöst, z. B. wenn eine neue Folie im **Karussell** oder wenn eine neue Registerkarte im **Registerkarte** -Komponente.

1. Auf der Seite können Sie die Karussellfolien umschalten und die Konsolenanweisungen beachten:

   ![Karussell umschalten und Ereignis-Listener anzeigen](assets/teaser-console-slides.png)

1. Entfernen Sie den Ereignis-Listener aus der Datenschicht, um das Listening auf die `cmp:show` event:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Kehren Sie zur Seite zurück und schalten Sie die Karussellfolien um. Beachten Sie, dass keine weiteren Anweisungen protokolliert werden und dass das Ereignis nicht überwacht wird.

1. Erstellen Sie anschließend einen Ereignis-Handler, der aufgerufen wird, wenn das Ereignis &quot;Seite angezeigt&quot;ausgelöst wird:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Beachten Sie, dass der Ressourcentyp `wknd/components/page` wird zum Filtern des Ereignisses verwendet.

1. Senden Sie als Nächstes einen Ereignis-Listener auf die Datenschicht, um auf die `cmp:show` -Ereignis, das die `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Sie sollten sofort eine Konsolenanweisung sehen, die mit den Seitendaten ausgelöst wird:

   ![Seitenanzeigedaten](assets/page-show-console-data.png)

   Die `cmp:show` -Ereignis für die Seite wird bei jedem Laden der Seite am Anfang der Seite ausgelöst. Sie fragen sich vielleicht, warum der Ereignishandler ausgelöst wurde, wenn die Seite eindeutig bereits geladen wurde.

   Dies ist eine der einzigartigen Funktionen der Adobe Client-Datenschicht, mit der Sie Ereignis-Listener registrieren können **before** oder **after** die Datenschicht initialisiert wurde. Dies ist ein wichtiges Merkmal, um Wettlaufsituationen zu vermeiden.

   Die Datenschicht verwaltet ein Warteschlangenarray aller Ereignisse, die nacheinander aufgetreten sind. Auf der Datenschicht werden standardmäßig Ereignisrückrufe für Trigger von Ereignissen durchgeführt, die in der Variablen **vergangene** sowie Ereignissen in **Future**. Es ist möglich, die Ereignisse nach Vergangenheit oder Zukunft zu filtern. [Weitere Informationen finden Sie in der Dokumentation .](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Nächste Schritte

Sehen Sie sich das folgende Tutorial an, um zu erfahren, wie Sie mit der ereignisgesteuerten Adobe Client Data-Schicht [Seitendaten erfassen und an Adobe Analytics senden](../analytics/collect-data-analytics.md).

Oder lernen Sie [Anpassen der Adobe Client-Datenschicht mit AEM Komponenten](./data-layer-customize.md)


## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
