---
title: AEM UI-Erweiterungs-Modal
description: Erfahren Sie, wie Sie ein AEM UI-Erweiterungsmodul erstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Modal der Erweiterung

![AEM UI-Erweiterungs-Modal](./assets/modal/modal.png){align="center"}

AEM UI-Erweiterungs-Modal bieten eine Möglichkeit, benutzerdefinierte Benutzeroberfläche an AEM UI-Erweiterungen anzuhängen.

Modale sind React-Anwendungen, die auf [Reaktionsspektrum](https://react-spectrum.adobe.com/react-spectrum/)und kann eine beliebige benutzerdefinierte Benutzeroberfläche erstellen, die für die Erweiterung erforderlich ist, einschließlich, aber nicht beschränkt auf:

+ Bestätigungsdialogfelder
+ [Formulare](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Fortschrittsanzeigen](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Ergebniszusammenfassung](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Fehlermeldungen
+ ... oder sogar eine Full-View React-Anwendung!

## Modale Routen

Das modale Erlebnis wird durch die Erweiterung App Builder React App definiert, die unter der `web-src` Ordner. Wie bei jeder React-App wird das gesamte Erlebnis mithilfe von [React-Routen](https://reactrouter.com/en/main/components/routes) dieser Renderer [React-Komponenten](https://reactjs.org/docs/components-and-props.html).

Zum Generieren der anfänglichen modalen Ansicht ist mindestens eine Route erforderlich. Diese anfängliche Route wird im [Erweiterungsregistrierung](#extension-registration)s `onClick(..)` -Funktion, wie unten dargestellt.


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

Um ein Modal zu öffnen, rufen Sie `guestConnection.host.modal.showUrl(..)` wird aus dem `onClick(..)` -Funktion. `showUrl(..)` wird ein JavaScript-Objekt mit Schlüssel/Werten übergeben:

+ `title` gibt den Namen des Titels des Modals an, das dem Benutzer angezeigt wird
+ `url` ist die URL, die die [React route](#modal-routes) verantwortlich für die anfängliche Ansicht des Modals.

Die `url` übergeben an `guestConnection.host.modal.showUrl(..)` wird in der Erweiterung in die Route aufgelöst, sonst wird nichts im Modal angezeigt.

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

## Modal-Komponente

Jede Route der Erweiterung, [das nicht der `index` route](./extension-registration.md#app-routes), wird einer React-Komponente zugeordnet, die im Modal der Erweiterung dargestellt werden kann.

Ein Modal kann aus einer beliebigen Anzahl von React-Routen bestehen, von einem einfachen Ein-Route-Modal bis hin zu einem komplexen, multiroute-Modal.

Die folgende Abbildung zeigt ein einfaches Einweg-Modal, diese modale Ansicht kann jedoch React-Links enthalten, die andere Routen oder Verhaltensweisen aufrufen.

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

## Modal schließen

![Schaltfläche zum Schließen des UI-Erweiterungsmodells AEM](./assets/modal/close.png){align="center"}

Modale müssen ihre eigene enge Kontrolle bieten. Dies geschieht durch Aufrufen von `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
