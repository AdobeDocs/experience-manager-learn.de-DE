---
title: Hinzufügen von Badges zum Rich-Text-Editor (RTE)
description: Erfahren Sie, wie Sie im AEM-Inhaltsfragment-Editor Badges zum Rich-Text-Editor (RTE) hinzufügen
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 100%

---

# Hinzufügen von Badges zum Rich-Text-Editor (RTE)

Erfahren Sie, wie Sie im AEM-Inhaltsfragmenteditor Badges zum Rich-Text-Editor (RTE) hinzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[Rich-Text-Editor-Badges](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) sind Erweiterungen, die Text im Rich-Text-Editor (RTE) unbearbeitbar machen. Das bedeutet, dass ein als solches deklariertes Badge nur vollständig entfernt, aber nicht teilweise bearbeitet werden kann. Diese Badges unterstützen auch eine spezielle Färbung im RTE, die Inhaltsautorinnen und Inhaltsautoren deutlich zeigt, dass der Text ein Badge ist und daher nicht bearbeitbar ist. Darüber hinaus bieten sie visuelle Hinweise zur Bedeutung des Badge-Textes.

Der häufigste Anwendungsfall für RTE-Badges besteht darin, diese zusammen mit [RTE-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) zu verwenden. Dadurch können Inhalte, die vom RTE-Widget in den RTE eingefügt werden, nicht bearbeitet werden.

Typischerweise werden die Badges in Verbindung mit den Widgets verwendet, um den dynamischen Inhalt hinzuzufügen, der eine externe Systemabhängigkeit hat, aber _Inhaltsautorinnen und Inhaltsautoren können den eingefügten dynamischen Inhalt nicht ändern_, damit die Integrität gewahrt wird. Sie können nur als ganzes Element entfernt werden.

Die **Badges** werden dem **RTE** im Inhaltsfragmenteditor über den Erweiterungspunkt `rte` hinzugefügt. Mit der Methode `getBadges()` des Erweiterungspunkts `rte` werden ein oder mehrere Badges hinzugefügt.

Dieses Beispiel zeigt, wie man ein Widget mit dem Namen _Großgruppenbuchungen-Kundenservice_ hinzufügt, um die abenteuerspezifischen WKND-Kundenservice-Details wie **Vertretername** und **Telefonnummer** innerhalb eines RTE-Inhalts zu finden, auszuwählen und hinzuzufügen. Mithilfe der Badges-Funktion wird die **Telefonnummer** **unbearbeitbar**, aber WKND-Inhaltsautorinnen und -Inhaltsautoren können den Vertreternamen bearbeiten.

Außerdem ist die **Telefonnummer** anders gestaltet (blau), was ein zusätzlicher Anwendungsfall für die Badges-Funktion ist.

Um die Dinge einfach zu halten, verwendet dieses Beispiel das [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html?lang=de)-Framework, um die Widget- oder Dialog-UI und die fest codierten Telefonnummern des WKND-Kundendienstes zu entwickeln. Um den Aspekt der Nicht-Bearbeitung und des unterschiedlichen Stils des Inhalts zu kontrollieren, wird das Zeichen `#` in den Attributen `prefix` und `suffix` der Badges-Definition verwendet.

## Erweiterungspunkte

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `rte`, um dem RTE im Inhaltsfragmenteditor ein Badge hinzuzufügen.

| Erweiterte AEM-Benutzeroberfläche | Erweiterungspunkte |
| ------------------------ | --------------------- | 
| [Inhaltsfragmenteditor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Badges](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) und [Rich-Text-Editor-Widgets](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Beispielerweiterung

Das folgende Beispiel erstellt ein Widget namens _Großgruppenbuchungen-Kundendienst_. Durch Drücken der Taste `{` im RTE wird das Kontextmenü der RTE-Widgets geöffnet. Durch Auswahl der Option _Großgruppenbuchungen-Kundenservice_ aus dem Kontextmenü wird das benutzerdefinierte Modal geöffnet.

Sobald die gewünschte Kundendienstnummer aus dem Modal hinzugefügt wird, machen die Badges die _Telefonnummer unbearbeitbar_ und gestalten sie in blauer Farbe.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, das der Route `index.html` zugeordnet ist, ist der Einstiegspunkt für die AEM-Erweiterung und definiert:

+ Die Definition des Badges wird in `getBadges()` unter Verwendung der Konfigurationsattribute `id`, `prefix`, `suffix`, `backgroundColor` und `textColor` festgelegt.
+ In diesem Beispiel wird das Zeichen `#` verwendet, um die Grenzen dieses Badges zu definieren. Das bedeutet, dass jede Zeichenkette im RTE, die von `#` umgeben ist, als eine Instanz dieses Badges behandelt wird.

Weitere Informationen sehen Sie in den Schlüsseldetails des RTE-Widgets:

+ Die Widget-Definition in der Funktion `getWidgets()` mit den Attributen `id`, `label` und `url`.
+ Der `url`-Attributwert, ein relativer URL-Pfad (`/index.html#/largeBookingsCustomerService`) zum Laden des Modals.


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

### Hinzufügen einer Route `largeBookingsCustomerService` in `App.js`{#add-widgets-route}

Fügen Sie in der React-Hauptkomponente `App.js` die Route `largeBookingsCustomerService` hinzu, um die Benutzeroberfläche für den oben genannten relativen URL-Pfad zu rendern.

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

### Erstellen der React-Komponente `LargeBookingsCustomerService`{#create-widget-react-component}

Die Widget- oder Dialog-UI wird mit dem [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html?lang=de)-Framework erstellt.

Der Code der React-Komponente beim Hinzufügen der Kundendienstdetails umgibt die Telefonnummer-Variable mit dem Zeichen für `#`-registrierte Badges, um sie in Badges zu konvertieren, wie `#${phoneNumber}#`, und macht sie somit unbearbeitbar.

Hier die wichtigsten Punkte vom `LargeBookingsCustomerService`-Code:

+ Die UI wird mit React Spectrum-Komponenten gerendert, z. B.[ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html?lang=de), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html?lang=de) und [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html?lang=de)
+ Das `largeGroupCustomerServiceList`-Array verfügt über eine fest kodierte Zuordnung von Name und Telefonnummer der oder des Mitarbeitenden. In einem realen Szenario können diese Daten von der Adobe AppBuilder-Aktion oder von externen Systemen oder von einem selbst entwickelten oder auf einem Cloud-Anbieter basierenden API-Gateway abgerufen werden.
+ Die `guestConnection` wird mithilfe des `useEffect` [React-Hooks](https://react.dev/reference/react/useEffect) initialisiert und als Komponentenstatus verwaltet. Das Objekt wird zur Kommunikation mit dem AEM-Host verwendet.
+ Die Funktion `handleCustomerServiceChange` ruft den Namen und die Telefonnummer der oder des Mitarbeitenden ab und aktualisiert die Komponentenstatusvariablen.
+ Die Funktion `addCustomerServiceDetails` stellt unter Ausführung des Objekts `guestConnection` die auszuführende RTE-Anweisung bereit. In diesem Fall die Anweisung `insertContent` und ein HTML-Code-Snippet.
+ Um die **Telefonnummer mit Hilfe von Badges uneditierbar** zu machen, wird das Sonderzeichen `#` vor und nach der `phoneNumber`-Variablen hinzugefügt, wie in `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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
