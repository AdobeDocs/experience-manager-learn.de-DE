---
title: Hinzufügen bearbeitbarer fester Komponenten zu Remote-SPA
description: Erfahren Sie, wie Sie bearbeitbare feste Komponenten zu einer Remote-SPA hinzufügen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---

# Bearbeitbare feste Komponenten

Bearbeitbare React-Komponenten können &quot;fest&quot;oder in den SPA-Ansichten hartcodiert sein. Auf diese Weise können Entwickler SPA Editor-kompatible Komponenten in die SPA-Ansichten platzieren und es Benutzern ermöglichen, den Inhalt der Komponenten im AEM Editor SPA zu erstellen.

![Feste Komponenten](./assets/spa-fixed-component/intro.png)

In diesem Kapitel ersetzen wir den Titel der Home-Ansicht &quot;Current Adventures&quot;, der hartcodierten Text in `Home.js` mit einer festen, aber bearbeitbaren Titelkomponente. Feste Komponenten garantieren die Platzierung des Titels, ermöglichen aber auch die Erstellung des Titeltextes und die Änderung außerhalb des Entwicklungszyklus.

## WKND-App aktualisieren

So fügen Sie eine __Fest__ -Komponente in der Startansicht:

+ Importieren Sie die Komponente Titel der AEM React-Kernkomponente und registrieren Sie sie im Ressourcentyp des Projekts Titel .
+ Platzieren Sie die bearbeitbare Titelkomponente in der SPA-Startansicht

### Importieren in die Titelkomponente der AEM React-Kernkomponente

Ersetzen Sie in der SPA-Startansicht den hartcodierten Text. `<h2>Current Adventures</h2>` mit der Komponente Titel der AEM React-Kernkomponenten. Bevor die Titelkomponente verwendet werden kann, müssen wir Folgendes tun:

1. Importieren Sie die Titelkomponente aus `@adobe/aem-core-components-react-base`
1. Registrieren Sie sie mithilfe von `withMappable` , damit Entwickler sie in die SPA platzieren können
1. Registrieren Sie sich auch bei `MapTo` damit sie in [Container-Komponente später](./spa-container-component.md).

Gehen Sie hierfür wie folgt vor:

1. Öffnen Sie das Remote SPA-Projekt unter `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in Ihrer IDE
1. Erstellen einer React-Komponente unter `react-app/src/components/aem/AEMTitle.js`
1. Fügen Sie den folgenden Code zu `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Lesen Sie die Kommentare des Codes für die Implementierungsdetails.

Die `AEMTitle.js` sollte wie folgt aussehen:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Verwenden der React AEMTitle-Komponente

Nachdem die Titelkomponente der AEM React-Kernkomponente in registriert ist und in der React-App zur Verwendung verfügbar ist, ersetzen Sie den hartcodierten Titeltext in der Startansicht.

1. Bearbeiten `react-app/src/Home.js`
1. Im `Home()` unten den hartcodierten Titel durch den neuen `AEMTitle` component:

   ```
   <h2>Current Adventures</h2>
   ```

   durch

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Aktualisieren `Home.js` mit dem folgenden Code:

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Die `Home.js` sollte wie folgt aussehen:

![Home.js](./assets/spa-fixed-component/home-js.png)

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

+ Import aus und Wiederverwendung einer AEM React-Kernkomponente im SPA
+ Fügen Sie eine feste, aber bearbeitbare Komponente zum SPA hinzu.
+ Erstellen Sie die feste Komponente in AEM
+ Anzeigen der erstellten Inhalte in der Remote-SPA

## Nächste Schritte

Die nächsten Schritte sind [Fügen Sie eine AEM ResponsiveGrid-Container-Komponente hinzu](./spa-container-component.md) auf die SPA, die es dem Autor ermöglicht, der SPA Komponenten hinzuzufügen und sie zu bearbeiten!
