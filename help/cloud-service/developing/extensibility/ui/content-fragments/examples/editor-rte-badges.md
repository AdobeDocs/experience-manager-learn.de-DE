---
title: Hinzufügen von Abzeichen zum Rich-Text-Editor (RTE)
description: Erfahren Sie, wie Sie im AEM Inhaltsfragment-Editor Abzeichen zum Rich-Text-Editor (RTE) hinzufügen
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: 8e99c660fed409d44d34cf4edf6bf1b59fa29e34
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---


# Hinzufügen von Abzeichen zum Rich-Text-Editor (RTE)

![Beispiel für Inhaltsfragment-Editor-Abzeichen](./assets/rte/rte-badge-home.png){align="center"}

[Rich-Text-Editor-Badge](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  sind Erweiterungen, die Text im Rich-Text-Editor (RTE) nicht bearbeitbar machen. Das bedeutet, dass ein als solches deklariertes Zeichen nur vollständig entfernt und nicht teilweise bearbeitet werden kann. Diese Abzeichen unterstützen auch eine spezielle Färbung im RTE, die den Inhaltsautoren deutlich zeigt, dass der Text ein Abzeichen ist und daher nicht bearbeitbar ist. Darüber hinaus bieten sie visuelle Hinweise zur Bedeutung des Badge-Textes.

Der häufigste Anwendungsfall für RTE-Abzeichen besteht darin, diese zusammen mit [RTE-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Dadurch können Inhalte, die vom RTE-Widget in den RTE eingefügt werden, nicht bearbeitet werden.

In der Regel werden die Abzeichen in Verbindung mit den Widgets verwendet, um den dynamischen Inhalt hinzuzufügen, der eine externe Systemabhängigkeit aufweist, aber _Inhaltsautoren können nicht ändern_ den hinzugefügten dynamischen Inhalt, um die Integrität zu wahren. Sie können nur als ganzes Element entfernt werden.

Die **Badges** werden der **RTE** im Inhaltsfragment-Editor mit der `rte` Erweiterungspunkt. Verwenden `rte` des Erweiterungspunkts `getBadges()` -Methode werden ein oder mehrere Abzeichen hinzugefügt.

Dieses Beispiel zeigt, wie Sie ein Widget mit dem Namen _Großgruppenbuchungs-Kundendienst_ , um die WKND-abenteuerspezifischen Kundendienstdetails wie **Name des Vertreters** und **Telefonnummer** in einem RTE-Inhalt. Verwenden der Badges-Funktion **Telefonnummer** hergestellt **nicht bearbeitbar** WKND-Inhaltsautoren können jedoch den repräsentativen Namen bearbeiten.

Außerdem wird die **Telefonnummer** anders formatiert (blau), was ein zusätzliches Anwendungsbeispiel für die Badges-Funktion ist.

Um die Dinge einfach zu halten, verwendet dieses Beispiel den [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) Framework zum Entwickeln der Widget- oder Dialogbenutzeroberfläche und der hartcodierten WKND-Kundendienst-Telefonnummern. Um die Nichtbearbeitung und den anderen Stil-Aspekt des Inhalts zu steuern, muss die `#` wird in der `prefix` und `suffix` -Attribut der Badges-Definition.

## Erweiterungspunkte

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `rte` , um dem RTE im Inhaltsfragment-Editor ein Abzeichen hinzuzufügen.

| AEM Benutzeroberfläche erweitert | Erweiterungspunkte |
| ------------------------ | --------------------- | 
| [Inhaltsfragmente-Editor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Abzeichen](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) und [Rich-Text-Editor-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Beispielerweiterung

Im folgenden Beispiel wird eine _Großgruppenbuchungs-Kundendienst_ Widget. Durch Drücken der `{` -Taste im RTE-Kontextmenü geöffnet. Durch Auswahl der _Großgruppenbuchungs-Kundendienst_ im Kontextmenü das benutzerdefinierte Modal geöffnet.

Sobald die gewünschte Kundendienstnummer aus dem Modal hinzugefügt wurde, machen die Abzeichen die _Telefonnummer nicht bearbeitbar_ und formatiert es in blauer Farbe.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, die der `index.html` route ist der Einstiegspunkt für die AEM-Erweiterung und definiert:

+ Die Badge-Definition ist definiert in `getBadges()` mit den Konfigurationsattributen `id`, `prefix`, `suffix`, `backgroundColor` und `textColor`.
+ In diesem Beispiel wird die `#` -Zeichen verwendet wird, um die Grenzen dieses Zeichens zu definieren - d. h. jede Zeichenfolge im RTE, die von `#` wird als eine Instanz dieses Badge behandelt.

Weitere Informationen finden Sie in den Schlüsseldetails des RTE-Widgets:

+ Die Widget-Definition in `getWidgets()` Funktion mit `id`, `label` und `url` -Attribute.
+ Die `url` -Attributwert, ein relativer URL-Pfad (`/index.html#/largeBookingsCustomerService`), um das Modal zu laden.


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

          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Hinzufügen `largeBookingsCustomerService` Route in `App.js`{#add-widgets-route}

In der Haupt-React-Komponente `App.js`, fügen Sie die `largeBookingsCustomerService` -Route, um die Benutzeroberfläche für den oben genannten relativen URL-Pfad zu rendern.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Erstellen `LargeBookingsCustomerService` React-Komponente{#create-widget-react-component}

Die Benutzeroberfläche des Widgets oder Dialogfelds wird mit der [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) Framework.

Der React-Komponenten-Code beim Hinzufügen der Kundendienstdetails umschließen die Telefonnummernvariable mit der `#` registriertes Zeichen, um es in Badges zu konvertieren, z. B. `#${phoneNumber}#`, sodass es nicht bearbeitbar ist.

Hier sind die wichtigsten Highlights von `LargeBookingsCustomerService` code:

+ Die Benutzeroberfläche wird mithilfe von React Spectrum-Komponenten gerendert, z. B. [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Schaltfläche](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ Die `largeGroupCustomerServiceList` -Array verfügt über eine fest codierte Zuordnung von repräsentativem Namen und Telefonnummer. In einem realen Szenario können diese Daten von der Adobe AppBuilder-Aktion oder von externen Systemen oder von eigenem oder Cloud-Provider-basiertem API-Gateway abgerufen werden.
+ Die `guestConnection` wird mithilfe der `useEffect` [React-Hook](https://react.dev/reference/react/useEffect) und als Komponentenstatus verwaltet werden. Sie wird zur Kommunikation mit dem AEM-Host verwendet.
+ Die `handleCustomerServiceChange` -Funktion ruft den repräsentativen Namen und die Telefonnummer ab und aktualisiert die Komponentenstatusvariablen.
+ Die `addCustomerServiceDetails` Funktion verwenden `guestConnection` -Objekt stellt die auszuführende RTE-Anweisung bereit. In diesem Fall `insertContent` -Anweisung und HTML-Code-Snippet.
+ Um **Telefonnummer nicht bearbeitbar** mit Badges, die `#` Sonderzeichen vor und nach dem `phoneNumber` beispielsweise `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
