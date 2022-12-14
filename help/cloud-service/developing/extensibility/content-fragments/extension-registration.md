---
title: Registrierung der Inhaltsfragment-Konsolenerweiterung AEM
description: Erfahren Sie, wie Sie eine Inhaltsfragment-Konsolenerweiterung registrieren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# Erweiterungsregistrierung

AEM Inhaltsfragmentkonsole-Erweiterungen sind spezielle App Builder-App, die auf React basiert und die [Reaktionsspektrum](https://react-spectrum.adobe.com/react-spectrum/) UI-Framework.

Um festzulegen, wo und wie die AEM Inhaltsfragment-Konsole der Erweiterung angezeigt wird, sind zwei spezifische Konfigurationen in der App Builder-App der Erweiterung erforderlich: App-Routing und die Registrierung der Erweiterung.

## App-Routen{#app-routes}

Die Erweiterung `App.js` deklariert die [React-Router](https://reactrouter.com/en/main) , die eine Indexroute enthält, die die Erweiterung in der AEM Inhaltsfragmentkonsole registriert.

Die Indexroute wird aufgerufen, wenn AEM Inhaltsfragmentkonsole anfänglich geladen wird. Das Ziel dieser Route definiert, wie die Erweiterung in der Konsole verfügbar gemacht wird.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Erweiterungsregistrierung

`ExtensionRegistration.js` muss sofort über die Indexroute der Erweiterung geladen werden und fungiert als Registrierungspunkt der Erweiterung und definiert Folgendes:

1. den Erweiterungstyp; a [Kopfzeilenmenü](./header-menu.md) oder [Aktionsleiste](./action-bar.md) Schaltfläche.
   + [Menü &quot;Kopfzeile&quot;](./header-menu.md#extension-registration) Erweiterungen werden durch die `headerMenu` Eigenschaft unter `methods`.
   + [Symbolleiste](./action-bar.md#extension-registration) Erweiterungen werden durch die `actionBar` Eigenschaft unter `methods`.
1. Die Definition der Erweiterungsschaltfläche in `getButton()` -Funktion. Diese Funktion gibt ein Objekt mit Feldern zurück:
   + `id` ist eine eindeutige ID für die Schaltfläche
   + `label` ist die Beschriftung der Erweiterungsschaltfläche in der AEM Inhaltsfragment-Konsole
   + `icon` ist das Symbol der Erweiterungsschaltfläche in der AEM Inhaltsfragment-Konsole. Das Symbol ist eine [Reaktionsspektrum](https://spectrum.adobe.com/page/icons/) Symbolname, wobei Leerzeichen entfernt werden.
1. Der Klick-Handler für die Schaltfläche, definiert in einer `onClick()` -Funktion.
   + [Kopfzeilenmenü](./header-menu.md#extension-registration) -Erweiterungen übergeben keine Parameter an den Klick-Handler.
   + [Aktionsleiste](./action-bar.md#extension-registration) Erweiterungen bieten eine Liste der ausgewählten Inhaltsfragmentpfade im `selections` Parameter.

### Header Menu-Erweiterung

![Header Menu-Erweiterung](./assets/extension-registration/header-menu.png)

Die Schaltflächen für die Kopfzeilenmenüerweiterung werden angezeigt, wenn keine Inhaltsfragmente ausgewählt sind. Da die Erweiterungen des Header-Menüs keine Auswirkungen auf eine Auswahl von Inhaltsfragmenten haben, werden keine Inhaltsfragmente bereitgestellt `onClick()` Handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Zum Erstellen einer Kopfzeilenmenüerweiterung überspringen</p>
      <p class="has-text-blackest">Erfahren Sie, wie Sie eine Kopfzeilenmenüerweiterung in der AEM Inhaltsfragmentkonsole registrieren und definieren.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Informationen zum Erstellen einer Header-Menüerweiterung">Informationen zum Erstellen einer Header-Menüerweiterung</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Aktionsleistenerweiterung

![Aktionsleistenerweiterung](./assets/extension-registration/action-bar.png)

Die Schaltflächen für die Erweiterung der Aktionsleiste werden angezeigt, wenn mindestens ein Inhaltsfragment ausgewählt ist. Die Pfade des ausgewählten Inhaltsfragments werden der Erweiterung über die `selections` -Parameter in der `onClick(..)` Handler.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Zum Erstellen einer Aktionsleistenerweiterung überspringen</p>
      <p class="has-text-blackest">Erfahren Sie, wie Sie eine Aktionsleistenerweiterung in der Konsole AEM Inhaltsfragmente registrieren und definieren.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Erfahren Sie, wie Sie eine Aktionsleistenerweiterung erstellen">Erfahren Sie, wie Sie eine Aktionsleistenerweiterung erstellen</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Bedingtes Einschließen von Erweiterungen

AEM Erweiterungen der Inhaltsfragmentkonsole können benutzerdefinierte Logik ausführen, um zu begrenzen, wann die Erweiterung in der AEM Inhaltsfragmentkonsole angezeigt wird. Diese Prüfung wird vor der `register` im `ExtensionRegistration` und sofort zurückgibt, wenn die Erweiterung nicht angezeigt werden soll.

Für diese Prüfung ist nur ein begrenzter Kontext verfügbar:

+ Der AEM-Host, auf dem die Erweiterung geladen wird.
+ Das AEM Zugriffstoken des aktuellen Benutzers.

Die häufigsten Prüfungen zum Laden einer Erweiterung sind:

+ Verwenden des AEM-Hosts (`new URLSearchParams(window.location.search).get('repo')`), um zu bestimmen, ob die Erweiterung geladen werden soll.
   + Zeigen Sie die Erweiterung nur in AEM Umgebungen an, die Teil eines bestimmten Programms sind (wie im Beispiel unten gezeigt).
   + Zeigen Sie die Erweiterung nur in einer bestimmten AEM Umgebung (d. h. AEM Host) an.
+ Verwenden eines [Adobe I/O Runtime-Aktion](./runtime-action.md) , um einen HTTP-Aufruf an AEM zu senden, um festzustellen, ob dem aktuellen Benutzer die Erweiterung angezeigt werden soll.

Das folgende Beispiel zeigt die Beschränkung der Erweiterung auf alle Umgebungen im Programm `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
