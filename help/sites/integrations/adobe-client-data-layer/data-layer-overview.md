---
title: Verwenden der Adobe Client-Datenschicht in Verbindung mit den (AEM) Kernkomponenten
description: Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über das Erlebnis eines Besuchers auf einer Webseite zu erfassen und zu speichern und den Zugriff auf diese Daten zu vereinfachen. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 55%

---

# Verwenden der Adobe Client-Datenschicht in Verbindung mit den (AEM) Kernkomponenten {#overview}

Die Adobe Client-Datenschicht führt eine Standardmethode ein, um Daten über das Erlebnis eines Besuchers auf einer Webseite zu erfassen und zu speichern und den Zugriff auf diese Daten zu vereinfachen. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Möchten Sie die Adobe Client-Datenschicht auf Ihrer AEM-Site aktivieren? [Anweisungen finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de#installation-activation).

## Erkunden der Datenschicht

Sie können sich einen Eindruck von der eingebauten Funktionalität der Client-Datenschicht von Adobe verschaffen, indem Sie einfach die Entwickler-Tools Ihres Browsers und die Live [WKND-Referenzseite](https://wknd.site/us/en.html) verwenden.

>[!NOTE]
>
> Die folgenden Screenshots stammen aus dem Chrome-Browser.

1. Navigieren Sie zu [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Öffnen Sie Ihre Entwicklertools und geben Sie den folgenden Befehl in die **Konsole**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Um den aktuellen Status der Datenschicht auf einer AEM Site anzuzeigen, überprüfen Sie die Antwort. Sie sollten Informationen über die Seite und einzelne Komponenten sehen.

   ![Adobe Datenschicht-Antwort](assets/data-layer-state-response.png)

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
1. Als Nächstes fügen Sie einen Pfadparameter hinzu, um nur den spezifischen Status einer Komponente zurückzugeben:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Nur einen einzelnen Komponentendaten-Eintrag zurückgeben](assets/return-just-single-component.png)

## Arbeiten mit Ereignissen

Es empfiehlt sich, jeden benutzerdefinierten Code auf der Grundlage eines Ereignisses aus der Datenschicht auszulösen. Erkunden Sie als Nächstes die Registrierung und das Listening verschiedener Ereignisse.

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

   Der obige Code prüft die `event` -Objekt und verwendet die `adobeDataLayer.getState` -Methode, um den aktuellen Status des Objekts abzurufen, das das Ereignis ausgelöst hat. Anschließend überprüft die Hilfsmethode die `filter` und nur, wenn die aktuelle `dataObject` erfüllt die Filterkriterien, die zurückgegeben werden.

   >[!CAUTION]
   >
   > Es ist wichtig **nicht** den Browser während dieser Übung zu aktualisieren, da sonst das Konsolen-JavaScript verloren geht.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn eine **Teaser**-Komponente innerhalb eines **Karussells** aufgerufen wird.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Die `teaserShownHandler` -Funktion die `getDataObjectHelper` -Funktion und übergibt einen Filter von `wknd/components/teaser` als `@type` , um Ereignisse herauszufiltern, die von anderen Komponenten ausgelöst wurden.

1. Pushen Sie als Nächstes einen Ereignis-Listener auf die Datenschicht, um auf das Ereignis `cmp:show` zu warten.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Die `cmp:show` -Ereignis wird von vielen verschiedenen Komponenten ausgelöst, z. B. wenn eine neue Folie im **Karussell** oder wenn eine neue Registerkarte im **Registerkarte** -Komponente.

1. Schalten Sie auf der Seite die Karussellfolien um und beachten Sie die Konsolenanweisungen:

   ![Karussell umschalten und Ereignis-Listener anzeigen](assets/teaser-console-slides.png)

1. So hören Sie nicht mehr auf `cmp:show` -Ereignis, entfernen Sie den Ereignis-Listener aus der Datenschicht

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Kehren Sie zur Seite zurück und schalten Sie die Karussellfolien um. Beachten Sie, dass keine weiteren Anweisungen protokolliert werden und dass das Ereignis nicht überwacht wird.

1. Erstellen Sie anschließend einen Ereignis-Handler, der aufgerufen wird, wenn das Ereignis „Seite angezeigt“ ausgelöst wird:

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

1. Pushen Sie als Nächstes einen Ereignis-Listener auf die Datenschicht, um auf das Ereignis `cmp:show` zu warten, und rufen die `pageShownHandler` auf.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Sie sollten sofort eine Konsolenanweisung sehen, die mit den Seitendaten ausgelöst wird:

   ![Seitenanzeigedaten](assets/page-show-console-data.png)

   Die `cmp:show` -Ereignis für die Seite wird bei jedem Laden der Seite am Anfang der Seite ausgelöst. Sie fragen sich vielleicht, warum der Ereignis-Handler ausgelöst wurde, wenn die Seite bereits eindeutig geladen ist.

   Eine der einzigartigen Funktionen der Adobe Client-Datenschicht besteht darin, Ereignislistener zu registrieren **before** oder **after** Wenn die Datenschicht initialisiert wurde, hilft dies, die Wettlaufsituationen zu vermeiden.

   Die Datenschicht verwaltet ein Warteschlangenarray aller Ereignisse, die nacheinander aufgetreten sind. Auf der Datenschicht werden standardmäßig Ereignisrückrufe für Trigger durchgeführt, die in **vergangene** und -Ereignissen in **Future**. Es ist möglich, die Ereignisse aus der Vergangenheit oder der Zukunft zu filtern. [Weitere Informationen finden Sie in der Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Nächste Schritte

Es gibt zwei Möglichkeiten, das Lernen fortzusetzen. Zunächst sehen Sie sich die [Seitendaten erfassen und an Adobe Analytics senden](../analytics/collect-data-analytics.md) Tutorial, das die Verwendung der Adobe Client-Datenschicht veranschaulicht. Die zweite Möglichkeit ist, zu erfahren, wie man [Anpassen der Adobe Client-Datenschicht mit AEM Komponenten](./data-layer-customize.md)


## Zusätzliche Ressourcen {#additional-resources}

* [Adobe Client-Datenschicht-Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de)
