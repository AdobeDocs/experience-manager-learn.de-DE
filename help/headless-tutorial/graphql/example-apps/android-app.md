---
title: Android-App - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Es wird eine Android-Anwendung bereitgestellt, die zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden. Der Apollo Client Android wird verwendet, um die GraphQL-Abfragen zu generieren und Daten Swift-Objekten zuzuordnen, um die App zu unterstützen. SwiftUI wird zum Rendern einer einfachen Listen- und Detailansicht des Inhalts verwendet.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 6%

---


# Android-App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Es wird eine Android-Anwendung bereitgestellt, die zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden. Die [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) wird verwendet, um die GraphQL-Abfragen auszuführen und Daten Java-Objekten zuzuordnen, um die App zu unterstützen.

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## AEM

Das Programm ist für die Verbindung mit einem AEM konzipiert **Veröffentlichen** Umgebung mit der neuesten Version der [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert.

* [ zu AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=de)

Wir empfehlen [Bereitstellen der WKND-Referenz-Site in einer Cloud Service-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) oder [AEM 6.5 QuickStart-JAR](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances) kann auch verwendet werden.

## Informationen zur Verwendung

1. Klonen Sie die `aem-guides-wknd-graphql` repository:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) und öffnen Sie den Ordner `android-app`
1. Datei ändern `config.properties` at `app/src/main/assets/config.properties` und aktualisieren `contentApi.endpoint` , um Ihre Ziel-AEM-Umgebung abzugleichen:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Herunterladen von [Android Virtual Device](https://developer.android.com/studio/run/managing-avds) (min-API 28)
1. Erstellen und stellen Sie die App mit dem Android-Emulator bereit.


### Herstellen einer Verbindung zu AEM Umgebungen

`10.0.2.2` ist [Spezialalias](https://developer.android.com/studio/run/emulator-networking) für localhost bei Verwendung des Emulators. Also `10.0.2.2:4502` entspricht `localhost:4502`. Wenn Sie eine Verbindung zu einer AEM Veröffentlichungsumgebung herstellen (empfohlen), ist keine Autorisierung erforderlich und `contentAPi.user` und `contentApi.password` kann leer gelassen werden.

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

### Abrufen von Inhalten

Die [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java) wird von der App verwendet, um die GraphQL-Abfrage gegen AEM auszuführen und den Abenteuerinhalt in die App zu laden.

`AdventuresLoader.java` ist die Datei, die die anfängliche Liste von Abenteuern abruft und auf den Startbildschirm der Anwendung lädt. Sie nutzt [Beständige Abfragen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) , [vorverpackt](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) mit der WKND-Referenz-Website. Der Endpunkt lautet `/wknd/adventures-all`. `AEMHeadlessClientBuilder` instanziiert eine neue Instanz basierend auf dem API-Endpunkt, der in `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` ist die Datei, die den Adventure-Inhalt für jede Detailansicht abruft und lädt. Die `AEMHeadlessClient` wird verwendet, um die Abfrage auszuführen. Eine normale graphQL-Abfrage wird basierend auf dem Pfad zum Adventure-Inhaltsfragment ausgeführt. Die Abfrage wird in einer separaten Datei mit dem Namen [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` ist ein POJO, das mit den JSON-Daten aus der GraphQL-Anfrage initialisiert wird.

`RemoteImagesCache.java` ist eine Dienstprogrammklasse, die bei der Vorbereitung von Remote-Bildern in einem Cache hilft, damit sie mit Elementen der Android-Benutzeroberfläche verwendet werden können. Der Adventure-Inhalt referenziert Bilder in AEM Assets über eine URL und diese Klasse wird zur Anzeige dieser Inhalte verwendet.

### Ansichten

`AdventureListFragment.java` - wenn Trigger aufgerufen werden, `AdventuresLoader` und zeigt die zurückgegebenen Abenteuer in einer Liste an.

`AdventureDetailFragment.java` - initialisiert `AdventureLoader` und zeigt die Details eines einzelnen Abenteuers an.

## Zusätzliche Ressourcen

* [Erste Schritte mit AEM Headless - GraphQL-Tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM Headless-Client für Java](https://github.com/adobe/aem-headless-client-java)

