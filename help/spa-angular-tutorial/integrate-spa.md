---
title: SPA integrieren | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie der Quellcode für eine Einzelseiten-App (SPA), die auf Angular geschrieben wurde, in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Erfahren Sie, wie Sie mit modernen Frontend-Tools wie dem Frontend-CLI-Tool die SPA schnell mit der AEM JSON-Modell-API entwickeln können.
sub-product: Sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2205'
ht-degree: 3%

---


# Integrieren eines SPA {#integrate-spa}

Erfahren Sie, wie der Quellcode für eine Einzelseiten-App (SPA), die auf Angular geschrieben wurde, in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Erfahren Sie, wie Sie mit modernen Frontend-Tools wie einem Webpack Development Server die SPA schnell mit der AEM JSON-Modell-API entwickeln können.

## Vorgabe

1. Erfahren Sie, wie das SPA-Projekt in AEM mit Client-seitigen Bibliotheken integriert ist.
2. Erfahren Sie, wie Sie einen lokalen Entwicklungsserver für die dedizierte Front-End-Entwicklung verwenden.
3. Erkunden Sie die Verwendung einer **proxy**- und statischen **mock**-Datei für die Entwicklung mit der AEM JSON-Modell-API.

## Was Sie erstellen werden

Dieses Kapitel fügt eine einfache `Header`-Komponente zum SPA hinzu. Bei der Erstellung dieser statischen `Header`-Komponente werden mehrere Ansätze für AEM SPA Entwicklung verwendet.

![Neue Kopfzeile in AEM](./assets/integrate-spa/final-header-component.png)

*Das SPA wird erweitert, um eine statische  `Header` Komponente hinzuzufügen.*

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/integrate-spa-solution` wechseln.

## Integrationsansatz {#integration-approach}

Im Rahmen des AEM wurden zwei Module erstellt: `ui.apps` und `ui.frontend`.

Das `ui.frontend`-Modul ist ein [webpack](https://webpack.js.org/)-Projekt, das den gesamten SPA Quellcode enthält. Ein Großteil der SPA wird im webpack-Projekt entwickelt und getestet. Wenn ein Produktions-Build ausgelöst wird, wird der SPA mithilfe des Webpack erstellt und kompiliert. Die kompilierten Artefakte (CSS und JavaScript) werden in das Modul `ui.apps` kopiert, das dann zur AEM Laufzeit bereitgestellt wird.

![ui.frontend-Architektur auf hoher Ebene](assets/integrate-spa/ui-frontend-architecture.png)

*Eine allgemeine Darstellung der SPA Integration.*

Weitere Informationen zum Front-End-Build finden Sie unter [hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect the SPA integration {#inspect-spa-integration}

Überprüfen Sie anschließend das Modul `ui.frontend` , um die SPA zu verstehen, die automatisch vom [AEM Projektarchetyp](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) generiert wurde.

1. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND-SPA. In diesem Tutorial wird die [Visual Studio Code IDE](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code) verwendet.

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. Erweitern und überprüfen Sie den Ordner `ui.frontend` . Öffnen Sie die Datei `ui.frontend/package.json`

3. Unter dem `dependencies` sollten Sie mehrere Verweise auf `@angular` sehen:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   Das Modul `ui.frontend` ist eine [Angular-Anwendung](https://angular.io), die mithilfe des [Angular-CLI-Tools](https://angular.io/cli) generiert wurde, das Routing umfasst.

4. Es gibt auch drei Abhängigkeiten mit dem Präfix `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Die oben genannten Module bilden das [AEM JS-SDK des Editors](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) und stellen die Funktionen bereit, mit denen SPA Komponenten AEM Komponenten zugeordnet werden können.

