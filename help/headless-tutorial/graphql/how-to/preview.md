---
title: Inhaltsfragmentvorschau
description: Erfahren Sie, wie Sie die Inhaltsfragmentvorschau für alle Autorinnen und Autoren verwenden können, um schnell zu sehen, wie sich Inhaltsänderungen auf Ihre AEM Headless-Erlebnisse auswirken.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
duration: 532
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 100%

---

# Inhaltsfragmentvorschau

AEM Headless-Anwendungen unterstützen eine integrierte Authoring-Vorschau. Das Vorschauerlebnis verknüpft den Inhaltsfragmenteditor von AEM Author mit Ihrer benutzerdefinierten App (über HTTP adressierbar), sodass ein Deeplink in die App eingefügt werden kann, mit der das in der Vorschau angezeigte Inhaltsfragment gerendert wird.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Zur Verwendung der Inhaltsfragmentvorschau müssen mehrere Bedingungen erfüllt sein:

1. Die App muss in einer URL bereitgestellt werden, auf die Autorinnen und Autoren zugreifen können.
1. Die App muss so konfiguriert sein, dass eine Verbindung zum AEM-Author-Service (und nicht zum AEM-Publish-Service) hergestellt wird.
1. Die App muss mit URLs oder Routen entwickelt werden, die [den Pfad oder die ID des Inhaltsfragments](#url-expressions) verwenden können, um die Inhaltsfragmente auszuwählen, die in einer Vorschau im App-Erlebnis angezeigt werden sollen.

## Vorschau-URLs

Vorschau-URLs mit [URL-Ausdrücken](#url-expressions) werden in den Eigenschaften des Inhaltsfragmentmodells festgelegt.

![Vorschau-URL des Inhaltsfragmentmodells](./assets/preview/cf-model-preview-url.png)

1. Melden Sie sich beim AEM-Author-Service als Admin an.
1. Navigieren Sie zu __Tools > Allgemein > Inhaltsfragmentmodelle__
1. Wählen Sie das __Inhaltsfragmentmodell__ und dann in der oberen Aktionsleiste die Option __Bearbeiten__ aus.
1. Geben Sie die Vorschau-URL für das Inhaltsfragmentmodell mithilfe von [URL-Ausdrücken](#url-expressions) ein.
   + Die Vorschau-URL muss auf eine Bereitstellung der App verweisen, die eine Verbindung zum AEM-Author-Service herstellt.

### URL-Ausdrücke

Jedes Inhaltsfragmentmodell kann über eine festgelegte Vorschau-URL verfügen. Die Vorschau-URL kann pro Inhaltsfragment mithilfe der in der folgenden Tabelle aufgeführten URL-Ausdrücke parametrisiert werden. In einer Vorschau-URL können mehrere URL-Ausdrücke verwendet werden.

|                                         | URL-Ausdruck | Wert |
| --------------------------------------- | ----------------------------------- | ----------- |
| Inhaltsfragmentpfad | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| Inhaltsfragment-ID | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Inhaltsfragmentvariante | `${contentFragment.variation}` | `main` |
| Inhaltsfragmentmodell-Pfad | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Inhaltsfragmentmodell-Name | `${contentFragment.model.name}` | `adventure` |

Beispiele für Vorschau-URLs:

+ Eine Vorschau-URL für das __Abenteuermodell__ könnte wie `https://preview.app.wknd.site/adventure${contentFragment.path}` aussehen, was zu `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` aufgelöst wird.
+ Eine Vorschau-URL für das __Artikelmodell__ könnte wie `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` aussehen, was zu `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main` aufgelöst wird.

## In-App-Vorschau

Jedes Inhaltsfragment, das das konfigurierte Inhaltsfragmentmodell verwendet, verfügt über eine Vorschau-Schaltfläche. Über die Vorschau-Schaltfläche wird die Vorschau-URL des Inhaltsfragmentmodells geöffnet und die Werte des geöffneten Inhaltsfragments werden in die [URL-Ausdrücke](#url-expressions) eingefügt.

![Schaltfläche „Vorschau“](./assets/preview/preview-button.png)

Führen Sie bei der Vorschau von Inhaltsfragmentänderungen in der App eine harte Aktualisierung durch (Leeren des lokalen Caches des Browsers).

## React-Beispiel

Im Folgenden wird die WKND-App vorgestellt, eine einfache React-Anwendung, die Abenteuer aus AEM mit AEM Headless-GraphQL-APIs anzeigt.

Der Beispiel-Code ist verfügbar unter [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URLs und Routen

Die URLs oder Routen, die zur Vorschau eines Inhaltsfragments verwendet werden, müssen mithilfe von [URL-Ausdrücken](#url-expressions) zusammensetzbar sein. In dieser für die Vorschau aktivierten Version der WKND-App werden die Inhaltsfragmente für das Abenteuer über die `AdventureDetail`-Komponente angezeigt, die an die Route `/adventure<CONTENT FRAGMENT PATH>` gebunden ist. Daher muss die Vorschau-URL des WKND-Abenteuermodells auf `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` festgelegt sein, um eine Auflösung zu dieser Route vorzunehmen.

Die Inhaltsfragmentvorschau funktioniert nur, wenn die App über eine adressierbare Route verfügt, die mit [URL-Ausdrücken](#url-expressions) aufgefüllt werden kann, die dieses Inhaltsfragment in der App in vorschaufähiger Weise rendern.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### Anzeigen des erstellten Inhalts

Die Komponente `AdventureDetail` analysiert einfach den Pfad des Inhaltsfragments, der aus der Route-URL über den [URL-Ausdruck](#url-expressions) `${contentFragment.path}` in die Vorschau-URL eingefügt wurde, und verwendet diesen zum Erfassen und Rendern des WKND-Abenteuers.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
