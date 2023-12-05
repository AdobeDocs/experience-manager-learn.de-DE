---
title: Registrierung einer AEM-Benutzeroberflächen-Erweiterung
description: Erfahren Sie, wie Sie eine AEM-Benutzeroberflächen-Erweiterung registrieren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 126
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 100%

---

# Registrierung der Erweiterung

AEM-Benutzeroberflächen-Erweiterungen sind spezielle App-Entwicklungs-Apps, die auf React basieren und das [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)-Benutzeroberflächen-Framework nutzen.

Um zu definieren, wo und wie die AEM-Benutzeroberflächen-Erweiterung angezeigt wird, sind zwei Konfigurationen in der App-Entwicklungs-App der Erweiterung erforderlich: App-Routing und die Registrierung der Erweiterung.

## App-Routen{#app-routes}

Die Datei `App.js` der Erweiterung deklariert den [React-Router](https://reactrouter.com/de/main), der eine Indexroute enthält, die die Erweiterung in der AEM-Benutzeroberfläche registriert.

Die Indexroute wird beim erstmaligen Laden der AEM-Benutzeroberfläche aufgerufen. Das Ziel dieser Route definiert, wie die Erweiterung in der Konsole bereitgestellt wird.

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

Abhängig von der Vorlage für die AEM-Benutzeroberflächen-Erweiterung, die beim [Initialisieren der App-Entwicklungs-Erweiterungs-App](./app-initialization.md) ausgewählt wurde, werden unterschiedliche Erweiterungspunkte unterstützt.

+ [Erweiterungspunkte der Inhaltsfragmente-Benutzeroberfläche](./content-fragments/overview.md#extension-points)


## Bedingtes Einschließen von Erweiterungen

AEM-Benutzeroberflächen-Erweiterungen können benutzerdefinierte Logik ausführen, um die AEM-Umgebungen zu beschränken, in denen die Erweiterung angezeigt wird. Diese Prüfung wird vor dem `register`-Aufruf in der `ExtensionRegistration`-Komponente durchgeführt. Es wird sofort zurückgekehrt, wenn die Erweiterung nicht angezeigt werden soll.

Für diese Prüfung ist nur begrenzter Kontext verfügbar:

+ der AEM-Host, auf dem die Erweiterung geladen wird
+ das AEM-Zugriffs-Token der aktuellen Benutzerin oder des aktuellen Benutzers

Die häufigsten Prüfungen zum Laden einer Erweiterung sind:

+ Verwenden des AEM-Hosts (`new URLSearchParams(window.location.search).get('repo')`), um zu bestimmen, ob die Erweiterung geladen werden soll.
   + Die Erweiterung wird nur in AEM-Umgebungen angezeigt, die Teil eines bestimmten Programms sind (wie im Beispiel unten gezeigt).
   + Die Erweiterung wird nur in einer bestimmten AEM-Umgebung (auf einem bestimmten AEM-Host) angezeigt.
+ Verwenden einer [Adobe I/O Runtime-Aktion](./runtime-action.md), um einen HTTP-Aufruf an AEM zu senden und festzustellen, ob die Erweiterung der aktuellen Benutzerin oder dem aktuellen Benutzer angezeigt werden soll.

Das folgende Beispiel zeigt, wie die Erweiterung auf alle Umgebungen im Programm `p12345` beschränkt wird.

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
