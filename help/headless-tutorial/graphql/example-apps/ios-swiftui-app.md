---
title: iOS App - Beispiel AEM Headless
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese iOS-Anwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen können.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: cd7cb89f407f5e0c465544593563534472daf928
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 4%

---

# iOS-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese iOS-Anwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen können.

![iOS SwiftUI-App mit AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (macOS erforderlich)
+ [Git](https://git-scm.com/)

## AEM

Die iOS-Anwendung funktioniert mit den folgenden AEM Bereitstellungsoptionen. Für alle Implementierungen ist die [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances)

Die iOS-Anwendung ist für die Verbindung mit einem __AEM-Veröffentlichung__ -Umgebung, kann jedoch Inhalte von der AEM-Autoreninstanz stammen, wenn die Authentifizierung in der Konfiguration der iOS-Anwendung bereitgestellt wird.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) und öffnen Sie den Ordner `ios-app`
1. Datei ändern `Config.xcconfig` Datei und Aktualisierung `AEM_SCHEME` und `AEM_HOST` , um Ihren AEM-Veröffentlichungsdienst als Ziel festzulegen.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   Wenn Sie eine Verbindung zur AEM-Autoreninstanz herstellen, fügen Sie die `AEM_AUTH_TYPE` und Unterstützung der Authentifizierungseigenschaften für `Config.xcconfig`.

   __Grundlegende Authentifizierung__

   Die `AEM_USERNAME` und `AEM_PASSWORD` authentifizieren Sie einen lokalen AEM-Benutzer mit Zugriff auf WKND-GraphQL-Inhalte.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Token-Authentifizierung__

   Die `AEM_TOKEN` ist [Zugriffstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) , der sich für einen AEM Benutzer mit Zugriff auf WKND-GraphQL-Inhalte authentifiziert.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Erstellen Sie die Anwendung mit Xcode und stellen Sie die App auf dem iOS-Simulator bereit.
1. Eine Liste von Abenteuern von der WKND-Site sollte auf der Anwendung angezeigt werden. Die Auswahl eines Abenteuers öffnet die Details des Abenteuers. Rufen Sie in der Liste der Abenteuer die Daten aus AEM auf.

## Der Code

Nachstehend finden Sie eine Zusammenfassung zur Erstellung der iOS-Anwendung, zur Verbindung mit AEM Headless zum Abrufen von Inhalten mithilfe von GraphQL-gespeicherten Abfragen und zur Darstellung dieser Daten. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Beständige Abfragen

Gemäß AEM Best Practices für Headless verwendet die iOS-Anwendung AEM von GraphQL gespeicherten Abfragen, um Abenteuerdaten abzufragen. Die Anwendung verwendet zwei persistente Abfragen:

+ `wknd/adventures-all` persistente Abfrage, die alle Abenteuer in AEM mit einer gekürzten Reihe von Eigenschaften zurückgibt. Diese beibehaltene Abfrage treibt die Erlebnisliste der ersten Ansicht an.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` persistente Abfrage, die ein einzelnes Abenteuer von `slug` (eine benutzerdefinierte Eigenschaft, die ein Abenteuer eindeutig identifiziert) mit einer vollständigen Reihe von Eigenschaften. Diese beibehaltene Abfrage ermöglicht die Detailansicht des Abenteuers.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL-persistente Abfrage ausführen

AEM persistente Abfragen werden über HTTP-GET ausgeführt und daher können keine gängigen GraphQL-Bibliotheken verwendet werden, die HTTP-POST wie Apollo verwenden. Erstellen Sie stattdessen eine benutzerdefinierte Klasse, die die persistenten HTTP-GET-Anfragen für die Abfrage an AEM ausführt.

`AEM/Aem.swift` instanziiert die `Aem` -Klasse, die für alle Interaktionen mit AEM Headless verwendet wird. Das Muster lautet:

1. Jede persistente Abfrage verfügt über eine entsprechende öffentliche Funktion (z. B. `getAdventures(..)` oder `getAdventureBySlug(..)`) die Ansichten der iOS-Anwendung aufrufen, um Erlebnisdaten zu erhalten.
1. Die öffentliche Funktion bezeichnet einen privaten Zweck `makeRequest(..)` , das eine asynchrone HTTP-GET-Anfrage an AEM Headless aufruft und die JSON-Daten zurückgibt.
1. Jede öffentliche Funktion dekodiert dann die JSON-Daten und führt alle erforderlichen Prüfungen oder Umwandlungen durch, bevor die Abenteuer-Daten an die Ansicht zurückgegeben werden.

+ AEM GraphQL-JSON-Daten werden mit den in `AEM/Models.swift`, die den JSON-Objekten zugeordnet ist, gab mein AEM Headless zurück.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }

    ...

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

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL-Antwortdatenmodelle

iOS bevorzugt die Zuordnung von JSON-Objekten zu typisierten Datenmodellen.

Die `src/AEM/Models.swift` definiert die [dekodierbar](https://developer.apple.com/documentation/swift/decodable) Swift-Strukturen und -Klassen, die den von AEM JSON-Antworten zurückgegebenen AEM JSON-Antworten zugeordnet sind.

### Ansichten

SwiftUI wird für die verschiedenen Ansichten in der Anwendung verwendet. Apple bietet ein Tutorial zu den ersten Schritten für [Erstellen von Listen und Navigation mit SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   Der Eintrag des Antrags und enthält `AdventureListView` which `.onAppear` Der Ereignishandler wird verwendet, um alle Abenteuerdaten abzurufen über `aem.getAdventures()`. Die freigegebene `aem` -Objekt wird hier initialisiert und anderen Ansichten als [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   Zeigt eine Liste der Abenteuer an (basierend auf den Daten aus `aem.getAdventures()`) und zeigt für jedes Abenteuer mithilfe der `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   Zeigt die einzelnen Elemente in der Abenteuerliste an (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   Zeigt Details zu einem Abenteuer an, einschließlich Titel, Beschreibung, Preis, Aktivitätstyp und Primärbild. In dieser Ansicht AEM Sie nach vollständigen Abenteuerdetails mit `aem.getAdventureBySlug(slug: slug)`, wobei `slug` -Parameter wird basierend auf der ausgewählten Listenzeile übergeben.

### Remote-Bilder

Bilder, auf die von abenteuerlichen Inhaltsfragmenten verwiesen wird, werden von AEM bereitgestellt. Diese iOS-App verwendet den Pfad `_path` in der GraphQL-Antwort ein und setzt das Präfix `AEM_SCHEME` und `AEM_HOST` , um eine vollständig qualifizierte URL zu erstellen.

Wenn eine Verbindung zu geschützten Ressourcen auf AEM hergestellt werden soll, für die eine Autorisierung erforderlich ist, müssen Bildanforderungen ebenfalls Anmeldeinformationen hinzugefügt werden.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) und [SDWebImage](https://github.com/SDWebImage/SDWebImage) werden verwendet, um die Remote-Bilder von AEM zu laden, die das Abenteuer-Bild auf der `AdventureListItemView` und `AdventureDetailView` Ansichten.

Die `aem` -Klasse (in `AEM/Aem.swift`) ermöglicht die Verwendung AEM Bilder auf zwei Arten:

1. `aem.imageUrl(path: String)` wird in Ansichten verwendet, um das AEM-Schema vorzuhängen und den Pfad des Bildes zu hosten und so eine vollständig qualifizierte URL zu erstellen.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. Die `convenience init(..)` in `Aem` Legen Sie HTTP-Autorisierungs-Header für die Bild-HTTP-Anforderung fest, basierend auf der Konfiguration der iOS-Anwendungen.

   + Wenn __einfache Authentifizierung__ konfiguriert ist, wird die einfache Authentifizierung an alle Bildanforderungen angehängt.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Wenn __Token-Authentifizierung__ konfiguriert ist, wird die Token-Authentifizierung an alle Bildanforderungen angehängt.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Wenn __keine Authentifizierung__ konfiguriert ist, wird keine Authentifizierung an Bildanforderungen angehängt.



Ein ähnlicher Ansatz kann mit der SwiftUI-nativen verwendet werden [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` wird ab iOS 15.0 unterstützt.

## Zusätzliche Ressourcen

+ [Erste Schritte mit AEM Headless - GraphQL-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Tutorial zu SwiftUI-Listen und Navigation](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
