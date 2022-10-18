---
title: Erstellen einer benutzerdefinierten Wetterkomponente | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie eine benutzerdefinierte Wetterkomponente erstellen, die mit dem AEM SPA Editor verwendet werden kann. Erfahren Sie, wie Sie Bearbeitungsdialogfelder und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen. Die Komponenten "Open Weather API"und "React Open Weather"werden verwendet.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 82466e0e-b573-440d-b806-920f3585b638
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 4%

---

# Erstellen einer benutzerdefinierten Wetterkomponente {#custom-component}

Erfahren Sie, wie Sie eine benutzerdefinierte Wetterkomponente erstellen, die mit dem AEM SPA Editor verwendet werden kann. Erfahren Sie, wie Sie Bearbeitungsdialogfelder und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen. Die [Wetter-API öffnen](https://openweathermap.org) und [React Open Weather-Komponente](https://www.npmjs.com/package/react-open-weather) verwendet werden.

## Ziel

1. Machen Sie sich mit der Rolle von Sling-Modellen bei der Bearbeitung der von AEM bereitgestellten JSON-Modell-API vertraut.
2. Erfahren Sie, wie Sie neue AEM-Komponentendialogfelder erstellen.
3. Erfahren Sie, wie Sie eine **custom** AEM Komponente, die mit dem SPA Editor-Framework kompatibel ist.

## Was Sie erstellen werden

Es wird eine einfache Wetterkomponente erstellt. Diese Komponente kann von Inhaltsautoren zum SPA hinzugefügt werden. Über ein AEM-Dialogfeld können Autoren den Speicherort für das anzuzeigende Wetter festlegen.  Die Implementierung dieser Komponente zeigt die Schritte, die zum Erstellen einer neuen AEM-Komponente erforderlich sind, die mit dem AEM SPA Editor-Framework kompatibel ist.

![Konfigurieren der Open Weather-Komponente](assets/custom-component/enter-dialog.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment). Dieses Kapitel ist eine Fortsetzung der [Navigation und Routing](navigation-routing.md) -Kapitel zu folgen, ist jedoch ein SPA-aktiviertes AEM-Projekt, das in einer lokalen AEM-Instanz bereitgestellt wird.

### Open Weather API Key

Ein API-Schlüssel aus [Offenes Wetter](https://openweathermap.org/) ist erforderlich, um dem Tutorial zu folgen. [Die Anmeldung ist kostenlos.](https://home.openweathermap.org/users/sign_up) für eine begrenzte Anzahl von API-Aufrufen.

## Definieren der AEM-Komponente

Eine AEM Komponente ist als Knoten und Eigenschaften definiert. Im Projekt werden diese Knoten und Eigenschaften als XML-Dateien im `ui.apps` -Modul. Erstellen Sie anschließend die AEM-Komponente im `ui.apps` -Modul.

>[!NOTE]
>
> Ein kurzer Auffrischungswert für die [Die Grundlagen AEM Komponenten können hilfreich sein](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Öffnen Sie in der IDE Ihrer Wahl die `ui.apps` Ordner.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` und erstellen Sie einen neuen Ordner mit dem Namen `open-weather`.
3. Erstellen Sie eine neue Datei mit dem Namen `.content.xml` unterhalb der `open-weather` Ordner. Füllen Sie die `open-weather/.content.xml` durch Folgendes ersetzen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Erstellen einer benutzerdefinierten Komponentendefinition](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - gibt an, dass dieser Knoten eine AEM Komponente ist.

   `jcr:title` ist der Wert, der den Inhaltsautoren angezeigt wird, und der `componentGroup` bestimmt die Gruppierung von Komponenten in der Authoring-Benutzeroberfläche.

4. Unter dem `custom-component` Ordner erstellen Sie einen weiteren Ordner mit dem Namen `_cq_dialog`.
5. Unter dem `_cq_dialog` Ordner erstellen Sie eine neue Datei mit dem Namen `.content.xml` und fügen Sie Folgendes hinzu:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![Definition benutzerdefinierter Komponenten](assets/custom-component/dialog-custom-component-defintion.png)

   Die obige XML-Datei generiert ein sehr einfaches Dialogfeld für die `Weather Component`. Der kritische Teil der Datei ist der innere `<label>`, `<lat>` und `<lon>` Knoten. Dieses Dialogfeld enthält zwei `numberfield`s und a `textfield` , mit dem ein Benutzer das anzuzeigende Wetter konfigurieren kann.

   Ein Sling-Modell wird erstellt, neben dem der Wert der `label`,`lat` und `long` -Eigenschaften über das JSON-Modell.

   >[!NOTE]
   >
   > Sie können viel mehr anzeigen [Beispiele für Dialogfelder durch Anzeigen der Kernkomponenten-Definitionen](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Sie können auch zusätzliche Formularfelder anzeigen, z. B. `select`, `textarea`, `pathfield`, verfügbar unter `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Mit einer herkömmlichen AEM-Komponente [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=de) -Skript ist normalerweise erforderlich. Da das SPA die Komponente rendert, ist kein HTL-Skript erforderlich.

## Sling-Modell erstellen

Sling-Modelle sind von Anmerkungen gesteuerte Java-&quot;POJOs&quot;(Plain Old Java Objects), die die Zuordnung von Daten aus dem JCR zu Java-Variablen erleichtern. [Sling-Modelle](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) typischerweise Funktion zum Einkapseln komplexer serverseitiger Geschäftslogik für AEM Komponenten.

Im Kontext des SPA-Editors stellen Sling-Modelle den Inhalt einer Komponente über das JSON-Modell mithilfe einer Funktion mit dem [Sling Model Exporter](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=de).

1. Öffnen Sie in der IDE Ihrer Wahl die `core` -Modul an `aem-guides-wknd-spa.react/core`.
1. Erstellen Sie eine Datei mit dem Namen `OpenWeatherModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Füllen Sie `OpenWeatherModel.java` wie folgt:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   Dies ist die Java-Schnittstelle für unsere Komponente. Damit unser Sling-Modell mit dem SPA Editor-Framework kompatibel ist, muss es die `ComponentExporter` -Klasse.

1. Erstellen Sie einen Ordner mit dem Namen `impl` unter `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Erstellen Sie eine Datei mit dem Namen `OpenWeatherModelImpl.java` unter `impl` und fügen Sie Folgendes hinzu:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   Die statische Variable `RESOURCE_TYPE` muss auf den Pfad in `ui.apps` der Komponente. Die `getExportedType()` wird verwendet, um die JSON-Eigenschaften der SPA-Komponente über zuzuordnen. `MapTo`. `@ValueMapValue` ist eine Anmerkung, die die vom Dialogfeld gespeicherte Eigenschaft jcr liest.

## SPA aktualisieren

Aktualisieren Sie anschließend den React-Code, um die [React Open Weather-Komponente](https://www.npmjs.com/package/react-open-weather) und lassen Sie sie der in den vorherigen Schritten erstellten AEM-Komponente zuordnen.

1. Installieren Sie die React Open Weather-Komponente als **npm** dependency:

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Erstellen Sie einen neuen Ordner mit dem Namen `OpenWeather` at `ui.frontend/src/components/OpenWeather`.
1. Fügen Sie eine Datei mit dem Namen `OpenWeather.js` und fügen Sie Folgendes hinzu:

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Aktualisieren `import-components.js` at `ui.frontend/src/components/import-components.js` , um `OpenWeather` component:

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Stellen Sie mithilfe Ihrer Maven-Kenntnisse alle Aktualisierungen in einer lokalen AEM-Umgebung aus dem Stammverzeichnis des Projektverzeichnisses bereit:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Vorlagenrichtlinie aktualisieren

Navigieren Sie als Nächstes zu AEM , um die Aktualisierungen zu überprüfen und die `OpenWeather` Komponente, die zum SPA hinzugefügt werden soll.

1. Überprüfen Sie die Registrierung des neuen Sling-Modells, indem Sie zu [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   Sie sollten die beiden obigen Zeilen sehen, die die `OpenWeatherModelImpl` ist mit dem `wknd-spa-react/components/open-weather` und dass sie über den Sling Model Exporter registriert ist.

1. Navigieren Sie zur SPA Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue `Open Weather` als zulässige Komponente:

   ![Aktualisieren der Layout-Container-Richtlinie](assets/custom-component/custom-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie die `Open Weather` als zulässige Komponente:

   ![Benutzerdefinierte Komponente als zulässige Komponente](assets/custom-component/custom-component-allowed-layout-container.png)

## Erstellen der Komponente &quot;Open Weather&quot;

Als Nächstes erstellen Sie die `Open Weather` -Komponente mit dem AEM-SPA-Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. In `Edit` -Modus, fügen Sie die `Open Weather` der `Layout Container`:

   ![Neue Komponente einfügen](assets/custom-component/insert-custom-component.png)

1. Öffnen Sie das Dialogfeld der Komponente und geben Sie einen **Titel**, **Breitengrad** und **Längengrad**. Beispiel **San Diego**, **32 7157** und **-117 16 11**. Die Zahlen der westlichen Hemisphäre und der südlichen Hemisphäre werden mit der Open Weather API als negative Zahlen dargestellt.

   ![Konfigurieren der Open Weather-Komponente](assets/custom-component/enter-dialog.png)

   Dies ist das Dialogfeld, das basierend auf der zuvor im Kapitel enthaltenen XML-Datei erstellt wurde.

1. Speichern Sie die Änderungen. Beachten Sie, dass das Wetter **San Diego** angezeigt wird:

   ![Wetterkomponente aktualisiert](assets/custom-component/weather-updated.png)

1. Anzeigen des JSON-Modells durch Navigieren zu [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Suchen Sie nach `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   Die JSON-Werte werden vom Sling-Modell ausgegeben. Diese JSON-Werte werden als Props an die React-Komponente übergeben.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie eine benutzerdefinierte AEM-Komponente erstellen, die mit dem SPA Editor verwendet werden soll. Außerdem haben Sie erfahren, wie Dialogfelder, JCR-Eigenschaften und Sling-Modelle interagieren, um das JSON-Modell auszugeben.

### Nächste Schritte {#next-steps}

[Erweitern einer Kernkomponente](extend-component.md) - Erfahren Sie, wie Sie eine vorhandene AEM Kernkomponente erweitern, die mit dem AEM SPA Editor verwendet werden kann. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern.
