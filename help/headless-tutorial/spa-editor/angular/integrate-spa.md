---
title: Integrieren einer SPA | Erste Schritte mit dem AEM-SPA-Editor und Angular
description: Erfahren Sie, wie der Quell-Code für eine in Angular geschriebene Single Page Application (SPA) in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Finden Sie heraus, wie Sie mit modernen Frontend-Tools wie dem Befehlszeilen-Tool (Command Line Interface, CLI) von Angular die SPA schnell für die AEM-JSON-Modell-API entwickeln können.
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: ht
source-wordcount: '2187'
ht-degree: 100%

---

# Integrieren einer SPA {#integrate-spa}

Erfahren Sie, wie der Quell-Code für eine in Angular geschriebene Single Page Application (SPA) in ein Adobe Experience Manager (AEM)-Projekt integriert werden kann. Finden Sie heraus, wie Sie mit modernen Frontend-Tools wie einem webpack-Dev-Server die SPA schnell für die AEM-JSON-Modell-API entwickeln können.

## Ziel

1. Erfahren Sie, wie das SPA-Projekt mit Client-seitigen Bibliotheken in AEM integriert wird
2. Erfahren, wie ein lokaler Entwicklungs-Server für die dedizierte Frontend-Entwicklung verwendet wird
3. Verwenden eines **Proxys** und einer statischen **Pseudodatei** zur Entwicklung für die AEM-JSON-Modell-API

## Was Sie erstellen werden

In diesem Kapitel wird beschrieben, wie der SPA eine einfache `Header`-Komponente hinzugefügt wird. Während der Erstellung dieser statischen `Header`-Komponente werden verschiedene Ansätze für die AEM-SPA-Entwicklung verwendet.

![Neuer Header in AEM](./assets/integrate-spa/final-header-component.png)

*Die SPA wird um eine statische `Header`-Komponente erweitert.*

