---
title: Widgets zum Rich-Text-Editor (RTE) hinzufügen
description: Erfahren Sie, wie Sie Widgets im Rich-Text-Editor (RTE) im AEM Inhaltsfragment-Editor hinzufügen.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: be4c0a6a-5c1f-4408-9ac6-56b8f0653d42
source-git-commit: 9c8c03df7c510ab697d5222f9dffd5111519b712
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Widgets zum Rich-Text-Editor (RTE) hinzufügen

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Um den dynamischen Inhalt im Rich-Text-Editor (RTE) hinzuzufügen, muss die **Widgets** verwendet werden. Die Widgets helfen bei der Integration der einfachen oder komplexen Benutzeroberfläche in den RTE und die Benutzeroberfläche, die mit dem JS-Framework Ihrer Wahl erstellt werden können. Sie können als Dialogfelder betrachtet werden, die durch Drücken von `{` spezieller Schlüssel im RTE.

Normalerweise werden die Widgets verwendet, um den dynamischen Inhalt einzufügen, der eine externe Systemabhängigkeit aufweist oder sich basierend auf dem aktuellen Kontext ändern könnte.

Die **Widgets** werden der **RTE** im Inhaltsfragment-Editor mit der `rte` Erweiterungspunkt. Verwenden `rte` des Erweiterungspunkts `getWidgets()` -Methode ein oder mehrere Widgets hinzugefügt werden. Sie werden durch Drücken der `{` Spezialschlüssel zum Öffnen der Kontextmenüoption und wählen Sie dann das gewünschte Widget aus, um die Benutzeroberfläche des benutzerdefinierten Dialogfelds zu laden.

Dieses Beispiel zeigt, wie Sie ein Widget mit dem Namen _Rabattcode-Liste_ , um in einem RTE-Inhalt den WKND-Adventure-spezifischen Rabattcode zu finden, auszuwählen und hinzuzufügen. Diese Rabattcodes können in einem externen System wie Order Management System (OMS), Product Information Management (PIM), einer eigenen Anwendung oder einer Adobe AppBuilder-Aktion verwaltet werden.

Um die Dinge einfach zu halten, verwendet dieses Beispiel den [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) Framework zum Entwickeln des Widgets oder der Dialog-Benutzeroberfläche und des hartcodierten WKND-Abenteuernamens, Discount-Code-Daten.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `rte` , um dem RTE im Inhaltsfragment-Editor ein Widget hinzuzufügen.

| AEM Benutzeroberfläche erweitert | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragmente-Editor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Beispielerweiterung

Im folgenden Beispiel wird eine _Rabattcode-Liste_ Widget. Durch Drücken der `{` speziellem Schlüssel im RTE, wird das Kontextmenü geöffnet, indem Sie die _Rabattcode-Liste_ -Option im Kontextmenü aus, wird die Benutzeroberfläche des Dialogfelds geöffnet.

Die WKND-Inhaltsautoren können aktuellen Adventure-spezifischen Rabattcode suchen, auswählen und hinzufügen, falls verfügbar.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, der der Route index.html zugeordnet ist, ist der Einstiegspunkt für die AEM Erweiterung und definiert:

+ Die Widget-Definition in `getWidgets()` Funktion mit `id, label and url` -Attribute.
+ Die `url` -Attributwert, ein relativer URL-Pfad (`/index.html#/discountCodes`), um die Benutzeroberfläche des Dialogfelds zu laden.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Hinzufügen `discountCodes` Route in `App.js`{#add-widgets-route}

In der Haupt-React-Komponente `App.js`, fügen Sie die `discountCodes` -Route, um die Benutzeroberfläche für den oben genannten relativen URL-Pfad zu rendern.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Erstellen `DiscountCodes` React-Komponente{#create-widget-react-component}

Die Benutzeroberfläche des Widgets oder Dialogfelds wird mit der [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) Framework. Die `DiscountCodes` Komponentencode lautet wie folgt: Hier sind die wichtigsten Highlights:

+ Die Benutzeroberfläche wird mithilfe von React Spectrum-Komponenten gerendert, z. B. [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Schaltfläche](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ Die `adventureDiscountCodes` -Array hat eine fest codierte Zuordnung von Abenteuername und Discount-Code. In einem realen Szenario können diese Daten von der Adobe AppBuilder-Aktion oder von externen Systemen wie PIM, OMS oder dem eigenständigen oder Cloud Provider-basierten API-Gateway abgerufen werden.
+ Die `guestConnection` wird mithilfe der `useEffect` [React-Hook](https://react.dev/reference/react/useEffect) und als Komponentenstatus verwaltet werden. Sie wird zur Kommunikation mit dem AEM-Host verwendet.
+ Die `handleDiscountCodeChange` -Funktion den Discount-Code für den ausgewählten Abenteuernamen abruft und die Statusvariable aktualisiert.
+ Die `addDiscountCode` Funktion verwenden `guestConnection` -Objekt stellt die auszuführende RTE-Anweisung bereit. In diesem Fall `insertContent` Anweisung und HTML-Codeausschnitt des tatsächlichen Discount-Codes, der in den RTE eingefügt werden soll.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
