---
title: Zuordnen SPA Komponenten zu AEM Komponenten | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie Angular-Komponenten Adobe Experience Manager-Komponenten (AEM) mit dem AEM SPA Editor JS SDK zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2380'
ht-degree: 1%

---

# Zuordnen SPA Komponenten zu AEM Komponenten {#map-components}

Erfahren Sie, wie Sie Angular-Komponenten Adobe Experience Manager-Komponenten (AEM) mit dem AEM SPA Editor JS SDK zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.

In diesem Kapitel werden die AEM JSON-Modell-API und die Möglichkeiten erläutert, wie der von einer AEM-Komponente angezeigte JSON-Inhalt automatisch als Props in eine Angular-Komponente eingefügt werden kann.

## Ziele

1. Erfahren Sie, wie Sie AEM Komponenten SPA Komponenten zuordnen.
2. Machen Sie sich mit dem Unterschied zwischen den Komponenten **Container** und **Inhalt** vertraut.
3. Erstellen Sie eine neue Angular-Komponente, die einer vorhandenen AEM zugeordnet ist.

## Was Sie erstellen werden

In diesem Kapitel wird untersucht, wie die bereitgestellte SPA `Text` der AEM `Text`Komponente zugeordnet wird. Es wird eine neue SPA `Image` erstellt, die in der SPA verwendet und in AEM erstellt werden kann. Die vordefinierten Funktionen der **Layout-Container**- und **Vorlagen-Editor**-Richtlinien werden auch verwendet, um eine Ansicht zu erstellen, die in ihrer Darstellung etwas abwechslungsreicher ist.

