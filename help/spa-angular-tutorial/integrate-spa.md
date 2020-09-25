---
title: SPA integrieren | Erste Schritte mit dem AEM SPA Editor und Angular
description: Verstehen Sie, wie der Quellcode für eine in Angular geschriebene Einzelseitenanwendung (SPA) in ein Adobe Experience Manager-Projekt (AEM) integriert werden kann. Erfahren Sie, wie Sie mit modernen Front-End-Tools wie dem CLI-Tool von Angular die SPA mit der AEM JSON-Modell-API schnell entwickeln.
sub-product: Sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 1%

---


# SPA integrieren {#integrate-spa}

Verstehen Sie, wie der Quellcode für eine in Angular geschriebene Einzelseitenanwendung (SPA) in ein Adobe Experience Manager-Projekt (AEM) integriert werden kann. Erfahren Sie, wie Sie mit modernen Front-End-Werkzeugen wie einem Webpack-Dev-Server die SPA mit der AEM JSON-Modell-API schnell entwickeln.

## Vorgabe

1. Verstehen Sie, wie das SPA-Projekt in AEM mit clientseitigen Bibliotheken integriert ist.
2. Erfahren Sie, wie Sie einen lokalen Entwicklungsserver für die dedizierte Front-End-Entwicklung verwenden.
3. Verwenden einer **Proxy** - und statischen **Modell** -Datei zur Entwicklung mit der AEM JSON-Modell-API

## Was Sie erstellen

In diesem Kapitel wird eine einfache `Header` Komponente zur SPA hinzugefügt. Bei der Erstellung dieser statischen `Header` Komponente werden verschiedene Ansätze zur Entwicklung AEM SPA verwendet.

![Neue Kopfzeile in AEM](./assets/integrate-spa/final-header-component.png)

