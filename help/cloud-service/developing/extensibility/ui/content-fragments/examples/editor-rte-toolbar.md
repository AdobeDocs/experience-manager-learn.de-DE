---
title: Hinzufügen benutzerdefinierter Schaltflächen zur Rich-Text-Editor (RTE)-Symbolleiste
description: Erfahren Sie, wie Sie eine benutzerdefinierte Schaltfläche zur Rich-Text-Editor-Symbolleiste (RTE) im AEM Inhaltsfragment-Editor hinzufügen.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: c54d078c6282f8ace936dd4a9ee0d5cc39490230
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# Hinzufügen benutzerdefinierter Schaltflächen zur Rich-Text-Editor (RTE)-Symbolleiste

![Beispiel für eine Erweiterung der Symbolleiste des Inhaltsfragment-Editors](./assets/rte-toolbar/hero.png){align="center"}

Benutzerdefinierte Schaltflächen können der **RTE-Symbolleiste** im Inhaltsfragment-Editor mit der `rte` Erweiterungspunkt. In diesem Beispiel wird gezeigt, wie eine benutzerdefinierte Schaltfläche namens _Tipp hinzufügen_ in der RTE-Symbolleiste und ändern Sie den Inhalt im RTE.

Verwenden `rte` des Erweiterungspunkts `getCustomButtons()` -Methode können eine oder mehrere benutzerdefinierte Schaltflächen zum **RTE-Symbolleiste**. Es ist auch möglich, standardmäßige RTE-Schaltflächen hinzuzufügen oder zu entfernen, wie _Kopieren, Einfügen, Fett und Kursiv_ using `getCoreButtons()` und `removeButtons)` -Methoden.

Dieses Beispiel zeigt, wie Sie einen hervorgehobenen Hinweis oder eine QuickInfo mithilfe von benutzerdefinierten _Tipp hinzufügen_ Symbolleiste. Der hervorgehobene Hinweis- oder Tippinhalt hat eine spezielle Formatierung, die über HTML-Elemente und die zugehörigen CSS-Klassen angewendet wird. Der Platzhalterinhalt und der HTML-Code werden mithilfe der `onClick()` Callback-Methode der `getCustomButtons()`.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich auf den Erweiterungspunkt `rte` , um der RTE-Symbolleiste im Inhaltsfragment-Editor eine benutzerdefinierte Schaltfläche hinzuzufügen.

| AEM Benutzeroberfläche erweitert | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragmente-Editor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Symbolleiste](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Beispielerweiterung

Im folgenden Beispiel wird eine _Tipp hinzufügen_ benutzerdefinierte Schaltfläche in der RTE-Symbolleiste. Die Klickaktion fügt den Platzhaltertext an der aktuellen Caret-Position im RTE ein.

Der Code zeigt, wie Sie die benutzerdefinierte Schaltfläche mit einem Symbol hinzufügen und die Klick-Handler-Funktion registrieren.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, der der Route index.html zugeordnet ist, ist der Einstiegspunkt für die AEM Erweiterung und definiert:

+ Die Definition der RTE-Symbolleiste in `getCustomButtons()` Funktion mit `id, tooltip and icon` -Attribute.
+ Der Klick-Handler für die Schaltfläche im `onClick()` -Funktion.
+ Die Click-Handler-Funktion erhält die `state` -Objekt als Argument verwenden, um den RTE-Inhalt im HTML- oder Textformat abzurufen. In diesem Beispiel wird sie jedoch nicht verwendet.
+ Die Click-Handler-Funktion gibt ein Anweisungen-Array zurück. Dieses Array hat ein Objekt mit `type` und `value` -Attribute. Um den Inhalt einzufügen, muss die `value` -Attribut-HTML-Codefragment, die `type` -Attribut verwendet `insertContent`. Wenn es einen Anwendungsfall gibt, um den Inhalt zu ersetzen, verwenden Sie den `replaceContent` Anweisungstyp.

Die `insertContent` -Wert eine HTML-Zeichenfolge ist, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Die CSS-Klassen `cmp-contentfragment__element-tip` verwendet, um den Wert anzuzeigen, werden nicht im Widget definiert, sondern im Web-Erlebnis implementiert, in dem dieses Inhaltsfragment-Feld angezeigt wird.


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

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
