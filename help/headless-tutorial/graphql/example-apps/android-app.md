---
title: Android-App - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Android-Anwendung zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden.
version: Cloud Service
mini-toc-levels: 2
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 8b2c116ceb6ab8c3a009dcec6629c2e97d815b7b
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 6%

---

# Android-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Android-Anwendung zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden. Die [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) wird verwendet, um die GraphQL-Abfragen auszuführen und Daten Java-Objekten zuzuordnen, um die App zu unterstützen.

![Android-Java-App mit AEM Headless](./assets/android-java-app/android-app.png)


Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM

Die Android-Anwendung funktioniert mit den folgenden AEM Bereitstellungsoptionen. Für alle Implementierungen ist die [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances)

Die Android-Anwendung ist für die Verbindung mit einer __AEM-Veröffentlichung__ -Umgebung, kann jedoch Inhalte von der AEM-Autoreninstanz stammen, wenn die Authentifizierung in der Konfiguration der Android-Anwendung bereitgestellt wird.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) und öffnen Sie den Ordner `android-app`
1. Datei ändern `config.properties` at `app/src/main/assets/config.properties` und aktualisieren `contentApi.endpoint` , um Ihre Ziel-AEM-Umgebung abzugleichen:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __Grundlegende Authentifizierung__

   Die `contentApi.user` und `contentApi.password` authentifizieren Sie einen lokalen AEM-Benutzer mit Zugriff auf WKND-GraphQL-Inhalte.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Herunterladen von [Android Virtual Device](https://developer.android.com/studio/run/managing-avds) (Minimum API 28).
1. Erstellen und stellen Sie die App mit dem Android-Emulator bereit.


### Herstellen einer Verbindung zu AEM Umgebungen

`10.0.2.2` ist [Spezialalias IP](https://developer.android.com/studio/run/emulator-networking) für localhost bei Verwendung des Emulators `10.0.2.2:4502` entspricht `localhost:4502`. Wenn Sie eine Verbindung zu einer AEM Veröffentlichungsumgebung herstellen (empfohlen), ist keine Autorisierung erforderlich und `contentAPi.user` und `contentApi.password` kann leer gelassen werden.

Wenn eine Verbindung zu einer AEM Autorenumgebung hergestellt wird [Autorisierung](https://github.com/adobe/aem-headless-client-java#using-authorization) ist erforderlich. Standardmäßig ist die Anwendung so eingerichtet, dass sie eine einfache Authentifizierung mit einem Benutzernamen und einem Kennwort von `admin:admin`. Die [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) bietet die Möglichkeit, [Token-basierte Authentifizierung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Um die Token-basierte Authentifizierung zu verwenden, aktualisieren Sie den Client-Builder in `AdventureLoader.java` und `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## Der Code

Nachstehend finden Sie eine kurze Zusammenfassung der wichtigen Dateien und des Codes, mit denen die Anwendung betrieben wird. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

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

AEM persistente Abfragen werden über HTTP-GET ausgeführt und daher die [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) wird verwendet, um die beibehaltenen GraphQL-Abfragen für AEM auszuführen und den Erlebnisinhalt in die App zu laden.

Jede beibehaltene Abfrage verfügt über eine entsprechende &quot;Ladeklasse&quot;, die den AEM HTTP-Endpunkt asynchron aufruft und die Abenteuerdaten unter Verwendung der benutzerdefinierten [Datenmodell](#data-models).

+ `loader/AdventuresLoader.java`

   Ruft die Liste der Abenteuer auf dem Startbildschirm der Anwendung ab, indem Sie die `wknd-shared/adventures-all` persistente Abfrage.

+ `loader/AdventureLoader.java`

   Ruft ein einzelnes Abenteuer ab, das es über die `slug` -Parameter mithilfe der `wknd-shared/adventure-by-slug` persistente Abfrage.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL-Antwortdatenmodelle{#data-models}

`Adventure.java` ist ein Java-POJO, das mit den JSON-Daten aus der GraphQL-Anfrage initialisiert wird und ein Abenteuer zur Verwendung in den Ansichten der Android-Anwendung modelliert.

### Ansichten

Die Android-Anwendung verwendet zwei Ansichten, um die Abenteuerdaten im mobilen Erlebnis darzustellen.

+ `AdventureListFragment.java`

   Ruft die `AdventuresLoader` und zeigt die zurückgegebenen Abenteuer in einer Liste an.

+ `AdventureDetailFragment.java`

   Ruft die `AdventureLoader` mithilfe der `slug` über die Abenteuerauswahl auf der `AdventureListFragment` und zeigt die Details eines einzelnen Abenteuers an.

### Remote-Bilder

`loader/RemoteImagesCache.java` ist eine Dienstprogrammklasse, die bei der Vorbereitung von Remote-Bildern in einem Cache hilft, damit sie mit Elementen der Android-Benutzeroberfläche verwendet werden können. Der Erlebnisinhalt referenziert Bilder in AEM Assets über eine URL und diese Klasse wird zur Anzeige dieser Inhalte verwendet.

## Zusätzliche Ressourcen

+ [Erste Schritte mit AEM Headless - GraphQL-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java)