*Die SPA wird erweitert, um eine statische`Header`Komponente hinzuzufügen*

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Bei Verwendung von [AEM 6.x](overview.md#compatibility) fügen Sie das `classic` Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `Angular/integrate-spa-solution`.

## Integrationsansatz {#integration-approach}

Im Rahmen des AEM Projekts wurden zwei Module erstellt: `ui.apps` und `ui.frontend`.

Das `ui.frontend` Modul ist ein [Webpack](https://webpack.js.org/) -Projekt, das den gesamten SPA-Quellcode enthält. Ein Großteil der SPA-Entwicklung und -Tests wird im Webpack-Projekt durchgeführt. Wenn ein Produktionsaufbau ausgelöst wird, wird die SPA mithilfe von Webpack erstellt und kompiliert. Die kompilierten Artefakte (CSS und JavaScript) werden in das `ui.apps` Modul kopiert, das dann zur AEM Laufzeit bereitgestellt wird.

![ui.frontend-Architektur auf hoher Ebene](assets/integrate-spa/ui-frontend-architecture.png)

*Eine detaillierte Darstellung der SPA-Integration.*

Weitere Informationen zum Front-End-Build finden Sie hier [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect - SPA-Integration {#inspect-spa-integration}

Überprüfen Sie anschließend das `ui.frontend` Modul, um die SPA zu verstehen, die vom [AEM Projektarchiv](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)automatisch generiert wurde.

1. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND SPA. In diesem Lernprogramm wird die [Visual Studio-Code-IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)verwendet.

   ![VSCode - AEM WKND SPA-Projekt](./assets/integrate-spa/vscode-ide-openproject.png)

2. Erweitern Sie den `ui.frontend` Ordner und überprüfen Sie ihn. Öffnen Sie die Datei `ui.frontend/package.json`

3. Unter dem `dependencies` Link sollten Sie einige mit `@angular`:

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

   Das `ui.frontend` Modul ist eine [Angular-Anwendung](https://angular.io) , die mithilfe des [Angular-CLI-Tools](https://angular.io/cli) generiert wird, das Routing enthält.

4. Es gibt auch drei Abhängigkeiten mit dem Präfix `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Die oben genannten Module bilden das [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) und bieten die Funktionalität, um SPA-Komponenten AEM Komponenten zuzuordnen.

5. In der `package.json` Datei `scripts` sind mehrere definiert:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Diese Skripten basieren auf gängigen [Angular-CLI-Befehlen](https://angular.io/cli/build) , wurden jedoch leicht modifiziert, um mit dem größeren AEM zu funktionieren.

   `start` - führt die Angular-App lokal mit einem lokalen Webserver aus. Es wurde aktualisiert, um den Inhalt der lokalen AEM zu verändern.

   `build` - kompiliert die Angular-App für die Produktionsverteilung. Der Zusatz von `&& clientlib` ist verantwortlich für das Kopieren der kompilierten SPA in das `ui.apps` Modul als clientseitige Bibliothek während eines Builds. Das Modul npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wird dazu verwendet.

   Weitere Informationen zu den verfügbaren Skripten finden Sie [hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect die Datei `ui.frontend/clientlib.config.js`. Diese Konfigurationsdatei wird von [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) verwendet, um zu bestimmen, wie die Client-Bibliothek generiert wird.

7. Inspect die Datei `ui.frontend/pom.xml`. Diese Datei wandelt den `ui.frontend` Ordner in ein [Maven-Modul](http://maven.apache.org/guides/mini/guide-multiple-modules.html)um. Die `pom.xml` Datei wurde aktualisiert, um das [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) zu verwenden, um die SPA während eines Maven-Builds zu **testen** und zu **erstellen** .

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

   `app.component.js` ist der Einstiegspunkt des BSG. `ModelManager` wird vom AEM SPA Editor JS SDK bereitgestellt. Es ist dafür zuständig, den `pageModel` (JSON-Inhalt) aufzurufen und in den Antrag einzufügen.

## hinzufügen einer Kopfzeilenkomponente {#header-component}

Fügen Sie dann der SPA eine neue Komponente hinzu und stellen Sie die Änderungen auf einer lokalen AEM bereit, um die Integration anzuzeigen.

1. Öffnen Sie ein neues Terminalfenster und navigieren Sie zum `ui.frontend` Ordner:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installieren [Angular CLI](https://angular.io/cli#installing-angular-cli) global Dies wird zur Generierung Angular-Komponenten sowie zum Aufbau und Bereitstellen der Angular-Anwendung über den **ng** -Befehl verwendet.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > Die Version von **@angular/cli** , die von diesem Projekt verwendet wird, ist **9.1.7**. Es wird empfohlen, die Angular-CLI-Versionen synchron zu halten.

3. Erstellen Sie eine neue `Header` Komponente, indem Sie den `ng generate component` Befehl &quot;Angular CLI&quot;aus dem `ui.frontend` Ordner ausführen.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Dadurch wird ein Skelett für die neue Angular Header-Komponente bei erstellt `ui.frontend/src/app/components/header`.

4. Öffnen Sie das `aem-guides-wknd-spa` Projekt in der IDE Ihrer Wahl. Navigieren Sie zum Ordner `ui.frontend/src/app/components/header`. 

   ![Pfad der Kopfzeilenkomponente in der IDE](assets/integrate-spa/header-component-path.png)

5. Öffnen Sie die Datei `header.component.html` und ersetzen Sie den Inhalt durch Folgendes:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Beachten Sie, dass dies statischen Inhalt anzeigt, sodass diese Angular-Komponente keine Anpassungen an der standardmäßig generierten Komponente erfordert `header.component.ts`.

6. Öffnen Sie die Datei **app.component.html** unter `ui.frontend/src/app/app.component.html`. hinzufügen `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Dazu gehört die `header` Komponente vor allem der Seiteninhalt.

7. Öffnen Sie ein neues Terminal, navigieren Sie zum `ui.frontend` Ordner und führen Sie den `npm run build` Befehl aus:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navigieren Sie zum Ordner `ui.apps`. Darunter sollten `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` Sie sehen, dass die kompilierten SPA-Dateien aus dem`ui.frontend/build` Ordner kopiert wurden.

   ![In ui.apps generierte Client-Bibliothek](assets/integrate-spa/compiled-spa-uiapps.png)

9. Kehren Sie zum Terminal zurück und navigieren Sie zum `ui.apps` Ordner. Führen Sie den folgenden Maven-Befehl aus:

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

   Dadurch wird das `ui.apps` Paket für eine lokale Instanz von AEM bereitgestellt.

10. Öffnen Sie eine Browser-Registerkarte und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Sie sollten nun den Inhalt der `Header` Komponente im SPA anzeigen.

   ![Erste Kopfzeilenimplementierung](assets/integrate-spa/initial-header-implementation.png)

   Die Schritte **7-9** werden automatisch ausgeführt, wenn ein Maven-Build aus dem Stammverzeichnis des Projekts (d.h. `mvn clean install -PautoInstallSinglePackage`) ausgelöst wird. Sie sollten nun die Grundlagen der Integration zwischen SPA und AEM clientseitigen Bibliotheken verstehen. Beachten Sie, dass Sie in AEM weiterhin `Text` Komponenten bearbeiten und hinzufügen können. Die `Header` Komponente kann jedoch nicht bearbeitet werden.

## Webpack Dev Server - Proxy der JSON-API {#proxy-json}

Wie in den vorherigen Übungen gezeigt, dauert die Ausführung eines Builds und die Synchronisierung der Client-Bibliothek mit einer lokalen Instanz von AEM einige Minuten. Dies ist für die Endprüfung annehmbar, aber nicht ideal für den Großteil der SPA-Entwicklung.

Ein [Webpack-Dev-Server](https://webpack.js.org/configuration/dev-server/) kann verwendet werden, um die SPA schnell zu entwickeln. Die SPA wird von einem von AEM erstellten JSON-Modell gesteuert. In dieser Übung wird der JSON-Inhalt einer laufenden Instanz von AEM in den vom **Angular-Projekt konfigurierten Entwicklungsserver** proximiert [](https://angular.io/guide/build).

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

   Die [Angular-App](https://angular.io/guide/build#proxying-to-a-backend-server) bietet einen einfachen Mechanismus zum Proxy von API-Anforderungen. Die in `context` angegebenen Muster werden über den lokalen AEM Schnellstart verteilt `localhost:4502`.

2. Öffnen Sie die Datei **index.html** unter `ui.frontend/src/index.html`. Dies ist die HTML-Stammdatei, die vom dev-Server verwendet wird.

   Beachten Sie, dass es einen Eintrag für `base href="/"`. Das [base-Tag](https://angular.io/guide/deployment#the-base-tag) ist entscheidend, damit die App relative URLs auflösen kann.

   ```html
   <base href="/">
   ```

3. Öffnen Sie ein Terminalfenster und navigieren Sie zum `ui.frontend` Ordner. Run the command `npm start`:

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

4. Öffnen Sie eine neue Browserregisterkarte (sofern noch nicht geöffnet) und navigieren Sie zu [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Webpack-Dev-Server - Proxy-JSON](assets/integrate-spa/webpack-dev-server-1.png)

   Sie sollten dieselben Inhalte wie in AEM sehen, jedoch keine der Authoring-Funktionen aktiviert haben.

5. Kehren Sie zur IDE zurück und erstellen Sie einen neuen Ordner mit dem Namen `img` at `ui.frontend/src/assets`.
6. Laden Sie das folgende WKND-Logo herunter und fügen Sie es dem `img` Ordner hinzu:

   ![WKND-Logo](./assets/integrate-spa/wknd-logo-dk.png)

7. Öffnen Sie **header.component.html** bei `ui.frontend/src/app/components/header/header.component.html` und fügen Sie das Logo ein:

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

8. Kehren Sie zum Browser zurück. Die Änderungen an der App sollten sofort angezeigt werden.

   ![Logo zur Kopfzeile hinzugefügt](assets/integrate-spa/added-logo-localhost.png)

   Sie können weiterhin Inhaltsaktualisierungen in **AEM** vornehmen und sie auf dem **Webpack-Dev-Server** anzeigen, da wir den Inhalt proxizieren. Beachten Sie, dass die Inhaltsänderungen nur auf dem **Webpack-Dev-Server** sichtbar sind.

9. Beenden Sie den lokalen Webserver mit `ctrl+c` im Terminal.

## Webpack Dev Server - JSON-API {#mock-json}

Ein weiterer Ansatz zur schnellen Entwicklung ist die Verwendung einer statischen JSON-Datei als JSON-Modell. Indem wir die JSON &quot;betrügen&quot;, entfernen wir die Abhängigkeit von einer lokalen AEM Instanz. Außerdem kann ein Front-End-Entwickler das JSON-Modell aktualisieren, um Funktionen zu testen und Änderungen an der JSON-API vorzunehmen, die später von einem Back-End-Entwickler implementiert werden.

Die anfängliche Einrichtung des JSON-Musters **erfordert eine lokale AEM Instanz**.

1. Navigieren Sie im Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Dies ist die von AEM exportierte JSON, die die Anwendung ausführt. Kopieren Sie die JSON-Ausgabe.

2. Kehren Sie zur IDE zurück, navigieren Sie zu `ui.frontend/src` und fügen Sie neue Ordner namens **mocks** und **json** hinzu, um der folgenden Ordnerstruktur zu entsprechen:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Erstellen Sie unten eine neue Datei mit dem Namen **en.model.json** `ui.frontend/public/mocks/json`. Fügen Sie hier die JSON-Ausgabe aus **Schritt 1** ein.

   ![JSON-Datei des Moodmodells](assets/integrate-spa/mock-model-json-created.png)

4. Erstellen Sie unten eine neue Datei **proxy.mock.conf.json** `ui.frontend`. Füllen Sie die Datei mit folgendem Inhalt:

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

   Diese Proxy-Konfiguration schreibt Anforderungen, die Beginn `/content/wknd-spa-angular/us` mit der entsprechenden statischen JSON-Datei enthalten, neu `/mocks/json` und stellt diese bereit. Beispiel:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Open the file **angular.json**. hinzufügen einer neuen **dev** -Konfiguration mit einem aktualisierten **assets** -Array, um auf den erstellten **Ordner &quot;mocks** &quot;zu verweisen.

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

   ![Aktualisierungsordner für angular JSON Dev Assets](assets/integrate-spa/dev-assets-update-folder.png)

   Durch das Erstellen einer dedizierten **dev** -Konfiguration wird sichergestellt, dass der Ordner &quot; **mocks** &quot;nur während der Entwicklung verwendet und nie in einem Produktionsaufbau AEM bereitgestellt wird.

6. Aktualisieren Sie in der Datei **angular.json** die **Konfiguration browserTarget** , um die neue **dev** -Konfiguration zu verwenden:

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

   ![Entwickleraktualisierung für Angular JSON-Build](assets/integrate-spa/angular-json-build-dev-update.png)

7. Öffnen Sie die Datei `ui.frontend/package.json` und fügen Sie einen neuen **Beginn:mock** -Befehl hinzu, um auf die Datei **proxy.mock.conf.json** zu verweisen.

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

   Durch Hinzufügen eines neuen Befehls ist es einfach, zwischen den Proxykonfigurationen zu wechseln.

8. Beenden Sie den **Webpack-Dev-Server**, wenn er gerade ausgeführt wird. Beginn des **Webpack-Dev-Servers** mithilfe des Skripts **Beginn:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navigieren Sie zu [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) , und Sie sollten dieselbe SPA sehen, aber der Inhalt wird jetzt aus der JSON- **Musterdatei** gezogen.

9. Nehmen Sie eine kleine Änderung an der zuvor erstellten Datei **en.model.json** vor. Der aktualisierte Inhalt sollte sofort im **Webpack-Dev-Server**&#x200B;übernommen werden.

   ![JSON-Aktualisierung für Modell](./assets/integrate-spa/webpack-mock-model.gif)

   Die Möglichkeit, das JSON-Modell zu manipulieren und die Auswirkungen auf eine Live-SPA zu sehen, kann einem Entwickler helfen, die JSON-Modell-API zu verstehen. Es ermöglicht auch die parallele Entwicklung von Front-End und Back-End.

## hinzufügen von Stilen mit Sass

Als Nächstes wird ein aktualisierter Stil zum Projekt hinzugefügt. In diesem Projekt wird [Sass](https://sass-lang.com/) -Unterstützung für einige nützliche Funktionen wie Variablen hinzugefügt.

1. Öffnen Sie ein Terminalfenster und beenden Sie den **Webpack-Dev-Server** , wenn dieser gestartet wird. Geben Sie von innerhalb des `ui.frontend` Ordners den folgenden Befehl ein, um die Angular-App zu aktualisieren, damit **.scss** -Dateien verarbeitet werden.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Dadurch wird die `angular.json` Datei mit einem neuen Eintrag am Ende der Datei aktualisiert:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installieren Sie `normalize-scss` die Installation, um die Stile über Browser hinweg zu normalisieren:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Kehren Sie zur IDE zurück und erstellen Sie unter `ui.frontend/src` Erstellen Sie einen neuen Ordner mit dem Namen `styles`.
4. Erstellen Sie eine neue Datei unter dem `ui.frontend/src/styles` Namen `_variables.scss` und füllen Sie sie mit den folgenden Variablen:

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

5. Benennen Sie die Erweiterung der Datei **styles.css** erneut `ui.frontend/src/styles.css` zu **styles.scss**. Ersetzen Sie den Inhalt durch Folgendes:

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

## Kopfzeilenstile aktualisieren

Anschließend fügen Sie der **Header** -Komponente mithilfe von Sass einige markenspezifische Stile hinzu.

1. Beginn des **Webpack-Dev-Servers** , um die Stile in Echtzeit zu aktualisieren:

   ```shell
   $ npm run start:mock
   ```

2. Unter `ui.frontend/src/app/components/header` Neuname **header.component.css** zu **header.component.scss**. Füllen Sie die Datei mit folgendem Inhalt:

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

3. Aktualisieren Sie **header.component.js** auf **header.component.scss**:

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

4. Kehren Sie zum Browser und zum **Webpack-Dev-Server** zurück:

   ![Styling Header - Webpack Dev-Server](assets/integrate-spa/styled-header.png)

   Sie sollten nun die aktualisierten Stile sehen, die der **Kopfzeilenkomponente** hinzugefügt wurden.

## Bereitstellen von SPA-Updates für AEM

Die an der **Kopfzeile** vorgenommenen Änderungen sind derzeit nur über den **Webpack-Dev-Server** sichtbar. Stellen Sie die aktualisierte SPA bereit, AEM die Änderungen anzuzeigen.

1. Beenden Sie den **Webpack-Dev-Server**.
2. Navigieren Sie zum Stammverzeichnis des Projekts `/aem-guides-wknd-spa` und stellen Sie das Projekt für AEM mithilfe von Maven bereit:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Die aktualisierte **Kopfzeile** mit angewendetem Logo und angewendeten Stilen sollte angezeigt werden:

   ![Aktualisierte Kopfzeile in AEM](assets/integrate-spa/final-header-component.png)

   Nachdem sich die aktualisierte SPA in AEM befindet, kann das Authoring fortgesetzt werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben die SPA aktualisiert und die Integration mit AEM! Sie kennen jetzt zwei verschiedene Ansätze zum Entwickeln der SPA mit der AEM JSON-Modell-API mit einem **Webpack-Dev-Server**.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `Angular/integrate-spa-solution`.

### Nächste Schritte {#next-steps}

[Zuordnen von SPA-Komponenten zu AEM Komponenten](map-components.md) - Erfahren Sie, wie Sie Angular-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM) Komponenten zuordnen. Mithilfe der Komponentenzuordnung können Autoren im AEM SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vornehmen, ähnlich wie beim herkömmlichen AEM Authoring.
