---
title: Beständige GraphQL-Abfragen - Erweiterte Konzepte von AEM Headless - GraphQL
description: In diesem Kapitel zu den erweiterten Konzepten von Adobe Experience Manager (AEM) Headless erfahren Sie, wie Sie persistente GraphQL-Abfragen mit Parametern erstellen und aktualisieren. Erfahren Sie, wie Sie Cache-Steuerungsparameter in persistenten Abfragen übergeben.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# Persistente GraphQL-Abfragen

Beständige Abfragen sind Abfragen, die auf dem Adobe Experience Manager-Server (AEM) gespeichert werden. Clients können eine HTTP-GET-Anfrage mit dem Abfragenamen senden, um sie auszuführen. Der Vorteil dieses Ansatzes ist die Cache-Fähigkeit. Während clientseitige GraphQL-Abfragen auch über HTTP-POST-Anfragen ausgeführt werden können, die nicht zwischengespeichert werden können, können persistente Abfragen über HTTP-Caches oder ein CDN zwischengespeichert werden, was die Leistung verbessert. Beständige Abfragen ermöglichen es Ihnen, Ihre Anforderungen zu vereinfachen und die Sicherheit zu verbessern, da Ihre Abfragen auf dem Server eingekapselt sind und der AEM Administrator die volle Kontrolle über sie hat. Es ist **Best Practice und dringend empfohlen** zur Verwendung persistenter Abfragen bei der Arbeit mit der AEM GraphQL-API.

Im vorherigen Kapitel haben Sie einige erweiterte GraphQL-Abfragen durchsucht, um Daten für die WKND-App zu erfassen. In diesem Kapitel speichern Sie die Abfragen, um zu AEM und zu erfahren, wie Sie die Cache-Steuerung für persistente Abfragen verwenden.

## Voraussetzungen {#prerequisites}

Dieses Dokument ist Teil eines mehrteiligen Tutorials. Stellen Sie sicher, dass [vorheriges Kapitel](explore-graphql-api.md) vor der Fortsetzung dieses Kapitels abgeschlossen wurde.

## Ziele {#objectives}

In diesem Kapitel erfahren Sie, wie Sie:

* Beibehalten von GraphQL-Abfragen mit Parametern
* Verwenden von Cache-Steuerungsparametern mit persistenten Abfragen

## Überprüfen _GraphQL - Beständige Abfragen_ Konfigurationseinstellungen

Sehen wir uns das einmal an _GraphQL - Beständige Abfragen_ sind für das WKND Site-Projekt in Ihrer AEM-Instanz aktiviert.

1. Navigieren Sie zu **Instrumente** > **Allgemein** > **Konfigurationsbrowser**.

1. Auswählen **WKND Shared**, wählen Sie **Eigenschaften** in der oberen Navigationsleiste, um die Konfigurationseigenschaften zu öffnen. Auf der Seite &quot;Konfigurationseigenschaften&quot;sollte Folgendes angezeigt werden: **GraphQL - Persistente Abfragen** -Berechtigung aktiviert ist.

   ![Konfigurationseigenschaften](assets/graphql-persisted-queries/configuration-properties.png)

## Beibehalten von GraphQL-Abfragen mithilfe des integrierten GraphiQL Explorer-Tools

Behalten wir in diesem Abschnitt die GraphQL-Abfrage bei, die später in der Clientanwendung zum Abrufen und Rendern der Daten des Abenteuer-Inhaltsfragments verwendet wird.

1. Geben Sie die folgende Abfrage in den GraphiQL Explorer ein:

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Stellen Sie sicher, dass die Abfrage funktioniert, bevor Sie sie speichern.

1. Tippen Sie anschließend auf Speichern unter und geben Sie ein `adventure-details-by-slug` als Abfragename.

   ![Persist GraphQL Query](assets/graphql-persisted-queries/persist-graphql-query.png)

## Persistente Abfrage mit Variablen durch Kodierung von Sonderzeichen ausführen

