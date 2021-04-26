---
title: hinzufügen bearbeitbare feste Komponenten auf einem Remote-SPA
description: Erfahren Sie, wie Sie bearbeitbare feste Komponenten zu einer Remote-SPA hinzufügen.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Hauptkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---


# Bearbeitbare feste Komponenten

Bearbeitbare Reaktionskomponenten können &quot;fest&quot;oder in die SPA Ansichten hartcodiert werden. Auf diese Weise können Entwickler SPA Editor-kompatible Komponenten in die SPA Ansichten einfügen und die Komponenteninhalte in AEM Editor bearbeiten.

![Feste Komponenten](./assets/spa-fixed-component/intro.png)

In diesem Kapitel ersetzen wir den Titel der Home-Ansicht &quot;Current Adventures&quot;, der in `Home.js` hartcodiert ist, durch eine feste, aber bearbeitbare Titelkomponente. Feste Komponenten garantieren die Platzierung des Titels, ermöglichen aber auch, dass der Titeltext verfasst und außerhalb des Entwicklungszyklus geändert werden kann.

## WKND-App aktualisieren

So fügen Sie der Startseite-Ansicht eine fixierte Komponente hinzu:

+ Importieren Sie die Komponente &quot;Titel der AEM-Reaktions-Kernkomponente&quot;und registrieren Sie sie im Ressourcentyp des Projekts
+ Platzieren Sie die bearbeitbare Titelkomponente auf der SPA Home-Ansicht

### Importieren in die Komponente &quot;Titel&quot;der AEM-React-Core-Komponente

Ersetzen Sie in der Ansicht SPA Home den hartkodierten Text `<h2>Current Adventures</h2>` durch die Komponente AEM React Core Components&#39; Title. Bevor die Komponente &quot;Titel&quot;verwendet werden kann, müssen wir:

1. Importieren der Komponente &quot;Title&quot;von `@adobe/aem-core-components-react-base`
1. Registrieren Sie es mit `withMappable`, damit Entwickler es in der SPA platzieren können
1. Registrieren Sie sich auch bei `MapTo`, damit sie später in [Container verwendet werden kann.](./spa-container-component.md)

Gehen Sie hierfür wie folgt vor:

1. Öffnen Sie das Remote-SPA unter `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` in Ihrer IDE
1. Erstellen einer React-Komponente unter `react-app/src/components/aem/AEMTitle.js`
1. hinzufügen Sie den folgenden Code zu `AEMTitle.js`.

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

Die Datei `AEMTitle.js` sollte wie folgt aussehen:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Verwenden der React AEMTitle-Komponente

Ersetzen Sie jetzt, da die Komponente &quot;Titel&quot;der Komponente &quot;Titel&quot;der AEM &quot;Aktiv-Core-Komponente&quot;in der React-App registriert ist und für die Verwendung in der React-App verfügbar ist, den hartkodierten Titeltext auf der Home-Ansicht.

1. Bearbeiten `react-app/src/App.js`
1. Ersetzen Sie im `Home()` unten den hartkodierten Titel durch die neue Komponente `AEMTitle`:

   ```
   <h2>Current Adventures</h2>
   ```

   durch

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Aktualisieren Sie `Apps.js` mit dem folgenden Code:

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
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

Die Datei `Apps.js` sollte wie folgt aussehen:

![App.js](./assets/spa-fixed-component/app-js.png)

## Erstellen Sie die Komponente &quot;Titel&quot;in AEM

1. Bei AEM Author anmelden
1. Navigieren Sie zu __Sites > WKND-App__
1. Tippen Sie auf __Home__ und wählen Sie __Bearbeiten__ in der oberen Aktionsleiste
1. Wählen Sie __Bearbeiten__ aus der Auswahl im Bearbeitungsmodus oben rechts im Seiten-Editor
1. Bewegen Sie den Mauszeiger über den Standardtiteltext unter dem WKND-Logo und über die Abenteuerungs-Liste, bis die blaue Umrandung angezeigt wird
1. Tippen Sie auf , um die Aktionsleiste der Komponente anzuzeigen, und tippen Sie dann zum Bearbeiten auf das __Schraubenschlüssel__

   ![Aktionsleiste der Titelkomponente](./assets/spa-fixed-component/title-action-bar.png)

1. Erstellen Sie die Komponente &quot;Titel&quot;:
   + Titel: __WKND Adventures__
   + Typ/Größe: __H2__

      ![Dialogfeld &quot;Titelkomponente&quot;](./assets/spa-fixed-component/title-dialog.png)

1. Tippen Sie zum Speichern auf __Fertig__
1. Vorschau Ihrer Änderungen im AEM SPA Editor
1. Aktualisieren Sie die WKND-App, die lokal auf [http://localhost:3000](http://localhost:3000) ausgeführt wird, und sehen Sie, wie sich die Änderungen an dem erstellten Titel sofort widerspiegeln.

   ![Titelkomponente in SPA](./assets/spa-fixed-component/title-final.png)

## Herzlichen Glückwunsch!

Sie haben der WKND-App eine feste, bearbeitbare Komponente hinzugefügt! Sie wissen jetzt, wie:

+ Importieren Sie eine AEM React-Core-Komponente in die SPA und verwenden Sie sie wieder.
+ hinzufügen einer festen, aber bearbeitbaren Komponente im SPA
+ Erstellen Sie die feste Komponente in AEM
+ Anzeigen der erstellten Inhalte in der Remote-SPA

## Nächste Schritte

Die nächsten Schritte sind [Hinzufügen einer AEM ResponsiveGrid-Container-Komponente](./spa-container-component.md) zu der SPA, die Autoren das Hinzufügen und Bearbeiten von Komponenten zur SPA ermöglicht!
