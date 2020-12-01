---
title: Verwenden der Adobe Client-Datenschicht mit AEM Kernkomponenten
description: Die Adobe Client Data Layer bietet eine Standardmethode zum Erfassen und Speichern von Daten zu Besuchern auf einer Webseite und erleichtert den Zugriff auf diese Daten. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 8%

---


# Verwenden der Adobe Client-Datenschicht mit AEM Kernkomponenten {#overview}

Die Adobe Client Data Layer bietet eine Standardmethode zum Erfassen und Speichern von Daten zu Besuchern auf einer Webseite und erleichtert den Zugriff auf diese Daten. Die Adobe Client-Datenschicht ist plattformunabhängig, aber für die Verwendung mit AEM vollständig in die Kernkomponenten integriert.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Möchten Sie die Adobe Client Data Layer auf Ihrer AEM Site aktivieren? [Siehe die Anweisungen hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Datenschicht

Sie können sich die integrierte Funktionalität der Adobe Client Data Layer mit den Entwicklerwerkzeugen Ihres Browsers und der Live-Referenz-Website [WKND](https://wknd.site/) vorstellen.

>[!NOTE]
>
> Screenshots unten, aufgenommen vom Chrome-Browser.

1. Navigieren Sie zu [https://wknd.site](https://wknd.site)
1. Öffnen Sie Ihre Entwicklerwerkzeuge und geben Sie in **Console** den folgenden Befehl ein:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect Sie die Antwort, um den aktuellen Status der Datenschicht auf einer AEM Site anzuzeigen. Sie sollten Informationen zur Seite und zu den einzelnen Komponenten anzeigen.

   ![Adobe Data Layer Response](assets/data-layer-state-response.png)

1. Schieben Sie ein Datenobjekt auf die Datenschicht, indem Sie Folgendes in die Konsole eingeben:

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

1. Führen Sie den Befehl `adobeDataLayer.getState()` erneut aus und suchen Sie den Eintrag für `training-data`.
1. Fügen Sie dann einen Pfadparameter hinzu, um nur den spezifischen Status einer Komponente zurückzugeben:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Nur die Dateneingabe einer einzelnen Komponente zurückgeben](assets/return-just-single-component.png)

## Arbeiten mit Ereignissen

Es empfiehlt sich, benutzerspezifischen Code auf der Grundlage eines Ereignisses aus der Datenschicht auszulösen. Erforschen Sie als Nächstes die Registrierung und das Listening verschiedener Ereignis.

1. Geben Sie die folgende Helper-Methode in Ihre Konsole ein:

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

   Der obige Code überprüft das `event`-Objekt und verwendet die `adobeDataLayer.getState`-Methode, um den aktuellen Status des Objekts abzurufen, das das Ereignis ausgelöst hat. Die Helper-Methode überprüft dann das `filter`-Kriterium und nur dann, wenn das aktuelle `dataObject` den Filter erfüllt, wird es zurückgegeben.

   >[!CAUTION]
   >
   > Es ist wichtig, dass **nicht** den Browser während dieser Übung aktualisiert wird, da sonst das Konsolen-JavaScript verloren geht.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn eine **Teaser**-Komponente in einem **Karussell** angezeigt wird.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Das `teaserShownHandler` ruft die `getDataObjectHelper`-Methode auf und übergibt einen Filter von `wknd/components/teaser` als `@type`, um Ereignis auszufiltern, die von anderen Komponenten ausgelöst werden.

1. Schieben Sie anschließend einen Ereignis-Listener auf die Datenschicht, um auf das Ereignis `cmp:show` zu hören.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   Das `cmp:show`-Ereignis wird von vielen verschiedenen Komponenten ausgelöst, z. B. wenn eine neue Folie in der Komponente **Karussell** angezeigt wird oder wenn eine neue Registerkarte in der Komponente **Registerkarte** ausgewählt ist.

1. Schalten Sie auf der Seite die Karussellfolien um und beachten Sie die Konsolenanweisungen:

   ![Karussell umschalten und Ereignis-Listener anzeigen](assets/teaser-console-slides.png)

1. Entfernen Sie den Ereignis-Listener aus der Datenschicht, um das Listening auf das `cmp:show`-Ereignis zu beenden:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Kehren Sie zur Seite zurück und schalten Sie die Karussellfolien um. Beachten Sie, dass keine weiteren Aussagen protokolliert werden und dass das Ereignis nicht angehört wird.

1. Geben Sie als Nächstes einen Ereignis-Handler ein, der aufgerufen wird, wenn das Ereignis der angezeigten Seite ausgelöst wird:

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

1. Schieben Sie anschließend einen Ereignis-Listener auf die Datenschicht, um auf das `cmp:show`-Ereignis zu warten, und rufen Sie das `pageShownHandler`-Ereignis auf.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Sie sollten sofort eine Konsolenanweisung sehen, die mit den Seitendaten ausgelöst wird:

   ![Seitenansichtsdaten](assets/page-show-console-data.png)

   Das `cmp:show`-Ereignis für die Seite wird bei jedem Seitenladevorgang am oberen Seitenrand ausgelöst. Sie fragen sich vielleicht, warum der Ereignis-Handler ausgelöst wurde, wenn die Seite eindeutig bereits geladen wurde.

   Dies ist eine der einzigartigen Funktionen der Adobe Client Data Layer, da Sie Ereignis-Listener **vor** oder **nach der Initialisierung der Datenschicht registrieren können.** Dies ist ein entscheidender Faktor, um Racebedingungen zu vermeiden.

   Die Datenschicht behält ein Warteschlangenarray aller sequenziell aufgetretenen Ereignis bei. Die Datenschicht löst standardmäßig Ereignis-Rückrufe für Ereignis aus, die in **after** aufgetreten sind, sowie für Ereignis in **future**. Es ist möglich, die Ereignis nach Vergangenheit oder Zukunft zu filtern. [Weitere Informationen finden Sie in der Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Nächste Schritte

Sehen Sie sich das folgende Lernprogramm an, um zu erfahren, wie Sie die Ereignis-basierte Adobe Client Data-Ebene verwenden, um [Seitendaten zu erfassen und an Adobe Analytics](../analytics/collect-data-analytics.md) zu senden.


## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Verwenden der Adobe Client-Datenschicht und der Dokumentation der Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