Im Folgenden wird erläutert, wie persistente Abfragen mit Variablen von clientseitigen Anwendungen ausgeführt werden, indem die Sonderzeichen kodiert werden.

Um eine persistente Abfrage auszuführen, sendet die Client-Anwendung eine GET-Anfrage mit der folgenden Syntax:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

So führen Sie eine persistente Abfrage aus _mit einer Variablen_, ändert sich die obige Syntax in:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Die Sonderzeichen wie Semikolons (;), Gleichheitszeichen (=), Schrägstriche (/) und Leerzeichen müssen konvertiert werden, um die entsprechende UTF-8-Kodierung zu verwenden.

Durch Ausführen der `getAllAdventureDetailsBySlug` -Abfrage vom Befehlszeilen-Terminal aus verwenden, werden diese Konzepte in Aktion überprüft.

1. Öffnen Sie den GraphiQL Explorer und klicken Sie auf die Schaltfläche **Ellipsen** (..) neben der persistenten Abfrage `getAllAdventureDetailsBySlug`Klicken Sie auf **URL kopieren**. Fügen Sie die kopierte URL in ein Textpad ein. Sie sieht wie folgt aus:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Hinzufügen `yosemite-backpacking` als Variablenwert

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Kodieren Sie die Sonderzeichen (;) und Gleichheitszeichen (=).

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Öffnen Sie ein Befehlszeilen-Terminal und verwenden Sie [Curl](https://curl.se/) Abfrage ausführen

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Wenn Sie die oben genannte Abfrage für die AEM-Autorenumgebung ausführen, müssen Sie die Anmeldeinformationen senden. Siehe [Lokales Entwicklungs-Zugriffstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) zur Veranschaulichung und [Aufrufen der AEM-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) für Dokumentationsdetails.

Lesen Sie auch [Ausführen einer beständigen Abfrage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Verwenden von Abfragevariablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)und [Kodieren der Abfrage-URL zur Verwendung durch eine App](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) , um die Ausführung persistenter Abfragen durch Clientanwendungen zu erfahren.

## Aktualisieren von Cache-Control-Parametern in persistenten Abfragen {#cache-control-all-adventures}

Mit der AEM GraphQL-API können Sie die standardmäßigen Cache-Steuerungsparameter auf Ihre Abfragen aktualisieren, um die Leistung zu verbessern. Die Standardwerte für die Cache-Steuerung sind:

* 60 Sekunden ist die Standard-TTL (max=60) für den Client (z. B. einen Browser)

* 7200 Sekunden ist die standardmäßige TTL (s-maxage=7200) für den Dispatcher und CDN. auch als freigegebene Zwischenspeicher bezeichnet

Verwenden Sie die `adventures-all` Abfrage zum Aktualisieren der Cache-Steuerelement-Parameter. Die Abfrageantwort ist groß und es ist nützlich, die `age` im Cache. Diese beibehaltene Abfrage wird später verwendet, um die [Clientanwendung](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Öffnen Sie den GraphiQL Explorer und klicken Sie auf die Schaltfläche **Ellipsen** (...) neben der persistenten Abfrage klicken Sie auf **Kopfzeilen** zum Öffnen **Cachekonfiguration** modal.

   ![GraphQL-Header-Option beibehalten](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. Im **Cachekonfiguration** modal, aktualisieren Sie die `max-age` Header-Wert zu `600 `Sekunden (10 Minuten), klicken Sie auf **Speichern**

   ![Beibehalten der GraphQL-Cache-Konfiguration](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Überprüfen [Zwischenspeichern persistenter Abfragen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) für weitere Informationen zu den standardmäßigen Cache-Control-Parametern.


## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben jetzt gelernt, wie GraphQL-Abfragen mit Parametern persistiert, persistente Abfragen aktualisiert und Cache-Steuerungsparameter mit persistenten Abfragen verwendet werden.

## Nächste Schritte

Im [Nächstes Kapitel](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), implementieren Sie die Anforderungen für persistente Abfragen in der WKND-App.
