---
title: Zuordnen von SPA-Komponenten zu AEM-Komponenten | Erste Schritte mit AEM-SPA-Editor und Angular
description: Erfahren Sie, wie Sie Angular-Komponenten Adobe Experience Manager (AEM)-Komponenten mit dem AEM SPA Editor JS SDK zuordnen. Die Komponentenzuordnung ermöglicht es Benutzenden, im AEM-SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM-Authoring.
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: ht
source-wordcount: '2372'
ht-degree: 100%

---

# Zuordnen von SPA-Komponenten zu AEM-Komponenten {#map-components}

Erfahren Sie, wie Sie Angular-Komponenten Adobe Experience Manager (AEM)-Komponenten mit dem AEM SPA Editor JS SDK zuordnen. Die Komponentenzuordnung ermöglicht es Benutzenden, im AEM-SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM-Authoring.

In diesem Kapitel werden die AEM JSON-Modell-API und die Möglichkeiten erläutert, wie der von einer AEM-Komponente angezeigte JSON-Inhalt automatisch als Props in eine Angular-Komponente eingefügt werden kann.

## Ziel

1. Erfahren Sie, wie Sie AEM-Komponenten SPA-Komponenten zuordnen.
2. Verstehen Sie den Unterschied zwischen **Container-Komponenten** und **Inhaltskomponenten**.
3. Erstellen Sie eine neue Angular-Komponente, die einer vorhandenen AEM zugeordnet ist.

## Was Sie erstellen werden

In diesem Kapitel wird untersucht, wie die `Text`-SPA-Komponente der AEM-`Text`-Komponente zugeordnet wird. Eine neue `Image`-SPA-Komponente wird erstellt, die in der SPA verwendet und in AEM verfasst werden kann. Vorkonfigurierte Funktionen der Richtlinien zum **Layout-Container** und zum **Vorlagen-Editor** werden auch verwendet, um eine Ansicht zu erstellen, die etwas abwechslungsreicher erscheint.

![Kapitel Beispiel endgültige Erstellung](./assets/map-components/final-page.png)

## Voraussetzungen

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Abrufen des Codes

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Stellen Sie die Code-Basis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ansehen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/map-components-solution` wechseln.

## Zuordnungsansatz

Das grundlegende Konzept besteht darin, eine SPA-Komponente einer AEM-Komponente zuzuordnen. Server-seitig ausgeführte AEM-Komponenten exportieren Inhalte als Teil der JSON-Modell-API. Der JSON-Inhalt wird von der SPA verwendet, die Client-seitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA-Komponenten und einer AEM-Komponente erstellt.

![Allgemeine Übersicht über die Zuordnung einer AEM-Komponente zu einer Angular-Komponente](./assets/map-components/high-level-approach.png)

*Allgemeine Übersicht über die Zuordnung einer AEM-Komponente zu einer Angular-Komponente*

## Überprüfen der Textkomponente

Der [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype) bietet eine `Text`-Komponente, die der AEM-[Textkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=de) zugeordnet ist. Dies ist ein Beispiel für eine **Inhaltskomponente**, da sie *Inhalte* von AEM rendert.

Sehen wir uns an, wie die Komponente funktioniert.

### Überprüfen des JSON-Modells

1. Bevor wir zum SPA-Code kommen, müssen Sie zunächst das von AEM bereitgestellte JSON-Modell verstehen. Navigieren Sie zur [Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) und zeigen Sie die Seite für die Textkomponente an. Die Kernkomponentenbibliothek enthält Beispiele für alle AEM-Kernkomponenten.
2. Wählen Sie die Registerkarte **JSON** für eines der folgenden Beispiele aus:

   ![Text – JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText` und `:type`.

   `:type` ist eine reservierte Eigenschaft, die den `sling:resourceType` (oder den Pfad) der AEM-Komponente auflistet. Der Wert von `:type` wird verwendet, um die AEM-Komponente der SPA-Komponente zuzuordnen.

   `text` und `richText` sind zusätzliche Eigenschaften, die für die SPA-Komponente bereitgestellt werden.

### Überprüfen der Textkomponente