5. In der Datei `package.json` sind mehrere `scripts` definiert:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Diese Skripte basieren auf gängigen [Angular-CLI-Befehlen](https://angular.io/cli/build), wurden jedoch etwas geändert, um mit dem größeren AEM-Projekt zu arbeiten.

   `start` - führt die Angular-App lokal über einen lokalen Webserver aus. Es wurde aktualisiert, um den Inhalt der lokalen AEM-Instanz zu simulieren.

   `build` - kompiliert die Angular-App für die Produktionsverteilung. Das Hinzufügen von `&& clientlib` ist für das Kopieren der kompilierten SPA in das Modul `ui.apps` als Client-seitige Bibliothek während eines Builds verantwortlich. Das npm-Modul [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wird verwendet, um dies zu erleichtern.

   Weitere Informationen zu den verfügbaren Skripten finden Sie [hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Prüfen Sie die Datei `ui.frontend/clientlib.config.js`. Diese Konfigurationsdatei wird von [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) verwendet, um zu bestimmen, wie die Client-Bibliothek generiert wird.

7. Prüfen Sie die Datei `ui.frontend/pom.xml`. Diese Datei wandelt den Ordner `ui.frontend` in einen Ordner [Maven-Modul](http://maven.apache.org/guides/mini/guide-multiple-modules.html) um. Die Datei `pom.xml` wurde aktualisiert, um das [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) zu **test** und **build** SPA während eines Maven-Builds zu verwenden.

8. Inspect die Datei `app.component.ts` unter `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` ist der Einstiegspunkt des SPA. `ModelManager` wird vom AEM SPA Editor JS SDK bereitgestellt. Er ist für den Aufruf und die Injektion von `pageModel` (dem JSON-Inhalt) in die Anwendung verantwortlich.

## Hinzufügen einer Kopfzeilenkomponente {#header-component}

Fügen Sie anschließend eine neue Komponente zum SPA hinzu und stellen Sie die Änderungen auf einer lokalen AEM-Instanz bereit, um die Integration anzuzeigen.

1. Öffnen Sie ein neues Terminal-Fenster und navigieren Sie zum Ordner `ui.frontend`:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installieren Sie [Angular CLI](https://angular.io/cli#installing-angular-cli) global Dies wird zum Generieren von Angular-Komponenten sowie zum Erstellen und Bereitstellen der Angular-Anwendung über den Befehl **ng** verwendet.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > Die von diesem Projekt verwendete Version von **@angular/cli** ist **9.1.7**. Es wird empfohlen, die Angular-CLI-Versionen synchron zu halten.

3. Erstellen Sie eine neue `Header`-Komponente, indem Sie den Angular-CLI-Befehl `ng generate component` aus dem Ordner `ui.frontend` ausführen.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Dadurch wird ein Skelett für die neue Angular-Header-Komponente unter `ui.frontend/src/app/components/header` erstellt.

4. Öffnen Sie das Projekt `aem-guides-wknd-spa` in der IDE Ihrer Wahl. Navigieren Sie zum Ordner `ui.frontend/src/app/components/header`. 

   ![Kopfzeilenkomponentenpfad in der IDE](assets/integrate-spa/header-component-path.png)

5. Öffnen Sie die Datei `header.component.html` und ersetzen Sie den Inhalt durch Folgendes:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Beachten Sie, dass dies statischen Inhalt anzeigt, sodass diese Angular-Komponente keine Anpassungen am standardmäßig generierten `header.component.ts` erfordert.

6. Öffnen Sie die Datei **app.component.html** unter `ui.frontend/src/app/app.component.html`. Fügen Sie `app-header` hinzu:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Dazu gehört die Komponente `header` vor allem der Seiteninhalt.

7. Öffnen Sie ein neues Terminal, navigieren Sie zum Ordner `ui.frontend` und führen Sie den Befehl `npm run build` aus:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navigieren Sie zum Ordner `ui.apps`. Unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` sollten Sie sehen, dass die kompilierten SPA aus dem Ordner `ui.frontend/build` kopiert wurden.

   ![In ui.apps generierte Client-Bibliothek](assets/integrate-spa/compiled-spa-uiapps.png)

9. Kehren Sie zum Terminal zurück und navigieren Sie zum Ordner `ui.apps` . Führen Sie den folgenden Maven-Befehl aus:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Dadurch wird das `ui.apps`-Paket auf einer lokalen laufenden Instanz von AEM bereitgestellt.

10. Öffnen Sie eine Browser-Registerkarte und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Jetzt sollte der Inhalt der Komponente `Header` in der SPA angezeigt werden.

   ![Erste Kopfzeilenimplementierung](assets/integrate-spa/initial-header-implementation.png)

   Die Schritte **7-9** werden automatisch ausgeführt, wenn ein Maven-Build aus dem Stammverzeichnis des Projekts ausgelöst wird (d. h. `mvn clean install -PautoInstallSinglePackage`). Sie sollten jetzt die Grundlagen der Integration zwischen den SPA- und AEM Client-seitigen Bibliotheken verstehen. Beachten Sie, dass Sie weiterhin `Text`-Komponenten in AEM bearbeiten und hinzufügen können. Die `Header`-Komponente kann jedoch nicht bearbeitet werden.

## Webpack Dev Server - Proxy der JSON-API {#proxy-json}

Wie in den vorherigen Übungen gezeigt, dauert es einige Minuten, einen Build durchzuführen und die Client-Bibliothek mit einer lokalen Instanz von AEM zu synchronisieren. Dies ist für Endtests akzeptabel, aber nicht ideal für den Großteil der SPA.

Ein [webpack-Dev-Server](https://webpack.js.org/configuration/dev-server/) kann verwendet werden, um die SPA schnell zu entwickeln. Die SPA wird von einem JSON-Modell gesteuert, das von AEM generiert wurde. In dieser Übung wird der JSON-Inhalt von einer laufenden Instanz von AEM **proxied** in den vom [Angular-Projekt](https://angular.io/guide/build) konfigurierten Entwicklungsserver übertragen.

1. Kehren Sie zur IDE zurück und öffnen Sie die Datei **proxy.conf.json** unter `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   Die [Angular-App](https://angular.io/guide/build#proxying-to-a-backend-server) bietet einen einfachen Mechanismus zum Proxy von API-Anfragen. Die in `context` angegebenen Muster werden über `localhost:4502` bereitgestellt, den lokalen AEM-Schnellstart.

2. Öffnen Sie die Datei **index.html** unter `ui.frontend/src/index.html`. Dies ist die HTML-Stammdatei, die vom Dev-Server verwendet wird.

   Beachten Sie, dass es einen Eintrag für `base href="/"` gibt. Das [base-Tag](https://angular.io/guide/deployment#the-base-tag) ist wichtig, damit die App relative URLs auflöst.

   ```html
   <base href="/">
   ```

3. Öffnen Sie ein Terminal-Fenster und navigieren Sie zum Ordner `ui.frontend` . Führen Sie den Befehl `npm start` aus:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Öffnen Sie eine neue Browser-Registerkarte (falls noch nicht geöffnet) und navigieren Sie zu [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Webpack-Dev-Server - Proxy-JSON](assets/integrate-spa/webpack-dev-server-1.png)

   Sie sollten denselben Inhalt wie in AEM sehen, jedoch ohne eine der Authoring-Funktionen aktiviert zu haben.

5. Kehren Sie zur IDE zurück und erstellen Sie einen neuen Ordner mit dem Namen `img` unter `ui.frontend/src/assets`.
6. Laden Sie das folgende WKND-Logo herunter und fügen Sie es zum Ordner `img` hinzu:

   ![WKND-Logo](./assets/integrate-spa/wknd-logo-dk.png)

7. Öffnen Sie **header.component.html** unter `ui.frontend/src/app/components/header/header.component.html` und fügen Sie das Logo ein:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Speichern Sie die Änderungen in **header.component.html**.

8. Kehren Sie zum Browser zurück. Sie sollten die Änderungen an der App sofort sehen.

   ![Logo zur Kopfzeile hinzugefügt](assets/integrate-spa/added-logo-localhost.png)

   Sie können weiterhin Inhaltsaktualisierungen in **AEM** vornehmen und sie im **webpack-Dev-Server** anzeigen, da wir den Inhalt proxieren. Beachten Sie, dass die Inhaltsänderungen nur im **webpack dev server** sichtbar sind.

9. Beenden Sie den lokalen Webserver mit `ctrl+c` im Terminal.

## Webpack Dev Server - JSON-API nachahmen {#mock-json}

Ein weiterer Ansatz für die schnelle Entwicklung besteht darin, eine statische JSON-Datei zu verwenden, um als JSON-Modell zu fungieren. Durch &quot;nachahmen&quot;der JSON entfernen wir die Abhängigkeit von einer lokalen AEM-Instanz. Darüber hinaus kann ein Frontend-Entwickler das JSON-Modell aktualisieren, um die Funktionalität zu testen und Änderungen an der JSON-API vorzunehmen, die später von einem Back-End-Entwickler implementiert würden.

Die anfängliche Einrichtung der JSON-nachgeahmten Instanz erfordert **eine lokale AEM**.

1. Navigieren Sie im Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Dies ist die von AEM exportierte JSON, die die Anwendung steuert. Kopieren Sie die JSON-Ausgabe.

2. Kehren Sie zur IDE zurück und navigieren Sie zu `ui.frontend/src` und fügen Sie neue Ordner mit den Namen **mocks** und **json** hinzu, die der folgenden Ordnerstruktur entsprechen:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Erstellen Sie eine neue Datei mit dem Namen **en.model.json** unter `ui.frontend/public/mocks/json`. Fügen Sie hier die JSON-Ausgabe aus **Schritt 1** ein.

   ![JSON-Modelldatei nachahmen](assets/integrate-spa/mock-model-json-created.png)

4. Erstellen Sie eine neue Datei **proxy.mock.conf.json** unter `ui.frontend`. Füllen Sie die Datei mit folgendem Inhalt:

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Diese Proxy-Konfiguration schreibt Anforderungen neu, die mit `/content/wknd-spa-angular/us` mit `/mocks/json` beginnen, und stellt die entsprechende statische JSON-Datei bereit, z. B.:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Öffnen Sie die Datei **angular.json**. Fügen Sie eine neue **dev**-Konfiguration mit einem aktualisierten **assets**-Array hinzu, um auf den erstellten Ordner **mocks** zu verweisen.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Angular JSON Dev Assets Update Folder](assets/integrate-spa/dev-assets-update-folder.png)

   Durch Erstellen einer dedizierten **dev**-Konfiguration wird sichergestellt, dass der Ordner **mocks** nur während der Entwicklung verwendet und nie in einem Produktions-Build AEM bereitgestellt wird.

6. Aktualisieren Sie in der Datei **angular.json** als Nächstes die **browserTarget**-Konfiguration, um die neue **dev**-Konfiguration zu verwenden:

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular JSON-Build-Dev-Update](assets/integrate-spa/angular-json-build-dev-update.png)

7. Öffnen Sie die Datei `ui.frontend/package.json` und fügen Sie den neuen Befehl **start:mock** hinzu, um auf die Datei **proxy.mock.conf.json** zu verweisen.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   Durch Hinzufügen eines neuen Befehls ist es einfach, zwischen den Proxy-Konfigurationen umzuschalten.

8. Wenn sie derzeit ausgeführt wird, beenden Sie den **webpack-Dev-Server**. Starten Sie den **webpack dev server** mithilfe des Skripts **start:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navigieren Sie zu [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) und Sie sollten die gleiche SPA sehen, aber der Inhalt wird jetzt aus der JSON-Datei **mock** abgerufen.

9. Nehmen Sie eine kleine Änderung an der zuvor erstellten Datei **en.model.json** vor. Der aktualisierte Inhalt sollte sofort im **webpack dev server** übernommen werden.

   ![JSON-Update für Modell nachahmen](./assets/integrate-spa/webpack-mock-model.gif)

   Die Fähigkeit, das JSON-Modell zu bearbeiten und die Auswirkungen auf eine Live-SPA zu sehen, kann Entwicklern dabei helfen, die JSON-Modell-API zu verstehen. Es ermöglicht auch die parallele Entwicklung von Frontend- und Backend-Anwendungen.

## Stile mit Sass hinzufügen

Als Nächstes wird dem Projekt ein Stil hinzugefügt, der aktualisiert wird. In diesem Projekt wird [Sass](https://sass-lang.com/) Unterstützung für einige nützliche Funktionen wie Variablen hinzugefügt.

1. Öffnen Sie ein Terminal-Fenster und stoppen Sie den **webpack-Dev-Server**, wenn er gestartet wird. Geben Sie im Ordner `ui.frontend` den folgenden Befehl ein, um die Angular-App zu aktualisieren und **.scss**-Dateien zu verarbeiten.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Dadurch wird die `angular.json`-Datei mit einem neuen Eintrag am unteren Rand der Datei aktualisiert:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installieren Sie `normalize-scss`, um die Stile über Browser hinweg zu normalisieren:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Kehren Sie zur IDE zurück und erstellen Sie unter `ui.frontend/src` einen neuen Ordner mit dem Namen `styles`.
4. Erstellen Sie eine neue Datei unter `ui.frontend/src/styles` mit dem Namen `_variables.scss` und fügen Sie sie mit den folgenden Variablen ein:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Benennen Sie die Erweiterung der Datei **styles.css** unter `ui.frontend/src/styles.css` in **styles.scss** um. Ersetzen Sie den Inhalt durch Folgendes:

   ```scss
   /* styles.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Aktualisieren Sie **angular.json** und benennen Sie alle Verweise auf **style.css** mit **styles.scss** um. Es sollte 3 Verweise geben.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Header-Stile aktualisieren

Als Nächstes fügen Sie der Komponente **Header** einige markenspezifische Stile mithilfe von Sass hinzu.

1. Starten Sie den **webpack Development Server**, um die Stile zu sehen, die in Echtzeit aktualisiert werden:

   ```shell
   $ npm run start:mock
   ```

2. Benennen Sie unter `ui.frontend/src/app/components/header` **header.component.css** um **header.component.scss**. Füllen Sie die Datei mit folgendem Inhalt:

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Aktualisieren Sie **header.component.js**, um auf **header.component.scss** zu verweisen:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Kehren Sie zum Browser und zum **webpack-Dev-Server** zurück:

   ![Styled Header - webpack Development Server](assets/integrate-spa/styled-header.png)

   Sie sollten nun die aktualisierten Stile sehen, die der Komponente **Header** hinzugefügt wurden.

## Bereitstellen SPA Updates für AEM

Die Änderungen, die an der **Kopfzeile** vorgenommen wurden, sind derzeit nur über den **webpack dev server** sichtbar. Stellen Sie die aktualisierte SPA für AEM bereit, um die Änderungen anzuzeigen.

1. Beenden Sie den **webpack-Dev-Server**.
2. Navigieren Sie zum Stammverzeichnis des Projekts `/aem-guides-wknd-spa` und stellen Sie das Projekt mithilfe von Maven für AEM bereit:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Sie sollten die aktualisierte **Kopfzeile** mit angewendetem Logo und angewendeten Stilen sehen:

   ![Aktualisierte Kopfzeile in AEM](assets/integrate-spa/final-header-component.png)

   Nachdem sich die aktualisierte SPA in AEM befindet, kann die Bearbeitung fortgesetzt werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben die SPA aktualisiert und die Integration mit AEM untersucht! Sie kennen jetzt zwei verschiedene Ansätze zur Entwicklung des SPA mit der AEM JSON-Modell-API mit einem **webpack-Dev-Server**.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/integrate-spa-solution` wechseln.

### Nächste Schritte {#next-steps}

[Zuordnen SPA Komponenten zu AEM Komponenten](map-components.md)  - Erfahren Sie, wie Sie Angular-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Autoren, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.
