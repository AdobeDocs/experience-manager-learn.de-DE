---
title: Instrument React-App zum Bearbeiten von Inhalten mit dem universellen Editor
description: Erfahren Sie, wie Sie die React-App instrumentieren, um den Inhalt mit dem universellen Editor zu bearbeiten.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---


# Instrument React-App zum Bearbeiten von Inhalten mit dem universellen Editor

Erfahren Sie, wie Sie mit der React-App Inhalte mit dem Universal Editor bearbeiten können.

## Voraussetzungen

Sie haben die lokale Entwicklungsumgebung wie im vorherigen Abschnitt beschrieben eingerichtet [Lokale Entwicklungseinrichtung](./local-development-setup.md) Schritt.

## Grundlegende Bibliothek des universellen Editors einschließen

Beginnen wir mit der Einbeziehung der Core-Bibliothek des universellen Editors in die WKND-Teams-React-App. Es handelt sich dabei um eine JavaScript-Bibliothek, die die Kommunikationsschicht zwischen der bearbeiteten App und dem universellen Editor bereitstellt.

Es gibt zwei Möglichkeiten, die Core-Bibliothek des universellen Editors in die React-App einzubinden:

1. Knotenmodulabhängigkeit von der NPM-Registrierung, siehe [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors).
1. Skript-Tag (`<script>`) in der HTML-Datei.

Verwenden wir für dieses Tutorial den Skript-Tag-Ansatz.

1. Installieren Sie die `react-helmet-async` -Paket zum Verwalten der `<script>` -Tag in der React-App.

   ```bash
   $ npm install react-helmet-async
   ```

1. Aktualisieren Sie die `src/App.js` -Datei der WKND Teams React-App , um die Core-Bibliothek des universellen Editors einzuschließen.

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

So verbinden Sie die WKND Teams React-App _mit der Inhaltsquelle_ zur Bearbeitung müssen Sie Verbindungsmetadaten angeben. Der Universal Editor-Dienst verwendet diese Metadaten, um eine Verbindung mit der Inhaltsquelle herzustellen.

Die Verbindungsmetadaten werden als `<meta>` Tags in der HTML-Datei. Die Syntax für die Verbindungsmetadaten lautet wie folgt:

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

Fügen wir die Verbindungsmetadaten zur WKND Teams React-App innerhalb der `<Helmet>` -Komponente. Aktualisieren Sie die `src/App.js` -Datei mit den folgenden `<meta>` -Tag. In diesem Beispiel ist die Inhaltsquelle eine lokale AEM-Instanz, die auf `https://localhost:8443`.

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

Die `aemconnection` stellt einen kurzen Namen für die Inhaltsquelle bereit. Die nachfolgende Instrumentierung verwendet den Kurznamen, um auf die Inhaltsquelle zu verweisen.

## Hinzufügen von Metadaten - lokale Konfiguration des Universal Editor-Dienstes

Anstelle des von Adobe gehosteten Universal Editor-Dienstes wird eine lokale Kopie des Universal Editor-Dienstes für die lokale Entwicklung verwendet. Der lokale Dienst bindet den Universal Editor und das AEM SDK. Fügen wir daher die lokalen Metadaten des Universal Editor-Dienstes zur WKND Teams React-App hinzu.

Diese Konfigurationseinstellungen werden auch als `<meta>` Tags in der HTML-Datei. Die Syntax für die Metadaten des lokalen Universal Editor-Dienstes lautet wie folgt:

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

Fügen wir die Verbindungsmetadaten zur WKND Teams React-App innerhalb der `<Helmet>` -Komponente. Aktualisieren Sie die `src/App.js` -Datei mit den folgenden `<meta>` -Tag. In diesem Beispiel wird der lokale Universal Editor-Dienst ausgeführt auf `https://localhost:8001`.

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

So bearbeiten Sie den Inhalt der WKND Teams-React-App wie _Teamtitel und Teambeschreibung_, müssen Sie die React-Komponenten instrumentieren. Die Instrumentierung umfasst das Hinzufügen relevanter Datenattribute (`data-aue-*`) zu den HTML-Elementen hinzu, die Sie mit dem universellen Editor bearbeitbar machen möchten. Weitere Informationen zu Datenattributen finden Sie unter [Attribute und Typen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types).

### Bearbeitbare Elemente definieren

Definieren wir zunächst die Elemente, die Sie mit dem universellen Editor bearbeiten möchten. In der WKND Teams React-App werden der Teamtitel und die Beschreibung im Team Content Fragment in AEM gespeichert, sodass die besten Kandidaten für die Bearbeitung zur Verfügung stehen.

Instrumentieren wir das `Teams` React component , damit der Teamtitel und die Beschreibung bearbeitbar sind.

