---
title: Hinzufügen bearbeitbarer Container-Komponenten zu Remote-SPA
description: Erfahren Sie, wie Sie bearbeitbare Container-Komponenten zu einer Remote-SPA hinzufügen, die es AEM Autoren ermöglichen, Komponenten per Drag-and-Drop in sie zu ziehen.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Kernkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 2%

---


# Bearbeitbare Container-Komponenten

[Die korrigierte ](./spa-fixed-component.md) Komponente bietet eine gewisse Flexibilität beim Authoring SPA Inhalts. Dieser Ansatz ist jedoch starr und erfordert, dass Entwickler die genaue Zusammensetzung des bearbeitbaren Inhalts definieren. Um die Erstellung außergewöhnlicher Erlebnisse durch Autoren zu unterstützen, unterstützt SPA Editor die Verwendung von Container-Komponenten in der SPA. Container-Komponenten ermöglichen es Autoren, zulässige Komponenten per Drag &amp; Drop in den Container zu ziehen und sie wie beim herkömmlichen AEM Sites-Authoring zu erstellen!

![Bearbeitbare Container-Komponenten](./assets/spa-container-component/intro.png)

In diesem Kapitel fügen wir der Startansicht einen bearbeitbaren Container hinzu, der es Autoren ermöglicht, Rich-Content-Erlebnisse mit AEM React-Kernkomponenten direkt in der SPA zu erstellen und anzuordnen.

## WKND-App aktualisieren

Hinzufügen einer Container-Komponente zur Startansicht:

+ Importieren der Komponente &quot;ResponsiveGrid&quot;der AEM React Editable-Komponente
+ Importieren und registrieren Sie AEM React-Kernkomponenten (Text und Bild) zur Verwendung in der Container-Komponente

### Importieren in die Komponente des responsivenGrid-Containers

Um einen bearbeitbaren Bereich in der Startansicht zu platzieren, müssen wir:

1. Importieren Sie die Komponente &quot;ResponsiveGrid&quot;aus `@adobe/aem-react-editable-components`
1. Registrieren Sie sie mit `withMappable` , damit Entwickler sie in der SPA platzieren können.
1. Registrieren Sie sich auch bei `MapTo` , damit sie in anderen Container-Komponenten wiederverwendet werden kann, sodass Container effektiv verschachtelt werden.

Gehen Sie hierfür wie folgt vor:

1. Öffnen Sie das SPA in Ihrer IDE.
1. Erstellen einer React-Komponente unter `src/components/aem/AEMResponsiveGrid.js`
1. Fügen Sie `AEMResponsiveGrid.js` den folgenden Code hinzu:

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

Der Code ähnelt `AEMTitle.js`, dass [die Titelkomponente der AEM-Kernkomponenten](./spa-fixed-component.md) importiert hat.


Die Datei `AEMResponsiveGrid.js` sollte wie folgt aussehen:

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### Verwenden der SPA-Komponente AEMResponsiveGrid

Nachdem AEM Komponente ResponsiveGrid in registriert ist und für die Verwendung in der SPA verfügbar ist, können wir sie in der Startansicht platzieren.

1. `react-app/src/App.js` öffnen und bearbeiten
1. Importieren Sie die Komponente `AEMResponsiveGrid` und platzieren Sie sie über der Komponente `<AEMTitle ...>`.
1. Legen Sie die folgenden Attribute für die Komponente `<AEMResponsiveGrid...>` fest
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Dadurch wird diese `AEMResponsiveGrid`-Komponente angewiesen, ihren Inhalt aus der AEM Ressource abzurufen:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   Der `itemPath` wird dem Knoten `responsivegrid` zugeordnet, der in der `Remote SPA Page`-AEM-Vorlage definiert ist, und wird automatisch auf neuen AEM Seiten erstellt, die aus der `Remote SPA Page`-AEM-Vorlage erstellt wurden.

   Aktualisieren Sie `App.js` , um die Komponente `<AEMResponsiveGrid...>` hinzuzufügen.

   ```
   ...
   import AEMResponsiveGrid from './components/aem/AEMResponsiveGrid';
   ...
   
   function Home() {
   return (
       <div className="Home">
           <AEMResponsiveGrid
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='root/responsivegrid'/>
   
           <AEMTitle
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='title'/>
           <Adventures />
       </div>
   );
   }
   ```

Die Datei `Apps.js` sollte wie folgt aussehen:

![App.js](./assets/spa-container-component/app-js.png)

## Erstellen bearbeitbarer Komponenten

