---
title: Hinzufügen von Navigation und Routing | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie mehrere Ansichten im SPA unterstützt werden können, indem Sie sie mit dem SPA Editor SDK AEM Seiten zuordnen. Die dynamische Navigation wird mit React-Router und React-Kernkomponenten implementiert.
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# Hinzufügen von Navigation und Routing {#navigation-routing}

Erfahren Sie, wie mehrere Ansichten im SPA unterstützt werden können, indem Sie sie mit dem SPA Editor SDK AEM Seiten zuordnen. Die dynamische Navigation wird mit React-Router und React-Kernkomponenten implementiert.

## Ziel

1. Machen Sie sich mit den SPA Routing-Optionen vertraut, die bei Verwendung des SPA-Editors verfügbar sind.
1. Verwendung [React-Router](https://reacttraining.com/react-router/) um zwischen verschiedenen Ansichten des SPA zu navigieren.
1. Verwenden Sie AEM React-Kernkomponenten, um eine dynamische Navigation zu implementieren, die von der AEM Seitenhierarchie gesteuert wird.

## Was Sie erstellen werden

Dieses Kapitel fügt einem SPA in AEM Navigation hinzu. Das Navigationsmenü wird von der AEM Seitenhierarchie gesteuert und nutzt das JSON-Modell, das von der [Navigations-Kernkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigation hinzugefügt](assets/navigation-routing/navigation-added.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment). Dieses Kapitel ist eine Fortsetzung der [Zuordnungskomponenten](map-components.md) -Kapitel zu folgen, ist jedoch ein SPA-aktiviertes AEM-Projekt, das in einer lokalen AEM-Instanz bereitgestellt wird.

## Hinzufügen der Navigation zur Vorlage {#add-navigation-template}

1. Öffnen Sie einen Browser und melden Sie sich bei AEM an. [http://localhost:4502/](http://localhost:4502/). Die Basis für den Startcode sollte bereits bereitgestellt werden.
1. Navigieren Sie zum **SPA Seitenvorlage**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Auswählen des äußersten **Root Layout Container** und klicken Sie auf **Politik** Symbol. Pass auf **not** zur Auswahl der **Layout-Container** nicht gesperrt für die Bearbeitung.

   ![Wählen Sie das Symbol für die Root Layout Container-Richtlinie aus](assets/navigation-routing/root-layout-container-policy.png)

1. Erstellen Sie eine neue Richtlinie mit dem Namen **SPA**:

   ![SPA](assets/navigation-routing/spa-policy-update.png)

   under **Zugelassene Komponenten** > **Allgemein** > wählen Sie die **Layout-Container** -Komponente.

   under **Zugelassene Komponenten** > **WKND SPA REACT - STRUKTUR** > wählen Sie die **Navigation** component:

   ![Navigationskomponente auswählen](assets/navigation-routing/select-navigation-component.png)

   under **Zugelassene Komponenten** > **WKND SPA REACT - Content** > wählen Sie die **Bild** und **Text** Komponenten. Es sollten vier Komponenten ausgewählt sein.

   Klicken **Fertig** , um die Änderungen zu speichern.

1. Aktualisieren Sie die Seite und fügen Sie die **Navigation** Komponente über der nicht gesperrten **Layout-Container**:

   ![Navigationskomponente zur Vorlage hinzufügen](assets/navigation-routing/add-navigation-component.png)

1. Wählen Sie die **Navigation** Komponente und klicken Sie auf ihre **Politik** zum Bearbeiten der Richtlinie.
1. Erstellen Sie eine neue Richtlinie mit einer **Richtlinienname** von **SPA**.

   Unter dem **Eigenschaften**:

   * Legen Sie die **Navigationsstamm** nach `/content/wknd-spa-react/us/en`.
   * Legen Sie die **Ausschließen von Stammebenen** nach **1**.
   * Deaktivieren **Sammlung aller untergeordneten Seiten**.
   * Legen Sie die **Navigationsstrukturtiefe** nach **3**.

   ![Navigationsrichtlinie konfigurieren](assets/navigation-routing/navigation-policy.png)

   Dadurch werden die Navigations-2-Ebenen unten erfasst `/content/wknd-spa-react/us/en`.

1. Nach dem Speichern der Änderungen sollte die `Navigation` als Teil der Vorlage:

   ![Füllte Navigationskomponente](assets/navigation-routing/populated-navigation.png)

## Untergeordnete Seiten erstellen

Als Nächstes erstellen Sie zusätzliche Seiten in AEM , die als verschiedene Ansichten in der SPA dienen. Wir werden auch die hierarchische Struktur des von AEM bereitgestellten JSON-Modells untersuchen.

1. Navigieren Sie zum **Sites** console: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Wählen Sie die **WKND SPA React-Homepage** und klicken Sie auf **Erstellen** > **Seite**:

   ![Neue Seite erstellen](assets/navigation-routing/create-new-page.png)

1. under **Vorlage** select **SPA**. under **Eigenschaften** enter **Seite 1** für **Titel** und **page-1** als Namen.

   ![anfängliche Seiteneigenschaften eingeben](assets/navigation-routing/initial-page-properties.png)

   Klicken **Erstellen** und klicken Sie im Popup-Dialogfeld auf **Öffnen** , um die Seite im AEM SPA Editor zu öffnen.

1. Hinzufügen neuer **Text** -Komponente in die Hauptkomponente **Layout-Container**. Bearbeiten Sie die Komponente und geben Sie den Text ein: **Seite 1** mithilfe des RTE und der **H2** -Element.

   ![Beispielinhaltsseite 1](assets/navigation-routing/page-1-sample-content.png)

   Sie können zusätzliche Inhalte hinzufügen, wie z. B. ein Bild.

1. Kehren Sie zur AEM Sites-Konsole zurück und wiederholen Sie die oben beschriebenen Schritte. Erstellen Sie dann eine zweite Seite mit dem Namen **Seite 2** als Geschwister **Seite 1**.
1. Erstellen Sie abschließend eine dritte Seite. **Seite 3** aber als **child** von **Seite 2**. Nach Abschluss der Site-Hierarchie sollte wie folgt aussehen:

   ![Beispiel-Site-Hierarchie](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Die Navigationskomponente kann jetzt verwendet werden, um zu verschiedenen Bereichen des SPA zu navigieren.

   ![Navigation und Routing](assets/navigation-routing/navigation-working.gif)

1. Öffnen Sie die Seite außerhalb des AEM-Editors: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Verwenden Sie die **Navigation** -Komponente, um zu verschiedenen Ansichten der App zu navigieren.

1. Verwenden Sie beim Navigieren die Entwicklertools Ihres Browsers, um die Netzwerkanforderungen zu überprüfen. Die folgenden Screenshots werden aus dem Chrome-Browser von Google erfasst.

   ![Überwachung von Netzwerkanfragen](assets/navigation-routing/inspect-network-requests.png)

   Beachten Sie, dass die nachfolgende Navigation nach dem ersten Laden der Seite nicht zu einer vollständigen Seitenaktualisierung führt und dass der Netzwerk-Traffic bei der Rückkehr zu zuvor besuchten Seiten minimiert wird.

## JSON-Modell für Hierarchieseite {#hierarchy-page-json-model}

Überprüfen Sie anschließend das JSON-Modell, das das mehrdimensionale Erlebnis des SPA steuert.

1. Öffnen Sie in einer neuen Registerkarte die von AEM bereitgestellte JSON-Modell-API: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Es kann hilfreich sein, eine Browsererweiterung zu verwenden, um [JSON formatieren](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   Dieser JSON-Inhalt wird angefordert, wenn die SPA zum ersten Mal geladen wird. Die äußere Struktur sieht wie folgt aus:

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

   under `:children` sollte für jede der erstellten Seiten ein Eintrag angezeigt werden. Der Inhalt für alle Seiten befindet sich in dieser ersten JSON-Anfrage. Beim Navigations-Routing werden nachfolgende Ansichten des SPA schnell geladen, da der Inhalt bereits clientseitig verfügbar ist.

   Es ist nicht ratsam, **ALL** des Inhalts einer SPA in der ersten JSON-Anfrage, da dies das anfängliche Laden der Seite verlangsamen würde. Als Nächstes wird untersucht, wie die Hierarchietiefe von Seiten erfasst wird.

1. Navigieren Sie zum **SPA** Vorlage unter: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Klicken Sie auf **Menü &quot;Seiteneigenschaften&quot;** > **Seitenrichtlinie**:

   ![Öffnen Sie die Seitenrichtlinie für SPA Stamm.](assets/navigation-routing/open-page-policy.png)

1. Die **SPA** -Vorlage weist eine zusätzliche **Hierarchische Struktur** -Tab, um den erfassten JSON-Inhalt zu steuern. Die **Strukturtiefe** bestimmt, wie tief in der Site-Hierarchie untergeordnete Seiten unterhalb der **root**. Sie können auch die **Strukturmuster** -Feld zum Filtern zusätzlicher Seiten basierend auf einem regulären Ausdruck.

   Aktualisieren Sie die **Strukturtiefe** nach **2**:

   ![Aktualisierung der Strukturtiefe](assets/navigation-routing/update-structure-depth.png)

   Klicken **Fertig** , um die Änderungen an der Richtlinie zu speichern.

1. Erneutes Öffnen des JSON-Modells [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   Beachten Sie, dass **Seite 3** path wurde entfernt: `/content/wknd-spa-react/us/en/home/page-2/page-3` vom ersten JSON-Modell aus. Dies liegt daran, dass **Seite 3** befindet sich auf einer Ebene 3 in der Hierarchie und wir haben die Richtlinie aktualisiert, sodass sie nur Inhalte mit einer maximalen Tiefe von Stufe 2 enthält.

1. Öffnen Sie die SPA Homepage erneut: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) und öffnen Sie die Entwicklertools Ihres Browsers.

   Aktualisieren Sie die Seite und Sie sollten die XHR-Anfrage an `/content/wknd-spa-react/us/en.model.json`, der SPA Stamm. Beachten Sie, dass nur drei untergeordnete Seiten enthalten sind, basierend auf der Hierarchietiefenkonfiguration der SPA-Stammvorlage, die zuvor im Tutorial vorgenommen wurde. Dies umfasst nicht **Seite 3**.

   ![Anfängliche JSON-Anfrage - SPA Stamm](assets/navigation-routing/initial-json-request.png)

1. Wenn die Entwicklertools geöffnet sind, verwenden Sie die `Navigation` Komponente zur direkten Navigation zu **Seite 3**:

   Beachten Sie, dass eine neue XHR-Anfrage an Folgendes gesendet wird: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Seite drei XHR-Anfrage](assets/navigation-routing/page-3-xhr-request.png)

   Der AEM Model Manager versteht, dass die **Seite 3** JSON-Inhalt ist nicht verfügbar und Trigger automatisch die zusätzliche XHR-Anforderung.

1. Experimentieren Sie mit Deep-Links, indem Sie direkt zu folgenden Elementen navigieren: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Beachten Sie außerdem, dass die Zurück-Schaltfläche des Browsers weiterhin funktioniert.

## Inspect React Routing  {#react-routing}

Die Navigation und das Routing werden mit [React-Router](https://reactrouter.com/). React Router sind eine Sammlung von Navigationskomponenten für React-Anwendungen. [AEM React-Kernkomponenten](https://github.com/adobe/aem-react-core-wcm-components-base) verwendet Funktionen des React-Routers, um die **Navigation** -Komponente, die in den vorherigen Schritten verwendet wurde.

Überprüfen Sie als Nächstes, wie der React-Router mit dem SPA integriert ist, und experimentieren Sie mit dem React-Router. [Link](https://reactrouter.com/web/api/Link) -Komponente.

1. Öffnen Sie die -Datei in der IDE. `index.js` at `ui.frontend/src/index.js`.

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

   Beachten Sie, dass `App` in der `Router` Komponente aus [React-Router](https://reacttraining.com/react-router/). Die `ModelManager`, bereitgestellt vom AEM SPA Editor JS SDK, fügt die dynamischen Routen zu AEM Seiten hinzu, basierend auf der JSON-Modell-API.

1. Öffnen Sie die Datei `Page.js` unter `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   Die `Page` SPA Komponente verwendet `MapTo` Funktion zum Zuordnen **Seiten** in eine entsprechende SPA Komponente AEM. Die `withRoute` -Dienstprogramm hilft beim dynamischen Routing des SPA zur entsprechenden untergeordneten Seite AEM basierend auf der `cqPath` -Eigenschaft.

1. Öffnen Sie die `Header.js` Komponente bei `ui.frontend/src/components/Header/Header.js`.
1. Aktualisieren Sie die `Header` um die `<h1>` Tag in einem [Link](https://reactrouter.com/web/api/Link) auf der Homepage:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   Statt eine Standardeinstellung zu verwenden `<a>` Anker-Tag verwenden `<Link>` bereitgestellt vom React-Router. Solange die Variable `to=` auf eine gültige Route verweist, wechselt der SPA zu dieser Route und **not** Führen Sie eine vollständige Seitenaktualisierung durch. Hier wird der Link zur Startseite einfach hartcodiert, um die Verwendung von `Link`.

1. Aktualisieren Sie den Test bei `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Da wir Funktionen des React-Routers innerhalb einer statischen Komponente verwenden, auf die in `App.js` müssen wir den Komponententest aktualisieren, um dies zu berücksichtigen.

1. Öffnen Sie ein Terminal, navigieren Sie zum Stammverzeichnis des Projekts und stellen Sie das Projekt mithilfe Ihrer Maven-Kenntnisse AEM bereit:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zu einer der Seiten im SPA in AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   Statt die `Navigation` -Komponente, um zu navigieren, verwenden Sie den Link im `Header`.

   ![Kopfzeilenlink](assets/navigation-routing/header-link.png)

   Beachten Sie, dass eine vollständige Seitenaktualisierung **not** ausgelöst wurde und das SPA Routing funktioniert.

1. Experimentieren Sie optional mit der `Header.js` Datei mit einer Standarddatei `<a>` Anker-Tag:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Dies kann den Unterschied zwischen SPA Routing und regulären Web-Seiten-Links veranschaulichen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben erfahren, wie mehrere Ansichten im SPA durch die Zuordnung zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wurde mit dem React-Router implementiert und zum `Header` -Komponente.