![Kapitelbeispiel für die endgültige Bearbeitung](./assets/map-components/final-page.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/map-components-solution` wechseln.

## Mapping-Ansatz

Das grundlegende Konzept besteht darin, eine SPA Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, Server-seitig ausführen, Inhalte als Teil der JSON-Modell-API exportieren. Der JSON-Inhalt wird vom SPA verwendet, der clientseitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA Komponenten und einer AEM Komponente erstellt.

![Allgemeine Übersicht über die Zuordnung einer AEM-Komponente zu einer Angular-Komponente](./assets/map-components/high-level-approach.png)

*Allgemeine Übersicht über die Zuordnung einer AEM-Komponente zu einer Angular-Komponente*

## Inspect der Textkomponente

Der [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) stellt eine `Text`-Komponente bereit, die der AEM [Textkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) zugeordnet ist. Dies ist ein Beispiel für eine **content** -Komponente, da sie *content* von AEM rendert.

Sehen wir uns an, wie die Komponente funktioniert.

### Inspect des JSON-Modells

1. Bevor Sie in den SPA-Code springen, müssen Sie das von AEM bereitgestellte JSON-Modell verstehen. Navigieren Sie zur [Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) und zeigen Sie die Seite für die Textkomponente an. Die Kernkomponentenbibliothek enthält Beispiele für alle AEM Kernkomponenten.
2. Wählen Sie die Registerkarte **JSON** für eines der Beispiele aus:

   ![Text-JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText` und `:type`.

   `:type` ist eine reservierte Eigenschaft, die die  `sling:resourceType` (oder den Pfad) der AEM Komponente auflistet. Der Wert `:type` wird verwendet, um die AEM Komponente der SPA zuzuordnen.

   `text` und  `richText` sind zusätzliche Eigenschaften, die der SPA-Komponente angezeigt werden.

### Inspect der Textkomponente

1. Öffnen Sie ein neues Terminal und navigieren Sie zum Ordner `ui.frontend` im Projekt. Führen Sie `npm install` und dann `npm start` aus, um den **webpack-Dev-Server** zu starten:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Das Modul `ui.frontend` ist derzeit so eingerichtet, dass das JSON-Modell [nachahmen](./integrate-spa.md#mock-json) verwendet wird.

2. Sie sollten ein neues Browserfenster sehen, das geöffnet ist für [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Webpack-Dev-Server mit nachgeahmten Inhalten](assets/map-components/initial-start.png)

3. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND-SPA. Erweitern Sie das Modul `ui.frontend` und öffnen Sie die Datei **text.component.ts** unter `ui.frontend/src/app/components/text/text.component.ts`:

   ![Angular-Komponenten-Quellcode von Text.js](assets/map-components/vscode-ide-text-js.png)

4. Der erste zu prüfende Bereich ist der `class TextComponent` bei ~line 35:

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

   `@HostBinding('innerHtml') get content()` ist eine Methode, die den erstellten Textinhalt aus dem Wert von  `this.text` verfügbar macht. Wenn es sich bei dem Inhalt um Rich-Text handelt (bestimmt durch das `this.richText`-Flag), wird die integrierte Sicherheitsfunktion von Angular umgangen. Angular [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) wird verwendet, um den rohen HTML-Code zu scrubben und Sicherheitslücken beim Cross Site Scripting zu vermeiden. Die Methode wird mit dem Dekorator [@HostBinding](https://angular.io/api/core/HostBinding) an die Eigenschaft `innerHtml` gebunden.

5. Überprüfen Sie dann `TextEditConfig` in Zeile 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Der obige Code ist für die Bestimmung verantwortlich, wann der Platzhalter in der AEM Autorenumgebung wiedergegeben werden soll. Wenn die `isEmpty`-Methode **true** zurückgibt, wird der Platzhalter gerendert.

6. Sehen Sie sich schließlich den `MapTo`-Aufruf an unter ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **** MapTois wird vom AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`) bereitgestellt. Der Pfad `wknd-spa-angular/components/text` stellt die `sling:resourceType` der AEM Komponente dar. Dieser Pfad wird mit dem `:type` übereinstimmen, der vom zuvor beobachteten JSON-Modell bereitgestellt wird. **** MapToparses die JSON-Modellantwort und übergibt die richtigen Werte an die  `@Input()` Variablen der SPA Komponente.

   Die AEM `Text`-Komponentendefinition finden Sie unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimentieren Sie, indem Sie die **en.model.json**-Datei unter `ui.frontend/src/mocks/json/en.model.json` ändern.

   Aktualisieren Sie in ~line 62 den ersten `Text` -Wert, um die Tags **`H1`** und **`u`** zu verwenden:

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

8. Inspect **text.component.html** unter `ui.frontend/src/app/components/text/text.component.html`.

   Diese Datei ist leer, da der gesamte Inhalt der Komponente durch die Eigenschaft `innerHTML` festgelegt wird.

9. Inspect **app.module.ts** unter `ui.frontend/src/app/app.module.ts`.

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

   Die **TextComponent** ist nicht explizit enthalten, sondern dynamisch über **AEMResponsiveGridComponent**, bereitgestellt vom AEM SPA Editor JS SDK. Daher muss im Array **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components) aufgeführt werden.

## Erstellen der Bildkomponente

Erstellen Sie anschließend eine `Image`-Angular-Komponente, die der AEM [Bildkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) zugeordnet ist. Die Komponente `Image` ist ein weiteres Beispiel für eine **content** -Komponente.

### Inspect the JSON

Bevor Sie in den SPA-Code springen, überprüfen Sie das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON der Bild-Kernkomponente](./assets/map-components/image-json.png)

   Die Eigenschaften `src`, `alt` und `title` werden zum Ausfüllen der SPA `Image`-Komponente verwendet.

   >[!NOTE]
   >
   > Es gibt weitere angezeigte Bildeigenschaften (`lazyEnabled`, `widths`), die es einem Entwickler ermöglichen, eine adaptive und verzögerte Ladekomponente zu erstellen. Die in diesem Tutorial erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

2. Kehren Sie zu Ihrer IDE zurück und öffnen Sie `en.model.json` unter `ui.frontend/src/mocks/json/en.model.json`. Da dies eine netto-neue Komponente für unser Projekt ist, müssen wir die Bild-JSON &quot;nachahmen&quot;.

   Fügen Sie in ~line 70 einen JSON-Eintrag für das Modell `image` hinzu (vergessen Sie nicht das nachfolgende Komma `,` nach dem zweiten `text_386303036`) und aktualisieren Sie das Array `:itemsOrder`.

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

   Das Projekt enthält ein Beispielbild unter `/mock-content/adobestock-140634652.jpeg` , das mit dem **webpack-Dev-Server** verwendet wird.

   Den vollständigen [en.model.json finden Sie hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Fügen Sie ein Lagerbild hinzu, das von der Komponente angezeigt werden soll.

   Erstellen Sie einen neuen Ordner mit dem Namen **images** unter `ui.frontend/src/mocks`. Laden Sie [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) herunter und legen Sie es im neu erstellten Ordner **images** ab. Sie können bei Bedarf Ihr eigenes Bild verwenden.

### Implementieren der Bildkomponente

1. Beenden Sie den **webpack-Dev-Server**, falls er gestartet wurde.
2. Erstellen Sie eine neue Bildkomponente, indem Sie den Befehl Angular CLI `ng generate component` aus dem Ordner `ui.frontend` ausführen:

   ```shell
   $ ng generate component components/image
   ```

3. Öffnen Sie in der IDE **image.component.ts** unter `ui.frontend/src/app/components/image/image.component.ts` und aktualisieren Sie wie folgt:

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

   `ImageEditConfig` ist die Konfiguration, um zu bestimmen, ob der Autoren-Platzhalter in AEM wiedergegeben werden soll, basierend darauf, ob die  `src` Eigenschaft gefüllt ist.

   `@Input()` von  `src`,  `alt` und  `title` sind die Eigenschaften, die von der JSON-API zugeordnet sind.

   `hasImage()` ist eine Methode, die bestimmt, ob das Bild gerendert werden soll.

   `MapTo` ordnet die SPA Komponente der AEM Komponente zu, die sich unter  `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`befindet.

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
   > Die `:host-context`-Regel ist **kritisch**, damit der Platzhalter für den AEM SPA-Editor ordnungsgemäß funktioniert. Alle SPA Komponenten, die im AEM Seiteneditor erstellt werden sollen, benötigen diese Regel mindestens.

6. Öffnen Sie `app.module.ts` und fügen Sie `ImageComponent` zum `entryComponents`-Array hinzu:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Wie `TextComponent` wird `ImageComponent` dynamisch geladen und muss im `entryComponents` -Array enthalten sein.

7. Starten Sie den **webpack-Dev-Server**, um den `ImageComponent`-Renderer anzuzeigen.

   ```shell
   $ npm run start:mock
   ```

   ![Bild wurde zur Nachahmung hinzugefügt](assets/map-components/image-added-mock.png)

   *Bild SPA hinzugefügt*

   >[!NOTE]
   >
   > **Bonusaufgabe**: Implementieren Sie eine neue Methode, um den Wert von  `title` als Beschriftung unter dem Bild anzuzeigen.

## Richtlinien in AEM aktualisieren

Die Komponente `ImageComponent` ist nur im **webpack-Dev-Server** sichtbar. Stellen Sie anschließend die aktualisierte SPA bereit, um die Vorlagenrichtlinien zu AEM und zu aktualisieren.

1. Beenden Sie den **webpack-Dev-Server** und stellen Sie über das **root** des Projekts die Änderungen AEM mithilfe Ihrer Maven-Fähigkeiten bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie im Bildschirm AEM Start zu **[!UICONTROL Tools]** > **[!UICONTROL Vorlagen]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Wählen Sie die **SPA Seite** aus und bearbeiten Sie sie:

   ![Bearbeiten SPA Seitenvorlage](assets/map-components/edit-spa-page-template.png)

3. Wählen Sie den **Layout-Container** aus und klicken Sie auf das Symbol **policy** , um die Richtlinie zu bearbeiten:

   ![Layout-Container-Richtlinie](./assets/map-components/layout-container-policy.png)

4. Überprüfen Sie unter **Zulässige Komponenten** > **WKND SPA Angular - Content** die Komponente **Bild**:

   ![Bildkomponente ausgewählt](assets/map-components/check-image-component.png)

   Wählen Sie unter **Standardkomponenten** > **Mapping** hinzufügen und die Komponente **Bild - WKND SPA Angular - Content** aus:

   ![Standardkomponenten festlegen](assets/map-components/default-components.png)

   Geben Sie einen **MIME-Typ** von `image/*` ein.

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierungen zu speichern.

5. Klicken Sie im **Layout-Container** auf das Symbol **policy** für die Komponente **Text**:

   ![Symbol für Textkomponentenrichtlinie](./assets/map-components/edit-text-policy.png)

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Aktivieren Sie unter **Plugins** > **Formatierung** > alle Kästchen, um zusätzliche Formatierungsoptionen zu aktivieren:

   ![Aktivieren der RTE-Formatierung](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plugins** > **Absatzstile** das Kontrollkästchen zu **Absatzstile aktivieren**:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierung zu speichern.

6. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Sie sollten auch in der Lage sein, die Komponente `Text` zu bearbeiten und zusätzliche Absatzstile im Modus **Vollbild** hinzuzufügen.

   ![Rich-Text-Bearbeitung im Vollbildmodus](assets/map-components/full-screen-rte.png)

7. Sie sollten auch ein Bild aus dem **Asset Finder** ziehen und ablegen können:

   ![Bild ziehen und ablegen](./assets/map-components/drag-drop-image.gif)

8. Fügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) hinzu oder installieren Sie die fertige Codebasis für die standardmäßige [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest). Die Referenz-Website [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect des Layout-Containers

Unterstützung für den **Layout-Container** wird automatisch vom AEM SPA Editor SDK bereitgestellt. Der **Layout-Container**, wie durch den Namen angegeben, ist eine **Container**-Komponente. Container-Komponenten sind Komponenten, die JSON-Strukturen akzeptieren, die *andere*-Komponenten darstellen und sie dynamisch instanziieren.

Überprüfen wir nun den Layout-Container weiter.

1. Öffnen Sie in der IDE **responsive-grid.component.ts** unter `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   `AEMResponsiveGridComponent` wird als Teil des AEM SPA Editor SDK implementiert und über `import-components` in das Projekt aufgenommen.

2. Navigieren Sie in einem Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON-Modell-API - Responsives Raster](./assets/map-components/responsive-grid-modeljson.png)

   Die Komponente **Layout-Container** hat den Wert `sling:resourceType` von `wcm/foundation/components/responsivegrid` und wird vom SPA Editor mit der Eigenschaft `:type` erkannt, genau wie die Komponenten `Text` und `Image`.

   Die gleichen Funktionen zum Neuskalieren einer Komponente mit [Layout-Modus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sind auch im SPA Editor verfügbar.

3. Kehren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) zurück. Fügen Sie weitere **Bild**-Komponenten hinzu und versuchen Sie, sie mithilfe der Option **Layout** neu zu skalieren:

   ![Bildgröße im Layout-Modus ändern](./assets/map-components/responsive-grid-layout-change.gif)

4. Öffnen Sie das JSON-Modell [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) erneut und beobachten Sie `columnClassNames` als Teil des JSON-Codes:

   ![Cloud-Klassennamen](./assets/map-components/responsive-grid-classnames.png)

   Der Klassenname `aem-GridColumn--default--4` gibt an, dass die Komponente basierend auf einem 12-Spalten-Raster 4 Spalten breit sein sollte. Weitere Informationen zum responsiven Raster [finden Sie hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Kehren Sie zur IDE zurück und im Modul `ui.apps` ist eine clientseitige Bibliothek definiert, die unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid` definiert ist. Öffnen Sie die Datei `less/grid.less`.

   Diese Datei bestimmt die Haltepunkte (`default`, `tablet` und `phone`), die vom **Layout-Container** verwendet werden. Diese Datei soll gemäß den Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `650px` festgelegt.

6. Sie sollten die responsiven Funktionen und die aktualisierten Rich-Text-Richtlinien der Komponente `Text` verwenden können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitelbeispiel für die endgültige Bearbeitung](assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie SPA Komponenten AEM Komponenten zuordnen und eine neue `Image` -Komponente implementiert haben. Sie haben auch die Möglichkeit erhalten, die responsiven Funktionen des **Layout-Containers** zu erkunden.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/map-components-solution` wechseln.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md)  - Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnung zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des Angular-Routers implementiert und einer vorhandenen Kopfzeilenkomponente hinzugefügt.

## Bonus - Beibehalten von Konfigurationen zur Quell-Code-Verwaltung {#bonus}

In vielen Fällen ist es insbesondere zu Beginn eines AEM-Projekts nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quell-Code-Verwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhalt und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen Umgebungen sichergestellt wird. Sobald ein Projekt einen gewissen Reifegrad erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Power-Benutzern übertragen werden.

Die nächsten Schritte werden mit der Visual Studio Code-IDE und [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) durchgeführt. Sie können jedoch jedes Tool und jede IDE verwenden, die Sie für **Pull** oder **Import**-Inhalte von einer lokalen Instanz von AEM konfiguriert haben.

1. Stellen Sie in der Visual Studio Code-IDE sicher, dass **VSCode AEM Sync** über die Marketplace-Erweiterung installiert ist:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Erweitern Sie das Modul **ui.content** im Projekt-Explorer und navigieren Sie zu `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Klicken Sie mit der rechten Maustaste** auf den  `templates` Ordner und wählen Sie  **Import von AEM Server** aus:

   ![VSCode-Importvorlage](assets/map-components/import-aem-servervscode.png)

4. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den Ordner **policies** unter `/conf/wknd-spa-angular/settings/wcm/policies` aus.

5. Inspect die Datei `filter.xml` unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Die `filter.xml`-Datei ist dafür verantwortlich, die Pfade von Knoten zu identifizieren, die mit dem Paket installiert werden. Beachten Sie die `mode="merge"` in jedem Filter, die darauf hinweisen, dass der vorhandene Inhalt nicht geändert wird, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsautoren diese Pfade möglicherweise aktualisieren, ist es wichtig, dass bei einer Codebereitstellung **nicht** Inhalte überschrieben werden. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html) .

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml` , um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.
