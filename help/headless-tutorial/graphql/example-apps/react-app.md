---
title: React-App - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Es wird eine React-Anwendung bereitgestellt, die zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden. Der AEM Headless-Client für JavaScript wird verwendet, um die GraphQL-Abfragen auszuführen, die die App unterstützen.
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
source-wordcount: '1007'
ht-degree: 6%

---


# React App

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Es wird eine React-Anwendung bereitgestellt, die zeigt, wie Inhalte mithilfe der GraphQL-APIs von AEM abgefragt werden. Der AEM Headless-Client für JavaScript wird verwendet, um die GraphQL-Abfragen auszuführen, die die App unterstützen.

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

![React Application](./assets/react-screenshot.png)

Eine vollständige Schritt-für-Schritt-Anleitung ist verfügbar [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=de).

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## AEM

Das Programm ist für die Verbindung mit einem AEM konzipiert **Autor** oder **Veröffentlichen** Umgebung mit der neuesten Version der [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest) installiert.

* [ zu AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=de)

Wir empfehlen [Bereitstellen der WKND-Referenz-Site in einer Cloud Service-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) oder [AEM 6.5 QuickStart-JAR](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances) kann auch verwendet werden.

## Informationen zur Verwendung

1. Klonen Sie die `aem-guides-wknd-graphql` repository:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bearbeiten Sie die `aem-guides-wknd/react-app/.env.development` und stellen Sie sicher, dass `REACT_APP_HOST_URI` verweist auf Ihre Ziel-AEM-Instanz. Aktualisieren Sie die Authentifizierungsmethode (wenn Sie eine Verbindung zu einer Autoreninstanz herstellen).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Öffnen Sie ein Terminal und führen Sie die Befehle aus:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. Ein neues Browser-Fenster sollte in [http://localhost:3000](http://localhost:3000)
1. Eine Liste der Abenteuer von der WKND-Referenz-Website sollte auf der Anwendung angezeigt werden.

## Der Code

Nachstehend finden Sie eine kurze Zusammenfassung der wichtigen Dateien und des Codes, mit denen die Anwendung betrieben wird. Den vollständigen Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM Headless-Client für JavaScript

Die [AEM Headless-Client](https://github.com/adobe/aem-headless-client-js) wird verwendet, um die GraphQL-Abfrage auszuführen. Der AEM Headless-Client bietet zwei Methoden zum Ausführen von Abfragen. [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) und [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` führt eine standardmäßige GraphQL-Abfrage für AEM Inhalt aus und ist der gängigste Typ der Abfrageausführung.

[Beständige Abfragen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) sind eine Funktion in AEM, die die Ergebnisse einer GraphQL-Abfrage zwischenspeichert und das Ergebnis dann über GET verfügbar macht. Beständige Abfragen sollten für allgemeine Abfragen verwendet werden, die wiederholt ausgeführt werden. In dieser Anwendung ist die Liste der Abenteuer die erste Abfrage, die auf dem Startbildschirm ausgeführt wird. Dies ist eine sehr beliebte Abfrage, weshalb eine persistente Abfrage verwendet werden sollte. `runPersistedQuery` führt eine Anfrage für einen persistenten Abfrageendpunkt aus.

`src/api/useGraphQL.js` ist [React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) , die auf Änderungen am Parameter überwacht `query` und `path`. Wenn `query` leer ist, wird eine persistente Abfrage basierend auf dem `path`. Hier wird der AEM Headless-Client erstellt und zum Abrufen von Daten verwendet.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Adventure-Inhalte

Die App zeigt in erster Linie eine Liste von Abenteuern an und gibt Benutzern die Möglichkeit, auf die Details eines Abenteuers zu klicken.

`Adventures.js` - Zeigt eine Kartenliste von Abenteuern an.  Der anfängliche Status verwendet [Beständige Abfragen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) , [vorverpackt](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) mit der WKND-Referenz-Website. Der Endpunkt lautet `/wknd/adventures-all`. Es gibt mehrere Schaltflächen, mit denen Benutzer anhand einer Aktivität mit Filterergebnissen experimentieren können:

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
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
  `;
}
```

`AdventureDetail.js` - Zeigt eine Detailansicht des Abenteuers an. Führt eine Abfrage des GraphQL auf Grundlage des Pfads zum Abenteuer durch, der aus der URL geparst wird:

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### Umgebungsvariablen

Mehrere [Umgebungsvariablen](https://create-react-app.dev/docs/adding-custom-environment-variables) werden von diesem Projekt verwendet, um eine Verbindung zu einer AEM Umgebung herzustellen. Standard stellt eine Verbindung zu einer AEM Autorenumgebung her, die unter http://localhost:4502 ausgeführt wird. Wenn Sie dieses Verhalten ändern möchten, aktualisieren Sie das `.env.development` Datei dementsprechend:

* `REACT_APP_HOST_URI=http://localhost:4502` - Festgelegt auf AEM Target-Host
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - Festlegen des GraphQL-Endpunktpfads
* `REACT_APP_AUTH_METHOD=` - Die bevorzugte Authentifizierungsmethode. Optional, standardmäßig wird keine Authentifizierung verwendet.
   * `service-token` - Service-Token-Austausch für Cloud Env PROD verwenden
   * `dev-token` - Verwenden des Dev-Tokens für die lokale Entwicklung mit Cloud Env
   * `basic` - Benutzer/Übergabe für die lokale Entwicklung mit der lokalen Autorenumgebung verwenden
   * leer lassen, keine Authentifizierungsmethode verwenden
* `REACT_APP_AUTHORIZATION=admin:admin` - Legen Sie grundlegende Authentifizierungsberechtigungen fest, die beim Herstellen einer Verbindung zu einer AEM-Autorenumgebung verwendet werden (nur für die Entwicklung). Wenn Sie eine Verbindung zu einer Veröffentlichungsumgebung herstellen, ist diese Einstellung nicht erforderlich.
* `REACT_APP_DEV_TOKEN` - Entwicklungstoken-Zeichenfolge. Um eine Verbindung zur Remote-Instanz herzustellen, können Sie neben der Option &quot;Grundlegende Authentifizierung (user:pass)&quot;die Option &quot;Bearer-Authentifizierung mit DEV-Token&quot;aus der Cloud-Konsole verwenden
* `REACT_APP_SERVICE_TOKEN` - Pfad zur Dienst-Token-Datei. Um eine Verbindung zur Remote-Instanz herzustellen, kann die Authentifizierung auch über das Service-Token erfolgen (Download-Datei von der Cloud-Konsole).

### Proxy-API-Anfragen

Bei Verwendung des Webpack Development Servers (`npm start`) das Projekt auf eine [Proxy-Einrichtung](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. Die Datei wird konfiguriert unter [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) und verwendet mehrere benutzerdefinierte Umgebungsvariablen, die auf `.env` und `.env.development`.

Wenn Sie eine Verbindung zu einer AEM Autorenumgebung herstellen, muss die entsprechende Authentifizierungsmethode konfiguriert werden.

### CORS - Cross Origin Resource Sharing

Dieses Projekt beruht auf einer CORS-Konfiguration, die in der Ziel-AEM-Umgebung ausgeführt wird, und geht davon aus, dass die App im Entwicklungsmodus auf http://localhost:3000 ausgeführt wird. Die [COR-Konfiguration](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) ist Teil der [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd).

![CORS-Konfiguration](assets/cross-origin-resource-sharing-configuration.png)

*Beispiel-CORS-Konfiguration für die Autorenumgebung*