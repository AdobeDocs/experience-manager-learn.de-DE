---
title: Instrument React-App zum Bearbeiten von Inhalten mit dem universellen Editor
description: Erfahren Sie, wie Sie die React-App instrumentieren, um den Inhalt mit dem universellen Editor zu bearbeiten.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---

# Instrument React-App zum Bearbeiten von Inhalten mit dem universellen Editor

Erfahren Sie, wie Sie mit der React-App Inhalte mit dem Universal Editor bearbeiten können.

## Voraussetzungen

Sie haben die lokale Entwicklungsumgebung wie im vorherigen Schritt [Lokale Entwicklungseinrichtung](./local-development-setup.md) beschrieben eingerichtet.

## Grundlegende Bibliothek des universellen Editors einschließen

Beginnen wir mit der Einbeziehung der Core-Bibliothek des universellen Editors in die WKND-Teams-React-App. Es handelt sich dabei um eine JavaScript-Bibliothek, die die Kommunikationsschicht zwischen der bearbeiteten App und dem universellen Editor bereitstellt.

Es gibt zwei Möglichkeiten, die Core-Bibliothek des universellen Editors in die React-App einzubinden:

1. Knotenmodulabhängigkeit von der NPM-Registrierung, siehe [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors).
1. Skript-Tag (`<script>`) in der HTML-Datei.

Verwenden wir für dieses Tutorial den Skript-Tag-Ansatz.

1. Installieren Sie das Paket `react-helmet-async` , um das Tag `<script>` in der React-App zu verwalten.

   ```bash
   $ npm install react-helmet-async
   ```

1. Aktualisieren Sie die Datei &quot;`src/App.js`&quot;der WKND Teams React-App, um die Core-Bibliothek des universellen Editors aufzunehmen.

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## Metadaten hinzufügen - Inhaltsquelle

Um die WKND-Teams-React-App _mit der Inhaltsquelle_ zur Bearbeitung zu verbinden, müssen Sie Verbindungsmetadaten angeben. Der Universal Editor-Dienst verwendet diese Metadaten, um eine Verbindung mit der Inhaltsquelle herzustellen.

Die Verbindungsmetadaten werden als `<meta>` -Tags in der HTML-Datei gespeichert. Die Syntax für die Verbindungsmetadaten lautet wie folgt:

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

Fügen wir die Verbindungsmetadaten der WKND Teams React-App innerhalb der Komponente `<Helmet>` hinzu. Aktualisieren Sie die Datei `src/App.js` mit dem folgenden `<meta>` -Tag. In diesem Beispiel ist die Inhaltsquelle eine lokale AEM, die auf `https://localhost:8443` ausgeführt wird.

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

Der `aemconnection` gibt einen kurzen Namen für die Inhaltsquelle an. Die nachfolgende Instrumentierung verwendet den Kurznamen, um auf die Inhaltsquelle zu verweisen.

## Hinzufügen von Metadaten - lokale Konfiguration des Universal Editor-Dienstes

Anstelle des von Adobe gehosteten Universal Editor-Dienstes wird eine lokale Kopie des Universal Editor-Dienstes für die lokale Entwicklung verwendet. Der lokale Dienst bindet den Universal Editor und das AEM SDK. Fügen wir daher die lokalen Metadaten des Universal Editor-Dienstes zur WKND Teams React-App hinzu.

Diese Konfigurationseinstellungen werden auch als `<meta>` -Tags in der HTML-Datei gespeichert. Die Syntax für die Metadaten des lokalen Universal Editor-Dienstes lautet wie folgt:

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

Fügen wir die Verbindungsmetadaten der WKND Teams React-App innerhalb der Komponente `<Helmet>` hinzu. Aktualisieren Sie die Datei `src/App.js` mit dem folgenden `<meta>` -Tag. In diesem Beispiel wird der lokale Universal Editor-Dienst auf `https://localhost:8001` ausgeführt.

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## Instrumentieren der React-Komponenten

Um den Inhalt der WKND Teams React-App wie _Teamtitel und Teambeschreibung_ zu bearbeiten, müssen Sie die React-Komponenten instrumentieren. Die Instrumentierung bedeutet, den HTML-Elementen, die Sie mit dem universellen Editor bearbeitbar machen möchten, relevante Datenattribute (`data-aue-*`) hinzuzufügen. Weitere Informationen zu Datenattributen finden Sie unter [Attribute und Typen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types).

### Bearbeitbare Elemente definieren

Definieren wir zunächst die Elemente, die Sie mit dem universellen Editor bearbeiten möchten. In der WKND Teams React-App werden der Teamtitel und die Beschreibung im Team Content Fragment in AEM gespeichert, sodass die besten Kandidaten für die Bearbeitung zur Verfügung stehen.

Instrumentieren wir die Komponente `Teams` React , damit der Teamtitel und die Beschreibung bearbeitbar sind.

