---
title: Laden und Auslösen eines Target-Aufrufs
description: Erfahren Sie, wie Sie mit einer Launch-Regel laden, Parameter an Seitenanfragen übergeben und einen Target-Aufruf von Ihrer Site-Seite aus auslösen können. Seiteninformationen werden mithilfe der Adobe Client-Datenschicht abgerufen und als Parameter übergeben, mit der Sie Daten zum Besuchererlebnis auf einer Webseite erfassen und speichern und anschließend den Zugriff auf diese Daten erleichtern können.
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 4%

---

# Laden und Auslösen eines Target-Aufrufs {#load-fire-target}

Erfahren Sie, wie Sie mit einer Launch-Regel laden, Parameter an Seitenanfragen übergeben und einen Target-Aufruf von Ihrer Site-Seite aus auslösen können. Informationen zu Webseiten werden mithilfe der Adobe Client-Datenschicht abgerufen und als Parameter übergeben, über die Sie Daten zum Besuchererlebnis auf einer Webseite erfassen und speichern und so den Zugriff auf diese Daten erleichtern können.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Seitenladeregel

Die Adobe Client-Datenschicht ist eine ereignisgesteuerte Datenschicht. Wenn die AEM-Datenschicht geladen wird, wird ein -Ereignis `cmp:show` Trigger. Im Video wird die Regel `Launch Library Loaded` mithilfe eines benutzerdefinierten Ereignisses aufgerufen. Unten finden Sie die Code-Snippets, die im Video für das benutzerspezifische Ereignis sowie für die Datenelemente verwendet werden.

### Benutzerdefiniertes Seitenereignis{#page-event}

![Auf der Seite angezeigte Ereigniskonfiguration und benutzerdefinierter Code](assets/load-and-fire-target-call.png)

Fügen Sie in der Launch-Eigenschaft der **Regel** ein neues **Ereignis** hinzu.

+ __Erweiterung:__ Core
+ __Ereignistyp:__ Benutzerspezifischer Code
+ __Name:__ Page Show Event Handler (oder eine Beschreibung)

Tippen Sie auf die Schaltfläche __Editor__ öffnen und fügen Sie das folgende Codefragment ein. Dieser Code __muss__ zur __Ereigniskonfiguration__ und einer nachfolgenden __Aktion__ hinzugefügt werden.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Eine benutzerdefinierte Funktion definiert das `pageShownEventHandler`-Objekt und überwacht die von AEM Kernkomponenten ausgegebenen Ereignisse, leitet die relevanten Informationen von der Kernkomponente ab, packt sie in ein Ereignisobjekt und Trigger das Launch-Ereignis mit den abgeleiteten Ereignisinformationen bei dessen Nutzlast.

Die Launch-Regel wird mit der Funktion `trigger(...)` von Launch ausgelöst, die __nur__ ist, die in der Codeausschnitt-Definition eines Regel-Ereignisses mit benutzerspezifischem Code verfügbar ist.

Die Funktion `trigger(...)` akzeptiert ein Ereignisobjekt als Parameter, der wiederum in Launch-Datenelementen durch einen anderen reservierten Namen in Launch namens `event` verfügbar gemacht wird. Datenelemente in Launch können jetzt auf Daten aus diesem Ereignisobjekt aus dem `event` -Objekt verweisen, indem eine Syntax wie `event.component['someKey']` verwendet wird.

Wenn `trigger(...)` außerhalb des Kontexts des Ereignistyps &quot;Benutzerspezifischer Code&quot;eines Ereignisses verwendet wird (z. B. in einer Aktion), wird der JavaScript-Fehler `trigger is undefined` auf der mit der Launch-Eigenschaft integrierten Website ausgelöst.


### Datenelemente

![Datenelemente](assets/data-elements.png)

Adobe Launch-Datenelemente ordnen die Daten aus dem Ereignisobjekt [, das im benutzerspezifischen Seitenanzeigeereignis](#page-event) ausgelöst wird, den in Adobe Target verfügbaren Variablen über den Datenelementtyp &quot;Benutzerspezifischer Code&quot;der Haupterweiterung zu.

#### Seiten-ID-Datenelement

```
if (event && event.id) {
    return event.id;
}
```

Dieser Code gibt die eindeutige ID der Kernkomponente zurück.

![Seiten-ID](assets/pageid.png)

### Datenelement &quot;Seitenpfad&quot;

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Dieser Code gibt den Pfad der AEM zurück.

![Seitenpfad](assets/pagepath.png)

### Datenelement &quot;Seitentitel&quot;

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Dieser Code gibt den Titel der AEM-Seite zurück.

![Seitentitel](assets/pagetitle.png)

## Fehlerbehebung

### Warum werden meine Mboxes nicht auf meinen Webseiten ausgelöst?

#### Fehlermeldung, wenn kein mboxDisable-Cookie gesetzt ist

![Target-Cookie-Domänenfehler](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Lösung

Target-Kunden verwenden mitunter Cloud-basierte Instanzen mit Target zum Testen oder für einfache Machbarkeitsprüfungen. Diese Domänen und viele andere sind Teil der öffentlichen Suffix-Liste .
Modere Browser speichern keine Cookies, wenn Sie diese Domänen verwenden, es sei denn, Sie passen die Einstellung `cookieDomain` mit `targetGlobalSettings()` an.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Nächste Schritte

+ [Experience Fragment nach Adobe Target exportieren](./export-experience-fragment-target.md)

## Unterstützende Links

+ [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de)
+ [Einführung in den Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)
