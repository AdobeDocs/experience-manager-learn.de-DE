---
title: Verwenden AEM bearbeitbaren React-Komponenten v2
description: Erfahren Sie, wie Sie mit AEM React Editable Components v2 eine React-App aktivieren können.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---


# Verwenden AEM bearbeitbaren React-Komponenten v2

AEM bietet [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components): ein Node.js-basiertes SDK, das die Erstellung von React-Komponenten ermöglicht, die die kontextbezogene Komponentenbearbeitung mit AEM SPA Editor unterstützen.

+ [npm-Modul](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [GitHub-Projekt](https://github.com/adobe/aem-react-editable-components)
+ [Dokumentation zur Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Weitere Informationen und Codebeispiele für AEM React Editable Components v2 finden Sie in der technischen Dokumentation:

+ [Integration in AEM Dokumentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Dokumentation zu bearbeitbaren Komponenten](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Hilfsdokumentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM

AEM bearbeitbare React-Komponenten funktionieren sowohl mit SPA Editor- als auch mit Remote SPA React-Apps. Inhalte, die die bearbeitbaren React-Komponenten ausfüllen, müssen über AEM Seiten verfügbar gemacht werden, die die [SPA Seitenkomponente](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). AEM Komponenten, die bearbeitbaren React-Komponenten zugeordnet sind, müssen AEM implementieren [Framework des Komponenten-Exporters](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=de) - wie z. B. [AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de).


## Abhängigkeiten

Stellen Sie sicher, dass die React-App auf Node.js ab 14 ausgeführt wird.

Die minimalen Abhängigkeiten, die die React-App AEM React-Bearbeitbare Komponenten v2 verwenden kann, sind: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`und  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base) und [AEM React Core WCM-Komponenten SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) sind nicht mit AEM React Editable Components v2 kompatibel.

## SPA-Editor

Wenn Sie die AEM bearbeitbaren React-Komponenten mit einer SPA Editor-basierten React-App verwenden, wird die AEM `ModelManager` SDK als SDK:

1. Ruft Inhalte von AEM ab
1. Füllt die react-editierbaren Komponenten mit AEM Inhalt

Schließen Sie die React-App mit einem initialisierten ModelManager ein und rendern Sie die React-App. Die React-App sollte eine Instanz der `<Page>` aus `@adobe/aem-react-editable-components`. Die `<Page>` Komponente verfügt über Logik zum dynamischen Erstellen von React-Komponenten basierend auf der `.model.json` von AEM.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

Die `<Page>` wird als Darstellung der AEM als JSON über die `pageModel` von `ModelManager`. Die `<Page>` Komponente erstellt dynamisch React-Komponenten für Objekte im `pageModel` durch Abgleich der `resourceType` mit einer React-Komponente, die sich zum Ressourcentyp registriert über `MapTo(..)`.

## Bearbeitbare Komponenten

Die `<Page>` wird die Darstellung der AEM-Seite als JSON über die `ModelManager`. Die `<Page>` -Komponente erstellt dann dynamisch React-Komponenten für jedes Objekt in der JSON, indem die `resourceType` -Wert mit einer React-Komponente, die sich zum Ressourcentyp über die `MapTo(..)` -Aufruf. Beispielsweise würde Folgendes verwendet, um eine Instanz zu instanziieren

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

Die oben von AEM bereitgestellte JSON-Datei kann verwendet werden, um eine bearbeitbare React-Komponente dynamisch zu instanziieren und zu füllen.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Einbetten von Komponenten

Bearbeitbare Komponenten können wiederverwendet und in einander eingebettet werden. Beim Einbetten einer bearbeitbaren Komponente in eine andere gibt es zwei wichtige Aspekte:

1. Der JSON-Inhalt von AEM für die Einbettungskomponente muss den Inhalt enthalten, damit die eingebetteten Komponenten erfüllt werden. Dies geschieht durch Erstellen eines Dialogfelds für die AEM Komponente, die die erforderlichen Daten erfasst.
1. Die &quot;nicht bearbeitbare&quot;Instanz der React-Komponente muss eingebettet sein und nicht die &quot;bearbeitbare&quot;Instanz, die mit `<EditableComponent>`. Der Grund ist, wenn die eingebettete Komponente die `<EditableComponent>` -Wrapper verwendet, versucht der SPA-Editor, die innere Komponente mit dem Bearbeitungs-Chrome (blaues Mausfeld) und nicht mit der äußeren Einbettungskomponente zu kleiden.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

Die oben von AEM bereitgestellte JSON-Datei kann verwendet werden, um eine bearbeitbare React-Komponente, die eine andere React-Komponente einbettet, dynamisch zu instanziieren und zu füllen.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```



