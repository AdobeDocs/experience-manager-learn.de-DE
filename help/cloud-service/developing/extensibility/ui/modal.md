---
title: AEM-Benutzeroberflächen-Erweiterungsmodal
description: Erfahren Sie, wie Sie ein AEM-Benutzeroberflächen-Erweiterungsmodul erstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 126
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 100%

---

# Erweiterungsmodal

![AEM-Benutzeroberflächen-Erweiterungsmodal](./assets/modal/modal.png){align="center"}

AEM-Benutzteroberflächen-Erweiterungsmodale bieten eine Möglichkeit, benutzerdefinierte Benutzeroberfläche an AEM-Benutzeroberflächen-Erweiterungen anzuhängen.

Modale sind React-Anwendungen, die auf [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/) basieren, und eine beliebige benutzerdefinierte Benutzeroberfläche erstellen können, die für die Erweiterung erforderlich ist, einschließlich, aber nicht beschränkt auf:

+ Bestätigungsdialogfelder
+ [Eingabeformulare](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Fortschrittsanzeigen](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Ergebniszusammenfassung](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Fehlermeldungen
+ … oder sogar eine vollständige React-Anwendung mit mehreren Ansichten!

## Modalrouten

Das Modalerlebnis wird durch die React-Erweiterungs-App der App-Entwicklung definiert, die unter dem Ordner `web-src` angegeben ist. Wie bei jeder React-App wird das Gesamterlebnis über [React-Routen](https://reactrouter.com/de/main/components/routes) orchestriert, die die [React-Komponenten](https://reactjs.org/docs/components-and-props.html) rendern.

Zum Generieren der ersten Modalansicht ist mindestens eine Route erforderlich. Diese erste Route wird in der `onClick(..)`-Funktion der [Erweiterungsregistrierung](#extension-registration) aufgerufen, wie unten dargestellt.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registrierung der Erweiterung

Um ein Modal zu öffnen, wird über die `onClick(..)`-Funktion der Erweiterung ein Aufruf an `guestConnection.host.modal.showUrl(..)` durchgeführt. `showUrl(..)` wird ein JavaScript-Objekt mit folgenden Schlüsseln/Werten übergeben:

+ `title` gibt den Namen des Titels des Modals an, das Benutzenden angezeigt wird.
+ `url` ist die URL, die die für die erste Ansicht des Modals verantwortliche [React-Route](#modal-routes) aufruft.

Es ist unbedingt erforderlich, dass die an `guestConnection.host.modal.showUrl(..)` weitergegebene `url` zur Route der Erweiterung aufgelöst wird. Andernfalls wird nichts im Modal angezeigt.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Modalkomponente

Jede Route der Erweiterung, [die sich nicht in der `index`-Route befindet](./extension-registration.md#app-routes), wird einer React-Komponente zugeordnet, die im Modal der Erweiterung gerendert werden kann.

Ein Modal kann aus einer beliebigen Anzahl von React-Routen bestehen, von einem einfachen Ein-Routen-Modal bis hin zu einem komplexen Modal mit mehreren Routen.

Im Folgenden wird ein einfaches Ein-Routen-Modal gezeigt. Diese Modalansicht kann jedoch React-Links enthalten, die andere Routen oder Verhaltensweisen aufrufen.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Schließen des Modals

![Schaltfläche zum Schließen des AEM-Benutzeroberflächen-Erweiterungsmodals](./assets/modal/close.png){align="center"}

Modale müssen über eigene, enge Steuerungsmöglichkeiten verfügen. Dies geschieht durch Aufrufen von `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