1. Öffnen Sie die Datei &quot;`src/components/Teams.js`&quot;der WKND Teams React-App.
1. Fügen Sie die Attribute `data-aue-prop`, `data-aue-type` und `data-aue-label` zum Teamtitel und zu den Beschreibungselementen hinzu.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die Elemente für den Teamtitel und die Beschreibung bearbeitbar sind.

   ![Universal Editor - WKND Teams Title and Desc editable](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. Wenn Sie versuchen, den Titel oder die Beschreibung des Teams mithilfe der Inline-Bearbeitung oder der Eigenschaftenleiste zu bearbeiten, wird ein Ladegerät angezeigt, Sie können den Inhalt jedoch nicht bearbeiten. Da der universelle Editor die AEM Ressourcendetails zum Laden und Speichern des Inhalts nicht kennt.

   ![Universal Editor - Titel und Laden der WKND-Teams](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

Zusammenfassend wird durch die obigen Änderungen der Teamtitel und die Beschreibungselemente im universellen Editor als bearbeitbar markiert. Sie können jedoch **nicht bearbeiten (über Inline- oder Eigenschaftenleiste) und die Änderungen noch nicht speichern**, da Sie dazu die AEM Ressourcendetails mit dem Attribut `data-aue-resource` hinzufügen müssen. Machen wir das im nächsten Schritt.

### Definieren AEM Ressourcendetails

Um den bearbeiteten Inhalt wieder in AEM zu speichern und den Inhalt in die Eigenschaftenleiste zu laden, müssen Sie die AEM Ressourcendetails für den universellen Editor angeben.

In diesem Fall ist die AEM Ressource der Pfad des Inhaltsfragments &quot;Team&quot;. Fügen wir daher die Ressourcendetails zur Komponente `Teams` React auf der obersten Ebene `<div>` hinzu.

1. Aktualisieren Sie die Datei `src/components/Teams.js` , um die Attribute `data-aue-resource`, `data-aue-type` und `data-aue-label` zum Element der obersten Ebene `<div>` hinzuzufügen.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   Der Wert des Attributs `data-aue-resource` ist der AEM Ressourcenpfad des Team Content Fragments. Das Präfix `urn:aemconnection:` verwendet den Kurznamen der Inhaltsquelle, der in den Verbindungsmetadaten definiert ist.

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass das Team-Element der obersten Ebene bearbeitbar ist, die Eigenschaftenleiste jedoch den Inhalt noch immer nicht lädt. Auf der Netzwerkregisterkarte des Browsers wird der Fehler 401 Unauthorized für die `details` -Anfrage angezeigt, die den Inhalt lädt. Es versucht, das IMS-Token für die Authentifizierung zu verwenden, aber das lokale AEM SDK unterstützt die IMS-Authentifizierung nicht.

   ![Universal Editor - WKND Teams Team bearbeitbar](./assets/universal-editor-wknd-teams-team-editable.png)

1. Um den Fehler 401 Unauthorized zu beheben, müssen Sie die lokalen AEM SDK-Authentifizierungsdetails mithilfe der Option **Authentifizierungskopfzeilen** im universellen Editor für den universellen Editor angeben. Setzen Sie als lokales AEM SDK den Wert für `admin:admin` -Anmeldedaten auf `Basic YWRtaW46YWRtaW4=`.

   ![Universal Editor - Hinzufügen von Authentifizierungskopfzeilen](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die Eigenschaftenleiste den Inhalt lädt, und Sie können den Teamtitel und die Beschreibung inline oder über die Eigenschaftenleiste bearbeiten.

   ![Universal Editor - WKND Teams Team bearbeitbar](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### Im Hintergrund

Die Leiste &quot;Eigenschaften&quot;lädt den Inhalt aus der AEM Ressource mit dem lokalen Universal Editor-Dienst. Auf der Registerkarte &quot;Netzwerk&quot;des Browsers können Sie die POST-Anfrage zum Laden des Inhalts an den lokalen Universal Editor-Dienst (`https://localhost:8001/details`) sehen.

Wenn Sie den Inhalt mit der Inline-Bearbeitung oder der Eigenschaftenleiste bearbeiten, werden die Änderungen mithilfe des lokalen Universal Editor-Dienstes wieder in der AEM Ressource gespeichert. Auf der Netzwerkregisterkarte des Browsers sehen Sie die POST-Anfrage zum Speichern des Inhalts an den lokalen Universal Editor-Dienst (`https://localhost:8001/update` oder `https://localhost:8001/patch`).

![Universal Editor - WKND Teams Team bearbeitbar](./assets/universal-editor-under-the-hood-request.png)

Das JSON-Objekt für die Anfrage-Payload enthält die erforderlichen Details wie den Inhaltsserver (`connections`), den Ressourcenpfad (`target`) und den aktualisierten Inhalt (`patch`).

![Universal Editor - WKND Teams Team bearbeitbar](./assets/universal-editor-under-the-hood-payload.png)

### Erweitern des bearbeitbaren Inhalts

Erweitern wir den bearbeitbaren Inhalt und wenden wir die Instrumentierung auf die **Team-Mitglieder** an, damit Sie die Team-Mitglieder über die Eigenschaftenleiste bearbeiten können.

Fügen wir wie oben die relevanten `data-aue-*` -Attribute den Teammitgliedern in der `Teams` React-Komponente hinzu.

1. Aktualisieren Sie die Datei `src/components/Teams.js` , um dem Element `<li key={index} className="team__member">` Datenattribute hinzuzufügen.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   Der Wert des Attributs `data-aue-type` ist `component`, da die Teammitglieder in AEM als `Person` Inhaltsfragmente gespeichert werden. Dies hilft bei der Angabe der beweglichen/löschbaren Teile des Inhalts.

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die Team-Mitglieder über die Eigenschaftenleiste bearbeitbar sind.

   ![Universal Editor - WKND Team-Mitglieder bearbeitbar](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### Im Hintergrund

Wie oben beschrieben erfolgt der Abruf und die Speicherung von Inhalten durch den lokalen Universal Editor-Dienst. Die Anforderungen `/details`, `/update` oder `/patch` werden zum Laden und Speichern des Inhalts an den lokalen Universal Editor-Dienst gesendet.

### Definition von Inhalten zum Hinzufügen und Löschen

Bisher haben Sie den vorhandenen Inhalt bearbeitbar gemacht, aber was ist, wenn Sie neue Inhalte hinzufügen möchten? Fügen wir mithilfe des universellen Editors die Möglichkeit hinzu, Team-Mitglieder zum WKND-Team hinzuzufügen oder daraus zu löschen. Daher müssen die Inhaltsautoren nicht zur AEM gehen, um Team-Mitglieder hinzuzufügen oder zu löschen.

Eine kurze Zusammenfassung enthält jedoch, dass die Mitglieder des WKND-Teams in AEM als `Person` Inhaltsfragmente gespeichert sind und mit dem Team-Inhaltsfragment über die Eigenschaft `teamMembers` verknüpft sind. So überprüfen Sie die Modelldefinition in AEM Besuch [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project).

1. Erstellen Sie zunächst die Komponentendefinitionsdatei `/public/static/component-definition.json`. Diese Datei enthält die Komponentendefinition für das Inhaltsfragment `Person` . Das Plug-in `aem/cf` ermöglicht das Einfügen von Inhaltsfragmenten basierend auf einem Modell und einer Vorlage, die die anzuwendenden Standardwerte bereitstellen.

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. Siehe als Nächstes die obige Komponentendefinitionsdatei in `index.html` der WKND Team React App. Aktualisieren Sie den Abschnitt `<head>` der `public/index.html`-Datei, um die Komponentendefinitionsdatei einzuschließen.

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. Aktualisieren Sie abschließend die Datei &quot;`src/components/Teams.js`&quot;, um Datenattribute hinzuzufügen. Fügen wir die Attribute `data-aue-prop`, `data-aue-type` und `data-aue-label` zum Element `<div>` hinzu, um als Container für die Teammitglieder zu fungieren.****

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass der Abschnitt **MITGLIEDER** als Container fungiert. Sie können neue Teammitglieder über die Eigenschaftenleiste und das Symbol **+** einfügen.

   ![Universal Editor - WKND Team-Mitglieder fügen](./assets/universal-editor-wknd-teams-add-team-members.png) ein.

1. Um ein Team-Mitglied zu löschen, wählen Sie das Team-Mitglied aus und klicken Sie auf das Symbol **Löschen** .

   ![Universeller Editor - Mitglieder des WKND-Teams löschen](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### Im Hintergrund

Die Vorgänge zum Hinzufügen und Löschen von Inhalten werden vom lokalen Universal Editor-Dienst ausgeführt. Die POST-Anfrage an `/add` oder `/remove` mit einer detaillierten Payload wird an den lokalen Universal Editor-Dienst gesendet, um den Inhalt zum AEM hinzuzufügen oder zu löschen.

## Lösungsdateien

Informationen dazu, wie Sie Ihre Implementierungsänderungen überprüfen oder ob Sie die WKND-Teams-React-App nicht mit dem universellen Editor verwenden können, finden Sie in der Lösungsverzweigung [basic-tutorial-instrumental-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) .

Der Dateivergleich mit der funktionierenden Verzweigung **basic-tutorial** ist [hier](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1) verfügbar.

## Herzlichen Glückwunsch!

Sie haben die WKND Teams React-App erfolgreich instrumentiert, um den Inhalt mit dem universellen Editor hinzuzufügen, zu bearbeiten und zu löschen. Sie haben gelernt, wie Sie die Hauptbibliothek einbeziehen, Verbindungen und die lokalen Metadaten des Universal Editor-Dienstes hinzufügen und die React-Komponente mit verschiedenen Datenattributen (`data-aue-*`) instrumentieren.
