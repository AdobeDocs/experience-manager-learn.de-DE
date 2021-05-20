---
title: Zuordnen SPA Komponenten zu AEM Komponenten | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.
sub-product: Sites
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 2%

---


# Zuordnen SPA Komponenten zu AEM Komponenten {#map-components}

Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.

In diesem Kapitel werden die AEM JSON-Modell-API und die Möglichkeiten erläutert, wie der von einer AEM-Komponente angezeigte JSON-Inhalt automatisch als Props in eine React-Komponente eingefügt werden kann.

## Vorgabe

1. Erfahren Sie, wie Sie AEM Komponenten SPA Komponenten zuordnen.
2. Machen Sie sich mit dem Unterschied zwischen den Komponenten **Container** und **Inhalt** vertraut.
3. Erstellen Sie eine neue React-Komponente, die einer vorhandenen AEM-Komponente zugeordnet ist.

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
   $ git checkout React/map-components-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `React/map-components-solution` wechseln.

## Mapping-Ansatz

Das grundlegende Konzept besteht darin, eine SPA Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, Server-seitig ausführen, Inhalte als Teil der JSON-Modell-API exportieren. Der JSON-Inhalt wird vom SPA verwendet, der clientseitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA Komponenten und einer AEM Komponente erstellt.

![Allgemeine Übersicht über die Zuordnung einer AEM Komponente zu einer React-Komponente](./assets/map-components/high-level-approach.png)

*Allgemeine Übersicht über die Zuordnung einer AEM Komponente zu einer React-Komponente*

## Inspect der Textkomponente

Der [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) stellt eine `Text`-Komponente bereit, die der AEM [Textkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/text.html) zugeordnet ist. Dies ist ein Beispiel für eine **content** -Komponente, da sie *content* von AEM rendert.

Sehen wir uns an, wie die Komponente funktioniert.

### Inspect des JSON-Modells