## Voraussetzungen

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Abrufen des Codes

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Stellen Sie die Code-Basis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Fügen Sie bei Verwendung von [AEM 6.x](overview.md#compatibility) das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `Angular/integrate-spa-solution` wechseln.

## Integrationsansatz {#integration-approach}

Im Rahmen des AEM-Projekts wurden zwei Module erstellt: `ui.apps` und `ui.frontend`.

Das Modul `ui.frontend` ist ein [webpack](https://webpack.js.org/)-Projekt, das den gesamten SPA-Quellcode enthält. Ein Großteil der SPA-Entwicklung und -Tests erfolgt im webpack-Projekt. Wenn ein Produktions-Build ausgelöst wird, wird die SPA mithilfe von webpack erstellt und kompiliert. Die kompilierten Artefakte (CSS und JavaScript) werden in das Modul `ui.apps` kopiert, das dann für die AEM-Runtime bereitgestellt wird.

![Hochrangige Architektur von ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Eine Darstellung der SPA-Integration auf hoher Ebene.*

Weitere Informationen zum Frontend-Build [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=de).

## Überprüfen der SPA-Integration {#inspect-spa-integration}

Überprüfen Sie anschließend das Modul `ui.frontend`, um die automatisch vom [AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=de) erstellte SPA zu verstehen.

1. Öffnen Sie in der IDE Ihrer Wahl das AEM-Projekt für die WKND-SPA. In diesem Tutorial wird die [Visual Studio Code-IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html?lang=de#microsoft-visual-studio-code) verwendet.

   ![VSCode – AEM-WKND-SPA-Projekt](./assets/integrate-spa/vscode-ide-openproject.png)

2. Erweitern und überprüfen Sie den Ordner `ui.frontend`. Öffnen Sie die Datei `ui.frontend/package.json`

3. Unter `dependencies` sollten mehrere `@angular`-Abhängigkeiten zu sehen sein:

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

   Das Modul `ui.frontend` ist eine [Angular-Anwendung](https://angular.io), die mithilfe des [Angular-Befehlszeilen-Tools](https://angular.io/cli) (Command Line Interface, CLI) erstellt wurde und Routing-Funktionen umfasst.

4. Es sind auch drei `@adobe`-Abhängigkeiten vorhanden:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Die oben genannten Module bilden das [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html?lang=de) und stellen die Funktionalität bereit, mit der es möglich ist, SPA-Komponenten AEM-Komponenten zuzuordnen.

5. In der Datei `package.json` sind unter `scripts` mehrere Skripte definiert:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Diese Skripte basieren auf gängigen [Angular-Befehlszeilenbefehlen](https://angular.io/cli/build), die zur Verwendung mit dem größeren AEM-Projekt allerdings geringfügig verändert wurden.

   `start` führt die Angular-App lokal über einen lokalen Webserver aus. Er wurde aktualisiert, um den Inhalt der lokalen AEM-Instanz heranzuziehen.

   `build` kompiliert die Angular-App für die Produktionsverteilung. Durch Hinzufügen von `&& clientlib` wird die kompilierte SPA während eines Build-Vorgangs als Client-seitige Bibliothek in das Modul `ui.apps` kopiert. Das npm-Modul [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wird verwendet, um dies zu erleichtern.

   Weitere Informationen zu den verfügbaren Skripten finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=de).

6. Überprüfen Sie die Datei `ui.frontend/clientlib.config.js`. Diese Konfigurationsdatei wird von [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) verwendet, um zu bestimmen, wie die Client-Bibliothek erstellt wird.

7. Überprüfen Sie die Datei `ui.frontend/pom.xml`. Durch diese Datei wird aus dem Ordner `ui.frontend` ein [Maven-Modul](https://maven.apache.org/guides/mini/guide-multiple-modules.html). Die Datei `pom.xml` wurde aktualisiert, um die SPA während eines Maven-Builds mithilfe von [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) zu **testen** und zu **erstellen**.

8. Überprüfen Sie die Datei `app.component.ts` unter `ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` ist der Einstiegspunkt der SPA. `ModelManager` wird vom AEM SPA Editor JS SDK bereitgestellt. Darüber wird `pageModel` (der JSON-Inhalt) aufgerufen und in die Anwendung eingefügt.

## Hinzufügen einer Header-Komponente {#header-component}

Als Nächstes fügen Sie eine neue Komponente zur SPA hinzu und stellen die Änderungen in einer lokalen AEM-Instanz bereit, um sich die Integration anzeigen zu lassen.

1. Öffnen Sie ein neues Terminal-Fenster und navigieren Sie zum Ordner `ui.frontend`:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installieren Sie das [Angular-Befehlszeilen-Tool](https://angular.io/cli#installing-angular-cli) global, um die Angular-Komponenten zu generieren und die Angular-Anwendung mit dem Befehl **ng** zu erstellen und zu bedienen.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > Die von diesem Projekt verwendete **@angular/cli**-Version ist **9.1.7**. Es wird empfohlen, die Version des Angular-Befehlszeilen-Tools synchron zu halten.

3. Erstellen Sie eine neue `Header`-Komponente, indem Sie den Angular-Befehlszeilenbefehl `ng generate component` im Ordner `ui.frontend` ausführen.

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

   ![Header-Komponentenpfad in der IDE](assets/integrate-spa/header-component-path.png)

5. Öffnen Sie die Datei `header.component.html` und ersetzen Sie den Inhalt durch Folgendes:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Beachten Sie, dass hier statische Inhalte angezeigt werden, sodass diese Angular-Komponente keine Anpassungen an der standardmäßig generierten Datei `header.component.ts` erfordert.

6. Öffnen Sie die Datei **app.component.html** unter `ui.frontend/src/app/app.component.html`. Fügen Sie den `app-header` hinzu:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Dieser umfasst die `header`-Komponente über sämtlichem Seiteninhalt.

7. Öffnen Sie ein neues Terminal, navigieren Sie zum Ordner `ui.frontend` und führen Sie den Befehl `npm run build` aus:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navigieren Sie zum Ordner `ui.apps`. Unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` sollten Sie sehen, dass die kompilierten SPA-Dateien aus dem Ordner`ui.frontend/build` kopiert wurden.

   ![In ui.apps generierte Client-Bibliothek](assets/integrate-spa/compiled-spa-uiapps.png)

9. Kehren Sie zum Terminal zurück und navigieren Sie zum Ordner `ui.apps`. Führen Sie den folgenden Maven-Befehl aus:

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

   Dadurch wird das `ui.apps`-Paket in einer lokalen aktiven AEM-Instanz bereitgestellt.

10. Öffnen Sie eine Browser-Registerkarte und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Der Inhalt der `Header`-Komponente sollte nun in der SPA zu sehen sein.

   ![Anfängliche Header-Implementierung](assets/integrate-spa/initial-header-implementation.png)

   Die Schritte **7–9** werden automatisch ausgeführt, wenn ein Maven-Build aus dem Stammverzeichnis des Projekts (d. h. `mvn clean install -PautoInstallSinglePackage`) ausgelöst wird. Sie sollten jetzt die Grundlagen der Integration zwischen der SPA und Client-seitigen AEM-Bibliotheken verstehen. Beachten Sie, dass Sie weiterhin `Text`-Komponenten in AEM bearbeiten und hinzufügen können, die `Header`-Komponente aber nicht bearbeitbar ist.

## webpack-Dev-Server – Proxy für JSON-API {#proxy-json}

Wie in den vorherigen Übungen gezeigt, dauert es einige Minuten, einen Build durchzuführen und die Client-Bibliothek mit einer lokalen Instanz von AEM zu synchronisieren. Dies ist für Endtests akzeptabel, aber nicht ideal für den Großteil der SPA-Entwicklung.

Ein [webpack-Dev-Server](https://webpack.js.org/configuration/dev-server/) kann zur schnellen Entwicklung der SPA verwendet werden. Der SPA liegt ein von AEM erstelltes JSON-Modell zugrunde. In dieser Übung wird der JSON-Inhalt einer laufenden AEM-Instanz durch einen **Proxy-Vorgang** an den vom [Angular-Projekt](https://angular.io/guide/build) konfigurierten Entwicklungs-Server übergeben.

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

   Die [Angular-App](https://angular.io/guide/build#proxying-to-a-backend-server) bietet einen einfachen Mechanismus zum Durchführen von Proxy-Vorgängen für API-Anfragen. Die in `context` spezifizierten Muster müssen durch einen Proxy-Vorgang über `localhost:4502`, dem lokalen AEM-Schnellstart, übermittelt werden.

2. Öffnen Sie die Datei **index.html** unter `ui.frontend/src/index.html`. Dies ist die HTML-Stammdatei, die vom Dev-Server verwendet wird.

   Beachten Sie, dass ein Eintrag für `base href="/"` vorhanden ist. Das [Base-Tag](https://angular.io/guide/deployment#the-base-tag) ist wichtig, damit die App relative URLs auflösen kann.

   ```html
   <base href="/">
   ```

3. Öffnen Sie ein Terminal-Fenster und navigieren Sie zum Ordner `ui.frontend`. Führen Sie den Befehl `npm start` aus:

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

   ![webpack-Dev-Server – Proxy-JSON](assets/integrate-spa/webpack-dev-server-1.png)

   Sie sollten denselben Inhalt wie in AEM sehen, jedoch ohne dass eine der Authoring-Funktionen aktiviert ist.

5. Kehren Sie zur IDE zurück und erstellen Sie unter `ui.frontend/src/assets` einen neuen Ordner namens `img`.
6. Laden Sie das folgende WKND-Logo herunter und fügen Sie es dem Ordner `img` hinzu:

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

   ![Logo zum Header hinzugefügt](assets/integrate-spa/added-logo-localhost.png)

   Sie können weitere Inhaltsaktualisierungen in **AEM** vornehmen. Diese werden auf dem **webpack-Dev-Server** wiedergegeben, da wir den Inhalt über einen Proxy-Vorgang spiegeln. Beachten Sie, dass die Inhaltsänderungen nur auf dem **webpack-Dev-Server** sichtbar sind.

9. Halten Sie den lokalen Webserver an, indem Sie am Terminal `ctrl+c` verwenden.

## webpack-Dev-Server – Mock-JSON-API {#mock-json}

Eine weitere Möglichkeit, die Entwicklung zu beschleunigen, besteht darin, eine statische JSON-Datei als JSON-Modell zu verwenden. Durch JSON-„Mocking“ entfernen wir die Abhängigkeit von einer lokalen AEM-Instanz. Außerdem können dadurch Frontend-Entwicklerinnen und -Entwickler das JSON-Modell aktualisieren, um die Funktionalität zu testen und Änderungen an der JSON-API vorzunehmen, die später von Backend-Entwicklerinnen und -Entwicklern implementiert werden.

Die Ersteinrichtung des JSON-Mockups **erfordert eine lokale AEM-Instanz**.

1. Navigieren Sie im Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Hierbei handelt es sich um den von AEM exportierten JSON-Inhalt, von dem die Anwendung gesteuert wird. Kopieren Sie die JSON-Ausgabe.

2. Kehren Sie zur IDE zurück, navigieren Sie zu `ui.frontend/src` und fügen Sie neue Ordner namens **mocks** und **json** entsprechend der folgenden Ordnerstruktur hinzu:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Erstellen Sie unter `ui.frontend/public/mocks/json` eine neue Datei mit dem Namen **en.model.json**. Fügen Sie die JSON-Ausgabe aus **Schritt 1** hier ein.

   ![JSON-Datei des Mock-Modells](assets/integrate-spa/mock-model-json-created.png)

4. Erstellen Sie unter `ui.frontend` eine neue Datei mit dem Namen **proxy.mock.conf.json**. Füllen Sie die Datei mit folgendem Inhalt:

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

   Diese Proxy-Konfiguration schreibt Anfragen, die mit `/content/wknd-spa-angular/us` beginnen, zu `/mocks/json` um und beliefert die entsprechende statische JSON-Datei, z. B.:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Öffnen Sie die Datei **angular.json**. Fügen Sie eine neue **dev**-Konfiguration mit aktualisiertem **Assets**-Array hinzu, das auf den erstellten Ordner **mocks** verweist.

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

   ![Angular-JSON-Dev-Assets – Aktualsierungsordner](assets/integrate-spa/dev-assets-update-folder.png)

   Durch Erstellung einer dedizierten **dev**-Konfiguration wird sichergestellt, dass der Ordner **mocks** nur während der Entwicklung verwendet und nie in einem Produktions-Build in AEM bereitgestellt wird.

6. Aktualisieren Sie als Nächstes in der Datei **angular.json** die **browserTarget**-Konfiguration so, dass die neue **dev**-Konfiguration verwendet wird:

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

   ![Angular-JSON-Build-Dev-Aktualisierung](assets/integrate-spa/angular-json-build-dev-update.png)

7. Öffnen Sie die Datei `ui.frontend/package.json` und fügen Sie einen neuen Befehl **start:mock** zum Verweisen auf die Datei **proxy.mock.conf.json** hinzu.

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

8. Halten Sie den **webpack-Dev-Server** an, falls er aktuell ausgeführt wird. Starten Sie den **webpack-Dev-Server** mit dem Skript **start:mock**:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navigieren Sie zu [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html). Sie sollten daraufhin dieselbe SPA sehen, wobei der Inhalt nun aber aus der **JSON-Pseudodatei** bezogen wird.

9. Nehmen Sie eine kleine Änderung an der zuvor erstellten Datei **en.model.json** vor. Der aktualisierte Inhalt sollte sofort auf dem **webpack-Dev-Server** widergespiegelt werden.

   ![Aktualisierung der JSON-Modell-Pseudodatei](./assets/integrate-spa/webpack-mock-model.gif)

   Die Fähigkeit, das JSON-Modell zu bearbeiten und die Auswirkungen auf eine Live-SPA zu sehen, kann Entwicklerinnen und Entwicklern dabei helfen, die JSON-Modell-API zu verstehen. Außerdem können Frontend- und Backend-Entwicklung parallel erfolgen.

## Hinzufügen von Stilen mit Sass

Als Nächstes fügen Sie dem Projekt einige aktualisierte Stile hinzu. Durch dieses Projekt wird [Sass](https://sass-lang.com/)-Unterstützung für einige nützliche Funktionen wie Variablen hinzugefügt.

1. Öffnen Sie ein Terminal-Fenster und halten Sie den **webpack-Dev-Server** an, sofern gestartet. Geben Sie innerhalb des Ordners `ui.frontend` den folgenden Befehl ein, um die Angular-App so zu aktualisieren, dass **scss**-Dateien verarbeitet werden.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Dadurch wird die Datei `angular.json` mit einem neuen Eintrag am Ende der Datei aktualisiert:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installieren Sie `normalize-scss`, um die Stile Browser-übergreifend zu normalisieren:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Kehren Sie zur IDE zurück und erstellen Sie unter `ui.frontend/src` einen neuen Ordner mit dem Namen `styles`.
4. Erstellen Sie unter `ui.frontend/src/styles` eine neue Datei mit dem Namen `_variables.scss` und füllen Sie sie mit den folgenden Variablen auf:

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

5. Ändern Sie die Erweiterung der Datei **styles.css** unter `ui.frontend/src/styles.css` zu **styles.scss**. Ersetzen Sie den Inhalt durch Folgendes:

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

6. Aktualisieren Sie **angular.json** und ändern Sie den Namen **style.css** in allen Verweisen zu **styles.scss**. Es sollte 3 Verweise geben.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Aktualisieren der Header-Stile

Als Nächstes fügen Sie mit Sass einige markenspezifische Stile zur **Header**-Komponente hinzu.

1. Starten Sie den **webpack-Dev-Server**, um zu sehen, wie die Stile in Echtzeit aktualisiert werden:

   ```shell
   $ npm run start:mock
   ```

2. Benennen Sie unter `ui.frontend/src/app/components/header` die Datei **header.component.css** in **header.component.scss** um. Befüllen Sie die Datei mit folgendem Inhalt:

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

3. Aktualisieren Sie **header.component.ts** so, dass auf **header.component.scss** verwiesen wird:

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

4. Kehren Sie zum Browser und **webpack-Dev-Server** zurück:

   ![Header mit Stilen – webpack-Entwicklungs-Server](assets/integrate-spa/styled-header.png)

   Sie sollten nun die aktualisierten Stile sehen, die zur **Header**-Komponente hinzugefügt wurden.

## Bereitstellen von SPA-Updates für AEM

Die **Header**-Änderungen sind aktuell nur über den **webpack-Dev-Server** sichtbar. Stellen Sie die aktualisierte SPA für AEM bereit, um die Änderungen anzuzeigen.

1. Halten Sie den **webpack-Dev-Server** an.
2. Navigieren Sie zum Stammverzeichnis des Projekts `/aem-guides-wknd-spa` und stellen Sie das Projekt mithilfe von Maven für AEM bereit:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Sie sollten den aktualisierten **Header** mit angewendetem Logo und angewendeten Stilen sehen:

   ![Aktualisierter Header in AEM](assets/integrate-spa/final-header-component.png)

   Nachdem sich jetzt die aktualisierte SPA in AEM befindet, kann die Bearbeitung fortgesetzt werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben die SPA aktualisiert und die Integration mit AEM untersucht! Sie kennen jetzt zwei verschiedene Ansätze, um die SPA für die AEM-JSON-Modell-API mit einem **webpack-Dev-Server** zu entwickeln.

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `Angular/integrate-spa-solution` wechseln.

### Nächste Schritte {#next-steps}

[Zuordnen von SPA-Komponenten zu AEM-Komponenten](map-components.md): Erfahren Sie, wie Sie Angular-Komponenten Adobe Experience Manager (AEM)-Komponenten mit dem AEM SPA Editor JS SDK zuordnen. Die Komponentenzuordnung ermöglicht es Autorinnen und Autoren, im AEM-SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM-Authoring.
