---
title: Zuordnen von SPA-Komponenten zu AEM Komponenten | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM) Komponenten zuordnen. Mithilfe der Komponentenzuordnung können Benutzer im AEM SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vornehmen, ähnlich wie beim herkömmlichen AEM Authoring.
sub-product: Sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 2%

---


# Zuordnen von SPA-Komponenten zu AEM Komponenten {#map-components}

Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM) Komponenten zuordnen. Mithilfe der Komponentenzuordnung können Benutzer im AEM SPA-Editor dynamische Aktualisierungen an SPA-Komponenten vornehmen, ähnlich wie beim herkömmlichen AEM Authoring.

Dieses Kapitel enthält einen tieferen Einstieg in die AEM JSON-Modell-API und wie der von einer AEM Komponente offen gelegte JSON-Inhalt automatisch als Props in eine React-Komponente eingefügt werden kann.

## Vorgabe

1. Erfahren Sie, wie Sie AEM Komponenten SPA-Komponenten zuordnen.
2. Machen Sie sich mit dem Unterschied zwischen **Container** - und **Inhaltskomponenten** vertraut.
3. Erstellen Sie eine neue React-Komponente, die einer vorhandenen AEM zugeordnet ist.

## Was Sie erstellen

In diesem Kapitel wird untersucht, wie die bereitgestellte `Text` SPA-Komponente der AEM `Text`Komponente zugeordnet wird. Es wird eine neue `Image` SPA-Komponente erstellt, die im SPA verwendet und in AEM verfasst werden kann. Außerdem werden vordefinierte Funktionen der Richtlinien **Layout Container** und **Vorlageneditor** verwendet, um eine Ansicht zu erstellen, die im Erscheinungsbild etwas abwechslungsreicher ist.