1. Öffnen Sie ein neues Terminal und navigieren Sie zum Ordner `ui.frontend` innerhalb des Projekts. Führen Sie `npm install` und dann `npm start` aus, um den **webpack-Dev-Server** zu starten:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   Das Modul `ui.frontend` ist aktuell auf die Verwendung des [JSON-Pseudomodells](./integrate-spa.md#mock-json) eingerichtet.

2. Es sollte ein neues Browser-Fenster zu sehen sein, in dem [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) geöffnet ist.

   ![webpack-Dev-Server mit Pseudoinhalten](assets/map-components/initial-start.png)

3. Öffnen Sie in der IDE Ihrer Wahl das AEM-Projekt für die WKND-SPA. Erweitern Sie das Modul `ui.frontend` und öffnen Sie die Datei **text.component.ts** unter `ui.frontend/src/app/components/text/text.component.ts`:

   ![Quell-Code der Angular-Komponente in text.js](assets/map-components/vscode-ide-text-js.png)

4. Der erste zu prüfende Bereich ist `class TextComponent` (ungefähr bei Zeile 35):

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

   Der Decorator [@Input()](https://angular.io/api/core/Input) wird verwendet, um Felder zu deklarieren, deren Werte über das zugeordnete und zuvor überprüfte JSON-Objekt festgelegt werden.

   `@HostBinding('innerHtml') get content()` ist eine Methode, die den erstellten Textinhalt aus dem Wert von `this.text` bereitstellt. Wenn es sich bei dem Inhalt um Rich-Text handelt (bestimmt durch die Markierung `this.richText`), wird die integrierte Sicherheitsfunktion von Angular umgangen. [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) von Angular wird verwendet, um unformatiertes HTML zu „bereinigen“ und Sicherheitslücken beim Cross-Site Scripting zu vermeiden. Die Methode ist über den Decorator [@HostBinding](https://angular.io/api/core/HostBinding) an die Eigenschaft `innerHtml` gebunden.

5. Überprüfen Sie als Nächstes `TextEditConfig` (ungefähr bei Zeile 24):

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   Der obige Code bestimmt, wann der Platzhalter in der AEM Author-Umgebung gerendert werden soll. Wenn die Methode `isEmpty` den Wert **true** zurückgibt, wird der Platzhalter gerendert.

6. Sehen Sie sich abschließend den `MapTo`-Aufruf (ungefähr bei Zeile 53) an:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** wird vom AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`) bereitgestellt. Der Pfad `wknd-spa-angular/components/text` steht für das `sling:resourceType`-Element der AEM-Komponente. Dieser Pfad wird mit `:type` abgeglichen, das von dem zuvor festgestellten JSON-Modell bereitgestellt wird. **MapTo** analysiert die JSON-Modellantwort und übergibt die richtigen Werte an die `@Input()`-Variablen der SPA-Komponente.

   Sie finden die AEM-`Text`-Komponentendefinition unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimentieren Sie ein wenig, indem Sie die Datei **en.model.json** Datei unter `ui.frontend/src/mocks/json/en.model.json` ändern.

   Aktualisieren Sie ungefähr bei Zeile 62 den ersten `Text`-Wert, um ein **`H1`**- und **`u`**-Tag zu verwenden:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Kehren Sie zum Browser zurück, um das vom **webpack-Dev-Server** bereitgestellte Ergebnis zu sehen:

   ![Aktualisiertes Textmodell](assets/map-components/updated-text-model.png)

   Schalten Sie bei der Eigenschaft `richText` zwischen **true**/**false** um, um die Render-Logik in Aktion zu sehen.

8. Überprüfen Sie **text.component.html** unter `ui.frontend/src/app/components/text/text.component.html`.

   Diese Datei ist leer, da der gesamte Inhalt der Komponente durch die Eigenschaft `innerHTML` festgelegt wird.

9. Überprüfen Sie **app.module.ts** unter `ui.frontend/src/app/app.module.ts`.

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

   **TextComponent** ist nicht explizit enthalten, sondern dynamisch über **AEMResponsiveGridComponent**, bereitgestellt vom AEM SPA Editor JS SDK. Daher muss dies im [entryComponents](https://angular.io/guide/entry-components)-Array von **app.module.ts** aufgelistet sein.

## Erstellen der Bildkomponente

Erstellen Sie als Nächstes eine `Image`-Angular-Komponente, die der [Bildkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=de) von AEM zugeordnet ist. Die `Image`-Komponente ist ein weiteres Beispiel für eine **Inhaltskomponente**.

### Überprüfen des JSON-Modells

Bevor wir zum SPA-Code kommen, überprüfen Sie das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Bild-Kernkomponente – JSON](./assets/map-components/image-json.png)

   Mit den Eigenschaften `src`, `alt` und `title` wird die `Image`-Komponente der SPA befüllt.

   >[!NOTE]
   >
   > Es werden andere Bildeigenschaften bereitgestellt (`lazyEnabled`, `widths`), die es Entwicklerinnen und Entwicklern ermöglichen, eine adaptive und Lazy-Loading-basierte Komponente zu erstellen. Die in diesem Tutorial erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

2. Kehren Sie zu Ihrer IDE zurück und öffnen Sie `en.model.json` unter `ui.frontend/src/mocks/json/en.model.json`. Da dies eine neue Komponente für unser Projekt ist, müssen wir das Bild-JSON mit Pseudoinhalten versehen.

   Fügen Sie ungefähr bei Zeile 70 einen JSON-Eintrag für das `image`-Modell hinzu (das nachfolgende Komma `,` nach dem zweiten `text_386303036`-Eintrag nicht vergessen) und aktualisieren Sie das Array `:itemsOrder`.

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

   Das Projekt enthält ein Beispielbild unter `/mock-content/adobestock-140634652.jpeg`, das mit dem **webpack-Dev-Server** verwendet wird.

   Die vollständige en.model.json-Datei können Sie [hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json) einsehen.

3. Fügen Sie ein lizenzfreies Foto hinzu, das von der Komponente angezeigt werden soll.

   Erstellen Sie einen neuen Ordner mit dem Namen **images** unterhalb von `ui.frontend/src/mocks`. Laden Sie [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) herunter und legen Sie das Bild im neu erstellten Ordner **images** ab. Sie können auch ein eigenes Bild verwenden.

### Implementieren der Bildkomponente

1. Halten Sie den **webpack-Dev-Server** an, sofern gestartet.
2. Erstellen Sie eine neue Bildkomponente durch Ausführen des Angular-Befehlszeilenbefehls `ng generate component` im Ordner `ui.frontend`:

   ```shell
   $ ng generate component components/image
   ```

3. Öffnen Sie in der IDE **image.component.ts** unter `ui.frontend/src/app/components/image/image.component.ts` und führen Sie folgende Aktualisierung aus:

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

   `ImageEditConfig` ist die Konfiguration, die bestimmt, ob der Autorenplatzhalter in AEM gerendert werden soll – und zwar abhängig davon, ob die Eigenschaft `src` angegeben ist.

   `@Input()` von `src`, `alt` und `title` sind die Eigenschaften, die von der JSON-API zugeordnet sind.

   `hasImage()` ist eine Methode, die bestimmt, ob das Bild gerendert werden soll.

   `MapTo` ordnet die SPA-Komponente der AEM-Komponente unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image` zu.

4. Öffnen Sie **image.component.html** und führen Sie folgende Aktualisierung aus:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Dadurch wird das `<img>`-Element gerendert, wenn `hasImage` den Wert **true** zurückgibt.

5. Öffnen Sie **image.component.scss** und führen Sie folgende Aktualisierung aus:

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
   > Die `:host-context`-Regel ist **wichtig**, damit der Platzhalter des AEM-SPA-Editors ordnungsgemäß funktioniert. Alle SPA-Komponenten, die im AEM-Seiten-Editor erstellt werden sollen, benötigen mindestens diese Regel.

6. Öffnen Sie `app.module.ts` und fügen Sie `ImageComponent` dem Array `entryComponents` hinzu:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   `ImageComponent` wird wie `TextComponent` dynamisch geladen und muss im Array `entryComponents` enthalten sein.

7. Starten Sie den **Webpack-Dev-Server**, um `ImageComponent` zu rendern.

   ```shell
   $ npm run start:mock
   ```

   ![Hinzugefügtes Pseudobild](assets/map-components/image-added-mock.png)

   *Der SPA hinzugefügtes Bild*

   >[!NOTE]
   >
   > **Bonusaufgabe**: Implementieren Sie eine neue Methode zum Anzeigen des Werts von `title` als Bildbeschriftung.

## Aktualisieren der Richtlinien in AEM

Die `ImageComponent`-Komponente ist nur im **webpack-Dev-Server** sichtbar. Implementieren Sie als Nächstes die aktualisierte SPA in AEM und aktualisieren Sie die Vorlagenrichtlinien.

1. Halten Sie den **Webpack-Dev-Server** an und implementieren Sie im **Stammprojekt** die Änderungen für AEM mithilfe von Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie auf dem AEM-Startbildschirm zu **[!UICONTROL Tools]** > **[!UICONTROL Vorlagen]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Wählen Sie die **SPA-Seite** aus und bearbeiten Sie diese:

   ![Bearbeiten der SPA-Seitenvorlage](assets/map-components/edit-spa-page-template.png)

3. Wählen Sie den **Layout-Container** aus und klicken Sie auf das **Richtliniensymbol**, um die Richtlinie zu bearbeiten:

   ![Layout-Container-Richtlinie](./assets/map-components/layout-container-policy.png)

4. Aktivieren Sie unter **Zugelassene Komponenten** > **WKND SPA Angular – Inhalt** das Kontrollkästchen für die **Bildkomponente**:

   ![Kontrollkästchen für die Bildkomponente aktiviert](assets/map-components/check-image-component.png)

   Wählen Sie unter **Standardkomponenten** > **Zuordnung hinzufügen** die Komponente **Bild – WKND SPA Angular – Inhalt** aus:

   ![Festlegen der Standardkomponenten](assets/map-components/default-components.png)

   Geben Sie `image/*` als **MIME-Typ** an.

   Klicken Sie auf **Fertig**, um die Richtlinienaktualisierungen zu speichern.

5. Klicken Sie unter **Layout-Container** auf das **Richtliniensymbol** für die **Textkomponente**:

   ![Richtliniensymbol für die Textkomponente](./assets/map-components/edit-text-policy.png)

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Markieren Sie unter **Plug-ins** > **Formatierung** alle Kontrollkästchen, um zusätzliche Formatierungsoptionen zu aktivieren:

   ![Aktivieren der RTE-Formatierung](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plug-ins** > **Absatzformate** > das Kontrollkästchen **Absatzformate aktivieren**:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig**, um die Richtlinienaktualisierung zu speichern.

6. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Sie sollten auch dazu in der Lage sein, die `Text`-Komponente zu bearbeiten und zusätzliche Absatzformate im **Vollbildmodus** hinzuzufügen.

   ![Rich-Text-Bearbeitung im Vollbildmodus](assets/map-components/full-screen-rte.png)

7. Sie sollten auch in der Lage sein, ein Bild per Drag-and-Drop aus dem **Asset-Finder** zu übernehmen:

   ![Ziehen eines Bilds per Drag-and-Drop](./assets/map-components/drag-drop-image.gif)

8. Fügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) hinzu oder installieren Sie die fertige Code-Basis für die Standard-[WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest). Die [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM-Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Installieren von „wknd.all“ durch den Package Manager](./assets/map-components/package-manager-wknd-all.png)

## Überprüfen des Layout-Containers

Unterstützung für den **Layout-Container** wird automatisch vom AEM SPA Editor SDK bereitgestellt. Der **Layout-Container** ist, wie der Name schon sagt, eine **Container**-Komponente. Container-Komponenten sind Komponenten, die JSON-Strukturen akzeptieren, die *andere* Komponenten repräsentieren, und diese dynamisch instanziieren.

Überprüfen wir den Layout-Container weiter.

1. Öffnen Sie in der IDE **responsive-grid.component.ts** unter `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   Die `AEMResponsiveGridComponent` wird als Teil des AEM SPA Editor SDK implementiert und über `import-components` in das Projekt eingefügt.

2. Navigieren Sie in einem Browser zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![JSON-Modell-API – Responsives Raster](./assets/map-components/responsive-grid-modeljson.png)

   Die **Layout-Container**-Komponente verfügt über eine `sling:resourceType` von `wcm/foundation/components/responsivegrid` und wird vom SPA-Editor mithilfe der `:type`-Eigenschaft erkannt, genau wie die `Text`- und die `Image`-Komponenten.

   Die gleichen Funktionen für die Neuskalierung einer Komponente über den [Layout-Modus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=de#defining-layouts-layout-mode) sind im SPA-Editor verfügbar.

3. Kehren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) zurück. Fügen Sie zusätzliche **Bildkomponenten** hinzu und versuchen Sie, sie mithilfe der Option **Layout** neu zu skalieren:

   ![Neuskalierung der Bildgröße im Layout-Modus](./assets/map-components/responsive-grid-layout-change.gif)

4. Öffnen Sie das JSON-Modell [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) erneut und betrachten Sie die `columnClassNames` als Teil von JSON:

   ![Spalten mit Klassennamen](./assets/map-components/responsive-grid-classnames.png)

   Der Klassenname `aem-GridColumn--default--4` gibt an, dass die Komponente basierend auf einem 12-Spalten-Raster 4 Spalten breit sein sollte. Weitere Informationen zum [responsiven Raster finden Sie hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Kehren Sie zur IDE zurück. Im Modul `ui.apps` gibt es eine Client-seitige Bibliothek, die unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid` definiert ist. Öffnen Sie die Datei `less/grid.less`.

   Diese Datei bestimmt die Haltepunkte (`default`, `tablet`und `phone`), die der **Layout-Container** verwendet. Diese Datei soll gemäß den Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `650px` festgelegt.

6. Sie sollten die responsiven Funktionen und die aktualisierten Rich-Text-Richtlinien der `Text`-Komponente nutzen können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitel Beispiel endgültige Erstellung](assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie SPA-Komponenten AEM-Komponenten zuordnen, und Sie haben eine neue `Image`-Komponente implementiert. Außerdem hatten Sie eine Gelegenheit, die responsiven Funktionen des **Layout-Containers** zu erkunden.

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `Angular/map-components-solution` wechseln.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md) – Erfahren Sie, wie mehrere Ansichten in der SPA unterstützt werden können, indem Sie sie mit dem SPA Editor SDK AEM-Seiten zuordnen. Die dynamische Navigation wird mithilfe des Angular-Routers implementiert und einer vorhandenen Header-Komponente hinzugefügt.

## Bonus – Beibehalten von Konfigurationen zur Verwaltung der Quelle {#bonus}

In vielen Fällen, insbesondere zu Beginn eines AEM-Projekts, ist es nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien beizubehalten, um die Quelle zu verwalten. Dadurch wird sichergestellt, dass alle Entwicklerinnen und Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten, und zusätzliche Konsistenz zwischen Umgebungen gewährleistet. Sobald ein Projekt einen gewissen Reifegrad erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Power-Benutzenden übertragen werden.

Die nächsten Schritte werden mit der Visual Studio Code-IDE und [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) durchgeführt, jedoch können Sie jedes beliebige Tool und jede IDE verwenden, die Sie auf das **Abrufen** oder **Importieren** von Inhalt von einer lokalen Instanz von AEM konfiguriert haben.

1. Stellen Sie in der Visual Studio Code-IDE sicher, dass Sie **VSCode AEM Sync** über die Marketplace-Erweiterung installiert haben:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Erweitern Sie das Modul **ui.content** im Project Explorer und navigieren Sie zu `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Klicken Sie mit der rechten Maustaste** auf den Ordner `templates` und wählen Sie **Importieren aus AEM-Server**:

   ![VSCode-Importvorlage](assets/map-components/import-aem-servervscode.png)

4. Wiederholen Sie die Schritte zum Importieren von Inhalten, aber wählen Sie unter `/conf/wknd-spa-angular/settings/wcm/policies` den Ordner **policies** aus.

5. Prüfen Sie die unter `ui.content/src/main/content/META-INF/vault/filter.xml` gespeicherte Datei `filter.xml`.

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

   Die Datei `filter.xml` ist für die Identifizierung der Pfade von Knoten verantwortlich, die mit dem Paket installiert werden. Beachten Sie `mode="merge"` bei jedem Filter. Damit wird angegeben, dass vorhandener Inhalt nicht geändert wird, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsautorinnen und Inhaltsautoren diese Pfade möglicherweise aktualisieren, ist es wichtig, dass Inhalt **nicht** durch eine Code-Implementierung überschrieben wird. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html) .

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml`, um sich mit den verschiedenen, von den einzelnen Modulen verwalteten Knoten vertraut zu machen.
