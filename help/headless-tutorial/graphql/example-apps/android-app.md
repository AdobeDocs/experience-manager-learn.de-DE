---
title: Android-App – AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Android-App zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden können.
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 679b9bf9f0948e2b24d613b530d3ae644c92057d
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 100%

---

# Android-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Android-App zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden können. Der [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) wird verwendet, um GraphQL-Abfragen auszuführen und Daten Java-Objekten zuzuordnen, um die App zu unterstützen.

![Android-Java-App mit AEM Headless](./assets/android-java-app/android-app.png)


Sie finden den [Quell-Code auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

## Voraussetzungen {#prerequisites}

Folgende Tools sollten lokal installiert werden:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM-Anforderungen

Die Android-App kann mit den folgenden AEM-Bereitstellungsoptionen verwendet werden. Für alle Bereitstellungen muss die [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert werden.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)

Die Android-App ist für die Verbindung mit einer __AEM Publish__-Umgebung konzipiert, kann jedoch Inhalte von AEM Author beziehen, wenn die Authentifizierung in der Konfiguration der Android-App bereitgestellt wird.

## Informationen zur Verwendung

1. Klonen Sie das Repository `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Starten Sie [Android Studio](https://developer.android.com/studio) und öffnen Sie den Ordner `android-app`.
1. Ändern Sie die Datei `config.properties` unter `app/src/main/assets/config.properties` und aktualisieren Sie `contentApi.endpoint` entsprechend Ihrer AEM-Zielumgebung:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Standardauthentifizierung__

   `contentApi.user` und `contentApi.password` authentifizieren lokale AEM-Benutzende mit Zugriff auf WKND GraphQL-Inhalte.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Laden Sie ein [virtuelles Android-Gerät](https://developer.android.com/studio/run/managing-avds) (Minimum API 28) herunter.
1. Erstellen Sie die App und stellen Sie sie mit dem Android-Emulator bereit.


### Verbinden mit AEM-Umgebungen

Wenn Sie eine Verbindung zu einer AEM Author-Umgebung herstellen, ist eine [Autorisierung](https://github.com/adobe/aem-headless-client-java#using-authorization) erforderlich. [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) bietet die Möglichkeit zur Nutzung der [Token-basierten Authentifizierung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=de). Um die Token-basierte Authentifizierung zu verwenden, aktualisieren Sie den Client-Builder in `AdventureLoader.java` und `AdventuresLoader.java`:

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

Nachstehend finden Sie eine kurze Zusammenfassung der wichtigen Dateien und des Codes für den Betrieb der App. Den vollständigen Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Persistierte Abfragen

Gemäß den Best Practices für AEM Headless verwendet die iOS-Anwendung AEM GraphQL-persistierte Abfragen, um Adventure-Daten abzufragen. Die Anwendung verwendet zwei persistierte Abfragen:

+ Die persistierte Abfrage `wknd/adventures-all` gibt alle Adventures in AEM mit einer gekürzten Reihe von Eigenschaften zurück. Diese persistierte Abfrage bestimmt die Erlebnisliste der ersten Ansicht.

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
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ Die persistierte Abfrage `wknd/adventure-by-slug` gibt ein einzelnes Adventure durch `slug` (eine benutzerdefinierte Eigenschaft, die ein Adventure eindeutig identifiziert) mit einer vollständigen Reihe von Eigenschaften zurück. Diese persistierte Abfrage ermöglicht Detailansichten des Adventures.

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
          _dynamicUrl
          _path
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

### Durchführen einer GraphQL-persistierten Abfrage

Persistierte Abfragen von AEM werden über HTTP-GET ausgeführt und daher wird der [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) verwendet, um persistierte GraphQL-Abfragen für AEM auszuführen und den Adventure-Inhalt in die App zu laden.

Jede persistierte Abfrage verfügt über eine entsprechende „Laderklasse“, die den AEM-HTTP-GET-Endpunkt asynchron aufruft und die Adventure-Daten unter Verwendung des benutzerdefinierten [Datenmodells](#data-models) zurückgibt.

+ `loader/AdventuresLoader.java`

  Ruft die Liste der Adventures auf dem Startbildschirm der App mithilfe der persistierten Abfrage `wknd-shared/adventures-all` ab.

+ `loader/AdventureLoader.java`

  Ruft ein einzelnes Adventure mithilfe der persistierten Abfrage `wknd-shared/adventure-by-slug` ab und wählt es über den `slug`-Parameter aus.

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

### GraphQL-Antwort-Datenmodelle{#data-models}

`Adventure.java` ist ein Java-POJO, das mit den JSON-Daten aus der GraphQL-Anfrage initialisiert wird und ein Adventure für die Ansichten der Android-App modelliert.

### Ansichten

Die Android-App verwendet zwei Ansichten, um die Adventure-Daten in mobilen Erlebnissen darzustellen.

+ `AdventureListFragment.java`

  Ruft `AdventuresLoader` auf und zeigt die zurückgegebenen Adventures in einer Liste an.

+ `AdventureDetailFragment.java`

  Ruft `AdventureLoader` mithilfe des über die Adventure-Auswahl in der Ansicht `AdventureListFragment` übergebenen `slug`-Parameters auf und zeigt die Details eines einzelnen Adventures an.

### Remote-Bilder

`loader/RemoteImagesCache.java` ist eine Dienstprogrammklasse, die bei der Vorbereitung von Remote-Bildern in einem Cache hilft, damit sie mit Elementen der Android-Benutzeroberfläche verwendet werden können. Der Adventure-Inhalt verweist über eine URL auf Bilder in AEM Assets, und diese Klasse wird zur Anzeige dieses Inhalts verwendet.

## Zusätzliche Ressourcen

+ [Erste Schritte mit AEM Headless – GraphQL-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de)
+ [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java)