1. Bevor Sie in den SPA-Code springen, müssen Sie das von AEM bereitgestellte JSON-Modell verstehen. Navigieren Sie zur [Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) und zeigen Sie die Seite für die Textkomponente an. Die Kernkomponentenbibliothek enthält Beispiele für alle AEM Kernkomponenten.
2. Wählen Sie die Registerkarte **JSON** für eines der Beispiele aus:

   ![Text-JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText` und `:type`.

   `:type` ist eine reservierte Eigenschaft, die die  `sling:resourceType` (oder den Pfad) der AEM Komponente auflistet. Der Wert `:type` wird verwendet, um die AEM Komponente der SPA zuzuordnen.

   `text` und  `richText` sind zusätzliche Eigenschaften, die der SPA-Komponente angezeigt werden.

### Inspect der Textkomponente

1. Öffnen Sie ein neues Terminal und navigieren Sie zum Ordner `ui.frontend` im Projekt. Führen Sie `npm install` und dann `npm start` aus, um **webpack-dev-server** zu starten:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   Das Modul `ui.frontend` ist derzeit so eingerichtet, dass das JSON-Modell [nachahmen](./integrate-spa.md#mock-json) verwendet wird.

2. Sie sollten ein neues Browserfenster sehen, das geöffnet ist für [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Webpack-Dev-Server mit nachgeahmten Inhalten](./assets/map-components/initial-start.png)

3. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die WKND-SPA. Erweitern Sie das Modul `ui.frontend` und öffnen Sie die Datei `Text.js` unter `ui.frontend/src/components/Text/Text.js`:

   ![Text.js React Component Source Code](./assets/map-components/vscode-ide-text-js.png)

4. Der erste Bereich, den wir untersuchen werden, ist `class Text` in ~line 40:

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

   `Text` ist eine standardmäßige React-Komponente. Die Komponente verwendet `this.props.richText`, um zu bestimmen, ob der zu rendernde Inhalt Rich-Text oder Nur-Text sein wird. Der tatsächlich verwendete &quot;Inhalt&quot;stammt von `this.props.text`. Um einen potenziellen XSS-Angriff zu vermeiden, wird der Rich-Text über `DOMPurify` maskiert, bevor [gefährlichSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) zum Rendern des Inhalts verwendet wird. Erinnern Sie sich an die Eigenschaften `richText` und `text` aus dem JSON-Modell, das Sie zuvor in der Übung erstellt haben.

5. Als Nächstes sehen Sie sich `TextEditConfig` an der Zeile ~29 an:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Der obige Code ist für die Bestimmung verantwortlich, wann der Platzhalter in der AEM Autorenumgebung wiedergegeben werden soll. Wenn die `isEmpty`-Methode **true** zurückgibt, wird der Platzhalter gerendert.

6. Sehen Sie sich schließlich den `MapTo`-Aufruf an unter ~line 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wird vom AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`) bereitgestellt. Der Pfad `wknd-spa-react/components/text` stellt die `sling:resourceType` der AEM Komponente dar. Dieser Pfad wird mit dem `:type` übereinstimmen, der vom zuvor beobachteten JSON-Modell bereitgestellt wird. `MapTo` analysiert die JSON-Modellantwort und übergibt die richtigen Werte  `props` an die SPA Komponente.

   Die AEM `Text`-Komponentendefinition finden Sie unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Experimentieren Sie, indem Sie die `mock.model.json`-Datei unter `ui.frontend/public/mock-content/mock.model.json` ändern. Aktualisieren Sie in ~line 62 den ersten `Text` -Wert, um die Tags **`H1`** und **`u`** zu verwenden:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Navigieren Sie zu [http://localhost:3000](http://localhost:3000) , um die Auswirkungen anzuzeigen:

   ![Aktualisiertes Textmodell](./assets/map-components/updated-text-model.png)

   Versuchen Sie, die `richText`-Eigenschaft zwischen **true** / **false** umzuschalten, um die Renderlogik in Aktion zu sehen.

8. Inspect `Text.scss` bei `ui.frontend/src/components/Text/Text.scss`.

   Diese Datei wurde von der Starter-Codebasis für dieses Kapitel hinzugefügt und verwendet die im vorherigen Kapitel hinzugefügte Funktion [Sass](https://sass-lang.com/) . Beachten Sie die Variablen, auf die von `ui.frontend/src/styles/_variables.scss` verwiesen wird.

## Erstellen der Bildkomponente

Als Nächstes erstellen Sie eine `Image` React-Komponente, die der AEM [Bildkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html) zugeordnet ist. Die Komponente `Image` ist ein weiteres Beispiel für eine **content** -Komponente.

### Inspect the JSON

Bevor Sie in den SPA-Code springen, überprüfen Sie das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON der Bild-Kernkomponente](./assets/map-components/image-json.png)

   Die Eigenschaften `src`, `alt` und `title` werden zum Ausfüllen der SPA `Image`-Komponente verwendet.

   >[!NOTE]
   >
   > Es gibt weitere angezeigte Bildeigenschaften (`lazyEnabled`, `widths`), die es einem Entwickler ermöglichen, eine adaptive und verzögerte Ladekomponente zu erstellen. Die in diesem Tutorial erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

2. Kehren Sie zu Ihrer IDE zurück und öffnen Sie `mock.model.json` unter `ui.frontend/public/mock-content/mock.model.json`. Da dies eine netto-neue Komponente für unser Projekt ist, müssen wir die Bild-JSON &quot;nachahmen&quot;.

   Fügen Sie in ~line 70 einen JSON-Eintrag für das Modell `image` hinzu (vergessen Sie nicht das nachfolgende Komma `,` nach dem zweiten `text_23828680`) und aktualisieren Sie das Array `:itemsOrder`.

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

   Das Projekt enthält ein Beispielbild unter `/mock-content/adobestock-140634652.jpeg` , das mit dem **webpack-Dev-Server** verwendet wird.

   Den vollständigen [mock.model.json finden Sie hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json).

### Implementieren der Bildkomponente

1. Erstellen Sie anschließend einen neuen Ordner mit dem Namen `Image` unter `ui.frontend/src/components`.
2. Erstellen Sie unter dem Ordner `Image` eine neue Datei mit dem Namen `Image.js`.

   ![Image.js-Datei](./assets/map-components/image-js-file.png)

3. Fügen Sie die folgenden `import`-Anweisungen zu `Image.js` hinzu:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Fügen Sie dann `ImageEditConfig` hinzu, um zu bestimmen, wann der Platzhalter in AEM angezeigt werden soll:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Der Platzhalter zeigt an, ob die Eigenschaft `src` nicht festgelegt ist.

5. Implementieren Sie als Nächstes die Klasse `Image` :

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

   Der obige Code rendert ein `<img>` basierend auf den Eigenschaften `src`, `alt` und `title`, die vom JSON-Modell übergeben werden.

6. Fügen Sie den Code `MapTo` hinzu, um die React-Komponente der AEM-Komponente zuzuordnen:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Beachten Sie, dass die Zeichenfolge `wknd-spa-react/components/image` dem Speicherort der AEM-Komponente in `ui.apps` unter: entspricht. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Erstellen Sie eine neue Datei mit dem Namen `Image.scss` im selben Verzeichnis und fügen Sie Folgendes hinzu:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. Fügen Sie in `Image.js` oben unter den `import` -Anweisungen einen Verweis auf die Datei hinzu:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   Sie können die fertigen [Image.js hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js) anzeigen.

9. Öffnen Sie die Datei `ui.frontend/src/components/import-components.js` und fügen Sie der neuen Komponente `Image` einen Verweis hinzu:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Starten Sie **webpack-dev-server**, falls noch nicht gestartet. Navigieren Sie zu [http://localhost:3000](http://localhost:3000) und Sie sollten das Bild-Rendering sehen:

   ![Bild wurde zur Nachahmung hinzugefügt](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Bonusaufgabe**: Implementieren Sie eine neue Methode in  `Image.js` , um den Wert von  `this.props.title` als Beschriftung unter dem Bild anzuzeigen.

## Richtlinien in AEM aktualisieren

Die Komponente `Image` ist nur im Ordner **webpack-dev-server** sichtbar. Stellen Sie anschließend die aktualisierte SPA bereit, um die Vorlagenrichtlinien zu AEM und zu aktualisieren.

1. Beenden Sie **webpack-dev-server** und stellen Sie die Änderungen mithilfe Ihrer Maven-Fähigkeiten AEM aus dem Stammverzeichnis des Projekts bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie im Bildschirm AEM Start zu **Tools** > **Vorlagen** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Wählen Sie die **SPA Seite** aus und bearbeiten Sie sie:

   ![Bearbeiten SPA Seitenvorlage](./assets/map-components/edit-spa-page-template.png)

3. Wählen Sie den **Layout-Container** aus und klicken Sie auf das Symbol **policy** , um die Richtlinie zu bearbeiten:

   ![Layout-Container-Richtlinie](./assets/map-components/layout-container-policy.png)

4. Überprüfen Sie unter **Zulässige Komponenten** > **WKND SPA React - Content** die Komponente **Bild**:

   ![Bildkomponente ausgewählt](./assets/map-components/check-image-component.png)

   Wählen Sie unter **Standardkomponenten** > **Mapping** hinzufügen und die Komponente **Bild - WKND SPA React - Content** aus:

   ![Standardkomponenten festlegen](./assets/map-components/default-components.png)

   Geben Sie einen **MIME-Typ** von `image/*` ein.

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierungen zu speichern.

5. Klicken Sie im **Layout-Container** auf das Symbol **policy** für die Komponente **Text**:

   ![Symbol für Textkomponentenrichtlinie](./assets/map-components/edit-text-policy.png)

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Aktivieren Sie unter **Plugins** > **Formatierung** > alle Kästchen, um zusätzliche Formatierungsoptionen zu aktivieren:

   ![Aktivieren der RTE-Formatierung](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plugins** > **Absatzstile** das Kontrollkästchen zu **Absatzstile aktivieren**:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierung zu speichern.

6. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   Sie sollten auch in der Lage sein, die Komponente `Text` zu bearbeiten und zusätzliche Absatzstile im Modus **Vollbild** hinzuzufügen.

   ![Rich-Text-Bearbeitung im Vollbildmodus](assets/map-components/full-screen-rte.png)

7. Sie sollten auch ein Bild aus dem **Asset Finder** ziehen und ablegen können:

   ![Bild ziehen und ablegen](./assets/map-components/drag-drop-image.gif)

8. Fügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) hinzu oder installieren Sie die fertige Codebasis für die standardmäßige [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest). Die Referenz-Website [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect des Layout-Containers

Unterstützung für den **Layout-Container** wird automatisch vom AEM SPA Editor SDK bereitgestellt. Der **Layout-Container**, wie durch den Namen angegeben, ist eine **Container**-Komponente. Container-Komponenten sind Komponenten, die JSON-Strukturen akzeptieren, die *andere*-Komponenten darstellen und sie dynamisch instanziieren.

Überprüfen wir nun den Layout-Container weiter.

1. Navigieren Sie in einem Browser zu [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![JSON-Modell-API - Responsives Raster](./assets/map-components/responsive-grid-modeljson.png)

   Die Komponente **Layout-Container** hat den Wert `sling:resourceType` von `wcm/foundation/components/responsivegrid` und wird vom SPA Editor mit der Eigenschaft `:type` erkannt, genau wie die Komponenten `Text` und `Image`.

   Die gleichen Funktionen zum Neuskalieren einer Komponente mit [Layout-Modus](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) sind auch im SPA Editor verfügbar.

2. Kehren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) zurück. Fügen Sie weitere **Bild**-Komponenten hinzu und versuchen Sie, sie mithilfe der Option **Layout** neu zu skalieren:

   ![Bildgröße im Layout-Modus ändern](./assets/map-components/responsive-grid-layout-change.gif)

3. Öffnen Sie das JSON-Modell [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) erneut und beobachten Sie `columnClassNames` als Teil des JSON-Codes:

   ![Cloud-Klassennamen](./assets/map-components/responsive-grid-classnames.png)

   Der Klassenname `aem-GridColumn--default--4` gibt an, dass die Komponente basierend auf einem 12-Spalten-Raster 4 Spalten breit sein sollte. Weitere Informationen zum responsiven Raster [finden Sie hier](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Kehren Sie zur IDE zurück und im Modul `ui.apps` ist eine clientseitige Bibliothek definiert, die unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid` definiert ist. Öffnen Sie die Datei `less/grid.less`.

   Diese Datei bestimmt die Haltepunkte (`default`, `tablet` und `phone`), die vom **Layout-Container** verwendet werden. Diese Datei soll gemäß den Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `650px` festgelegt.

5. Sie sollten die responsiven Funktionen und die aktualisierten Rich-Text-Richtlinien der Komponente `Text` verwenden können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitelbeispiel für die endgültige Bearbeitung](./assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie SPA Komponenten AEM Komponenten zuordnen und eine neue `Image` -Komponente implementiert haben. Sie haben auch die Möglichkeit erhalten, die responsiven Funktionen des **Layout-Containers** zu erkunden.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `React/map-components-solution` wechseln.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md)  - Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnung zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mithilfe des React-Routers implementiert und zu einer vorhandenen Kopfzeilenkomponente hinzugefügt.

## Bonus - Beibehalten von Konfigurationen zur Quell-Code-Verwaltung {#bonus}

In vielen Fällen ist es insbesondere zu Beginn eines AEM-Projekts nützlich, Konfigurationen wie Vorlagen und zugehörige Inhaltsrichtlinien zur Quell-Code-Verwaltung beizubehalten. Dadurch wird sichergestellt, dass alle Entwickler mit demselben Inhalt und denselben Konfigurationen arbeiten und zusätzliche Konsistenz zwischen Umgebungen sichergestellt wird. Sobald ein Projekt einen gewissen Reifegrad erreicht hat, kann die Verwaltung von Vorlagen einer speziellen Gruppe von Power-Benutzern übertragen werden.

Die nächsten Schritte werden mit der Visual Studio Code-IDE und [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) durchgeführt. Sie können jedoch jedes Tool und jede IDE verwenden, die Sie für **Pull** oder **Import**-Inhalte von einer lokalen Instanz von AEM konfiguriert haben.

1. Stellen Sie in der Visual Studio Code-IDE sicher, dass **VSCode AEM Sync** über die Marketplace-Erweiterung installiert ist:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Erweitern Sie das Modul **ui.content** im Projekt-Explorer und navigieren Sie zu `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Klicken Sie mit der rechten Maustaste** auf den  `templates` Ordner und wählen Sie  **Import von AEM Server** aus:

   ![VSCode-Importvorlage](./assets/map-components/import-aem-servervscode.png)

4. Wiederholen Sie die Schritte zum Importieren von Inhalten, wählen Sie jedoch den Ordner **policies** unter `/conf/wknd-spa-react/settings/wcm/templates/policies` aus.

5. Inspect die Datei `filter.xml` unter `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Die `filter.xml`-Datei ist dafür verantwortlich, die Pfade von Knoten zu identifizieren, die mit dem Paket installiert werden. Beachten Sie die `mode="merge"` in jedem Filter, die darauf hinweisen, dass der vorhandene Inhalt nicht geändert wird, sondern nur neue Inhalte hinzugefügt werden. Da Inhaltsautoren diese Pfade möglicherweise aktualisieren, ist es wichtig, dass bei einer Codebereitstellung **nicht** Inhalte überschrieben werden. Weitere Informationen zum Arbeiten mit Filterelementen finden Sie in der [FileVault-Dokumentation](https://jackrabbit.apache.org/filevault/filter.html) .

   Vergleichen Sie `ui.content/src/main/content/META-INF/vault/filter.xml` und `ui.apps/src/main/content/META-INF/vault/filter.xml` , um die verschiedenen Knoten zu verstehen, die von den einzelnen Modulen verwaltet werden.
