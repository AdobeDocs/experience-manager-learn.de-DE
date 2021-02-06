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
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 4%

---


# Laden und Auslösen eines Zielgruppe-Aufrufs {#load-fire-target}

Erfahren Sie, wie Sie mithilfe einer Startregel Zielgruppen laden, Parameter an Seitenanfragen übergeben und einen Seitenaufruf von Ihrer Site auslösen können. Webseiteninformationen werden mithilfe der Adobe Client Data Layer als Parameter abgerufen und weitergegeben, mit der Sie Daten über das Erlebnis der Besucher auf einer Webseite erfassen und speichern können, um so den Zugriff auf diese Daten zu erleichtern.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Seitenladeregel

Die Adobe Client Data Layer ist eine vom Ereignis gesteuerte Datenschicht. Wenn die AEM-Seitendatenschicht geladen wird, wird ein Ereignis `cmp:show` Trigger. Im Video wird die `Launch Library Loaded`-Regel mithilfe eines benutzerdefinierten Ereignisses aufgerufen. Unten finden Sie die Codeausschnitte, die im Video sowohl für das benutzerdefinierte Ereignis als auch für die Datenelemente verwendet werden.

### Angezeigtes benutzerdefiniertes Ereignis{#page-event}

![Seitendarstellung und benutzerdefinierter Ereignis](assets/load-and-fire-target-call.png)

Fügen Sie der Eigenschaft &quot;Start&quot;ein neues **Ereignis** zur **Regel** hinzu.

+ __Erweiterung:__ Core
+ __Ereignistyp:__ Benutzerdefinierter Code
+ __Name:__ Seitenansichts-Handler (oder eine Beschreibung)

Tippen Sie auf die Schaltfläche __Editor öffnen__ und fügen Sie das folgende Codefragment ein. Dieser Code __muss der__ Ereignis-Konfiguration __und einer nachfolgenden__ Aktion __hinzugefügt werden.__

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

Eine benutzerdefinierte Funktion definiert das `pageShownEventHandler` und überwacht die von AEM Core-Komponenten emittierten Ereignis, leitet die relevanten Informationen aus der Core-Komponente ab, packt sie in ein Ereignis-Objekt und Trigger das Launch-Ereignis mit den abgeleiteten Ereignis-Informationen bei der Nutzlast.

Die Startregel wird mithilfe der Funktion `trigger(...)` des Launches ausgelöst, die nur __in der Codeausschnittdefinition des Ereignisses für benutzerspezifischen Code verfügbar ist.__

Die Funktion `trigger(...)` akzeptiert ein Ereignis-Objekt als Parameter, der wiederum in Datenelemente starten angezeigt wird, und zwar unter &quot;Start&quot;, wobei der Name `event` ein anderer reservierter Name lautet. Datenelemente in Launch können nun auf Daten aus diesem Ereignis-Objekt vom `event`-Objekt mit Syntax wie `event.component['someKey']` verweisen.

Wenn `trigger(...)` außerhalb des Kontexts des benutzerspezifischen Code-Ereignistyps eines Ereignisses verwendet wird (z. B. in einer Aktion), wird der JavaScript-Fehler `trigger is undefined` auf der mit der Launch-Eigenschaft integrierten Website ausgegeben.


### Datenelemente

![Datenelemente](assets/data-elements.png)

Datenelemente zum Starten von Adoben ordnen die Daten des im benutzerspezifischen Ereignis für die Seitenanzeige ausgelösten Ereignis-Objekts [den in Adobe Target verfügbaren Variablen über den Datenelementtyp für benutzerdefinierte  der Core-Erweiterung zu.](#page-event)

#### Seiten-ID-Datenelement

```
if (event && event.id) {
    return event.id;
}
```

Dieser Code gibt die generierte eindeutige ID der Core-Komponente zurück.

![Seiten-ID](assets/pageid.png)

### Seitenpfad-Datenelement

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Dieser Code gibt den Pfad der AEM Seite zurück.

![Seitenpfad](assets/pagepath.png)

### Datenelement für Seitentitel

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Dieser Code gibt den Titel der AEM Seite zurück.

![Seitentitel](assets/pagetitle.png)

## Fehlerbehebung

### Warum werden meine Mboxes nicht auf meinen Webseiten ausgelöst?

#### Fehlermeldung, wenn das Cookie &quot;mboxDisable&quot;nicht gesetzt wurde**

![Zielgruppe Cookie-Domänenfehler](assets/target-cookie-error.png)

#### Lösung

Zielgruppe-Kunden verwenden manchmal Cloud-basierte Instanzen mit Zielgruppe zum Testen oder zum einfachen Testversand des Konzepts. Diese Domänen und viele andere gehören zur Public Suffix Liste .
Moderne Browser speichern keine Cookies, wenn Sie diese Domänen verwenden, es sei denn, Sie passen die `cookieDomain`-Einstellung mit `targetGlobalSettings()` an.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Nächste Schritte

+ [Erlebnisfragment nach Adobe Target exportieren](./export-experience-fragment-target.md)

## Unterstützende Links

+ [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Verwenden der Adobe Client-Datenschicht und der Dokumentation der Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Einführung in den Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)