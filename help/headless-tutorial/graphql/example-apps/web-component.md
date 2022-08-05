---
title: Webkomponente/JS - AEM Headless-Beispiel
description: Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Webkomponente/JS-Anwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen können.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 8%

---


# Webkomponente

Beispielanwendungen eignen sich hervorragend, um die Headless-Funktionen von Adobe Experience Manager (AEM) zu erkunden. Diese Webkomponentenanwendung zeigt, wie Sie Inhalte mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen abfragen und einen Teil der Benutzeroberfläche rendern können, was mit reinem JavaScript-Code erreicht wird.

![Webkomponente mit AEM Headless](./assets/web-component/web-component.png)

Anzeigen der [Quellcode auf GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Voraussetzungen {#prerequisites}

Die folgenden Tools sollten lokal installiert werden:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (bei Verbindung mit dem lokalen AEM 6.5 oder AEM SDK)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM

Die Webkomponente funktioniert mit den folgenden AEM Bereitstellungsoptionen.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=de)
+ Lokales Setup mit [das AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de?lang=de#install-local-aem-instances)

Bei allen Implementierungen ist die `tutorial-solution-content.zip` von [Lösungsdateien](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) installiert und erforderlich [Bereitstellungskonfigurationen](../deployment/web-component.md) ausgeführt werden.


>[!IMPORTANT]
>
>Die Webkomponente ist für die Verbindung mit einer __AEM-Veröffentlichung__ -Umgebung kann jedoch Inhalte von der AEM-Autoreninstanz beziehen, wenn die Authentifizierung in der Webkomponente [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) -Datei.

## Informationen zur Verwendung

1. Klonen Sie die `adobe/aem-guides-wknd-graphql` repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigieren Sie zu `web-component` Unterverzeichnis.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Bearbeiten Sie die `.../src/person.js` -Datei, die die AEM Verbindungsdetails enthält:

   Im `aemHeadlessService` -Objekt, aktualisieren Sie die `aemHost` , um auf Ihren AEM-Veröffentlichungsdienst zu verweisen.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Wenn Sie eine Verbindung zu einem AEM-Autorendienst herstellen, finden Sie im `aemCredentials` -Objekt, geben Sie die Anmeldeinformationen der lokalen AEM an.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Öffnen Sie ein Terminal und führen Sie die Befehle aus `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Ein neues Browserfenster öffnet die statische HTML-Seite, die die Webkomponente unter [http://localhost:8080](http://localhost:8080).
1. Die _Personeninformationen_ Die Webkomponente wird auf der Webseite angezeigt.

## Der Code

Nachstehend finden Sie eine Zusammenfassung dazu, wie die Webkomponente erstellt wurde, wie sie eine Verbindung zu AEM Headless herstellt, um Inhalte mithilfe von GraphQL-gespeicherten Abfragen abzurufen, und wie diese Daten dargestellt werden. Der vollständige Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### HTML-Tag der Webkomponente

Eine wiederverwendbare Webkomponente (auch als benutzerdefiniertes Element bezeichnet) `<person-info>` wird zum `../src/assets/aem-headless.html` HTML. Es unterstützt `host` und `query-param-value` -Attribute, um das Verhalten der Komponente zu fördern. Die `host` -Wertüberschreibungen des -Attributs `aemHost` Wert aus `aemHeadlessService` -Objekt in `person.js`und `query-param-value` wird verwendet, um die Person auszuwählen, die gerendert werden soll.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementierung von Webkomponenten

Die `person.js` definiert die Webkomponentenfunktion und darunter sind die wichtigsten Highlights.

#### Element-Implementierung von PersonInfo

Die `<person-info>` Das Klassenobjekt des benutzerdefinierten Elements definiert die Funktionalität mithilfe der `connectedCallback()` Lebenszyklusmethoden, Anhängen eines Shadow-Stamms, Abrufen einer von GraphQL beibehaltenen Abfrage und DOM-Manipulation, um die interne Shadow-DOM-Struktur des benutzerdefinierten Elements zu erstellen.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registrieren Sie die `<person-info>` element

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Cross-Origin Resource Sharing (CORS)

Diese Webkomponente beruht auf einer AEM-basierten CORS-Konfiguration, die in der Ziel-AEM-Umgebung ausgeführt wird, und geht davon aus, dass die Hostseite auf `http://localhost:8080` im Entwicklungsmodus und darunter finden Sie eine Beispielkonfiguration für CORS OSGi für den lokalen AEM-Autorendienst.

Bitte überprüfen Sie [Bereitstellungskonfigurationen](../deployment/web-component.md) für den jeweiligen AEM.

![CORS-Konfiguration](assets/react-app/cross-origin-resource-sharing-configuration.png)
