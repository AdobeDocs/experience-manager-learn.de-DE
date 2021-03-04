---
title: hinzufügen Navigation und Routing | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnen zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des React Routers implementiert und einer vorhandenen Header-Komponente hinzugefügt.
sub-product: Sites
feature: SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2117'
ht-degree: 2%

---


# hinzufügen Navigation und Routing {#navigation-routing}

Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnen zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des React Routers implementiert und einer vorhandenen Header-Komponente hinzugefügt.

## Vorgabe

1. Machen Sie sich mit den Optionen für das Routing des SPA-Modells vertraut, die bei Verwendung des SPA-Editors verfügbar sind.
1. Hier erfahren Sie, wie Sie mit [React Router](https://reacttraining.com/react-router/) zwischen verschiedenen Ansichten der SPA navigieren.
1. Implementieren Sie eine dynamische Navigation, die von der AEM Seitenhierarchie gesteuert wird.

## Was Sie erstellen werden

In diesem Kapitel wird ein Navigationsmenü zu einer vorhandenen Komponente `Header` hinzugefügt. Das Navigationsmenü wird von der AEM-Seitenhierarchie gesteuert und verwendet das JSON-Modell, das von der [Navigationskernkomponente](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/navigation.html) bereitgestellt wird.

![Implementierung der Navigation](assets/navigation-routing/final-navigation-implemented.gif)

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das `classic`-Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installieren Sie das fertige Paket für die herkömmliche [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest). Die von [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest) bereitgestellten Bilder werden auf der WKND-SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) oder lokal prüfen, indem Sie zur Verzweigung `React/navigation-routing-solution` wechseln.

## Inspect Header-Updates {#inspect-header}

In vorherigen Kapiteln wurde die Komponente `Header` als reine React-Komponente hinzugefügt, die über `App.js` eingeschlossen wurde. In diesem Kapitel wurde die Komponente `Header` entfernt und wird über den [Vorlageneditor](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html) hinzugefügt. Auf diese Weise können Benutzer das Navigationsmenü von `Header` innerhalb von AEM konfigurieren.

>[!NOTE]
>
> Zur Beginn dieses Kapitels wurden bereits mehrere CSS- und JavaScript-Aktualisierungen an der Codebasis vorgenommen. Um sich auf Kernkonzepte zu konzentrieren, werden nicht **alle** der Codeänderungen besprochen. Sie können die vollständigen Änderungen [hier](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start) Ansicht vornehmen.

1. Öffnen Sie in der IDE Ihrer Wahl das SPA Startprojekt für dieses Kapitel.
1. Unterhalb des Moduls `ui.frontend` überprüfen Sie die Datei `Header.js` unter: `ui.frontend/src/components/Header/Header.js`.

   Es wurden verschiedene Aktualisierungen vorgenommen, darunter das Hinzufügen von `HeaderEditConfig` und einem `MapTo`, um die Zuordnung der Komponente zu einer AEM Komponente `wknd-spa-react/components/header` zu ermöglichen.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Überprüfen Sie im Modul `ui.apps` die Komponentendefinition der Komponente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Die AEM `Header`-Komponente übernimmt alle Funktionen der [Navigationskernkomponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) über die `sling:resourceSuperType`-Eigenschaft.

## hinzufügen der Kopfzeile die Vorlage {#add-header-template}