1. Öffnen Sie die `src/components/Teams.js` Datei der WKND Teams React-App.
1. Fügen Sie die `data-aue-prop`, `data-aue-type` und `data-aue-label` -Attribut dem Teamtitel und den Beschreibungselementen zuordnen.

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

   ![Universeller Editor - bearbeitbare WKND-Teams Titel und Desc](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. Wenn Sie versuchen, den Titel oder die Beschreibung des Teams mithilfe der Inline-Bearbeitung oder der Eigenschaftenleiste zu bearbeiten, wird ein Ladegerät angezeigt, Sie können den Inhalt jedoch nicht bearbeiten. Da der universelle Editor die AEM Ressourcendetails zum Laden und Speichern des Inhalts nicht kennt.

   ![Universeller Editor - WKND-Teams - Titel und Desk-Laden](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

Zusammenfassend wird durch die obigen Änderungen der Teamtitel und die Beschreibungselemente im universellen Editor als bearbeitbar markiert. Allerdings **Sie können die Änderungen noch nicht bearbeiten (über Inline- oder Eigenschaftenleiste) und speichern**, zu dem Sie die AEM Ressourcendetails mithilfe der `data-aue-resource` -Attribut. Machen wir das im nächsten Schritt.

### Definieren AEM Ressourcendetails

Um den bearbeiteten Inhalt wieder in AEM zu speichern und den Inhalt in die Eigenschaftenleiste zu laden, müssen Sie die AEM Ressourcendetails für den universellen Editor angeben.

In diesem Fall ist die AEM Ressource der Pfad des Team-Inhaltsfragments. Fügen wir daher die Ressourcendetails zum `Teams` React-Komponente auf oberster Ebene `<div>` -Element.

1. Aktualisieren Sie die `src/components/Teams.js` -Datei, die hinzugefügt werden soll `data-aue-resource`, `data-aue-type` und `data-aue-label` -Attribute auf der obersten Ebene `<div>` -Element.

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

   Der Wert der `data-aue-resource` -Attribut ist der AEM Ressourcenpfad des Team-Inhaltsfragments. Die `urn:aemconnection:` -Präfix verwendet den Kurznamen der Inhaltsquelle, der in den Verbindungsmetadaten definiert ist.

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass das Team-Element der obersten Ebene bearbeitbar ist, die Eigenschaftenleiste jedoch den Inhalt noch immer nicht lädt. Auf der Netzwerkregisterkarte des Browsers sehen Sie den Fehler 401 Unauthorized für die `details` -Anfrage, die den Inhalt lädt. Es versucht, das IMS-Token für die Authentifizierung zu verwenden, aber das lokale AEM SDK unterstützt die IMS-Authentifizierung nicht.

   ![Universal Editor - bearbeitbare WKND-Teams](./assets/universal-editor-wknd-teams-team-editable.png)

1. Um den Fehler &quot;401 Unauthorized&quot;zu beheben, müssen Sie die lokalen AEM SDK-Authentifizierungsdetails dem universellen Editor mithilfe der **Authentifizierungskopfzeilen** im universellen Editor. Setzen Sie als lokales AEM SDK den Wert auf `Basic YWRtaW46YWRtaW4=` für `admin:admin` Anmeldedaten.

   ![Universal Editor - Hinzufügen von Authentifizierungskopfzeilen](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die Eigenschaftenleiste den Inhalt lädt, und Sie können den Teamtitel und die Beschreibung inline oder über die Eigenschaftenleiste bearbeiten.

   ![Universal Editor - bearbeitbare WKND-Teams](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### Im Hintergrund

Die Leiste &quot;Eigenschaften&quot;lädt den Inhalt aus der AEM Ressource mit dem lokalen Universal Editor-Dienst. Auf der Netzwerkregisterkarte des Browsers können Sie die Anforderung der POST an den lokalen Universal Editor-Dienst (`https://localhost:8001/details`), um den Inhalt zu laden.

Wenn Sie den Inhalt mit der Inline-Bearbeitung oder der Eigenschaftenleiste bearbeiten, werden die Änderungen mithilfe des lokalen Universal Editor-Dienstes wieder in der AEM Ressource gespeichert. Auf der Netzwerkregisterkarte des Browsers können Sie die Anforderung der POST an den lokalen Universal Editor-Dienst (`https://localhost:8001/update` oder `https://localhost:8001/patch`), um den Inhalt zu speichern.

![Universal Editor - bearbeitbare WKND-Teams](./assets/universal-editor-under-the-hood-request.png)

Das JSON-Objekt für die Anfrage-Payload enthält die erforderlichen Details wie den Inhaltsserver (`connections`), Ressourcenpfad (`target`) und dem aktualisierten Inhalt (`patch`).

![Universal Editor - bearbeitbare WKND-Teams](./assets/universal-editor-under-the-hood-payload.png)

### Erweitern des bearbeitbaren Inhalts

Erweitern wir den bearbeitbaren Inhalt und wenden wir die Instrumentierung auf die **Team-Mitglieder** damit Sie die Team-Mitglieder über die Eigenschaftenleiste bearbeiten können.

Fügen wir wie oben die relevanten `data-aue-*` -Attributen für die Teammitglieder in der `Teams` React-Komponente

1. Aktualisieren Sie die `src/components/Teams.js` -Datei, um Datenattribute zum `<li key={index} className="team__member">` -Element.

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

   Der Wert der `data-aue-type` Attribut ist `component` , da die Teammitglieder als `Person` Inhaltsfragmente in AEM und helfen bei der Angabe der beweglichen/löschbaren Teile des Inhalts.

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die Team-Mitglieder über die Eigenschaftenleiste bearbeitbar sind.

   ![Universeller Editor - bearbeitbare WKND-Team-Mitglieder](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### Im Hintergrund

Wie oben beschrieben erfolgt der Abruf und die Speicherung von Inhalten durch den lokalen Universal Editor-Dienst. Die `/details`, `/update` oder `/patch` -Anfragen werden zum Laden und Speichern des Inhalts an den lokalen Universal Editor-Dienst gesendet.

### Definition von Inhalten zum Hinzufügen und Löschen

Bisher haben Sie den vorhandenen Inhalt bearbeitbar gemacht, aber was ist, wenn Sie neue Inhalte hinzufügen möchten? Fügen wir mithilfe des universellen Editors die Möglichkeit hinzu, Team-Mitglieder zum WKND-Team hinzuzufügen oder daraus zu löschen. Daher müssen die Inhaltsautoren nicht zur AEM gehen, um Team-Mitglieder hinzuzufügen oder zu löschen.

Eine kurze Zusammenfassung: Die Mitglieder des WKND-Teams werden jedoch als `Person` Inhaltsfragmente in AEM und sind mit dem Team-Inhaltsfragment verknüpft, das die `teamMembers` -Eigenschaft. So überprüfen Sie die Modelldefinition in AEM Besuch [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project).

1. Erstellen Sie zunächst die Komponentendefinitionsdatei. `/public/static/component-definition.json`. Diese Datei enthält die Komponentendefinition für die `Person` Inhaltsfragment. Die `aem/cf` -Plug-in ermöglicht das Einfügen von Inhaltsfragmenten basierend auf einem Modell und einer Vorlage, die die anzuwendenden Standardwerte bereitstellen.

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

1. Weiter siehe die obige Komponentendefinitionsdatei unter `index.html` der WKND Team React App. Aktualisieren Sie die `public/index.html` -Datei `<head>` -Abschnitt, um die Komponentendefinitionsdatei einzuschließen.

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

1. Aktualisieren Sie abschließend die `src/components/Teams.js` -Datei, um Datenattribute hinzuzufügen. Die **MITGLIEDER** -Abschnitt, um als Container für die Team-Mitglieder zu fungieren, fügen wir die `data-aue-prop`, `data-aue-type`, und `data-aue-label` -Attributen `<div>` -Element.

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

1. Aktualisieren Sie die Seite Universal Editor im Browser, der die WKND Teams React-App lädt. Sie können jetzt sehen, dass die **MITGLIEDER** -Abschnitt dient als Container. Sie können neue Team-Mitglieder über die Eigenschaftenleiste und die **+** Symbol.

   ![Universeller Editor - Mitglieder des WKND-Teams fügen](./assets/universal-editor-wknd-teams-add-team-members.png)

1. Um ein Team-Mitglied zu löschen, wählen Sie das Team-Mitglied aus und klicken Sie auf **Löschen** Symbol.

   ![Universeller Editor - Löschen von Mitgliedern des WKND-Teams](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### Im Hintergrund

Die Vorgänge zum Hinzufügen und Löschen von Inhalten werden vom lokalen Universal Editor-Dienst ausgeführt. Die POST-Anfrage an `/add` oder `/remove` mit einer detaillierten Payload werden zum lokalen Universal Editor-Dienst zum Hinzufügen oder Löschen des Inhalts zum AEM hinzugefügt.

## Lösungsdateien

Informationen dazu, wie Sie Ihre Implementierungsänderungen überprüfen oder ob Sie die WKND-Teams-React-App nicht mit dem universellen Editor verwenden können, finden Sie im Abschnitt [basic-tutorial-instrumental-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) Lösungsverzweigung.

Dateivergleich mit der funktionierenden **Grundlegendes Tutorial** Verzweigung ist verfügbar [here](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1).

## Herzlichen Glückwunsch!

Sie haben die WKND Teams React-App erfolgreich instrumentiert, um den Inhalt mit dem universellen Editor hinzuzufügen, zu bearbeiten und zu löschen. Sie haben gelernt, wie Sie die Kernbibliothek einschließen, Verbindungen und die lokalen Metadaten des Universal Editor-Dienstes hinzufügen und die React-Komponente mit verschiedenen Daten instrumentieren (`data-aue-*`).
