---
title: Zuordnen SPA Komponenten zu AEM Komponenten | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie Angular-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM) Komponenten zuordnen. Mithilfe der Komponentenzuordnung können Benutzer im AEM SPA Editor dynamische Aktualisierungen an SPA Komponenten vornehmen, ähnlich wie beim herkömmlichen AEM Authoring.
sub-product: Sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 2%

---


# Zuordnen SPA Komponenten zu AEM Komponenten {#map-components}

Erfahren Sie, wie Sie Angular-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM) Komponenten zuordnen. Mithilfe der Komponentenzuordnung können Benutzer im AEM SPA Editor dynamische Aktualisierungen an SPA Komponenten vornehmen, ähnlich wie beim herkömmlichen AEM Authoring.

Dieses Kapitel enthält einen tieferen Einstieg in die AEM JSON-Modell-API und wie der JSON-Inhalt, der von einer AEM Komponente offen gelegt wird, automatisch als Props in eine Angular-Komponente injiziert werden kann.

## Vorgabe

1. Erfahren Sie, wie Sie AEM Komponenten SPA Komponenten zuordnen.
2. Verstehen Sie den Unterschied zwischen den Komponenten **Container** und **Content**.
3. Erstellen Sie eine neue Angular-Komponente, die einer vorhandenen AEM zugeordnet ist.

## Was Sie erstellen werden

In diesem Kapitel wird untersucht, wie die bereitgestellte SPA der AEM `Text`Komponente zugeordnet wird. `Text` Es wird eine neue Komponente `Image` SPA erstellt, die in der SPA verwendet und in AEM verfasst werden kann. Standardmäßig werden die Funktionen der **Layout-Container**- und **Vorlagen-Editor**-Richtlinien auch verwendet, um eine Ansicht zu erstellen, die im Erscheinungsbild etwas abwechslungsreicher ist.

![Kapitelbeispiel für fertiges Authoring](./assets/map-components/final-page.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das `classic`-Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) oder lokal prüfen, indem Sie zur Verzweigung `Angular/map-components-solution` wechseln.

## Zuordnungsansatz

Das Grundkonzept besteht darin, eine SPA Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, serverseitig ausführen, Inhalte als Teil der JSON-Modell-API exportieren Der JSON-Inhalt wird vom SPA genutzt, wobei clientseitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA Komponenten und einer AEM Komponente erstellt.

![Überblick über die Zuordnung einer AEM Komponente zu einer Angular-Komponente auf hoher Ebene](./assets/map-components/high-level-approach.png)

*Überblick über die Zuordnung einer AEM Komponente zu einer Angular-Komponente auf hoher Ebene*

## Inspect der Textkomponente