1. Öffnen Sie einen Browser und melden Sie sich bei AEM [http://localhost:4502/](http://localhost:4502/) an. Die Basis für den Startcode sollte bereits bereitgestellt werden.
1. Navigieren Sie zum Ordner **SPA Seitenvorlage**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Wählen Sie den äußeren **Root Layout Container** und klicken Sie auf das Symbol **Richtlinie**. Achten Sie darauf, **not** auszuwählen, um den **Layout-Container** nicht gesperrt für Authoring auszuwählen.

   ![Container-Richtliniensymbol für das Stammlayout auswählen](assets/navigation-routing/root-layout-container-policy.png)

1. Erstellen Sie eine neue Richtlinie mit dem Namen **SPA Struktur**:

   ![SPA](assets/navigation-routing/spa-policy-update.png)

   Wählen Sie unter **Zulässige Komponenten** > **Allgemein** > die Komponente **Layout Container** aus.

   Wählen Sie unter **Zulässige Komponenten** > **WKND SPA REACT - STRUKTUR** > die Komponente **Kopfzeile** aus:

   ![Kopfzeilenkomponente auswählen](assets/navigation-routing/select-header-component.png)

   Wählen Sie unter **Zulässige Komponenten** > **WKND SPA REACT - Content** > die Komponenten **Bild** und **Text** aus. Sie sollten vier Komponenten auswählen.

   Klicken Sie auf **Fertig**, um die Änderungen zu speichern.

1. Aktualisieren Sie die Seite und fügen Sie die Komponente **Kopfzeile** über dem nicht gesperrten **Layout-Container** hinzu:

   ![Kopfzeilenkomponente zur Vorlage hinzufügen](./assets/navigation-routing/add-header-component.gif)

1. Wählen Sie die Komponente **Kopfzeile** und klicken Sie auf das Symbol **Richtlinie**, um die Richtlinie zu bearbeiten.
1. Erstellen Sie eine neue Richtlinie mit einem **Richtlinientitel** von **WKND SPA Kopfzeile**.

   Unter **Eigenschaften**:

   * Legen Sie **Navigationsstamm** auf `/content/wknd-spa-react/us/en` fest.
   * Stellen Sie die **Root-Stufen ausschließen** auf **1** ein.
   * Deaktivieren Sie **Alle untergeordneten Seiten** erfassen.
   * Setzen Sie die **Navigationsstrukturtiefe** auf **3**.

   ![Kopfzeilenrichtlinie konfigurieren](assets/navigation-routing/header-policy.png)

   Dadurch werden die 2 Navigationsstufen unterhalb von `/content/wknd-spa-react/us/en` erfasst.

1. Nach dem Speichern der Änderungen sollten Sie das ausgefüllte `Header` als Teil der Vorlage sehen:

   ![Aufgefüllte Kopfzeilenkomponente](assets/navigation-routing/populated-header.png)

## Untergeordnete Seiten erstellen

Anschließend erstellen Sie zusätzliche Seiten in AEM, die als verschiedene Ansichten im SPA dienen. Wir werden auch die hierarchische Struktur des von AEM bereitgestellten JSON-Modells überprüfen.

1. Navigieren Sie zur Konsole **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Wählen Sie **WKND SPA React Startseite** und klicken Sie auf **Create** > **Page**:

   ![Neue Seite erstellen](assets/navigation-routing/create-new-page.png)

1. Wählen Sie unter **Vorlage** **SPA Seite**. Geben Sie unter **Eigenschaften** **Seite 1** für **Titel** und **Seite-1** als Namen ein.

   ![Eigenschaften der ersten Seite eingeben](assets/navigation-routing/initial-page-properties.png)

   Klicken Sie auf **Erstellen** und klicken Sie im Popup-Dialogfeld auf **Öffnen**, um die Seite im AEM SPA Editor zu öffnen.

1. hinzufügen Sie eine neue Komponente **Text** in den Container **Layout**. Bearbeiten Sie die Komponente und geben Sie den Text ein: **Seite 1** mit dem RTE- und dem **H1**-Element (Sie müssen in den Vollbildmodus wechseln, um die Absatzelemente zu ändern)

   ![Beispielinhaltsseite 1](assets/navigation-routing/page-1-sample-content.png)

   Fügen Sie zusätzliche Inhalte hinzu, z. B. ein Bild.

1. Kehren Sie zur AEM Sites-Konsole zurück und wiederholen Sie die oben genannten Schritte und erstellen Sie eine zweite Seite mit dem Namen **Seite 2** als Geschwister von **Seite 1**.
1. Erstellen Sie abschließend eine dritte Seite, **Seite 3**, aber als **untergeordnetes** von **Seite 2**. Nach Abschluss des Vorgangs sollte die Site-Hierarchie wie folgt aussehen:

   ![Beispiel-Site-Hierarchie](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Öffnen Sie auf einer neuen Registerkarte die von AEM bereitgestellte JSON-Modell-API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Dieser JSON-Inhalt wird angefordert, wenn der SPA zum ersten Mal geladen wird. Die äußere Struktur sieht wie folgt aus:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {},
       "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Unter `:children` sollte für jede erstellte Seite ein Eintrag angezeigt werden. Der Inhalt für alle Seiten ist in dieser ersten JSON-Anforderung enthalten. Nach der Implementierung des Navigationssystems werden nachfolgende Ansichten des SPA schnell geladen, da der Routing bereits clientseitig verfügbar ist.

   Es ist nicht sinnvoll, **ALL** des Inhalts eines SPA in der anfänglichen JSON-Anforderung zu laden, da dies das Laden der ersten Seite verlangsamen würde. Als Nächstes sehen wir uns an, wie die Hierarchietiefe der Seiten erfasst wird.

1. Navigieren Sie zur Vorlage **SPA Stamm** unter: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Klicken Sie auf das Menü **Seiteneigenschaften** > **Seitenrichtlinie**:

   ![Seitenrichtlinie für SPA Stamm öffnen](assets/navigation-routing/open-page-policy.png)

1. Die Vorlage **SPA Stamm** verfügt über eine zusätzliche Registerkarte **Hierarchische Struktur**, um den erfassten JSON-Inhalt zu steuern. Die **Strukturtiefe** bestimmt, wie tief in der Site-Hierarchie untergeordnete Seiten unterhalb der **root** erfasst werden. Sie können auch das Feld **Strukturmuster** verwenden, um weitere Seiten basierend auf einem regulären Ausdruck zu filtern.

   Aktualisieren Sie die **Strukturtiefe** auf **2**:

   ![Strukturtiefe aktualisieren](assets/navigation-routing/update-structure-depth.png)

   Klicken Sie auf **Fertig**, um die Änderungen an der Richtlinie zu speichern.

1. Öffnen Sie das JSON-Modell [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) erneut.

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {}
       }
   }
   ```

   Beachten Sie, dass der Pfad **Seite 3** entfernt wurde: `/content/wknd-spa-react/us/en/home/page-2/page-3` aus dem anfänglichen JSON-Modell.

   Später werden wir beobachten, wie das AEM SPA Editor SDK zusätzliche Inhalte dynamisch laden kann.

## Implementierung der Navigation

Implementieren Sie anschließend das Navigationsmenü als Teil von `Header`. Wir könnten den Code direkt in `Header.js` hinzufügen, aber eine bessere Vorgehensweise ist, große Komponenten zu vermeiden. Stattdessen implementieren wir eine `Navigation` SPA Komponente, die später möglicherweise wiederverwendet werden könnte.

1. Überprüfen Sie die JSON, die von der Komponente AEM `Header` unter [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) bereitgestellt wird:

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   Die hierarchische Natur der AEM Seiten wird im JSON modelliert, das zum Ausfüllen eines Navigationsmenüs verwendet werden kann. Denken Sie daran, dass die `Header`-Komponente alle Funktionen der [Navigationskernkomponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) erbt und der über JSON offen gelegte Inhalt automatisch React Props zugeordnet wird.

1. Öffnen Sie ein neues Terminalfenster und navigieren Sie zum Ordner `ui.frontend` des SPA. Beginn Sie den **webpack-dev-server** mit dem Befehl `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Öffnen Sie eine neue Browser-Registerkarte und navigieren Sie zu [http://localhost:3000/](http://localhost:3000/).

   Der **webpack-dev-server** sollte so konfiguriert werden, dass das JSON-Modell von einer lokalen Instanz von AEM (`ui.frontend/.env.development`) proxyliert wird. Auf diese Weise können wir direkt mit den Inhalten vergleichen, die in AEM vorherigen Übung erstellt wurden. Vergewissern Sie sich, dass Sie in derselben Browsersitzung in AEM authentifiziert sind.

   ![Menüumschaltung](./assets/navigation-routing/nav-toggle-static.gif)

   Die Menüschaltfläche `Header` ist derzeit bereits implementiert. Implementieren Sie anschließend das Navigationsmenü.

1. Kehren Sie zur IDE Ihrer Wahl zurück und öffnen Sie die `Header.js` bei `ui.frontend/src/components/Header/Header.js`.
1. Aktualisieren Sie die `homeLink()`-Methode, um den hartcodierten String zu entfernen und die von der AEM-Komponente übergebenen dynamischen props zu verwenden:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   Der obige Code füllt eine URL basierend auf dem von der Komponente konfigurierten Stammnavigationselement. `homeLink()` wird verwendet, um das Logo in der  `logo()` Methode zu füllen und zu bestimmen, ob die Zurück-Taste angezeigt werden soll  `backButton()`.

   Speichern Sie die Änderungen in `Header.js`.

1. hinzufügen Sie eine Zeile oben auf `Header.js`, um die `Navigation`-Komponente unter den anderen Importen zu importieren:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Aktualisieren Sie anschließend die `get navigation()`-Methode, um die `Navigation`-Komponente zu instanziieren:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Wie bereits erwähnt, implementieren wir anstelle der Implementierung der Navigation innerhalb der `Header` den Großteil der Logik in der `Navigation`-Komponente.  Die Eigenschaftsvariablen von `Header` enthalten die JSON-Struktur, die zum Erstellen des Menüs erforderlich ist. Wir übergeben alle Props.
1. Öffnen Sie die Datei `Navigation.js` unter `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementieren Sie die `renderGroupNav(children)`-Methode:

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Diese Methode nimmt das Array der Navigationselemente `children` und erstellt eine ungeordnete Liste. Anschließend wird das Array durchlaufen und das Element an das `renderNavItem` übergeben, das dann implementiert wird.

1. Implementieren Sie Folgendes:`renderNavItem`

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Diese Methode rendert ein Element der Liste, wobei CSS-Klassen auf den Eigenschaften `level` und `active` basieren. Die Methode ruft dann `renderLink` auf, um das Anker-Tag zu erstellen. Da der `Navigation`-Inhalt hierarchisch ist, wird eine rekursive Strategie verwendet, um das `renderGroupNav` für die untergeordneten Elemente des aktuellen Elements aufzurufen.

1. Implementieren Sie die `renderLink`-Methode:

   hinzufügen eine Importmethode für die Komponente [Link](https://reacttraining.com/react-router/web/api/Link), Teil des React-Routers, oben in der Datei mit den anderen Importen:

   ```js
   import {Link} from "react-router-dom";
   ```

   Als Nächstes beenden Sie die Implementierung der `renderLink`-Methode:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Beachten Sie, dass anstelle eines normalen Anker-Tags `<a>` die Komponente [Link](https://reacttraining.com/react-router/web/api/Link) verwendet wird. Dadurch wird sichergestellt, dass keine vollständige Seitenaktualisierung ausgelöst wird, und stattdessen wird der React-Router vom AEM SPA Editor JS SDK genutzt.

1. Speichern Sie die Änderungen an `Navigation.js` und kehren Sie zum **webpack-dev-server** zurück: [http://localhost:3000](http://localhost:3000)

   ![Abgeschlossene Kopfzeilennavigation](assets/navigation-routing/completed-header.png)

   Öffnen Sie die Navigation, indem Sie auf den Menüschalter klicken, und Sie sollten die ausgefüllten Navigationslinks sehen. Sie sollten zu verschiedenen Ansichten der SPA navigieren können.

## Inspect - SPA Routing

Nachdem die Navigation implementiert wurde, überprüfen Sie das Routing in AEM.

1. Öffnen Sie in der IDE die Datei `index.js` unter `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Beachten Sie, dass `App` in die Komponente `Router` von [React Router](https://reacttraining.com/react-router/) eingeschlossen ist. Das vom JS-SDK für AEM SPA Editor bereitgestellte `ModelManager` fügt die dynamischen Routen zu AEM Seiten hinzu, die auf der JSON-Modell-API basieren.

1. Öffnen Sie ein Terminal, navigieren Sie zum Projektstamm und stellen Sie das Projekt bereit, AEM Sie Ihre Maven-Fähigkeiten verwenden:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zur SPA Homepage in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) und öffnen Sie die Entwicklerwerkzeuge Ihres Browsers. Screenshots unten werden aus dem Google Chrome-Browser erfasst.

   Aktualisieren Sie die Seite und Sie sollten eine XHR-Anforderung an `/content/wknd-spa-react/us/en.model.json` sehen, was der SPA Stamm ist. Beachten Sie, dass nur drei untergeordnete Seiten enthalten sind, basierend auf der Hierarchietiefenkonfiguration der SPA-Stammvorlage, die zuvor im Lernprogramm vorgenommen wurde. Dies umfasst nicht **Seite 3**.

   ![Initial-JSON-Anforderung - SPA Stamm](assets/navigation-routing/initial-json-request.png)

1. Wenn die Entwicklerwerkzeuge geöffnet sind, verwenden Sie die Navigation `Header`, um zu **Seite 3** zu navigieren:

   ![Seite 3 Navigieren](assets/navigation-routing/page-three-navigation.png)

   Beachten Sie, dass eine neue XHR-Anforderung an Folgendes gerichtet wird: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Seite drei XHR-Anforderung](assets/navigation-routing/page-3-xhr-request.png)

   Der AEM Modell-Manager erkennt, dass der JSON-Inhalt von **Seite 3** nicht verfügbar ist und Trigger automatisch die zusätzliche XHR-Anforderung.

1. Fahren Sie mit den verschiedenen Navigationslinks der Komponente `Header` weiter durch die SPA. Beachten Sie, dass keine weiteren XHR-Anforderungen gestellt werden und keine vollständige Aktualisierung der Seite erfolgt. Dadurch wird der SPA für den Endbenutzer schnell und unnötige Anforderungen werden auf AEM reduziert.

   ![Implementierung der Navigation](assets/navigation-routing/final-navigation-implemented.gif)

1. Experimentieren Sie mit Deep Links, indem Sie direkt zu Folgendem navigieren: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Beachten Sie, dass die Zurück-Taste des Browsers weiterhin funktioniert.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie mehrere Ansichten im SPA durch Zuordnen zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wurde mit dem React Router implementiert und der Komponente `Header` hinzugefügt.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) oder lokal prüfen, indem Sie zur Verzweigung `React/navigation-routing-solution` wechseln.
