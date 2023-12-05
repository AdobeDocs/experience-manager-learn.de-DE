---
title: Hinzufügen von Widgets zum Rich-Text-Editor (RTE)
description: Erfahren Sie, wie Sie Widgets zum Rich-Text-Editor (RTE) im AEM-Inhaltsfragmenteditor hinzufügen.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 572
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 100%

---

# Hinzufügen von Widgets zum Rich-Text-Editor (RTE)

Erfahren Sie, wie Sie im AEM-Inhaltsfragmenteditor Widgets zum Rich-Text-Editor (RTE) hinzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Um dynamische Inhalte im Rich-Text-Editor (RTE) hinzuzufügen, muss die Funktionalität **Widgets** verwendet werden. Die Widgets helfen bei der Integration der einfachen oder komplexen Benutzeroberfläche in den RTE, wobei die Benutzeroberfläche mit dem JS-Framework Ihrer Wahl erstellt werden kann. Widgets können als Dialogfelder angesehen werden, die durch Drücken der Sondertaste `{` im RTE geöffnet werden.

Normalerweise werden die Widgets verwendet, um dynamische Inhalte einzufügen, die eine externe Systemabhängigkeit aufweisen oder sich basierend auf dem aktuellen Kontext ändern könnten.

Die **Widgets** werden zum **RTE** im Inhaltsfragmenteditor mithilfe des Erweiterungspunkts `rte` hinzugefügt. Durch Verwendung der `getWidgets()`-Methode des Erweiterungspunkts `rte` werden ein oder mehrere Widgets hinzugefügt. Auslösen können Sie sie, indem Sie die Sondertaste `{` drücken, um die Kontextmenüoption zu öffnen. Wählen Sie dann das gewünschte Widget aus, um die benutzerdefinierte Dialog-Benutzeroberfläche zu laden.

Dieses Beispiel zeigt, wie Sie das Widget _Discount Code List_ hinzufügen, um in RTE-Inhalt den WKND-abenteuerspezifischen Rabatt-Code zu suchen, auszuwählen und hinzuzufügen. Diese Rabatt-Codes können in einem externen System wie einem Order Management System (OMS), Product Information Management (PIM), einer eigenen Anwendung oder einer Adobe AppBuilder-Aktion verwaltet werden.

Der Einfachheit halber verwendet dieses Beispiel das [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html?lang=de)-Framework zum Entwickeln des Widgets oder der Dialog-Benutzeroberfläche und des fest codierten WKND-Abenteuernamens sowie der Rabatt-Code-Daten.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich bis zum Erweiterungspunkt `rte`, um dem RTE im Inhaltsfragmenteditor ein Widget hinzuzufügen.

| Erweiterte AEM-Benutzeroberfläche | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragmenteditor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Beispielerweiterung

Im folgenden Beispiel wird das Widget _Discount Code List_ erstellt. Durch Drücken der Sondertaste `{` im RTE wird das Kontextmenü geöffnet. Durch Auswahl der Option _Discount Code List_ im Kontextmenü wird dann die Dialogfeld-Benutzeroberfläche geöffnet.

Die WKND-Inhaltsautorinnen und -Inhaltsautoren können aktuellen abenteuerspezifischen Rabatt-Code suchen, auswählen und hinzufügen, sofern verfügbar.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, die der Route „index.html“ zugeordnet ist, ist der Einstiegspunkt für die AEM-Erweiterung und definiert Folgendes:

+ die Widget-Definition in der Funktion `getWidgets()` mit den Attributen `id, label and url`
+ den `url`-Attributwert, einen relativen URL-Pfad (`/index.html#/discountCodes`), um die Dialog-Benutzeroberfläche zu laden

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

### Hinzufügen der Route `discountCodes` in `App.js`{#add-widgets-route}

Fügen Sie in der React-Hauptkomponente `App.js` die Route `discountCodes` hinzu, um die Benutzeroberfläche für den oben genannten relativen URL-Pfad zu rendern.

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

### Erstellen der React-Komponente `DiscountCodes`{#create-widget-react-component}

Das Widget bzw. die Benutzeroberfläche wird mit dem [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html?lang=de)-Framework erstellt. Der `DiscountCodes`-Komponenten-Code lautet wie unten angegeben. Hier sind die wichtigsten Informationen:

+ Die Benutzeroberfläche wird mithilfe von React Spectrum-Komponenten gerendert, z. B. [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html?lang=de), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html?lang=de) und [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html?lang=de).
+ Das Array `adventureDiscountCodes` hat eine fest codierte Zuordnung von Abenteuernamen und Rabatt-Code. In einem realen Szenario können diese Daten von der Adobe AppBuilder-Aktion oder von externen Systemen wie PIM, OMS oder einem eigenen oder Cloud-Anbieter-basierten API-Gateway abgerufen werden.
+ `guestConnection` wird mithilfe des [React-Hooks](https://react.dev/reference/react/useEffect) `useEffect` initialisiert und als Komponentenstatus verwaltet. Das Objekt wird zur Kommunikation mit dem AEM-Host verwendet.
+ Die Funktion `handleDiscountCodeChange` ruft den Rabatt-Code für den ausgewählten Abenteuernamen ab und aktualisiert die Statusvariable.
+ Die Funktion `addDiscountCode` stellt unter Verwendung des Objekts `guestConnection` die auszuführende RTE-Anweisung bereit. In diesem Fall handelt es sich um die Anweisung `insertContent` und das HTML-Codesnippet des tatsächlichen Rabatt-Codes, der in den RTE eingefügt werden soll.

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
