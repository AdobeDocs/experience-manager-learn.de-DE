---
title: Registrierung der AEM UI-Erweiterung
description: Erfahren Sie, wie Sie eine AEM UI-Erweiterung registrieren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---

# Registrierung der Erweiterung

AEM Benutzeroberflächen-Erweiterungen sind spezielle App Builder-Apps, die auf React basieren, und verwenden die [Reaktionsspektrum](https://react-spectrum.adobe.com/react-spectrum/) UI-Framework.

Um festzulegen, wo und wie die AEM UI-Erweiterung angezeigt wird, sind zwei Konfigurationen in der App Builder-App der Erweiterung erforderlich: App-Routing und die Registrierung der Erweiterung.

## App-Routen{#app-routes}

Die Erweiterung `App.js` deklariert die [React-Router](https://reactrouter.com/en/main) , die eine Indexroute enthält, die die Erweiterung in der AEM UI registriert.

Die Indexroute wird beim erstmaligen Laden der AEM-Benutzeroberfläche aufgerufen. Das Ziel dieser Route definiert, wie die Erweiterung in der Konsole verfügbar gemacht wird.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

## Registrierung der Erweiterung

`ExtensionRegistration.js` muss sofort über die Indexroute der Erweiterung geladen werden und fungiert als Registrierungspunkt der Erweiterung.

Basierend auf der Vorlage für die AEM UI-Erweiterung, die beim [Initialisieren der App Builder-App-Erweiterung](./app-initialization.md), werden unterschiedliche Erweiterungspunkte unterstützt.

+ [Erweiterungspunkte der Inhaltsfragmente-Benutzeroberfläche](./content-fragments/overview.md#extension-points)


## Bedingtes Einschließen von Erweiterungen

AEM UI-Erweiterungen können benutzerdefinierte Logik ausführen, um die AEM Umgebungen zu beschränken, in denen die Erweiterung angezeigt wird. Diese Prüfung wird vor der `register` im `ExtensionRegistration` und sofort zurückgibt, wenn die Erweiterung nicht angezeigt werden soll.

Für diese Prüfung ist nur ein begrenzter Kontext verfügbar:

+ Der AEM-Host, auf dem die Erweiterung geladen wird.
+ Das AEM Zugriffstoken des aktuellen Benutzers.

Die häufigsten Prüfungen zum Laden einer Erweiterung sind:

+ Verwenden des AEM-Hosts (`new URLSearchParams(window.location.search).get('repo')`), um zu bestimmen, ob die Erweiterung geladen werden soll.
   + Zeigen Sie die Erweiterung nur in AEM Umgebungen an, die Teil eines bestimmten Programms sind (wie im Beispiel unten gezeigt).
   + Zeigen Sie die Erweiterung nur in einer bestimmten AEM Umgebung (AEM Host) an.
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
