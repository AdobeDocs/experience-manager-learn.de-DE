---
title: Verwalten von AEM-Hosts für AEM GraphQL
description: Erfahren Sie, wie Sie AEM-Hosts in der AEM Headless-App konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1669'
ht-degree: 100%

---

# Verwalten von AEM-Hosts

Beim Bereitstellen einer AEM Headless-Anwendung muss beachtet werden, wie AEM-URLs aufgebaut sind, um sicherzustellen, dass der richtige AEM-Host/die richtige Domain verwendet wird. Die folgenden primären URL-/Anfragetypen sind zu beachten:

+ HTTP-Anfragen an __[AEM GraphQL-APIs](#aem-graphql-api-requests)__
+ __[Bild-URLs](#aem-image-urls)__ für Bild-Assets, die in Inhaltsfragmenten referenziert und von AEM bereitgestellt werden

In der Regel interagiert eine AEM Headless-App mit einem einzigen AEM-Service für GraphQL-API- und Bildanforderungen. Der AEM-Dienst ändert sich je nach Bereitstellung der AEM Headless-App:

| Typ der AEM Headless-Bereitstellung | AEM-Umgebung | AEM-Service |
|-------------------------------|:---------------------:|:----------------:|
| Produktion | Produktion | Veröffentlichen  |
| Authoring-Vorschau | Produktion | Vorschau |
| Entwicklung | Entwicklung | Veröffentlichen  |

Um Permutationen des Bereitstellungstyps zu handhaben, wird jede App-Bereitstellung mit einer Konfiguration erstellt, die angibt, mit welchem AEM-Service eine Verbindung hergestellt werden soll. Der Host/die Domain des konfigurierten AEM-Services wird dann verwendet, um die AEM GraphQL-API-URLs und Bild-URLs zu erstellen. Um den richtigen Ansatz für die Verwaltung von Build-abhängigen Konfigurationen zu bestimmen, verweisen Sie auf die Dokumentation zum Framework der AEM Headless-App (z. B. React, iOS, Android™ usw.), da der Ansatz je nach Framework unterschiedlich ist.

| Client-Typ | [Single Page App (SPA)](../spa.md) | [Web-Komponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM-Host-Konfiguration | ✔ | ✔ | ✔ | ✔ |

Im Folgenden finden Sie Beispiele für mögliche Ansätze zum Erstellen von URLs für [AEM GraphQL-API](#aem-graphql-api-requests)- und [Bildanfragen](#aem-image-requests) für mehrere beliebte Headless-Frameworks und Plattformen.

## AEM GraphQL-API-Anfragen

Die HTTP-GET-Anfragen von der Headless-App an AEM GraphQL-APIs müssen für die Interaktion mit dem richtigen AEM-Service konfiguriert werden, wie in der [obigen Tabelle](#managing-aem-hosts) beschrieben.

Bei Verwendung von [AEM Headless-SDKs](../../how-to/aem-headless-sdk.md) (für Browser-basiertes JavaScript, serverbasiertes JavaScript und Java™ verfügbar) kann ein AEM-Host das AEM Headless-Client-Objekt mit dem AEM-Service initialisieren, mit dem eine Verbindung hergestellt werden soll.

Stellen Sie bei der Entwicklung eines benutzerdefinierten AEM Headless-Clients sicher, dass der Host des AEM-Services basierend auf Build-Parametern parametrisierbar ist.

### Beispiele

Im Folgenden finden Sie Beispiele dafür, wie AEM GraphQL-API-Anfragen den Wert des AEM-Hosts für verschiedene Headless-App-Frameworks konfigurierbar machen können.

+++ React-Beispiel

Dieses Beispiel, grob basierend auf der [AEM Headless React-App](../../example-apps/react-app.md), veranschaulicht, wie AEM GraphQL-API-Anforderungen so konfiguriert werden können, dass sie sich auf der Grundlage von Umgebungsvariablen mit verschiedenen AEM-Services verbinden.

React-Apps sollten den [AEM Headless-Client für JavaScript](../../how-to/aem-headless-sdk.md) verwenden, um mit den GraphQL-APIs von AEM zu interagieren. Der AEM Headless-Client, der vom AEM Headless-Client für JavaScript bereitgestellt wird, muss mit dem Host des AEM-Services initialisiert werden, mit dem eine Verbindung hergestellt wird.

#### React-Umgebungsdatei

React benutzt [benutzerdefinierte Umgebungsdateien](https://create-react-app.dev/docs/adding-custom-environment-variables/), oder `.env`-Dateien, die in dem Stammverzeichnis des Projekts gespeichert sind, um Build-spezifische Werte zu definieren. Die Datei `.env.development` enthält beispielsweise Werte für die Entwicklungsphase, während `.env.production` Werte für Produktions-Builds enthält.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env`-Dateien für andere Verwendungszwecke [können](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) durch Anhängen von `.env` und eines semantischen Deskriptors wie `.env.stage` oder `.env.production` angegeben werden. Verschiedene `.env`-Dateien können beim Ausführen oder Bauen der React-App verwendet werden, indem man `REACT_APP_ENV` vor das Ausführen eines `npm`-Befehls setzt.

Zum Beispiel kann die `package.json` einer React-App die folgende `scripts`-Konfiguration enthalten:

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

Der [AEM Headless-Client für JavaScript](../../how-to/aem-headless-sdk.md) enthält einen AEM Headless-Client, der HTTP-Anfragen an AEM GraphQL-APIs sendet. Der AEM Headless-Client muss mit dem AEM-Host initialisiert werden, mit dem er interagiert, wobei der Wert aus der aktiven `.env`-Datei verwendet wird.

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

Benutzerdefinierte useEffect-Hooks in React rufen den AEM Headless-Client auf, der mit dem AEM-Host im Namen der React-Komponente initialisiert wurde, die die Ansicht rendert.

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

Der benutzerdefinierte useEffect-Hook `useAdventureByPath` wird importiert und verwendet, um die Daten über den AEM Headless-Client abzurufen und den Inhalt für die Endbenutzenden zu rendern.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Beispiel für iOS™

Dieses Beispiel basiert auf der [beispielhaften iOS™-App für AEM Headless](../../example-apps/ios-swiftui-app.md) basiert und veranschaulicht, wie AEM GraphQL-API-Anfragen so konfiguriert werden können, dass sie auf der Grundlage von [Build-spezifischen Konfigurationsvariablen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) eine Verbindung zu verschiedenen AEM-Hosts herstellen.

Für iOS™-Apps ist ein benutzerdefinierter AEM Headless-Client erforderlich, um mit AEM GraphQL-APIs zu interagieren. Der AEM Headless-Client muss so geschrieben sein, dass der Host des AEM-Services konfigurierbar ist.

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

Die [beispielhafte iOS-App für AEM Headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) verwendet einen benutzerdefinierten AEM Headless-Client, der mit den Konfigurationswerten für `AEM_SCHEME` und `AEM_HOST` initialisiert wurde.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

Der benutzerdefinierte AEM-Headless-Client (`api/Aem.swift`) enthält eine Methode `makeRequest(..)`, die AEM GraphQL-API-Anfragen mit dem konfigurierten AEM-`scheme` und `host`-Prefix versehen.

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

[Es können neue Build-Konfigurationsdateien erstellt werden](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3), um eine Verbindung zu verschiedenen AEM-Services herzustellen. Die Build-spezifischen Werte für `AEM_SCHEME` und `AEM_HOST` werden basierend auf dem ausgewählten Build in XCode verwendet, was dazu führt, dass der benutzerdefinierte AEM Headless-Client eine Verbindung mit dem richtigen AEM-Service herstellt.

+++

+++ Android™-Beispiel

Dieses Beispiel, das auf der [beispielhaften Android™-App für AEM Headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app) basiert, veranschaulicht, wie AEM GraphQL-API-Anforderungen so konfiguriert werden können, dass sie auf der Grundlage von Build-spezifischen (oder „Flavors“) Konfigurationsvariablen eine Verbindung zu verschiedenen AEM-Services herstellen.

Android™-Apps (wenn sie in Java™ geschrieben sind) sollten den [AEM Headless-Client für Java™](https://github.com/adobe/aem-headless-client-java) verwenden, um mit AEM GraphQL-APIs zu interagieren. Der AEM Headless-Client, der vom AEM Headless-Client für Java™ bereitgestellt wird, muss mit dem Host des AEM-Services initialisiert werden, mit dem er eine Verbindung herstellt.

#### Erstellen der Konfigurationsdatei

Android™-Apps definieren Produktvarianten („productFlavors“), die zum Erstellen von Artefakten für verschiedene Verwendungen verwendet werden.
Dieses Beispiel zeigt, wie zwei Android™-Produktvarianten definiert werden können, die unterschiedliche Werte für Hosts von AEM-Services (`AEM_HOST`) für die Entwicklung (`dev`) bzw. die Produktion (`prod`) verwenden.

In der Datei `build.gradle` der App wird eine neue `flavorDimension` mit dem Namen `env` erstellt.

In der Dimension `env` sind zwei `productFlavors` definiert: `dev` und `prod`. Jeder `productFlavor` verwendet ein `buildConfigField`, um Build-spezifische Variablen festzulegen, die definieren, mit welchem AEM-Service eine Verbindung hergestellt werden soll.

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

#### Android™-Lader

Initialisieren Sie den `AEMHeadlessClient`-Builder, der vom AEM Headless-Client für Java™ bereitgestellt wird, mit dem Wert `AEM_HOST` aus dem Feld `buildConfigField`.

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

Wenn Sie die Android™-App für verschiedene Verwendungszwecke erstellen, geben Sie die `env`-Variante an, woraufhin der entsprechende Wert für den AEM-Host verwendet wird.

+++

## AEM-Bild-URLs

Die Bildanfragen der Headless-App an AEM müssen so konfiguriert sein, dass sie mit dem richtigen AEM-Service interagieren, wie in der [obigen Tabelle](#managing-aem-hosts) beschrieben.

Adobe empfiehlt die Verwendung [optimierter Bilder](../../how-to/images.md), die über das `_dynamicUrl`-Feld in AEM GraphQL-APIs verfügbar gemacht wurden. Das `_dynamicUrl`-Feld gibt eine URL ohne Host zurück, die mit dem zum Abfragen von AEM GraphQL-APIs verwendeten AEM-Servicehost als Präfix versehen werden kann. Für das Feld `_dynamicUrl` sieht die GraphQL-Antwort wie folgt aus:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Beispiele

Im Folgenden finden Sie Beispiele dafür, wie in Bild-URLs der Wert des AEM-Hosts vorangestellt werden kann, der für verschiedene Headless-App-Frameworks konfigurierbar ist. Die Beispiele gehen von der Verwendung von GraphQL-Abfragen aus, die Bildreferenzen unter Verwendung des Feldes `_dynamicUrl` zurückgeben.

Zum Beispiel:

#### GraphQL-persistierte Abfrage

Diese GraphQL-Abfrage gibt den `_dynamicUrl` einer Bildreferenz zurück. Wie in der [GraphQL-Antwort](#examples-react-graphql-response) zu sehen, die einen Host ausschließt.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL-Antwort

Diese GraphQL-Antwort gibt den `_dynamicUrl` der Bildreferenz zurück, der einen Host ausschließt.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ React-Beispiel

Dieses Beispiel basiert auf der [beispielhaften AEM Headless-React-App](../../example-apps/react-app.md) und zeigt, wie Bild-URLs so konfiguriert werden können, dass sie basierend auf Umgebungsvariablen eine Verbindung zu den richtigen AEM-Services herstellen.

Dieses Beispiel zeigt, wie dem Feld `_dynamicUrl` der Bildreferenz eine konfigurierbare React-Umgebungsvariable `REACT_APP_AEM_HOST` vorangestellt wird.

#### React-Umgebungsdatei

React benutzt [benutzerdefinierte Umgebungsdateien](https://create-react-app.dev/docs/adding-custom-environment-variables/), oder `.env`-Dateien, die in dem Stammverzeichnis des Projekts gespeichert sind, um Build-spezifische Werte zu definieren. Die Datei `.env.development` enthält beispielsweise Werte für die Entwicklungsphase, während `.env.production` Werte für Produktions-Builds enthält.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env`-Dateien für andere Verwendungszwecke [können](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) durch Anhängen von `.env` und eines semantischen Deskriptors wie `.env.stage` oder `.env.production` angegeben werden. Verschiedene `.env`-Dateien können beim Ausführen oder Bauen der React-App verwendet werden, indem man `REACT_APP_ENV` vor das Ausführen eines `npm`-Befehls setzt.

Zum Beispiel kann die `package.json` einer React-App die folgende `scripts`-Konfiguration enthalten:

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

Die React-Komponente importiert die Umgebungsvariable `REACT_APP_AEM_HOST` und stellt dem `_dynamicUrl`-Wert des Bilds ein Präfix voran, um eine vollständig auflösbare Bild-URL zu erhalten.

Dieselbe Umgebungsvariable `REACT_APP_AEM_HOST` wird verwendet, um den AEM Headless-Client zu initialisieren, der vom benutzerdefinierten useEffect-Hook `useAdventureByPath(..)` verwendet wird, um die GraphQL-Daten von AEM abzurufen. Stellen Sie mithilfe derselben Variablen zur Erstellung der GraphQL-API-Anfrage als Bild-URL sicher, dass die React-App für beide Anwendungsfälle mit demselben AEM-Service interagiert.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Beispiel für iOS™

Dieses Beispiel basiert auf der [beispielhaften iOS™-App für AEM Headless](../../example-apps/ios-swiftui-app.md) und veranschaulicht, wie AEM-Bild-URLs so konfiguriert werden können, dass sie auf der Grundlage von [Build-spezifischen Konfigurationsvariablen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) eine Verbindung zu verschiedenen AEM-Hosts herstellen.

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

In `Aem.swift`, der benutzerdefinierten Client-Implementierung von AEM Headless, nimmt eine benutzerdefinierte Funktion `imageUrl(..)` den Bildpfad, der im Feld `_dynamicUrl` in der GraphLQ-Antwort angegeben ist, und stellt ihm den AEM-Host voran. Diese Funktion wird dann in den iOS-Ansichten aufgerufen, wenn ein Bild gerendert wird.

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
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS-Ansicht

In der iOS-Ansicht wird dem `_dynamicUrl`-Wert des Bildes ein Präfix vorangestellt, um eine vollständig auflösbare Bild-URL zu erhalten.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[Es können neue Build-Konfigurationsdateien erstellt werden](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3), um eine Verbindung zu verschiedenen AEM-Services herzustellen. Die Build-spezifischen Werte für `AEM_SCHEME` und `AEM_HOST` werden basierend auf dem ausgewählten Build in XCode verwendet, was dazu führt, dass der benutzerdefinierte AEM Headless-Client mit dem richtigen AEM-Service interagiert.

+++

+++ Android™-Beispiel

Dieses Beispiel basiert auf der [beispielhaften Android™-App für AEM Headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app) und veranschaulicht, wie AEM-Bild-URLs so konfiguriert werden können, dass sie auf der Grundlage von Build-spezifischen (oder „Flavors“) Konfigurationsvariablen eine Verbindung zu verschiedenen AEM-Services herstellen.

#### Erstellen der Konfigurationsdatei

Android™-Apps definieren „productFlavors“, die für die Erstellung von Artefakten für verschiedene Zwecke verwendet werden.
Dieses Beispiel zeigt, wie zwei Android™-Produktvarianten definiert werden können, die unterschiedliche Werte für Hosts von AEM-Services (`AEM_HOST`) für die Entwicklung (`dev`) bzw. die Produktion (`prod`) verwenden.

In der Datei `build.gradle` der App wird eine neue `flavorDimension` mit dem Namen `env` erstellt.

In der Dimension `env` sind zwei `productFlavors` definiert: `dev` und `prod`. Jeder `productFlavor` verwendet ein `buildConfigField`, um Build-spezifische Variablen festzulegen, die definieren, mit welchem AEM-Service eine Verbindung hergestellt werden soll.

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

#### Laden des AEM-Bildes

Android™ verwendet einen `ImageGetter`, um Bilddaten aus AEM abzurufen und lokal zwischenzuspeichern. In `prepareDrawableFor(..)` wird der Host des AEM-Services verwendet, der in der aktiven Build-Konfiguration definiert ist, um dem Bildpfad ein Präfix voranzustellen, das eine auflösbare URL für AEM erzeugt.

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
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™-Ansicht

Die Android™-Ansicht erhält die Bilddaten über den `RemoteImagesCache` unter Verwendung des `_dynamicUrl`-Wertes aus der GraphQL-Antwort.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Wenn Sie die Android™-App für verschiedene Verwendungszwecke erstellen, geben Sie die Option `env` an, woraufhin der entsprechende Wert für den AEM-Host verwendet wird.

+++
