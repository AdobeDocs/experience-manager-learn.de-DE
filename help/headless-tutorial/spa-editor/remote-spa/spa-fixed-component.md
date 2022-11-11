---
title: Hinzufügen bearbeitbarer fester Komponenten zu Remote-SPA
description: Erfahren Sie, wie Sie bearbeitbare feste Komponenten zu einer Remote-SPA hinzufügen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# Bearbeitbare feste Komponenten

Bearbeitbare React-Komponenten können &quot;fest&quot;oder in den SPA-Ansichten hartcodiert sein. Auf diese Weise können Entwickler SPA Editor-kompatible Komponenten in die SPA-Ansichten platzieren und es Benutzern ermöglichen, den Inhalt der Komponenten im AEM Editor SPA zu erstellen.

![Feste Komponenten](./assets/spa-fixed-component/intro.png)

In diesem Kapitel ersetzen wir den Titel der Home-Ansicht &quot;Current Adventures&quot;, der hartcodierten Text in `Home.js` mit einer festen, aber bearbeitbaren Titelkomponente. Feste Komponenten garantieren die Platzierung des Titels, ermöglichen aber auch die Erstellung des Titeltextes und die Änderung außerhalb des Entwicklungszyklus.

## WKND-App aktualisieren

So fügen Sie eine __Fest__ -Komponente in der Startansicht:

+ Erstellen Sie eine benutzerdefinierte bearbeitbare Titelkomponente und registrieren Sie sie im Ressourcentyp des Projekts Titel .
+ Platzieren Sie die bearbeitbare Titelkomponente in der SPA-Startansicht

### Erstellen einer bearbeitbaren React Title-Komponente

Ersetzen Sie in der SPA-Startansicht den hartcodierten Text. `<h2>Current Adventures</h2>` mit einer benutzerdefinierten bearbeitbaren Titelkomponente. Bevor die Titelkomponente verwendet werden kann, müssen wir Folgendes tun:

1. Erstellen einer benutzerdefinierten Komponente &quot;Title React&quot;
1. Dekorieren Sie die benutzerdefinierte Titelkomponente mit Methoden aus `@adobe/aem-react-editable-components` , damit es bearbeitbar wird.
1. Registrieren Sie die bearbeitbare Titelkomponente bei `MapTo` damit sie in [Container-Komponente später](./spa-container-component.md).

Gehen Sie hierfür wie folgt vor:

1. Öffnen Sie das Remote SPA-Projekt unter `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` in Ihrer IDE
1. Erstellen einer React-Komponente unter `react-app/src/components/editable/core/Title.js`
1. Fügen Sie den folgenden Code zu `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   Beachten Sie, dass diese React-Komponente mit AEM SPA Editor noch nicht bearbeitet werden kann. Diese Basiskomponente wird im nächsten Schritt bearbeitbar gemacht.

   Lesen Sie die Kommentare des Codes für die Implementierungsdetails.

1. Erstellen einer React-Komponente unter `react-app/src/components/editable/EditableTitle.js`
1. Fügen Sie den folgenden Code zu `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   Diese `EditableTitle` React-Komponente umschließt die `Title` React-Komponente, Umbrechen und Dekorieren, um sie im AEM SPA Editor bearbeitbar zu sein.

### Verwenden der React EditableTitle-Komponente

Nachdem die Komponente &quot;EditableTitle React&quot;in der React-App registriert und für die Verwendung in der React-App verfügbar ist, ersetzen Sie den hartcodierten Titeltext in der Startansicht.

1. Bearbeiten `react-app/src/components/Home.js`
1. Im `Home()` unten, importieren `EditableTitle` und ersetzen Sie den hartcodierten Titel durch den neuen `AEMTitle` component:

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Die `Home.js` sollte wie folgt aussehen:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Erstellen Sie die Titelkomponente in AEM

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND-App__
1. Tippen __Startseite__ und wählen Sie __Bearbeiten__ in der oberen Aktionsleiste
1. Auswählen __Bearbeiten__ über die Auswahl des Bearbeitungsmodus oben rechts im Seiteneditor
1. Bewegen Sie den Mauszeiger über den Standardtiteltext unter dem WKND-Logo und über der Abenteuerliste, bis der blaue Bearbeitungsentwurf angezeigt wird.
1. Tippen Sie auf , um die Aktionsleiste der Komponente anzuzeigen, und tippen Sie dann auf __Schraubenschlüssel__  bearbeiten

   ![Aktionsleiste der Komponente &quot;Titel&quot;](./assets/spa-fixed-component/title-action-bar.png)

1. Erstellen Sie die Titelkomponente:
   + Titel: __WKND Adventures__
   + Typ/Größe: __H2__

      ![Dialogfeld &quot;Titelkomponente&quot;](./assets/spa-fixed-component/title-dialog.png)

1. Tippen __Fertig__ speichern
1. Vorschau der Änderungen in AEM SPA Editor
1. Aktualisieren Sie die WKND-App, die lokal ausgeführt wird auf [http://localhost:3000](http://localhost:3000) und sehen Sie, wie sich der erstellte Titel sofort ändert.

   ![Titelkomponente in SPA](./assets/spa-fixed-component/title-final.png)

## Herzlichen Glückwunsch!

Sie haben der WKND-App eine feste, bearbeitbare Komponente hinzugefügt! Sie wissen jetzt, wie:

+ Erstellen einer festen, aber bearbeitbaren Komponente für die SPA
+ Erstellen Sie die feste Komponente in AEM
+ Anzeigen der erstellten Inhalte in der Remote-SPA

## Nächste Schritte

Die nächsten Schritte sind [Fügen Sie eine AEM ResponsiveGrid-Container-Komponente hinzu](./spa-container-component.md) auf die SPA, die es dem Autor ermöglicht, der SPA Komponenten hinzuzufügen und sie zu bearbeiten!
