---
title: Aktionsleisten-Erweiterungen der AEM Inhaltsfragment-Konsole
description: Erfahren Sie, wie Sie eine AEM Inhaltsfragment-Konsolenaktionserweiterungen erstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 1%

---


# Erweiterung der Aktionsleiste

![Erweiterung der Aktionsleiste](./assets/action-bar/action-bar.png){align="center"}

Erweiterungen, die eine Aktionsleiste enthalten, fügen eine Schaltfläche zur Aktion der AEM Inhaltsfragment-Konsole hinzu, die angezeigt wird, wenn __1 oder mehr__ Inhaltsfragmente sind ausgewählt. Da die Schaltflächen zur Erweiterung der Aktionsleiste nur angezeigt werden, wenn mindestens ein Inhaltsfragment ausgewählt ist, werden sie normalerweise auf die ausgewählten Inhaltsfragmente angewendet. Beispiele dafür sind:

+ Aufrufen eines Geschäftsprozesses oder Workflows für die ausgewählten Inhaltsfragmente.
+ Aktualisieren oder Ändern der Daten der ausgewählten Inhaltsfragmente.

## Erweiterungsregistrierung

`ExtensionRegistration.js` ist der Einstiegspunkt für die AEM Erweiterung und definiert:

1. den Erweiterungstyp; im Fall einer Schaltfläche in der Symbolleiste.
1. Die Definition der Erweiterungsschaltfläche in `getButton()` -Funktion.
1. Der Klick-Handler für die Schaltfläche im `onClick()` -Funktion.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Modal

![Modal](./assets/modal/modal.png)

AEM Aktionsleisten-Erweiterungen der Inhaltsfragmentkonsole erfordern möglicherweise Folgendes:

+ Zusätzliche Eingabe des Benutzers zur Durchführung der gewünschten Aktion.
+ Die Möglichkeit, den Benutzern Informationen zum Ergebnis der Aktion bereitzustellen.

Um diese Anforderungen zu unterstützen, ermöglicht die Erweiterung der AEM Inhaltsfragment-Konsole ein benutzerdefiniertes Modal, das als React-Anwendung gerendert wird.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Zum Erstellen eines Modals springen</p>
      <p class="has-text-blackest">Erfahren Sie, wie Sie ein Modal erstellen, das angezeigt wird, wenn Sie auf die Schaltfläche zum Erweitern der Aktionsleiste klicken.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Erfahren Sie, wie Sie ein Modal erstellen">Erfahren Sie, wie Sie ein Modal erstellen</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Kein Modal

Gelegentlich erfordern AEM Aktionsleisten-Erweiterungen der Inhaltsfragmentkonsole keine weitere Interaktion mit dem Benutzer, z. B.:

+ Rufen Sie einen Backend-Prozess auf, für den keine Benutzereingabe erforderlich ist, z. B. Import oder Export.

In diesen Fällen ist für die AEM Inhaltsfragment-Konsolenerweiterung keine [modal](#modal)und führen Sie die Arbeit direkt in der Symbolleiste durch. `onClick` Handler.

Die AEM Content Fragment-Konsolenerweiterung ermöglicht es einem Fortschrittsanzeiger, die AEM Inhaltsfragmentkonsole während der Arbeit zu überlagern, wodurch der Benutzer an weiteren Aktionen gehindert wird. Die Verwendung der Fortschrittsanzeige ist optional, aber nützlich, um den Fortschritt der synchronen Arbeit dem Benutzer mitzuteilen.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```