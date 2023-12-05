---
title: Hinzufügen benutzerdefinierter Schaltflächen zur Rich-Text-Editor(RTE)-Symbolleiste
description: Erfahren Sie, wie Sie eine benutzerdefinierte Schaltfläche zur Rich-Text-Editor(RTE)-Symbolleiste im AEM-Inhaltsfragmenteditor hinzufügen.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 397
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 95%

---

# Hinzufügen benutzerdefinierter Schaltflächen zur Rich-Text-Editor(RTE)-Symbolleiste

Erfahren Sie, wie Sie eine benutzerdefinierte Schaltfläche zur Rich-Text-Editor (RTE)-Symbolleiste im AEM Inhaltsfragment-Editor hinzufügen.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

Benutzerdefinierte Schaltflächen können der **RTE-Symbolleiste** im Inhaltsfragmenteditor über den Erweiterungspunkt `rte` hinzugefügt werden. In diesem Beispiel wird gezeigt, wie eine benutzerdefinierte Schaltfläche zum _Hinzufügen eines Tipps_ in der RTE-Symbolleiste hinzugefügt und der Inhalt im RTE geändert wird.

Über die Methode `getCustomButtons()` des Erweiterungspunkts `rte` ist es möglich, eine oder mehrere benutzerdefinierte Schaltflächen zur **RTE-Symbolleiste** hinzuzufügen. Ebenfalls können standardmäßige RTE-Schaltflächen wie _„Kopieren“, „Einfügen“, „Fett“ und „Kursiv“_ mit der Methode `getCoreButtons()` bzw. `removeButtons)` hinzugefügt oder entfernt werden.

Dieses Beispiel zeigt, wie Sie einen hervorgehobenen Hinweis oder einen Tipp mithilfe der benutzerdefinierten Symbolleisten-Schaltfläche zum _Hinzufügen eines Tipps_ einfügen können. Der Inhalt des hervorgehobenen Hinweises oder Tipps hat eine spezielle Formatierung, die über HTML-Elemente und die zugehörigen CSS-Klassen angewendet wird. Der Platzhalterinhalt und der HTML-Code werden mithilfe der `onClick()`-Callback-Methode von `getCustomButtons()` eingefügt.

## Erweiterungspunkt

Dieses Beispiel erstreckt sich bis zum Erweiterungspunkt `rte`, um der RTE-Symbolleiste im Inhaltsfragmenteditor eine benutzerdefinierte Schaltfläche hinzuzufügen.

| Erweiterte AEM-Benutzeroberfläche | Erweiterungspunkt |
| ------------------------ | --------------------- | 
| [Inhaltsfragmenteditor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Rich-Text-Editor-Symbolleiste](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Beispielerweiterung

Im folgenden Beispiel wird eine benutzerdefinierte Schaltfläche zum _Hinzufügen eines Tipps_ in der RTE-Symbolleiste erstellt. Die Klickaktion fügt den Platzhaltertext an der aktuellen Caret-Position im RTE ein.

Der Code zeigt, wie Sie die benutzerdefinierte Schaltfläche mit einem Symbol hinzufügen und die Klick-Handler-Funktion registrieren.

### Registrierung der Erweiterung

`ExtensionRegistration.js`, die der Route „index.html“ zugeordnet ist, ist der Einstiegspunkt für die AEM-Erweiterung und definiert Folgendes:

+ Die Definition der RTE-Symbolleisten-Schaltfläche in der Funktion `getCustomButtons()` mit den Attributen `id, tooltip and icon`.
+ Den Klick-Handler für die Schaltfläche in der Funktion `onClick()`.
+ Die Klick-Handler-Funktion erhält das `state`-Objekt als Argument, um den RTE-Inhalt im HTML- oder Textformat abzurufen. In diesem Beispiel wird es jedoch nicht verwendet.
+ Die Klick-Handler-Funktion gibt ein Anweisungs-Array zurück. Dieses Array verfügt über ein Objekt mit den Attributen `type` und `value`. Um den Inhalt einzufügen, verwendet das `value`-Attribut ein HTML-Codesnippet und das `type`-Attribut den `insertContent`. Wenn es einen Anwendungsfall gibt, um den Inhalt zu ersetzen, verwenden Sie den Anweisungstyp `replaceContent`.

Der Wert von `insertContent` ist eine HTML-Zeichenfolge: `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Die zum Anzeigen des Werts verwendeten CSS-Klassen `cmp-contentfragment__element-tip` werden nicht im Widget definiert, sondern in dem Web-Erlebnis implementiert, in dem dieses Inhaltsfragmentfeld angezeigt wird.


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
