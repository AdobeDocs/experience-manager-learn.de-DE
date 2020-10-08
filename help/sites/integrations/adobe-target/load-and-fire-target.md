---
title: Laden und Auslösen eines Zielgruppe-Aufrufs
description: Erfahren Sie, wie Sie mithilfe einer Startregel Zielgruppen laden, Parameter an Seitenanfragen übergeben und einen Seitenaufruf von Ihrer Site auslösen können. Seiteninformationen werden mithilfe der Adobe Client Data Layer abgerufen und als Parameter übergeben, mit denen Sie Daten über das Erlebnis der Besucher auf einer Webseite erfassen und speichern können, um so den Zugriff auf diese Daten zu erleichtern.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 3%

---


# Laden und Auslösen eines Zielgruppe-Aufrufs {#load-fire-target}

Erfahren Sie, wie Sie mithilfe einer Startregel Zielgruppen laden, Parameter an Seitenanfragen übergeben und einen Seitenaufruf von Ihrer Site auslösen können. Seiteninformationen werden mithilfe der Adobe Client Data Layer abgerufen und als Parameter übergeben, mit denen Sie Daten über das Erlebnis der Besucher auf einer Webseite erfassen und speichern können, um so den Zugriff auf diese Daten zu erleichtern.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Seitenladeregel

Die Adobe Client Data Layer ist eine vom Ereignis gesteuerte Datenschicht. Wenn die AEM-Datenschicht geladen wird, löst sie ein Ereignis aus `cmp:show` . Im Video wird die `Launch Library Loaded` Regel mithilfe eines benutzerdefinierten Ereignisses aufgerufen. Unten finden Sie die Codeausschnitte, die im Video sowohl für das benutzerdefinierte Ereignis als auch für die Datenelemente verwendet werden.

### Benutzerdefiniertes Ereignis

Das folgende Codefragment fügt einen Ereignis-Listener hinzu, indem eine Funktion in die Datenschicht gedrängt wird. Wenn das `cmp:show` Ereignis ausgelöst wird, wird die `pageShownEventHandler` Funktion aufgerufen. In dieser Funktion werden einige Sanitätsüberprüfungen hinzugefügt und ein neuer erstellt, der den neuesten Status der Datenschicht für die Komponente enthält, die das Ereignis ausgelöst hat. `dataObject`

Danach `trigger(dataObject)` wird gerufen. `trigger()` ist ein reservierter Name in Launch und löst die Launch-Regel aus. Wir übergeben das Ereignis-Objekt als Parameter, der wiederum durch einen anderen reservierten Namen in Launch namens Ereignis verfügbar gemacht wird. Datenelemente in Launch können jetzt auf verschiedene Eigenschaften verweisen: `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
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

### Seiten-ID der Datenschicht

```
if(event && event.id) {
    return event.id;
}
```

![Seiten-ID](assets/pageid.png)

### Seitenpfad

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![Seitenpfad](assets/pagepath.png)

### Seitentitel

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![Seitentitel](assets/pagetitle.png)

### Häufige Probleme

#### Warum werden meine Mboxes nicht auf meinen Webseiten ausgelöst?

**Fehlermeldung, wenn kein mboxDisable-Cookie gesetzt wurde**

![Zielgruppe Cookie-Domänenfehler](assets/target-cookie-error.png)

**Lösung**

Zielgruppe-Kunden verwenden manchmal Cloud-basierte Instanzen mit Zielgruppe zum Testen oder zum einfachen Testversand des Konzepts. Diese Domänen und viele andere gehören zur Public Suffix Liste .
Moderne Browser speichern keine Cookies, wenn Sie diese Domänen verwenden, es sei denn, Sie passen die `cookieDomain` Einstellung mit `targetGlobalSettings()`an.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## Nächste Schritte

1. [Erlebnisfragment nach Adobe Target exportieren](./export-experience-fragment-target.md)

## Unterstützende Links

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Verwenden der Adobe Client-Datenschicht und der Dokumentation der Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Einführung in den Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)