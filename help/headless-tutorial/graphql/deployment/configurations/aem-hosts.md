---
title: Verwalten AEM Hosts für AEM GraphQL
description: Erfahren Sie, wie Sie AEM Hosts in AEM Headless-App konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# AEM Hosts verwalten

Die Bereitstellung einer AEM Headless-Anwendung erfordert die Beachtung, wie AEM URLs erstellt werden, um sicherzustellen, dass der richtige AEM Host/die richtige Domäne verwendet wird. Die folgenden primären URL-/Anforderungstypen sind zu beachten:

+ HTTP-Anforderungen an __[AEM GraphQL-APIs](#aem-graphql-api-requests)__
+ __[Bild-URLs](#aem-image-urls)__ für Bild-Assets, die in Inhaltsfragmenten referenziert und von AEM bereitgestellt werden

In der Regel interagiert eine AEM Headless-App mit einem einzigen AEM-Dienst für die GraphQL-API und Bildanforderungen. Der AEM-Dienst ändert sich je nach AEM Implementierung der Headless-App:

| AEM Headless-Bereitstellungstyp | AEM | AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produktion | Produktion | Veröffentlichen  |
| Authoring-Vorschau | Produktion | Vorschau |
| Entwicklung | Entwicklung | Veröffentlichen  |

Zur Verarbeitung von Permutationen vom Typ Bereitstellung wird jede App-Bereitstellung mithilfe einer Konfiguration erstellt, die den AEM Dienst angibt, mit dem eine Verbindung hergestellt werden soll. Der Host/die Domäne des konfigurierten AEM-Dienstes wird dann verwendet, um die AEM GraphQL-API-URLs und Bild-URLs zu erstellen. Um den richtigen Ansatz für die Verwaltung von Build-abhängigen Konfigurationen zu bestimmen, verweisen Sie auf die Dokumentation zum Framework der AEM Headless-App (z. B. React, iOS, Android™ usw.), da der Ansatz je nach Framework unterschiedlich ist.

| Client-Typ | [Einzelseitenanwendung (SPA)](../spa.md) | [Webkomponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM Hostkonfiguration | ms | ms | ms | ms |

Im Folgenden finden Sie Beispiele für mögliche Ansätze zum Erstellen von URLs für [AEM GraphQL-API](#aem-graphql-api-requests) und [Bildanforderungen](#aem-image-requests), für mehrere beliebte Headless-Frameworks und Plattformen.

## AEM GraphQL-API-Anfragen

Die HTTP-GET-Anfragen von der Headless App an AEM GraphQL-APIs müssen für die Interaktion mit dem richtigen AEM-Dienst konfiguriert werden, wie im Abschnitt [Tabelle oben](#managing-aem-hosts).

Bei Verwendung von [AEM Headless-SDKs](../../how-to/aem-headless-sdk.md) (für browserbasiertes JavaScript, serverbasiertes JavaScript und Java™ verfügbar) kann ein AEM-Host das AEM Headless-Client-Objekt mit dem AEM-Dienst initialisieren, mit dem eine Verbindung hergestellt werden soll.

Stellen Sie bei der Entwicklung eines benutzerdefinierten AEM Headless-Clients sicher, dass der Host des AEM-Dienstes basierend auf Build-Parametern parametrisierbar ist.

### Beispiele

Im Folgenden finden Sie Beispiele dafür, wie AEM GraphQL-API-Anfragen den AEM Hostwert für verschiedene Headless-App-Frameworks konfigurierbar machen können.

+++ React-Beispiel

Dieses Beispiel basiert lose auf der [AEM Headless-React-App](../../example-apps/react-app.md)zeigt, wie AEM GraphQL-API-Anfragen so konfiguriert werden können, dass basierend auf Umgebungsvariablen eine Verbindung zu verschiedenen AEM Services hergestellt wird.

React-Apps sollten die [AEM Headless-Client für JavaScript](../../how-to/aem-headless-sdk.md) , um mit AEM GraphQL-APIs zu interagieren. Der AEM Headless-Client, der vom AEM Headless-Client für JavaScript bereitgestellt wird, muss mit dem AEM Service-Host initialisiert werden, mit dem eine Verbindung hergestellt wird.

#### React-Umgebungsdatei

React-Anwendungen [benutzerdefinierte Umgebungsdateien](https://create-react-app.dev/docs/adding-custom-environment-variables/)oder `.env` -Dateien, die im Stammverzeichnis des Projekts gespeichert sind, um Build-spezifische Werte zu definieren. Beispiel: die `.env.development` -Datei enthält Werte, die während der Entwicklung verwendet werden, während `.env.production` enthält Werte, die für Produktions-Builds verwendet werden.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` Dateien für andere Zwecke [kann angegeben werden](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) durch Postfixierung `.env` und einen semantischen Deskriptor, z. B. `.env.stage` oder `.env.production`. Unterschiedlich `.env` -Dateien können beim Ausführen oder Erstellen der React-App verwendet werden, indem Sie die `REACT_APP_ENV` vor dem Ausführen einer `npm` Befehl.

Beispiel: Die `package.json` kann Folgendes enthalten: `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM Headless-Client

Die [AEM Headless-Client für JavaScript](../../how-to/aem-headless-sdk.md) enthält einen AEM Headless-Client, der HTTP-Anfragen an AEM GraphQL-APIs sendet. Der AEM Headless-Client muss mit dem AEM-Host initialisiert werden, mit dem er interagiert, wobei der Wert aus dem aktiven `.env` -Datei.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) Hook

Benutzerdefinierte React-useEffect-Hooks rufen den AEM Headless-Client auf, der mit dem AEM-Host im Namen der React-Komponente initialisiert wurde, die die Ansicht rendert.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### React-Komponente

Den benutzerdefinierten useEffect-Erweiterungspunkt, `useAdventureByPath` wird importiert und verwendet, um die Daten mit dem AEM Headless-Client abzurufen und schließlich den Inhalt an den Endbenutzer zu rendern.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™-Beispiel

Dieses Beispiel basiert auf dem [Beispiel AEM Headless iOS™-App](../../example-apps/ios-swiftui-app.md)zeigt, wie AEM GraphQL-API-Anfragen so konfiguriert werden können, dass basierend auf [Build-spezifische Konfigurationsvariablen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Für iOS™-Apps ist ein benutzerdefinierter AEM Headless-Client erforderlich, um mit AEM GraphQL-APIs zu interagieren. Der AEM Headless-Client muss so geschrieben sein, dass der AEM-Diensthost konfigurierbar ist.

#### Build-Konfiguration

Die XCode-Konfigurationsdatei enthält die Standardkonfigurationsdetails.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Initialisieren des benutzerdefinierten AEM Headless-Clients

Die [Beispiel AEM Headless-iOS-App](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) verwendet einen benutzerdefinierten AEM Headless-Client, der mit den Konfigurationswerten für `AEM_SCHEME` und `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Der benutzerdefinierte Client AEM Headless (`api/Aem.swift`) enthält eine Methode `makeRequest(..)` , das AEM GraphQL-APIs-Anfragen mit dem konfigurierten AEM `scheme` und `host`.

+ `api/Aem.swift`

```swift
/// #makeRequest(..)
/// Generic method for constructing and executing AEM GraphQL persisted queries
private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
    // Encode optional parameters as required by AEM
    let persistedQueryParams = params.map { (param) -> String in
        encode(string: ";\(param.key)=\(param.value)")
    }.joined(separator: "")
    
    // Construct the AEM GraphQL persisted query URL, including optional query params
    let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

    var request = URLRequest(url: URL(string: url)!);
    
    return request;
}
```

[Es können neue Build-Konfigurationsdateien erstellt werden](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) , um eine Verbindung zu verschiedenen AEM-Diensten herzustellen. Die Build-spezifischen Werte für die `AEM_SCHEME` und `AEM_HOST` werden basierend auf dem ausgewählten Build in XCode verwendet, was dazu führt, dass der benutzerdefinierte AEM Headless-Client eine Verbindung mit dem richtigen AEM-Dienst herstellt.

+++

+++ Android™-Beispiel

Dieses Beispiel basiert auf dem [Beispiel AEM Headless-Android™-App](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)zeigt, wie AEM GraphQL-API-Anfragen so konfiguriert werden können, dass sie basierend auf Build-spezifischen (oder Varianten-)Konfigurationsvariablen eine Verbindung zu verschiedenen AEM Services herstellen.

Android™-Apps (wenn sie in Java™ geschrieben sind) sollten die [AEM Headless-Client für Java™](https://github.com/adobe/aem-headless-client-java) , um mit AEM GraphQL-APIs zu interagieren. Der AEM Headless-Client, der vom AEM Headless-Client für Java™ bereitgestellt wird, muss mit dem AEM Service-Host initialisiert werden, mit dem er eine Verbindung herstellt.

#### Konfigurationsdatei erstellen

Android™-Apps definieren &quot;productFlavors&quot;, die zum Erstellen von Artefakten für verschiedene Verwendungen verwendet werden.
In diesem Beispiel wird gezeigt, wie zwei Android™-Produktarten definiert werden können, die verschiedene AEM-Service-Hosts bereitstellen (`AEM_HOST`) Werte für die Entwicklung (`dev`) und Produktion (`prod`) verwendet.

Im `build.gradle` -Datei, eine neue `flavorDimension` benannt `env` erstellt.

Im `env` Dimension, zwei `productFlavors` definiert werden: `dev` und `prod`. Jeder `productFlavor` uses `buildConfigField` , um Build-spezifische Variablen festzulegen, die den AEM Dienst definieren, mit dem eine Verbindung hergestellt werden soll.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™ loader

Initialisieren Sie die `AEMHeadlessClient` Builder, bereitgestellt vom AEM Headless-Client für Java™ mit der `AEM_HOST` Wert aus `buildConfigField` -Feld.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Geben Sie beim Erstellen der Android™-App für verschiedene Verwendungszwecke die `env` und der entsprechende AEM Host-Wert verwendet wird.

+++

## AEM Bild-URLs

Die Bildanforderungen der Headless-App an AEM müssen so konfiguriert sein, dass sie mit dem richtigen AEM-Dienst interagieren, wie in der [obige Tabelle](#managing-aem-hosts).

Während AEM GraphQLs `... on ImageRef` stellt Felder bereit `_authorUrl` und `_publishUrl` mit absoluten URLs zu den jeweiligen AEM-Diensten ist es häufig am einfachsten, die `_path` -Feld und Präfix des AEM Service-Hosts, der zum Abfragen AEM GraphQL-APIs verwendet wird.

Verwenden `_path` kann besonders dann von Vorteil sein, wenn die Headless-App je nach Bereitstellungskontext eine Verbindung zur AEM-Autoren- oder AEM-Veröffentlichungsinstanz herstellen kann.

Wenn die Headless-App ausschließlich mit AEM Author oder Publish interagiert, `_authorUrl` oder `_publishUrl` -Felder können verwendet werden, um die Implementierung zu vereinfachen. Die Anleitungen in den Beispielen unten können ignoriert werden.

### Beispiele

Im Folgenden finden Sie Beispiele dafür, wie Bild-URLs den AEM Host-Wert voranstellen können, der für verschiedene Headless-App-Frameworks konfigurierbar ist. In den Beispielen wird von der Verwendung von GraphQL-Abfragen ausgegangen, die Bildverweise mithilfe der `_path` -Feld.

Beispiel:

#### GraphQL-persistente Abfrage

Diese GraphQL-Abfrage gibt die `_path`. Wie in [GraphQL-Antwort](#examples-react-graphql-response) , der einen Host ausschließt.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### GraphQL-Antwort

Diese GraphQL-Antwort gibt die `_path` , der einen Host ausschließt.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ React-Beispiel

Dieses Beispiel basiert auf dem [Beispiel AEM Headless-React-App](../../example-apps/react-app.md)zeigt, wie Bild-URLs so konfiguriert werden können, dass sie basierend auf Umgebungsvariablen eine Verbindung zu den richtigen AEM-Diensten herstellen.

In diesem Beispiel wird gezeigt, wie das Präfix der Bildreferenz `_path` mit konfigurierbarem `REACT_APP_AEM_HOST` React-Umgebungsvariable.

#### React-Umgebungsdatei

React-Anwendungen [benutzerdefinierte Umgebungsdateien](https://create-react-app.dev/docs/adding-custom-environment-variables/)oder `.env` -Dateien, die im Stammverzeichnis des Projekts gespeichert sind, um Build-spezifische Werte zu definieren. Beispiel: die `.env.development` -Datei enthält Werte, die während der Entwicklung verwendet werden, während `.env.production` enthält Werte, die für Produktions-Builds verwendet werden.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` Dateien für andere Zwecke [kann angegeben werden](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) durch Postfixierung `.env` und einen semantischen Deskriptor, z. B. `.env.stage` oder `.env.production`. Unterschiedlich `.env` -Datei kann beim Ausführen oder Erstellen der React-App verwendet werden, indem Sie die `REACT_APP_ENV` vor dem Ausführen einer `npm` Befehl.

Beispiel: Die `package.json` kann Folgendes enthalten: `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### React-Komponente

Die React-Komponente importiert die `REACT_APP_AEM_HOST` Umgebungsvariable und Präfixe für das Bild `_path` -Wert, um eine vollständig auflösbare Bild-URL bereitzustellen.

Dasselbe `REACT_APP_AEM_HOST` Umgebungsvariable wird verwendet, um den AEM Headless-Client zu initialisieren, der von `useAdventureByPath(..)` benutzerdefinierter useEffect-Erweiterungspunkt, mit dem die GraphQL-Daten aus AEM abgerufen werden. Stellen Sie mithilfe derselben Variablen zur Erstellung der GraphQL-API-Anfrage wie der Bild-URL sicher, dass die React-App für beide Anwendungsfälle mit demselben AEM-Dienst interagiert.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™-Beispiel

Dieses Beispiel basiert auf dem [Beispiel AEM Headless iOS™-App](../../example-apps/ios-swiftui-app.md)zeigt, wie AEM Bild-URLs so konfiguriert werden können, dass basierend auf [Build-spezifische Konfigurationsvariablen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Build-Konfiguration

Die XCode-Konfigurationsdatei enthält die Standardkonfigurationsdetails.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Bild-URL-Generator

In `Aem.swift`, die benutzerdefinierte AEM Headless-Client-Implementierung, eine benutzerdefinierte Funktion `imageUrl(..)` nimmt den Bildpfad wie im Abschnitt `_path` in der GraphLQ-Antwort ein und stellt AEM Host vor. Diese Funktion wird dann bei jeder Bildwiedergabe in den iOS-Ansichten aufgerufen.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### iOS-Ansicht

Die iOS-Ansicht und das Präfix für das Bild `_path` -Wert, um eine vollständig auflösbare Bild-URL bereitzustellen.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[Es können neue Build-Konfigurationsdateien erstellt werden](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) , um eine Verbindung zu verschiedenen AEM-Diensten herzustellen. Die Build-spezifischen Werte für die `AEM_SCHEME` und `AEM_HOST` werden basierend auf dem ausgewählten Build in XCode verwendet, was dazu führt, dass der benutzerdefinierte AEM Headless-Client mit dem richtigen AEM-Dienst interagiert.

+++

+++ Android™-Beispiel

Dieses Beispiel basiert auf dem [Beispiel AEM Headless-Android™-App](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)zeigt, wie AEM Bild-URLs so konfiguriert werden können, dass basierend auf Build-spezifischen (oder Geschmackskonfigurationsvariablen) Konfigurationsvariablen eine Verbindung zu verschiedenen AEM Services hergestellt wird.

#### Konfigurationsdatei erstellen

Android™-Apps definieren &quot;productFlavors&quot;, die zum Erstellen von Artefakten für verschiedene Verwendungen verwendet werden.
In diesem Beispiel wird gezeigt, wie zwei Android™-Produktarten definiert werden können, die verschiedene AEM-Service-Hosts bereitstellen (`AEM_HOST`) Werte für die Entwicklung (`dev`) und Produktion (`prod`) verwendet.

Im `build.gradle` -Datei, eine neue `flavorDimension` benannt `env` erstellt.

Im `env` Dimension, zwei `productFlavors` definiert werden: `dev` und `prod`. Jeder `productFlavor` uses `buildConfigField` , um Build-spezifische Variablen festzulegen, die den AEM Dienst definieren, mit dem eine Verbindung hergestellt werden soll.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Laden des AEM

Android™ verwendet eine `ImageGetter` , um Bilddaten aus AEM abzurufen und lokal zwischenzuspeichern. In `prepareDrawableFor(..)` Der in der aktiven Build-Konfiguration definierte AEM-Service-Host wird verwendet, um dem Bildpfad das Präfix voranzustellen und eine auflösbare URL zu AEM zu erstellen.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Android™-Ansicht

Die Android™-Ansicht ruft die Bilddaten über die `RemoteImagesCache` mithilfe der `_path` -Wert aus der GraphQL-Antwort.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

Geben Sie beim Erstellen der Android™-App für verschiedene Verwendungszwecke die `env` und der entsprechende AEM Host-Wert verwendet wird.

+++