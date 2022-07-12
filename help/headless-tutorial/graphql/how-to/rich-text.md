---
title: Verwenden von Rich-Text mit AEM Headless
description: Erfahren Sie, wie Sie mit einem mehrzeiligen Rich-Text-Editor mit Adobe Experience Manager-Inhaltsfragmenten Inhalte erstellen und referenzierte Inhalte einbetten können und wie Rich-Text von AEM GraphQL-APIs als JSON bereitgestellt wird, um von Headless-Anwendungen genutzt zu werden.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# Rich-Text mit AEM Headless

Das mehrzeilige Textfeld ist ein Datentyp von Inhaltsfragmenten, mit dem Autoren Rich-Text-Inhalte erstellen können. Verweise auf andere Inhalte wie Bilder oder andere Inhaltsfragmente können dynamisch in Zeilen innerhalb des Textflusses eingefügt werden. Das einzeilige Textfeld ist ein weiterer Datentyp von Inhaltsfragmenten, der für einfache Textelemente verwendet werden sollte.

AEM GraphQL-API bietet eine robuste Möglichkeit, Rich-Text als HTML, Nur-Text oder reinen JSON zurückzugeben. Die JSON-Darstellung ist leistungsstark, da sie der Clientanwendung die volle Kontrolle darüber gibt, wie der Inhalt gerendert werden kann.

## Mehrzeiliger Editor

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)