Um die volle Wirkung der flexiblen Authoring-Erlebnis-Container im SPA Editor zu erzielen. Wir haben bereits eine bearbeitbare Titelkomponente erstellt, aber lassen Sie uns noch ein paar weitere Aspekte anstellen, die es Autoren ermöglichen, Text und Bild AEM WCM-Kernkomponenten in der neu hinzugefügten Container-Komponente zu verwenden.

### Textkomponente

1. Öffnen Sie das SPA in Ihrer IDE.
1. Erstellen einer React-Komponente unter `src/components/aem/AEMText.js`
1. Fügen Sie `AEMText.js` den folgenden Code hinzu:

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

Die Datei `AEMText.js` sollte wie folgt aussehen:

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### Bildkomponente

1. Öffnen Sie das SPA in Ihrer IDE.
1. Erstellen einer React-Komponente unter `src/components/aem/AEMImage.js`
1. Fügen Sie `AEMImage.js` den folgenden Code hinzu:

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. Erstellen Sie eine SCSS-Datei `src/components/aem/AEMImage.scss` , die benutzerdefinierte Stile für `AEMImage.scss` bereitstellt. Diese Stile zielen auf die CSS-Klassen der AEM React-Kernkomponente mit BEM-Notation ab.
1. Fügen Sie die folgende SCSS zu `AEMImage.scss` hinzu.

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importieren Sie `AEMImage.scss` in `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

Die `AEMImage.js` und `AEMImage.scss` sollten wie folgt aussehen:

![AEMImage.js und AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### Importieren der bearbeitbaren Komponenten

Die neu erstellten SPA `AEMText` und `AEMImage` werden im SPA referenziert und basierend auf der von AEM zurückgegebenen JSON dynamisch instanziiert. Um sicherzustellen, dass diese Komponenten für die SPA verfügbar sind, erstellen Sie Importanweisungen für sie in `App.js`

1. Öffnen Sie das SPA in Ihrer IDE.
1. Öffnen Sie die Datei `src/App.js`
1. Fügen Sie Importanweisungen für `AEMText` und `AEMImage` hinzu.

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


Das Ergebnis sollte wie folgt aussehen:

![Apps.js](./assets/spa-container-component/app-js-imports.png)

Wenn diese Importe _nicht_ hinzugefügt werden, werden der `AEMText`- und der `AEMImage`-Code von SPA nicht aufgerufen und daher werden die Komponenten nicht für die bereitgestellten Ressourcentypen registriert.

## Container in AEM konfigurieren

AEM Container-Komponenten verwenden Richtlinien, um ihre zulässigen Komponenten anzugeben. Dies ist eine kritische Konfiguration bei der Verwendung des SPA-Editors, da nur AEM WCM-Kernkomponenten, die SPA Komponenten-Entsprechungen zugeordnet haben, vom SPA gerenderbar sind. Stellen Sie sicher, dass nur die Komponenten zulässig sind, für die wir SPA Implementierungen bereitgestellt haben:

+ `AEMTitle` zugeordnet zu  `wknd-app/components/title`
+ `AEMText` zugeordnet zu  `wknd-app/components/text`
+ `AEMImage` zugeordnet zu  `wknd-app/components/image`

So konfigurieren Sie den Reponsivegrid-Container der Vorlage Remote-SPA:

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Tools > Allgemein > Vorlagen > WKND-App__
1. Bearbeiten Sie __SPA Seite des Berichts__

   ![Richtlinien für responsive Raster](./assets/spa-container-component/templates-remote-spa-page.png)

1. Wählen Sie __Struktur__ im Modusschalter oben rechts aus.
1. Tippen Sie, um den __Layout-Container__ auszuwählen.
1. Tippen Sie in der Popup-Leiste auf das Symbol __Richtlinie__ .

   ![Richtlinien für responsive Raster](./assets/spa-container-component/templates-policies-action.png)

1. Erweitern Sie rechts auf der Registerkarte __Zulässige Komponenten__ __WKND APP - CONTENT__
1. Stellen Sie sicher, dass nur Folgendes ausgewählt ist:
   + Bild
   + Text
   + Titel

   ![Remote-SPA](./assets/spa-container-component/templates-allowed-components.png)

1. Tippen Sie auf __Fertig__

## Container in AEM erstellen

Nachdem die SPA aktualisiert wurde, um die `<AEMResponsiveGrid...>`, Wrapper für drei AEM React-Kernkomponenten (`AEMTitle`, `AEMText` und `AEMImage`) einzubetten, und AEM mit einer übereinstimmenden Vorlagenrichtlinie aktualisiert wurde, können wir mit der Inhaltserstellung in der Container-Komponente beginnen.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND App__
1. Tippen Sie auf __Home__ und wählen Sie __Edit__ aus der oberen Aktionsleiste
   + Eine Textkomponente &quot;Hello World&quot;wird angezeigt, da diese automatisch hinzugefügt wurde, wenn das Projekt über den AEM Projektarchetyp generiert wurde
1. Wählen Sie in der Modusauswahl oben rechts im Seiteneditor __Bearbeiten__ aus.
1. Suchen Sie den bearbeitbaren Bereich __Layout-Container__ unter dem Titel .
1. Öffnen Sie die Seitenleiste __Seiten-Editor__ und wählen Sie die Ansicht __Komponenten__ aus.
1. Ziehen Sie die folgenden Komponenten in den __Layout-Container__
   + Bild
   + Titel
1. Ziehen Sie die Komponenten in die folgende Reihenfolge, um sie neu anzuordnen:
   1. Titel
   1. Bild
   1. Text
1. ____ Erstellen der  ____ Titellecomponent
   1. Tippen Sie auf die Titelkomponente und tippen Sie auf das Symbol __Schraubenschlüssel__, um __edit__ die Titelkomponente
   1. Fügen Sie den folgenden Text hinzu:
      + Titel: __Der Sommer kommt, lassen Sie uns das Beste daraus machen!__
      + Typ: __H1__
   1. Tippen Sie auf __Fertig__
1. ____ Authoring der  ____ ImageComponent
   1. Ziehen Sie ein Bild aus der Seitenleiste (nach dem Wechsel zur Asset-Ansicht) auf die Bildkomponente
   1. Tippen Sie auf die Bildkomponente und dann auf das Symbol __Schraubenschlüssel__, um sie zu bearbeiten.
   1. Aktivieren Sie das Kontrollkästchen __Bild ist dekorativ__ .
   1. Tippen Sie auf __Fertig__
1. ____ Authoring der  ____ Textkomponente
   1. Bearbeiten Sie die Textkomponente, indem Sie auf die Textkomponente tippen und auf das Symbol __Schraubenschlüssel__ tippen.
   1. Fügen Sie den folgenden Text hinzu:
      + _Jetzt erhalten Sie 15% auf allen 1-wöchigen Abenteuern und 20% Rabatt auf alle Abenteuer, die 2 Wochen oder länger sind! Fügen Sie beim Checkout einfach den Kampagnencode SUMMERISCOMING hinzu, um Ihre Rabatte zu erhalten!_
   1. Tippen Sie auf __Fertig__

1. Ihre Komponenten werden jetzt erstellt, aber vertikal stapelt.

   ![Erstellte Komponenten](./assets/spa-container-component/authored-components.png)

   Verwenden Sie AEM Layout-Modus , um die Größe und das Layout der Komponenten anzupassen.

1. Wechseln Sie mithilfe der Modusauswahl oben rechts in den Modus __Layout-Modus__.
1. ____ Ändern der Größe der Bild- und Textkomponenten, sodass sie nebeneinander angeordnet sind
   + ____ Bildkomponente sollte  __8 Spalten breit sein__
   + ____ Textkomponente sollte  __3 Spalten breit sein__

   ![Layout-Komponenten](./assets/spa-container-component/layout-components.png)

1. ____ Vorschau der Änderungen im AEM Seiteneditor
1. Aktualisieren Sie die WKND-App, die lokal auf [http://localhost:3000](http://localhost:3000) ausgeführt wird, um die erstellten Änderungen zu sehen!

   ![Container-Komponente in SPA](./assets/spa-container-component/localhost-final.png)


## Herzlichen Glückwunsch!

Sie haben eine Container-Komponente hinzugefügt, mit der Autoren bearbeitbare Komponenten zur WKND-App hinzufügen können! Sie wissen jetzt, wie:

+ Verwenden Sie die Komponente &quot;ResponsiveGrid&quot;der AEM React Editable-Komponente im SPA
+ Registrieren AEM React-Kernkomponenten (Text und Bild) zur Verwendung in der SPA über die Container-Komponente
+ Konfigurieren Sie die Vorlage für die Remote-SPA, um die SPA aktivierten Kernkomponenten zuzulassen.
+ Hinzufügen bearbeitbarer Komponenten zur Container-Komponente
+ Autoren- und Layoutkomponenten im SPA Editor

## Nächste Schritte

Im nächsten Schritt wird dieselbe Technik verwendet, um [eine bearbeitbare Komponente zu einer Abenteuer-Details-Route](./spa-dynamic-routes.md) in der SPA hinzuzufügen.