Der [AEM Projektarchiv](https://github.com/adobe/aem-project-archetype) stellt eine `Text`-Komponente bereit, die der AEM [Textkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/text.html) zugeordnet wird. Dies ist ein Beispiel für eine Komponente **content**, in der *content* von AEM gerendert wird.

Sehen wir uns an, wie die Komponente funktioniert.

### Inspect-JSON-Modell

1. Bevor Sie in den SPA-Code springen, sollten Sie das JSON-Modell, das AEM bereitstellt, kennen. Navigieren Sie zur [Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) und Ansicht der Textkomponente. Die Core Component Library enthält Beispiele für alle AEM Core-Komponenten.
2. Wählen Sie die Registerkarte **JSON** für eines der folgenden Beispiele:

   ![Text-JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText` und `:type`.

   `:type` ist eine reservierte Eigenschaft, die die  `sling:resourceType` (oder den Pfad) der AEM Komponente Liste. Der Wert von `:type` wird verwendet, um die AEM Komponente der SPA zuzuordnen.

   `text` und  `richText` sind zusätzliche Eigenschaften, die der SPA Komponente ausgesetzt werden.

### Inspect der Textkomponente

1. Öffnen Sie ein neues Terminal und navigieren Sie zum Ordner `ui.frontend` innerhalb des Projekts. Führen Sie `npm install` und dann `npm start` aus, um den **webpack dev server** Beginn:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Das `ui.frontend`-Modul ist derzeit so eingerichtet, dass das [JSON-Modell](./integrate-spa.md#mock-json) verwendet wird.

2. Sie sollten ein neues Browserfenster mit [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) öffnen.

   ![Webpack-Dev-Server mit Musterinhalt](assets/map-components/initial-start.png)

3. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND SPA. Erweitern Sie das Modul `ui.frontend` und öffnen Sie die Datei **text.component.ts** unter `ui.frontend/src/app/components/text/text.component.ts`:

   ![Text.js Angular Component Source Code](assets/map-components/vscode-ide-text-js.png)

4. Der erste zu prüfende Bereich ist der `class TextComponent` in ~Zeile 35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   [@Input()](https://angular.io/api/core/Input) decorator wird verwendet, um Felder zu deklarieren, deren Werte über das zugeordnete JSON-Objekt festgelegt werden, das zuvor überprüft wurde.

   `@HostBinding('innerHtml') get content()` ist eine Methode, die den verfassten Textinhalt aus dem Wert  `this.text`von verfügbar macht. Wenn der Inhalt Rich Text (bestimmt durch das `this.richText`-Flag) ist, wird die integrierte Sicherheit von Angular umgangen. Angular&#39;s [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) wird verwendet, um die Rohdaten-HTML zu &quot;scrubben&quot; und um Site-übergreifende Skriptverwundbarkeiten zu verhindern. Die Methode wird mit dem Dekorator [@HostBinding](https://angular.io/api/core/HostBinding) an die Eigenschaft `innerHtml` gebunden.

5. Überprüfen Sie anschließend `TextEditConfig` in ~Zeile 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Der obige Code bestimmt, wann der Platzhalter in der Umgebung AEM Autors wiedergegeben wird. Wenn die `isEmpty`-Methode **true** zurückgibt, wird der Platzhalter gerendert.

6. Sehen Sie sich schließlich den `MapTo`-Aufruf unter ~Zeile 53 an:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapTois wird vom AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`) bereitgestellt. Der Pfad `wknd-spa-angular/components/text` stellt die `sling:resourceType` der AEM Komponente dar. Dieser Pfad wird mit dem `:type` übereinstimmen, das vom JSON-Modell, das zuvor beobachtet wurde, bereitgestellt wird. **** MapToparses übergibt die JSON-Modellantwort und die richtigen Werte an die  `@Input()` Variablen der SPA Komponente.

   Die AEM `Text`-Komponentendefinition finden Sie unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimentieren Sie, indem Sie die Datei **en.model.json** unter `ui.frontend/src/mocks/json/en.model.json` ändern.

   Aktualisieren Sie in ~Zeile 62 den ersten `Text`-Wert, um die Tags **`H1`** und **`u`** zu verwenden:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Kehren Sie zum Browser zurück, um die vom **webpack dev server** bereitgestellten Effekte anzuzeigen:

   ![Aktualisiertes Textmodell](assets/map-components/updated-text-model.png)

   Versuchen Sie, die `richText`-Eigenschaft zwischen **true** / **false** umzuschalten, um die Renderlogik in Aktion zu sehen.

8. Inspect **text.component.html** bei `ui.frontend/src/app/components/text/text.component.html`.

   Diese Datei ist leer, da der gesamte Inhalt der Komponente durch die `innerHTML`-Eigenschaft festgelegt wird.

9. Inspect Sie **app.module.ts** bei `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   Die **TextComponent** ist nicht explizit enthalten, sondern dynamisch über **AEMResponsiveGridComponent**, bereitgestellt vom AEM SPA Editor JS SDK. Daher muss im **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components)-Array aufgeführt werden.

## Erstellen der Bildkomponente

Erstellen Sie anschließend eine Angular-Komponente `Image`, die der AEM [Image-Komponente](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/image.html) zugeordnet wird. Die Komponente `Image` ist ein weiteres Beispiel für eine Komponente **content**.

### Inspect the JSON

Prüfen Sie vor dem Aufrufen des SPA-Codes das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Core Component Library](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON-Komponente &quot;Bildkern&quot;](./assets/map-components/image-json.png)

   Die Komponenten `src`, `alt` und `title` werden mit den Eigenschaften `Image` SPA gefüllt.

   >[!NOTE]
   >
   > Es sind andere Bildeigenschaften verfügbar (`lazyEnabled`, `widths`), mit denen Entwickler eine adaptive und verzögertes Laden-Komponente erstellen können. Die in diesem Lernprogramm erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

2. Kehren Sie zu Ihrer IDE zurück und öffnen Sie die `en.model.json` bei `ui.frontend/src/mocks/json/en.model.json`. Da dies eine netto-neue Komponente für unser Projekt ist, müssen wir das Bild JSON &quot;spotten&quot;.

   Fügen Sie in ~Zeile 70 einen JSON-Eintrag für das `image`-Modell hinzu (vergessen Sie nicht das nachfolgende Komma `,` nach dem zweiten `text_386303036`) und aktualisieren Sie das `:itemsOrder`-Array.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   Das Projekt enthält ein Beispielbild unter `/mock-content/adobestock-140634652.jpeg`, das mit dem **webpack dev server** verwendet wird.

   Sie können die vollständige [en.model.json hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json) Ansicht durchführen.

3. hinzufügen ein von der Komponente anzuzeigendes Lagerfoto.

   Erstellen Sie einen neuen Ordner mit dem Namen **images** unter `ui.frontend/src/mocks`. Laden Sie [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) herunter und platzieren Sie es im neu erstellten Ordner **images**. Sie können Ihr eigenes Bild bei Bedarf verwenden.

### Komponente &quot;Bild&quot;implementieren

1. Beenden Sie den **webpack dev server**, falls gestartet.
2. Erstellen Sie eine neue Image-Komponente, indem Sie den Befehl für die Angular-CLI `ng generate component` aus dem Ordner `ui.frontend` ausführen:

   ```shell
   $ ng generate component components/image
   ```

3. Öffnen Sie in der IDE **image.component.ts** bei `ui.frontend/src/app/components/image/image.component.ts` und aktualisieren Sie wie folgt:

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` ist die Konfiguration, um zu bestimmen, ob der Autor-Platzhalter in AEM wiedergegeben werden soll, basierend darauf, ob die  `src` Eigenschaft gefüllt ist.

   `@Input()` von  `src`,  `alt`und  `title` sind die Eigenschaften, die von der JSON-API zugeordnet werden.

   `hasImage()` ist eine Methode, die bestimmt, ob das Bild wiedergegeben werden soll.

   `MapTo` ordnet die SPA Komponente der AEM Komponente zu, die sich unter  `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image` befindet.

4. Öffnen Sie **image.component.html** und aktualisieren Sie es wie folgt:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Dadurch wird das Element `<img>` gerendert, wenn `hasImage` **true** zurückgibt.

5. Öffnen Sie **image.component.scss** und aktualisieren Sie es wie folgt:

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > Die `:host-context`-Regel ist **kritisch**, damit der Platzhalter für den AEM SPA korrekt funktioniert. Alle SPA Komponenten, die im AEM Seiteneditor erstellt werden sollen, benötigen diese Regel mindestens.

6. Öffnen Sie `app.module.ts` und fügen Sie `ImageComponent` zum `entryComponents`-Array hinzu:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Wie bei `TextComponent` wird `ImageComponent` dynamisch geladen und muss im `entryComponents`-Array enthalten sein.

7. Beginn Sie den **webpack dev server**, um das `ImageComponent` render anzuzeigen.

   ```shell
   $ npm run start:mock
   ```

   ![Bild zum Modell hinzugefügt](assets/map-components/image-added-mock.png)

   *Dem SPA hinzugefügtes Bild*

   >[!NOTE]
   >
   > **Bonusherausforderung**: Implementieren Sie eine neue Methode, um den Wert von  `title` als Beschriftung unter dem Bild anzuzeigen.

## Richtlinien in AEM aktualisieren

Die Komponente `ImageComponent` ist nur im **webpack dev server** sichtbar. Stellen Sie anschließend die aktualisierte SPA bereit, um die Vorlagenrichtlinien zu AEM und zu aktualisieren.

1. Beenden Sie den **webpack dev server** und stellen Sie die Änderungen an AEM unter Verwendung Ihrer Maven-Fähigkeiten aus dem **root** des Projekts bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie im Bildschirm &quot;AEM Beginn&quot;zu **[!UICONTROL Tools]** > **[!UICONTROL Vorlagen]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Wählen Sie die **SPA Seite** aus und bearbeiten Sie sie:

   ![Bearbeiten SPA Seitenvorlage](assets/map-components/edit-spa-page-template.png)

3. Wählen Sie den Container **Layout** und klicken Sie auf das Symbol **policy**, um die Richtlinie zu bearbeiten:

   ![Container-Richtlinie](./assets/map-components/layout-container-policy.png)

4. Aktivieren Sie unter **Zulässige Komponenten** > **WKND SPA Angular - Content** > die Komponente **Bild**:

   ![Bildkomponente ausgewählt](assets/map-components/check-image-component.png)

   Wählen Sie unter **Standardkomponenten** > **Hinzufügen Zuordnung** die Komponente **Bild - WKND SPA Angular - Content** aus:

   ![Standardkomponenten festlegen](assets/map-components/default-components.png)

   Geben Sie einen **mime-Typ** von `image/*` ein.

   Klicken Sie auf **Fertig**, um die Richtlinienaktualisierungen zu speichern.

5. Klicken Sie im **Layout-Container** auf das Symbol **policy** für die Komponente **text**:

   ![Richtliniensymbol für Textkomponenten](./assets/map-components/edit-text-policy.png)

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Aktivieren Sie unter **Plugins** > **Formatierung** > alle Kontrollkästchen, um weitere Formatierungsoptionen zu aktivieren:

   ![RTE-Formatierung aktivieren](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plugins** > **Absatzstile** > das Kontrollkästchen **Absatzstile** aktivieren:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig**, um die Richtlinienaktualisierung zu speichern.

6. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Sie sollten auch die Komponente `Text` bearbeiten und im Modus **Vollbild** weitere Absatzstile hinzufügen können.

   ![Vollbild-Rich-Text-Bearbeitung](assets/map-components/full-screen-rte.png)

7. Sie sollten auch ein Bild aus der **Asset Finder** ziehen und ablegen können:

   ![Bild ziehen und ablegen](./assets/map-components/drag-drop-image.gif)

8. hinzufügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) oder installieren Sie die fertige Code-Basis für die Standard [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest). Die [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect Layout Container

Der **Layout-Container** wird automatisch vom AEM SPA Editor SDK unterstützt. Der **Layout-Container** ist, wie durch den Namen angegeben, eine **Container**-Komponente. Container-Komponenten sind Komponenten, die JSON-Strukturen akzeptieren, die *andere*-Komponenten darstellen und diese dynamisch instanziieren.

Lassen Sie uns den Layout-Container näher untersuchen.

1. Öffnen Sie in der IDE **responsive-grid.component.ts** bei `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent` wird als Teil des AEM SPA Editor SDK implementiert und über `import-components` in das Projekt aufgenommen.

2. Navigieren Sie in einem Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON-Modell-API - Responsive Grid](./assets/map-components/responsive-grid-modeljson.png)

   Die Komponente **Layout Container** hat die Eigenschaft `sling:resourceType` von `wcm/foundation/components/responsivegrid` und wird vom SPA-Editor mit der Eigenschaft `:type` erkannt, genau wie die Komponenten `Text` und `Image`.

   Die gleichen Funktionen zum Neuskalieren einer Komponente mit [Layoutmodus](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sind im SPA Editor verfügbar.

3. Kehren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) zurück. hinzufügen Sie weitere **Bild**-Komponenten und versuchen Sie, sie mit der Option **Layout** neu zu skalieren:

   ![Bildgröße im Layoutmodus ändern](./assets/map-components/responsive-grid-layout-change.gif)

4. Öffnen Sie das JSON-Modell [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) erneut und beobachten Sie das `columnClassNames` als Teil des JSON:

   ![Klassennamen in der Cloud](./assets/map-components/responsive-grid-classnames.png)

   Der Klassenname `aem-GridColumn--default--4` gibt an, dass die Komponente basierend auf einem 12-Spalten-Raster 4 Spalten breit sein sollte. Weitere Informationen zum interaktiven Raster finden Sie hier.[](https://adobe-marketing-cloud.github.io/aem-responsivegrid/)

5. Kehren Sie zur IDE zurück und im Modul `ui.apps` ist eine clientseitige Bibliothek definiert, die unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid` definiert ist. Öffnen Sie die Datei `less/grid.less`.

   Diese Datei legt die Haltepunkte (`default`, `tablet` und `phone`) fest, die vom **Layout-Container** verwendet werden. Diese Datei soll nach Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `650px` eingestellt.

6. Sie sollten die reaktionsfähigen Funktionen und die aktualisierten Rich-Text-Richtlinien der Komponente `Text` verwenden können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitelbeispiel für fertiges Authoring](assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie Sie SPA Komponenten zu AEM Komponenten zuordnen und Sie haben eine neue `Image`-Komponente implementiert. Sie haben auch die Möglichkeit, die reaktionsfähigen Funktionen des **Layout-Containers** zu erkunden.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) oder lokal prüfen, indem Sie zur Verzweigung `Angular/map-components-solution` wechseln.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md)  - Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnen zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des Angular-Routers implementiert und einer vorhandenen Header-Komponente hinzugefügt.

## Bonus - Beständige Konfigurationen zur Quellcodeverwaltung {#bonus}

In vielen Fällen, besonders zu Beginn eines AEM Projekts, ist es nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quellcodeverwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen den Umgebung sicherstellen. Sobald ein Projekt eine gewisse Reife erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Stromverbrauchern überlassen werden.

Die nächsten Schritte werden mit der Code-IDE von Visual Studio und [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) durchgeführt. Sie können jedoch jedes Tool und jede IDE verwenden, die Sie für **ull**- oder **import**-Inhalte aus einer lokalen Instanz von AEM konfiguriert haben.

1. Stellen Sie in der Code-IDE von Visual Studio sicher, dass **VSCode AEM Sync** über die Marketplace-Erweiterung installiert ist:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Erweitern Sie das Modul **ui.content** im Project Explorer und navigieren Sie zu `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Klicken Sie mit der rechten** Maustaste auf den  `templates` Ordner und wählen Sie  **Aus AEM Server** importieren:

   ![VSCode-Importvorlage](assets/map-components/import-aem-servervscode.png)

4. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den Ordner **policies** unter `/conf/wknd-spa-angular/settings/wcm/templates/policies` aus.

5. Inspect Sie die Datei `filter.xml` unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   Die `filter.xml`-Datei ist dafür verantwortlich, die Pfade von Knoten zu identifizieren, die mit dem Paket installiert werden. Beachten Sie, dass `mode="merge"` auf jedem der Filter angezeigt wird, dass vorhandene Inhalte nicht geändert werden, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsersteller diese Pfade möglicherweise aktualisieren, ist es wichtig, dass bei einer Codebereitstellung **kein** Inhalt überschrieben wird. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html).

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml`, um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.
