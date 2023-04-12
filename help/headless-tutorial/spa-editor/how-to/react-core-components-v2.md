---
title: Verwenden von AEM React Editable Components v2
description: Erfahren Sie, wie Sie mit AEM React Editable Components v2 eine React-App erstellen können.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: f02d5e01388ee61228254951b05c37c336423348
workflow-type: ht
source-wordcount: '586'
ht-degree: 100%

---


# Verwenden von AEM React Editable Components v2

AEM stellt das Node.js-basierte SDK [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components) zur Verfügung. Es ermöglicht die Erstellung von React-Komponenten, die die kontextbezogene Komponentenbearbeitung mit dem AEM-SPA-Editor unterstützen.

+ [NPM-Modul](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [GitHub-Projekt](https://github.com/adobe/aem-react-editable-components)
+ [Adobe-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html?lang=de)


Weitere Informationen und Code-Beispiele für AEM React Editable Components v2 finden Sie in der technischen Dokumentation:

+ [Dokumentation zur Integration in AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Dokumentation zu bearbeitbaren Komponenten](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Helper-Dokumentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM-Seiten

AEM React Editable Components funktionieren für React-Apps sowohl mit dem SPA-Editor als auch mit einer Remote-SPA. Inhalte, die die bearbeitbaren React-Komponenten befüllen, müssen über AEM-Seiten verfügbar gemacht werden, die die [SPA-Seitenkomponente](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html?lang=de) erweitern. AEM-Komponenten, die bearbeitbaren React-Komponenten zugeordnet sind, müssen das [Component Exporter Framework](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=de) von AEM implementieren – wie etwa [AEM Core WCM Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de).


## Abhängigkeiten

Stellen Sie sicher, dass die React-App auf Node.js ab Version 14 ausgeführt wird.

Der minimale Satz von Abhängigkeiten für die React-App zur Verwendung von AEM React Editable Components v2 besteht aus `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` und `@adobe/aem-spa-page-model-manager`.


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
> [AEM React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base) und [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) sind nicht mit AEM React Editable Components v2 kompatibel.

## SPA-Editor

Wenn Sie die AEM React Editable Components mit einer auf dem SPA-Editor basierenden React-App verwenden, wird das AEM `ModelManager` SDK als SDK Folgendes ausführen:

1. Inhalte von AEM abrufen
1. die editierbaren React-Komponenten mit AEM-Inhalten befüllen

Umschließen Sie die React-App mit einem initialisierten ModelManager und rendern Sie die React-App. Die React-App sollte eine Instanz der `<Page>`-Komponente enthalten, die von `@adobe/aem-react-editable-components` exportiert wurde. Die `<Page>`-Komponente verfügt über Logiken zum dynamischen Erstellen von React-Komponenten basierend auf der von AEM bereitgestellten `.model.json`.

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

Die `<Page>` wird als JSON-Darstellung der AEM-Seite über das `pageModel` übergeben, das durch den `ModelManager` bereitgestellt wird. Die `<Page>`-Komponente erstellt dynamisch React-Komponenten für Objekte im `pageModel` durch Abgleich des `resourceType` mit einer React-Komponente, die sich über `MapTo(..)` beim Ressourcentyp registriert.

## Bearbeitbare Komponenten

Der `<Page>` wird die Darstellung der AEM-Seite als JSON über den `ModelManager` übergeben. Die `<Page>`-Komponente erstellt dann dynamisch React-Komponenten für jedes Objekt im JSON-Code, indem sie den `resourceType`-Wert des JS-Objekts mit einer React-Komponente abgleicht, die sich über den `MapTo(..)`-Aufruf der Komponente beim Ressourcentyp registriert. Beispielsweise würde Folgendes verwendet, um eine Instanz zu instanziieren

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

Der obige, von AEM bereitgestellte JSON-Code kann verwendet werden, um eine bearbeitbare React-Komponente dynamisch zu instanziieren und zu befüllen.

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

Bearbeitbare Komponenten können wiederverwendet und ineinander eingebettet werden. Beim Einbetten einer bearbeitbaren Komponente in eine andere gibt es zwei wichtige Aspekte:

1. Der JSON-Inhalt von AEM für die einbettende Komponente muss den Inhalt enthalten, um die eingebetteten Komponenten zu versorgen. Dies geschieht durch Erstellen eines Dialogfelds für die AEM-Komponente, die die erforderlichen Daten erfasst.
1. Die „nicht bearbeitbare“ Instanz der React-Komponente muss eingebettet sein und nicht die „bearbeitbare“ Instanz, die durch `<EditableComponent>` umschlossen ist. Der Grund dafür ist folgender: Wenn die eingebettete Komponente von `<EditableComponent>` umschlossen ist, versucht der SPA-Editor, die innere Komponente mit dem Bearbeitungs-Chrom (blaues Hover-Feld) zu versehen und nicht die äußere einbettende Komponente.

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

Der oben von AEM bereitgestellte JSON-Code kann verwendet werden, um eine bearbeitbare React-Komponente, die eine andere React-Komponente einbettet, dynamisch zu instanziieren und zu befüllen.


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