![Kapitelbeispiel für fertiges Authoring](./assets/map-components/final-page.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Bei Verwendung von [AEM 6.x](overview.md#compatibility) fügen Sie das `classic` Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `React/map-components-solution`.

## Zuordnungsansatz

Das Grundkonzept besteht darin, eine SPA-Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, serverseitig ausführen, Inhalte als Teil der JSON-Modell-API exportieren Der JSON-Inhalt wird vom SPA genutzt und im Browser clientseitig ausgeführt. Es wird eine 1:1-Zuordnung zwischen SPA-Komponenten und einer AEM-Komponente erstellt.

![Überblick über die Zuordnung einer AEM Komponente zu einer React-Komponente auf hoher Ebene](./assets/map-components/high-level-approach.png)

*Überblick über die Zuordnung einer AEM Komponente zu einer React-Komponente auf hoher Ebene*

## Inspect der Textkomponente

Der [AEM Projektarchiv](https://github.com/adobe/aem-project-archetype) stellt eine `Text` Komponente bereit, die der AEM [Textkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/text.html)zugeordnet wird. Dies ist ein Beispiel für eine **Inhaltskomponente** , da sie *Inhalte* aus AEM rendert.

Sehen wir uns an, wie die Komponente funktioniert.

### Inspect-JSON-Modell

1. Bevor Sie in den SPA-Code springen, sollten Sie das JSON-Modell, das AEM bietet, kennen. Navigieren Sie zur [Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) und Ansicht der Textseite. Die Core Component Library enthält Beispiele für alle AEM Core-Komponenten.
2. Wählen Sie für eines der Beispiele die Registerkarte **JSON** aus:

   ![Text-JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText`und `:type`.

   `:type` ist eine reservierte Eigenschaft, die die `sling:resourceType` (oder den Pfad) der AEM-Komponente Liste. Der Wert von `:type` ist der Wert, mit dem die AEM Komponente der SPA-Komponente zugeordnet wird.

   `text` und `richText` sind zusätzliche Eigenschaften, die der SPA-Komponente bereitgestellt werden.

### Inspect der Textkomponente

1. Öffnen Sie ein neues Terminal und navigieren Sie zum `ui.frontend` Ordner im Projekt. Führen Sie `npm install` den `npm start` Webpack-dev-server **aus und führen Sie ihn dann aus**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   Das `ui.frontend` Modul ist derzeit für die Verwendung des [JSON-Modell](./integrate-spa.md#mock-json)eingerichtet.

2. Es sollte ein neues Browserfenster geöffnet werden, das [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Webpack-Dev-Server mit Musterinhalt](./assets/map-components/initial-start.png)

3. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND SPA. Erweitern Sie das `ui.frontend` Modul und öffnen Sie die Datei `Text.js` unter `ui.frontend/src/components/Text/Text.js`:

   ![Text.js - Komponentenquellcode react](./assets/map-components/vscode-ide-text-js.png)

4. Der erste Bereich, den wir überprüfen werden, ist die `class Text` bei ~Zeile 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` ist eine standardmäßige React-Komponente. Die Komponente bestimmt `this.props.richText` anhand der Komponente, ob der zu rendernde Inhalt Rich-Text oder Nur-Text sein soll. Der tatsächlich verwendete &quot;Inhalt&quot;stammt aus `this.props.text`. Um einen potenziellen XSS-Angriff zu vermeiden, wird der Rich-Text über Escape-Zeichen `DOMPurify` vor der Verwendung von [gefährlichSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) , um den Inhalt zu rendern. Rufen Sie die Eigenschaften `richText` und `text` Eigenschaften aus dem JSON-Modell früher in der Übung auf.

5. Sehen Sie sich als Nächstes die `TextEditConfig` Zeile ~29 an:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Der obige Code bestimmt, wann der Platzhalter in der Umgebung AEM Autors wiedergegeben wird. Wenn die `isEmpty` Methode **true** zurückgibt, wird der Platzhalter gerendert.

6. Sehen Sie sich schließlich den `MapTo` Aufruf von ~Zeile 62 an:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wird vom AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`) bereitgestellt. Der Pfad `wknd-spa-react/components/text` stellt die `sling:resourceType` Komponente AEM dar. Dieser Pfad wird mit dem zuvor beobachteten JSON-Modell `:type` abgeglichen. `MapTo` übernimmt die Analyse der JSON-Modellantwort und Übergabe der korrekten Werte `props` an die SPA-Komponente.

   Die AEM `Text` Komponentendefinition finden Sie unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Experimentieren Sie, indem Sie die `mock.model.json` Datei unter `ui.frontend/public/mock-content/mock.model.json`. Aktualisieren Sie in ~Zeile 62 den ersten `Text` Wert, um ein **`H1`** und **`u`** -Tags zu verwenden:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Navigieren Sie zu [http://localhost:3000](http://localhost:3000) , um die folgenden Effekte anzuzeigen:

   ![Aktualisiertes Textmodell](./assets/map-components/updated-text-model.png)

   Versuchen Sie, die `richText` Eigenschaft zwischen **true** / **false** umzuschalten, um die Renderlogik in Aktion zu sehen.

8. Inspect `Text.scss` bei `ui.frontend/src/components/Text/Text.scss`.

   Diese Datei wurde von der Start-Code-Basis für dieses Kapitel hinzugefügt und nutzt die [Sass](https://sass-lang.com/) -Funktion, die im vorherigen Kapitel hinzugefügt wurde. Notieren Sie die Variablen, auf die verwiesen wird `ui.frontend/src/styles/_variables.scss`.

## Erstellen der Bildkomponente

Erstellen Sie anschließend eine `Image` React-Komponente, die der AEM [Image-Komponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html)zugeordnet ist. Die `Image` Komponente ist ein weiteres Beispiel für eine **Inhaltskomponente** .

### Inspect the JSON

Prüfen Sie vor dem Aufrufen des SPA-Codes das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Core Component-Bibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON-Komponente &quot;Bildkern&quot;](./assets/map-components/image-json.png)

   Eigenschaften von `src`, `alt`und `title` werden zum Füllen der SPA- `Image` Komponente verwendet.

   >[!NOTE]
   >
   > Es sind andere Bildeigenschaften verfügbar (`lazyEnabled`, `widths`), die es einem Entwickler ermöglichen, eine adaptive und verzögertes Laden-Komponente zu erstellen. Die in diesem Lernprogramm erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

2. Kehren Sie zu Ihrer IDE zurück und öffnen Sie das `mock.model.json` unter `ui.frontend/public/mock-content/mock.model.json`. Da dies eine netto-neue Komponente für unser Projekt ist, müssen wir das Bild JSON &quot;spotten&quot;.

   Fügen Sie in ~Zeile 70 einen JSON-Eintrag für das `image` Modell hinzu (vergessen Sie nicht das nachfolgende Komma `,` nach der zweiten `text_23828680`) und aktualisieren Sie das `:itemsOrder` Array.

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   Das Projekt enthält ein Beispielbild, `/mock-content/adobestock-140634652.jpeg` das mit dem **webpack-dev-server** verwendet wird.

   Sie können die vollständige [Datei &quot;mock.model.json&quot;hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json)Ansicht haben.

### Komponente &quot;Bild&quot;implementieren

1. Erstellen Sie als Nächstes einen neuen Ordner mit dem Namen `Image` unter `ui.frontend/src/components`.
2. Erstellen Sie unter dem `Image` Ordner eine neue Datei mit dem Namen `Image.js`.

   ![Image.js-Datei](./assets/map-components/image-js-file.png)

3. hinzufügen die folgenden `import` Anweisungen an `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Fügen Sie dann die hinzu, `ImageEditConfig` um festzulegen, wann der Platzhalter in AEM angezeigt werden soll:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Der Platzhalter zeigt an, ob die `src` Eigenschaft nicht eingestellt ist.

5. Implementieren Sie anschließend die `Image` Klasse:

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   Der obige Code gibt eine `<img>` auf Grundlage der Eigenschaften aus `src`, `alt`und wird vom JSON-Modell übergeben `title` .

6. hinzufügen Sie den `MapTo` Code zur Zuordnung der React-Komponente zur AEM Komponente an:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Beachten Sie, dass die Zeichenfolge `wknd-spa-react/components/image` dem Speicherort der AEM-Komponente in `ui.apps` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Erstellen Sie eine neue Datei mit dem Namen `Image.scss` im selben Ordner und fügen Sie Folgendes hinzu:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. Fügen Sie der Datei oben unter den `Image.js` `import` Anweisungen einen Verweis hinzu:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   Sie können die vollständige Datei [Image.js hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js)Ansicht geben.

9. Öffnen Sie die Datei `ui.frontend/src/components/import-components.js` und fügen Sie der neuen `Image` Komponente einen Verweis hinzu:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Beginn den **webpack-dev-server**, falls noch nicht gestartet. Navigieren Sie zu [http://localhost:3000](http://localhost:3000) und Sie sollten das Bild-Rendering sehen:

   ![Bild zum Modell hinzugefügt](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Bonusherausforderung**: Implementieren Sie eine neue Methode in, `Image.js` um den Wert von `this.props.title` als Beschriftung unter dem Bild anzuzeigen.

## Richtlinien in AEM aktualisieren

Die `Image` Komponente ist nur im **webpack-dev-server** sichtbar. Stellen Sie dann die aktualisierte SPA bereit, um die Vorlagenrichtlinien zu AEM und zu aktualisieren.

1. Beenden Sie den **webpack-dev-server** und stellen Sie die Änderungen an AEM aus dem Stammverzeichnis des Projekts bereit, indem Sie Ihre Maven-Fähigkeiten verwenden:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie im Bildschirm &quot;AEM Beginn&quot;zu **Extras** > **Vorlagen** > **[WKND-SPA-Reaktion](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Wählen Sie die **SPA-Seite** aus und bearbeiten Sie sie:

   ![Vorlage für SPA-Seiten bearbeiten](./assets/map-components/edit-spa-page-template.png)

3. Wählen Sie den **Layout-Container** aus und klicken Sie auf das **Richtliniensymbol** , um die Richtlinie zu bearbeiten:

   ![Container-Richtlinie](./assets/map-components/layout-container-policy.png)

4. Unter **Zulässige Komponenten** > **WKND-SPA-Reaktion - Inhalt** > Überprüfen Sie die **Image** -Komponente:

   ![Bildkomponente ausgewählt](./assets/map-components/check-image-component.png)

   Wählen Sie unter &quot; **Standardkomponenten** &quot;> &quot; **Hinzufügen zuordnen** &quot;die Komponente &quot; **Bild - WKND-SPA-Reaktion - Content** &quot;:

   ![Standardkomponenten festlegen](./assets/map-components/default-components.png)

   Geben Sie einen **Mime-Typ** von ein `image/*`.

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierungen zu speichern.

5. Klicken Sie im Container **Layout** auf das Symbol **Richtlinie** für die **Textkomponente** :

   ![Richtliniensymbol für Textkomponenten](./assets/map-components/edit-text-policy.png)

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Aktivieren Sie unter **Plugins** > **Formatierung** > alle Kontrollkästchen, um weitere Formatierungsoptionen zu aktivieren:

   ![RTE-Formatierung aktivieren](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plugins** > **Absatzformate** > das Kontrollkästchen, um Absatzformate **zu** aktivieren:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierung zu speichern.

6. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   Sie sollten auch die `Text` Komponente bearbeiten und zusätzliche Absatzstile im **Vollbildmodus** hinzufügen können.

   ![Vollbild-Rich-Text-Bearbeitung](assets/map-components/full-screen-rte.png)

7. Sie sollten auch ein Bild aus der **Asset-Suche** ziehen und ablegen können:

   ![Bild ziehen und ablegen](./assets/map-components/drag-drop-image.gif)

8. hinzufügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) oder installieren Sie die fertige Code-Basis für die Standard- [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest). Die [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect Layout Container

Der **Layout-Container** wird automatisch vom AEM SPA Editor SDK unterstützt. Der **Layout-Container** ist, wie durch den Namen angegeben, eine **Container** -Komponente. Container-Komponenten sind Komponenten, die JSON-Strukturen akzeptieren, die *andere* Komponenten darstellen und dynamisch instanziieren.

Lassen Sie uns den Layout-Container näher untersuchen.

1. Navigieren Sie in einem Browser zu [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON-Modell-API - Responsive Grid](./assets/map-components/responsive-grid-modeljson.png)

   Die Komponente **Layout Container** verfügt über einen `sling:resourceType` von `wcm/foundation/components/responsivegrid` und wird vom SPA-Editor mithilfe der `:type` Eigenschaft erkannt, genau wie die `Text` und die `Image` Komponenten.

   Mit dem SPA-Editor stehen die gleichen Funktionen zum Neuskalieren einer Komponente im [Layoutmodus](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) zur Verfügung.

2. Kehren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)zurück. hinzufügen Sie weitere **Bildkomponenten** und versuchen Sie, die Größe mithilfe der Option &quot; **Layout** &quot;zu ändern:

   ![Bildgröße im Layoutmodus ändern](./assets/map-components/responsive-grid-layout-change.gif)

3. Öffnen Sie das JSON-Modell erneut [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) und beobachten Sie das `columnClassNames` JSON-Modell:

   ![Klassennamen in der Cloud](./assets/map-components/responsive-grid-classnames.png)

   Der Klassenname `aem-GridColumn--default--4` gibt an, dass die Komponente basierend auf einem 12-Spalten-Raster vier Spalten breit sein sollte. Weitere Informationen zum [reaktionsfähigen Raster finden Sie hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Kehren Sie zur IDE zurück und im `ui.apps` Modul ist eine clientseitige Bibliothek definiert `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Öffnen Sie die Datei `less/grid.less`.

   Diese Datei legt die vom`default`Layout-Container `tablet`verwendeten Haltepunkte ( `phone`, **und**) fest. Diese Datei soll nach Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `650px`festgelegt.

5. Sie sollten die reaktionsfähigen Funktionen und die aktualisierten Rich-Text-Richtlinien der `Text` Komponente verwenden können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitelbeispiel für fertiges Authoring](./assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie Sie SPA-Komponenten zu AEM Komponenten zuordnen und Sie haben eine neue `Image` Komponente implementiert. Sie haben auch die Möglichkeit, die reaktionsfähigen Funktionen des **Layout-Containers** zu erkunden.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `React/map-components-solution`.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md) - Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnen zu AEM mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des React Routers implementiert und einer vorhandenen Header-Komponente hinzugefügt.

## Bonus - Beständige Konfigurationen zur Quellcodeverwaltung {#bonus}

In vielen Fällen, besonders zu Beginn eines AEM Projekts, ist es nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quellcodeverwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhaltssatz und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen den Umgebung sicherstellen. Sobald ein Projekt eine gewisse Reife erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Stromverbrauchern überlassen werden.

Die nächsten Schritte werden mit der Code-IDE und [VSCode AEM Synchronisierung](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) von Visual Studio durchgeführt. Sie können jedoch jedes Tool und jede IDE verwenden, die Sie zum **Abrufen** oder **Importieren** von Inhalten aus einer lokalen Instanz von AEM konfiguriert haben.

1. Stellen Sie in der Code-IDE von Visual Studio sicher, dass **VSCode AEM Sync** über die Erweiterung Marketplace installiert ist:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Erweitern Sie das Modul **ui.content** im Projektexplorer und navigieren Sie zu `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Klicken Sie mit der rechten Maustaste auf** den `templates` Ordner und wählen Sie **Aus AEM Server** importieren:

   ![VSCode-Importvorlage](./assets/map-components/import-aem-servervscode.png)

4. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den **Ordner &quot;policies** &quot;unter `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect Sie die `filter.xml` Datei unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   Die `filter.xml` Datei ist dafür verantwortlich, die Pfade der Knoten zu identifizieren, die mit dem Paket installiert werden. Beachten Sie die `mode="merge"` auf jedem Filter, die anzeigen, dass vorhandene Inhalte nicht geändert werden, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsersteller diese Pfade möglicherweise aktualisieren, ist es wichtig, dass eine Codebereitstellung Inhalte **nicht** überschreibt. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html) .

   Vergleichen `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml` verstehen Sie die verschiedenen Knoten, die von den einzelnen Modulen verwaltet werden.