Im Inhaltsfragment-Editor bietet die Menüleiste des mehrzeiligen Textfelds Autoren standardmäßige Rich-Text-Formatierungsfunktionen wie **fett**, *kursiv* und unterstreichen. Das Öffnen des mehrzeiligen Felds im Vollbildmodus aktiviert [zusätzliche Formatierungswerkzeuge wie Absatztyp, Suchen und Ersetzen, Rechtschreibprüfung und mehr](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

>[!NOTE]
>
> Die Rich-Text-Plug-ins im mehrzeiligen Editor können nicht angepasst werden.

## Mehrzeiliger Texttyp {#multi-line-data-type}

Verwenden Sie die **Mehrzeiliger Text** Datentyp bei der Definition Ihres Inhaltsfragmentmodells, um die Erstellung von Rich-Text zu ermöglichen.

![Datentyp &quot;Rich-Text mehrzeilig&quot;](assets/rich-text/multi-line-rich-text.png)

Es können mehrere Eigenschaften des mehrzeiligen Felds konfiguriert werden.

Die **Rendern als** -Eigenschaft kann auf Folgendes festgelegt werden:

* Textbereich - rendert ein einzelnes mehrzeiliges Feld
* Mehrere Felder - rendert mehrere mehrzeilige Felder.


Die **Standardtyp** kann auf Folgendes festgelegt werden:

* Rich-Text
* Markdown
* Nur Text

Die **Standardtyp** beeinflusst direkt das Bearbeitungserlebnis und bestimmt, ob die Rich-Text-Tools vorhanden sind.

Sie können auch [Inline-Verweise aktivieren](#insert-fragment-references) zu anderen Inhaltsfragmenten hinzugefügt werden, indem Sie die **Fragmentverweis zulassen** und konfigurieren Sie die **Zulässige Inhaltsfragmentmodelle**.

Überprüfen Sie die **Übersetzbar** , wenn der Inhalt lokalisiert wird. Nur Rich Text und Nur Text können lokalisiert werden. Siehe [Arbeiten mit lokalisierten Inhalten für weitere Informationen](./localized-content.md).

## Rich-Text-Antwort mit GraphQL-API

Beim Erstellen einer GraphQL-Abfrage können Entwickler verschiedene Antworttypen von `html`, `plaintext`, `markdown`und `json` aus einem mehrzeiligen Feld.

Entwickler können die [JSON-Vorschau](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) im Inhaltsfragment-Editor, um alle Werte des aktuellen Inhaltsfragments anzuzeigen, die mit der GraphQL-API zurückgegeben werden können.

## GraphQL-persistente Abfrage

Auswählen der `json` Das Antwortformat für das mehrzeilige Feld bietet die größte Flexibilität beim Arbeiten mit Rich-Text-Inhalten. Der Rich-Text-Inhalt wird als Array von JSON-Knotentypen bereitgestellt, die basierend auf der Client-Plattform eindeutig verarbeitet werden können.

Nachstehend finden Sie einen JSON-Antworttyp eines mehrzeiligen Felds mit dem Namen `main` , der einen Absatz enthält: &quot;*Dies ist ein Absatz mit **wichtig**Inhalt.*&quot;, wobei &quot;wichtig&quot;als **fett**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

Die `$path` in der Variablen `_path` Der Filter erfordert den vollständigen Pfad zum Inhaltsfragment (z. B. `/content/dam/wknd/en/magazine/sample-article`).

**GraphQL-Antwort:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Weitere Beispiele

Nachfolgend finden Sie einige Beispiele für Antworttypen eines mehrzeiligen Felds mit dem Namen `main` , der einen Absatz enthält: &quot;Dies ist ein Absatz mit **wichtig** Inhalt.&quot; wobei &quot;wichtig&quot;als **fett**.

Beispiel +++HTML

**GraphQL-persistente Abfrage:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL-Antwort:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

Beispiel +++Markdown

**GraphQL-persistente Abfrage:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL-Antwort:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++Plaintext-Beispiel

**GraphQL-persistente Abfrage:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL-Antwort:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

Die `plaintext` Render-Option schneidet alle Formatierungen ab.

+++


## Rendern einer Rich-Text-JSON-Antwort {#render-multiline-json-richtext}

Die Rich-Text-JSON-Antwort des mehrzeiligen Felds ist als hierarchischer Baum strukturiert. Jedes Objekt oder jeder Knoten stellt einen anderen HTML-Block des Rich-Text-Elements dar.

Nachfolgend finden Sie eine JSON-Beispielantwort eines mehrzeiligen Textfelds. Beachten Sie, dass jedes Objekt bzw. jeder Knoten einen `nodeType` , der den HTML-Block aus dem Rich-Text wie `paragraph`, `link`und `text`. Jeder Knoten enthält optional `content` : ein Unterarray, das alle untergeordneten Elemente des aktuellen Knotens enthält.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

Die einfachste Methode zum Rendern der mehrzeiligen `json` -Antwort besteht darin, jedes Objekt oder jeden Knoten in der Antwort zu verarbeiten und anschließend alle untergeordneten Elemente des aktuellen Knotens zu verarbeiten. Eine rekursive Funktion kann verwendet werden, um die JSON-Struktur zu durchlaufen.

Nachfolgend finden Sie ein Beispiel für einen Code, der einen rekursiven Traversal-Ansatz veranschaulicht. Die Beispiele sind JavaScript-basiert und verwenden React&#39;s [JSX](https://reactjs.org/docs/introducing-jsx.html), jedoch können die Programmierkonzepte auf jede Sprache angewendet werden.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` ist eine rekursive Funktion, die ein Array von `childNodes`. Jeder Knoten im Array wird dann an eine Funktion übergeben `renderNode`, die wiederum aufruft `renderNodeList` , wenn der Knoten untergeordnete Elemente hat.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

Die `renderNode` -Funktion erwartet ein einzelnes Objekt mit dem Namen `node`. Ein Knoten kann untergeordnete Elemente haben, die rekursiv mithilfe der `renderNodeList` -Funktion beschrieben. Schließlich wird eine `nodeMap` wird verwendet, um den Inhalt des Knotens basierend auf seiner `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

Die `nodeMap` ist ein JavaScript-Objektliteral, das als Zuordnung verwendet wird. Jeder der &quot;Schlüssel&quot;stellt eine andere `nodeType`. Parameter `node` und `children` kann an die resultierenden Funktionen übergeben werden, die den Knoten rendern. Der in diesem Beispiel verwendete Rückgabetyp ist JSX. Der Ansatz kann jedoch angepasst werden, um ein Zeichenfolgenliteral zu erstellen, das HTML-Inhalte darstellt.

### Beispiel für vollständigen Code

Ein wiederverwendbares Rich-Text-Rendering-Dienstprogramm finden Sie im [WKND GraphQL-React-Beispiel](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - wiederverwendbares Dienstprogramm, das eine Funktion verfügbar macht `mapJsonRichText`. Dieses Dienstprogramm kann von Komponenten verwendet werden, die eine Rich-Text-JSON-Antwort als React JSX rendern möchten.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Beispielkomponente, die eine GraphQL-Anforderung ausführt, die Rich-Text enthält. Die Komponente verwendet die `mapJsonRichText` -Dienstprogramm zum Rendern des Rich-Text und aller Verweise.


## Hinzufügen von Inline-Verweisen zu Rich-Text {#insert-fragment-references}

Mit dem Feld &quot;Mehrere Zeilen&quot;können Autoren Bilder oder andere digitale Assets aus AEM Assets in den Fluss des Rich-Text einfügen.

![Bild einfügen](assets/rich-text/insert-image.png)

Der obige Screenshot zeigt ein Bild, das im mehrzeiligen Feld mithilfe der **Asset einfügen** Schaltfläche.

Verweise auf andere Inhaltsfragmente können auch über die **Inhaltsfragment einfügen** Schaltfläche.

![Inhaltsfragmentverweis einfügen](assets/rich-text/insert-contentfragment.png)

Der obige Screenshot zeigt ein weiteres Inhaltsfragment, den Ultimate Guide to LA Skate Parks, das in das mehrzeilige Feld eingefügt wird. Die Typen von Inhaltsfragmenten, die in Felder eingefügt werden können, werden durch die **Zulässige Inhaltsfragmentmodelle** Konfiguration in der [Mehrzeiliger Datentyp](#multi-line-data-type) im Inhaltsfragmentmodell.

## Abfragen von Inline-Referenzen mit GraphQL

Mit der GraphQL-API können Entwickler eine Abfrage erstellen, die zusätzliche Eigenschaften zu Verweisen enthält, die in ein mehrzeiliges Feld eingefügt wurden. Die JSON-Antwort enthält eine separate `_references` -Objekt, das diese zusätzlichen Eigenschaften auflistet. Die JSON-Antwort gibt Entwicklern die volle Kontrolle darüber, wie die Verweise oder Links gerendert werden, anstatt sich mit oppositioniertem HTML befassen zu müssen.

Sie können beispielsweise:

* Schließen Sie bei der Implementierung einer Einzelseiten-App eine benutzerdefinierte Routing-Logik für die Verwaltung von Links zu anderen Inhaltsfragmenten ein, z. B. React-Router oder Next.js
* Rendern Sie ein Inline-Bild unter Verwendung des absoluten Pfads zu einer AEM-Veröffentlichungsumgebung als `src` -Wert.
* Ermitteln Sie, wie Sie einen eingebetteten Verweis auf ein anderes Inhaltsfragment mit zusätzlichen benutzerdefinierten Eigenschaften rendern.

Verwenden Sie die `json` Rückgabetyp und schließen Sie die `_references` -Objekt beim Erstellen einer GraphQL-Abfrage:

**GraphQL-persistente Abfrage:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }
      
    }
  }
}
```

In der obigen Abfrage wird die `main` wird als JSON zurückgegeben. Die `_references` -Objekt enthält Fragmente zum Umgang mit Verweisen vom Typ `ImageRef` oder des Typs `ArticleModel`.

**JSON-Antwort:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://publish-p123-e456.adobeaemcloud.com/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920,
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

Die JSON-Antwort enthält, wo die Referenz mit der `"nodeType": "reference"`. Die `_references` -Objekt enthält dann jede Referenz mit den angeforderten zusätzlichen Eigenschaften. Beispiel: die `ImageRef` gibt die `width` des im Artikel referenzierten Bildes.

## Rendern von Inline-Referenzen in Rich-Text

Um Inline-Referenzen zu rendern, wird der rekursive Ansatz beschrieben unter [Rendern einer mehrzeiligen JSON-Antwort](#render-multiline-json-richtext) kann erweitert werden.

Wo `nodeMap` ist die Zuordnung, die die JSON-Knoten rendert.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

Der allgemeine Ansatz besteht darin, jedes Mal, wenn eine `nodeType` gleich `reference` in der &quot;Mutli Line JSON&quot;-Antwort. Anschließend kann eine benutzerdefinierte Renderfunktion aufgerufen werden, die die `_references` -Objekt, das in der GraphQL-Antwort zurückgegeben wird.

Der Inline-Referenzpfad kann dann mit dem entsprechenden Eintrag im `_references` Objekt und eine andere benutzerdefinierte Zuordnung `renderReference` aufgerufen werden.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._publishUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

Die `__typename` des `_references` -Objekt kann verwendet werden, um verschiedene Referenztypen verschiedenen Renderfunktionen zuzuordnen.

### Beispiel für vollständigen Code

Ein vollständiges Beispiel für das Schreiben eines benutzerdefinierten Referenzen-Renderers finden Sie unter [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) als Teil der [WKND GraphQL-React-Beispiel](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## End-to-End-Beispiel

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

Das vorherige Video zeigt ein Beispiel von Ende zu Ende:

1. Aktualisieren des mehrzeiligen Textfelds eines Inhaltsfragmentmodells, um Fragmentverweise zuzulassen
1. Verwendung des Inhaltsfragment-Editors zum Einschließen eines Bildes und zum Verweis auf ein anderes Fragment in ein mehrzeiliges Textfeld.
1. Erstellen einer GraphQL-Abfrage, die die mehrzeilige Textantwort als JSON und alle `_references` verwendet.
1. Schreiben einer React-SPA, die die Inline-Verweise der Rich-Text-Antwort rendert.
