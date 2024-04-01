---
title: Laden und Auslösen eines Target-Aufrufs
description: Erfahren Sie, wie Sie laden, Parameter an Seitenanfragen übergeben und mithilfe einer Tagregel einen Target-Aufruf von Ihrer Site-Seite aus auslösen können.
feature: Core Components, Adobe Client Data Layer
version: Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 610
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 58%

---

# Laden und Auslösen eines Target-Aufrufs {#load-fire-target}

Erfahren Sie, wie Sie laden, Parameter an Seitenanfragen übergeben und mithilfe einer Tagregel einen Target-Aufruf von Ihrer Site-Seite aus auslösen können. Informationen zu Web-Seiten werden mithilfe der Adobe Client-Datenschicht abgerufen und als Parameter übergeben, über die Sie Daten zum Besuchererlebnis auf einer Web-Seite erfassen und speichern und dann den Zugriff auf diese Daten erleichtern können.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Seitenladeregel

Die Adobe Client-Datenschicht ist eine ereignisgesteuerte Datenschicht. Wenn die Datenschicht der AEM-Seite geladen wird, löst das ein `cmp:show`-Ereignis aus. Im Video wird die Regel `tags Library Loaded` mit einem benutzerspezifischen Ereignis aufgerufen. Unten finden Sie die Code-Snippets, die im Video für das benutzerspezifische Ereignis sowie für die Datenelemente verwendet werden.

### Benutzerspezifisches Ereignis „Seite angezeigt“{#page-event}

![Konfiguration des Ereignisses „Seite angezeigt“ und benutzerdefinierter Code](assets/load-and-fire-target-call.png)

Fügen Sie in der Eigenschaft &quot;tags&quot;eine neue **Ereignis** der **Regel**

+ __Erweiterung:__ Core
+ __Ereignistyp:__ Benutzerdefinierter Code
+ __Name:__ Seitenanzeige-Ereignis-Handler (oder eine Beschreibung)

Tippen Sie auf __Editor öffnen__ und fügen Sie folgendes Code-Snippet ein. Dieser Code __muss__ zur __Ereigniskonfiguration__ und zu einer sich anschließenden __Aktion__ hinzugefügt werden.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Eine benutzerdefinierte Funktion definiert die `pageShownEventHandler`und überwacht Ereignisse, die von AEM Kernkomponenten ausgegeben werden, leitet die relevanten Informationen von der Kernkomponente ab, packt sie in ein Ereignisobjekt und Trigger die Tags &quot;Event&quot;mit den abgeleiteten Ereignisinformationen bei der Nutzlast.

Die Tags-Regel wird mithilfe des Tags `trigger(...)` Funktion, die __only__ in der Codeausschnitt-Definition eines Ereignisses mit benutzerspezifischem Code einer Regel verfügbar.

Die `trigger(...)` -Funktion nimmt ein Ereignisobjekt als Parameter an, der wiederum in den Tags Datenelemente durch einen anderen reservierten Namen in Tags namens verfügbar gemacht wird. `event`. Datenelemente in -Tags können jetzt auf Daten aus diesem Ereignisobjekt aus dem `event` -Objekt mit Syntax wie `event.component['someKey']`.

Wenn `trigger(...)` außerhalb des Kontexts des Ereignistyps &quot;Benutzerspezifischer Code&quot;eines Ereignisses verwendet wird (z. B. in einer Aktion), wird der JavaScript-Fehler `trigger is undefined` wird auf der Website ausgelöst, die mit der Eigenschaft &quot;tags&quot;integriert ist.


### Datenelemente

![Datenelemente](assets/data-elements.png)

Tags-Datenelemente ordnen die Daten aus dem Ereignisobjekt zu [ausgelöst im benutzerspezifischen Ereignis &quot;Seite angezeigt&quot;](#page-event) auf Variablen, die in Adobe Target über den Datenelementtyp &quot;Benutzerspezifischer Code&quot;der Haupterweiterung verfügbar sind.

#### Seiten-ID-Datenelement

```
if (event && event.id) {
    return event.id;
}
```

Dieser Code gibt die eindeutige ID der Kernkomponente zurück.

![Seiten-ID](assets/pageid.png)

### Datenelement „Seitenpfad“

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Dieser Code gibt den Pfad der AEM-Seite zurück.

![Seitenpfad](assets/pagepath.png)

### Datenelement „Seitentitel“

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Dieser Code gibt den Titel der AEM-Seite zurück.

![Seitentitel](assets/pagetitle.png)

## Fehlerbehebung

### Warum werden meine Mboxes auf meinen Web-Seiten nicht ausgelöst?

#### Fehlermeldung, wenn kein mboxDisable-Cookie gesetzt ist

![Target-Cookie-Domain-Fehler](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Lösung

Target-Kundinnen und -Kunden verwenden mitunter Cloud-basierte Instanzen mit Target zum Testen oder für einfache Machbarkeitsprüfungen. Diese Domains sind wie viele andere Teil der öffentlichen Suffix-Liste.
Modere Browser speichern keine Cookies, wenn Sie diese Domains verwenden, es sei denn, Sie passen die `cookieDomain`-Einstellung mit `targetGlobalSettings()` an.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Nächste Schritte

+ [Exportieren von Experience Fragments nach Adobe Target](./export-experience-fragment-target.md)

## Unterstützende Links

+ [Adobe Client-Datenschicht-Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger – Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger – Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de)
+ [Einführung in Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=de)
