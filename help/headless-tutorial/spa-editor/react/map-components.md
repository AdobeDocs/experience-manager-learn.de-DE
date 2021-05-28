---
title: Zuordnen SPA Komponenten zu AEM Komponenten | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM. Außerdem erfahren Sie, wie Sie vordefinierte AEM React-Kernkomponenten verwenden können.
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
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '2273'
ht-degree: 2%

---


# Zuordnen SPA Komponenten zu AEM Komponenten {#map-components}

Erfahren Sie, wie Sie React-Komponenten mit dem AEM SPA Editor JS SDK Adobe Experience Manager (AEM)-Komponenten zuordnen. Die Komponentenzuordnung ermöglicht es Benutzern, im AEM SPA Editor dynamische Aktualisierungen an SPA -Komponenten vorzunehmen, ähnlich wie beim herkömmlichen AEM.

In diesem Kapitel werden die AEM JSON-Modell-API und die Möglichkeiten erläutert, wie der von einer AEM-Komponente angezeigte JSON-Inhalt automatisch als Props in eine React-Komponente eingefügt werden kann.

## Vorgabe

1. Erfahren Sie, wie Sie AEM Komponenten SPA Komponenten zuordnen.
1. Inspect wie eine React-Komponente dynamische Eigenschaften verwendet, die von AEM übergeben werden.
1. Erfahren Sie, wie Sie vordefinierte [React AEM Kernkomponenten](https://github.com/adobe/aem-react-core-wcm-components-examples) verwenden.

## Was Sie erstellen werden

In diesem Kapitel wird untersucht, wie die bereitgestellte SPA `Text` der AEM `Text`Komponente zugeordnet wird. React-Kernkomponenten wie die `Image`-SPA-Komponente werden in der SPA verwendet und in AEM erstellt. Die vordefinierten Funktionen der **Layout-Container**- und **Vorlagen-Editor**-Richtlinien werden auch verwendet, um eine Ansicht zu erstellen, die in ihrer Darstellung etwas abwechslungsreicher ist.

![Kapitelbeispiel für die endgültige Bearbeitung](./assets/map-components/final-page.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment). Dieses Kapitel ist eine Fortsetzung des Kapitels [Integrieren Sie das SPA](integrate-spa.md). Es ist jedoch ein SPA-aktiviertes AEM-Projekt, alles, was Sie benötigen, zu folgen.

## Mapping-Ansatz

Das grundlegende Konzept besteht darin, eine SPA Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, Server-seitig ausführen, Inhalte als Teil der JSON-Modell-API exportieren. Der JSON-Inhalt wird vom SPA verwendet, der clientseitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA Komponenten und einer AEM Komponente erstellt.

![Allgemeine Übersicht über die Zuordnung einer AEM Komponente zu einer React-Komponente](./assets/map-components/high-level-approach.png)

*Allgemeine Übersicht über die Zuordnung einer AEM Komponente zu einer React-Komponente*

## Inspect der Textkomponente

Der [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) stellt eine `Text`-Komponente bereit, die der AEM [Textkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/text.html) zugeordnet ist. Dies ist ein Beispiel für eine **content** -Komponente, da sie *content* von AEM rendert.

Sehen wir uns an, wie die Komponente funktioniert.

### Inspect des JSON-Modells

1. Bevor Sie in den SPA-Code springen, müssen Sie das von AEM bereitgestellte JSON-Modell verstehen. Navigieren Sie zur [Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) und zeigen Sie die Seite für die Textkomponente an. Die Kernkomponentenbibliothek enthält Beispiele für alle AEM Kernkomponenten.
1. Wählen Sie die Registerkarte **JSON** für eines der Beispiele aus:

   ![Text-JSON-Modell](./assets/map-components/text-json.png)

   Es sollten drei Eigenschaften angezeigt werden: `text`, `richText` und `:type`.

   `:type` ist eine reservierte Eigenschaft, die die  `sling:resourceType` (oder den Pfad) der AEM Komponente auflistet. Der Wert `:type` wird verwendet, um die AEM Komponente der SPA zuzuordnen.

   `text` und  `richText` sind zusätzliche Eigenschaften, die der SPA-Komponente angezeigt werden.

1. Zeigen Sie die JSON-Ausgabe unter [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) an. Sie sollten einen Eintrag finden können, der in etwa wie folgt aussieht:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect der SPA-Komponente

1. Öffnen Sie in der IDE Ihrer Wahl das AEM Projekt für die SPA. Erweitern Sie das Modul `ui.frontend` und öffnen Sie die Datei `Text.js` unter `ui.frontend/src/components/Text/Text.js`.

1. Der erste Bereich, den wir untersuchen werden, ist `class Text` in ~line 40:

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

   `Text` ist eine standardmäßige React-Komponente. Die Komponente verwendet `this.props.richText`, um zu bestimmen, ob der zu rendernde Inhalt Rich-Text oder Nur-Text sein wird. Der tatsächlich verwendete &quot;Inhalt&quot;stammt von `this.props.text`.

   Um einen potenziellen XSS-Angriff zu vermeiden, wird der Rich-Text über `DOMPurify` maskiert, bevor [gefährlichSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) zum Rendern des Inhalts verwendet wird. Erinnern Sie sich an die Eigenschaften `richText` und `text` aus dem JSON-Modell, das Sie zuvor in der Übung erstellt haben.

1. Als Nächstes sehen Sie sich `TextEditConfig` an der Zeile ~29 an:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   Der obige Code ist für die Bestimmung verantwortlich, wann der Platzhalter in der AEM Autorenumgebung wiedergegeben werden soll. Wenn die `isEmpty`-Methode **true** zurückgibt, wird der Platzhalter gerendert.

1. Sehen Sie sich schließlich den `MapTo`-Aufruf an unter ~line 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` wird vom AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`) bereitgestellt. Der Pfad `wknd-spa-react/components/text` stellt die `sling:resourceType` der AEM Komponente dar. Dieser Pfad wird mit dem `:type` übereinstimmen, der vom zuvor beobachteten JSON-Modell bereitgestellt wird. `MapTo` analysiert die JSON-Modellantwort und übergibt die richtigen Werte  `props` an die SPA Komponente.

   Die AEM `Text`-Komponentendefinition finden Sie unter `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Verwenden von React-Kernkomponenten

[AEM WCM-Komponenten - React Core-](https://github.com/adobe/aem-react-core-wcm-components-base) Implementierung und  [AEM WCM-Komponenten - SPA-Editor - React Core-Implementierung](https://github.com/adobe/aem-react-core-wcm-components-spa). Hierbei handelt es sich um einen Satz wiederverwendbarer Komponenten der Benutzeroberfläche, die vordefinierten AEM Komponenten zugeordnet sind. Die meisten Projekte können diese Komponenten als Ausgangspunkt für ihre eigene Implementierung wiederverwenden.

1. Öffnen Sie im Projektcode die Datei `import-components.js` unter `ui.frontend/src/components`.
Diese Datei importiert alle SPA Komponenten, die AEM Komponenten zugeordnet sind. Angesichts der Dynamik der SPA Editor-Implementierung müssen wir explizit auf alle SPA Komponenten verweisen, die mit AEM Authoring-fähigen Komponenten verknüpft sind. Dadurch kann ein AEM-Autor wählen, ob er eine Komponente überall in der Anwendung verwenden möchte.
1. Die folgenden Importanweisungen enthalten SPA Komponenten, die in das Projekt geschrieben wurden:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Es gibt mehrere weitere `imports` von `@adobe/aem-core-components-react-spa` und `@adobe/aem-core-components-react-base`. Diese importieren die React-Kernkomponenten und stellen sie im aktuellen Projekt zur Verfügung. Diese werden dann mit `MapTo` projektspezifischen AEM-Komponenten zugeordnet, genau wie im Komponentenbeispiel `Text` zuvor.

### AEM aktualisieren

Richtlinien sind eine Funktion AEM Vorlagen, die Entwicklern und Power-Benutzern eine granulare Steuerung darüber gibt, welche Komponenten verwendet werden können. Die React-Kernkomponenten sind im SPA-Code enthalten, müssen jedoch über eine Richtlinie aktiviert werden, bevor sie in der Anwendung verwendet werden können.

1. Navigieren Sie im Bildschirm AEM Start zu **Tools** > **Vorlagen** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Wählen Sie die Vorlage **SPA Seite** aus und öffnen Sie sie zur Bearbeitung.

1. Wählen Sie den **Layout-Container** aus und klicken Sie auf das Symbol **policy** , um die Richtlinie zu bearbeiten:

   ![Layout-Container-Richtlinie](assets/map-components/edit-spa-page-template.png)

1. Unter **Zulässige Komponenten** > **WKND SPA React - Content** > check **Image**, **Teaser** und **Title**.

   ![Aktualisierte verfügbare Komponenten](assets/map-components/update-components-available.png)

   Wählen Sie unter **Standardkomponenten** > **Mapping** hinzufügen und die Komponente **Bild - WKND SPA React - Content** aus:

   ![Standardkomponenten festlegen](./assets/map-components/default-components.png)

   Geben Sie einen **MIME-Typ** von `image/*` ein.

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierungen zu speichern.

1. Klicken Sie im **Layout-Container** auf das Symbol **policy** für die Komponente **Text**.

   Erstellen Sie eine neue Richtlinie mit dem Namen **WKND SPA Text**. Aktivieren Sie unter **Plugins** > **Formatierung** > alle Kästchen, um zusätzliche Formatierungsoptionen zu aktivieren:

   ![Aktivieren der RTE-Formatierung](assets/map-components/enable-formatting-rte.png)

   Aktivieren Sie unter **Plugins** > **Absatzstile** das Kontrollkästchen zu **Absatzstile aktivieren**:

   ![Absatzformate aktivieren](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Klicken Sie auf **Fertig** , um die Richtlinienaktualisierung zu speichern.

### Autoreninhalt

1. Navigieren Sie zur **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Sie sollten jetzt in der Lage sein, die zusätzlichen Komponenten **Bild**, **Teaser** und **Titel** auf der Seite zu verwenden.

   ![Zusätzliche Komponenten](assets/map-components/additional-components.png)

1. Sie sollten auch in der Lage sein, die Komponente `Text` zu bearbeiten und zusätzliche Absatzstile im Modus **Vollbild** hinzuzufügen.

   ![Rich-Text-Bearbeitung im Vollbildmodus](assets/map-components/full-screen-rte.png)

1. Sie sollten auch ein Bild aus dem **Asset Finder** ziehen und ablegen können:

   ![Bild ziehen und ablegen](assets/map-components/drag-drop-image.png)

1. Erfahren Sie mehr über die Komponenten **Titel** und **Teaser** .

1. Fügen Sie Ihre eigenen Bilder über [AEM Assets](http://localhost:4502/assets.html/content/dam) hinzu oder installieren Sie die fertige Codebasis für die standardmäßige [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest). Die Referenz-Website [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) enthält viele Bilder, die auf der WKND-SPA wiederverwendet werden können. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

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

   Diese Datei bestimmt die Haltepunkte (`default`, `tablet` und `phone`), die vom **Layout-Container** verwendet werden. Diese Datei soll gemäß den Projektspezifikationen angepasst werden. Derzeit sind die Haltepunkte auf `1200px` und `768px` festgelegt.

5. Sie sollten die responsiven Funktionen und die aktualisierten Rich-Text-Richtlinien der Komponente `Text` verwenden können, um eine Ansicht wie die folgende zu erstellen:

   ![Kapitelbeispiel für die endgültige Bearbeitung](assets/map-components/final-page.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie SPA Komponenten AEM Komponenten zuordnen und die React-Kernkomponenten verwendet haben. Sie haben auch die Möglichkeit erhalten, die responsiven Funktionen des **Layout-Containers** zu erkunden.

### Nächste Schritte {#next-steps}

[Navigation und Routing](navigation-routing.md)  - Erfahren Sie, wie mehrere Ansichten im SPA durch Zuordnung zu AEM Seiten mit dem SPA Editor SDK unterstützt werden können. Die dynamische Navigation wird mit React-Router und React-Kernkomponenten implementiert.

## (Bonus) Beibehalten von Konfigurationen zur Quell-Code-Verwaltung {#bonus-configs}

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

## (Bonus) Benutzerdefinierte Bildkomponente erstellen {#bonus-image}

Eine SPA Bildkomponente wurde bereits von den React-Kernkomponenten bereitgestellt. Wenn Sie jedoch eine zusätzliche Übung wünschen, erstellen Sie Ihre eigene React-Implementierung, die der AEM [Bildkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html) zugeordnet ist. Die Komponente `Image` ist ein weiteres Beispiel für eine **content** -Komponente.

### Inspect the JSON

Bevor Sie in den SPA-Code springen, überprüfen Sie das von AEM bereitgestellte JSON-Modell.

1. Navigieren Sie zu den [Bildbeispielen in der Kernkomponentenbibliothek](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![JSON der Bild-Kernkomponente](./assets/map-components/image-json.png)

   Die Eigenschaften `src`, `alt` und `title` werden zum Ausfüllen der SPA `Image`-Komponente verwendet.

   >[!NOTE]
   >
   > Es gibt weitere angezeigte Bildeigenschaften (`lazyEnabled`, `widths`), die es einem Entwickler ermöglichen, eine adaptive und verzögerte Ladekomponente zu erstellen. Die in diesem Tutorial erstellte Komponente ist einfach und verwendet **nicht** diese erweiterten Eigenschaften.

### Implementieren der Bildkomponente

1. Erstellen Sie anschließend einen neuen Ordner mit dem Namen `Image` unter `ui.frontend/src/components`.
1. Erstellen Sie unter dem Ordner `Image` eine neue Datei mit dem Namen `Image.js`.

   ![Image.js-Datei](./assets/map-components/image-js-file.png)

1. Fügen Sie die folgenden `import`-Anweisungen zu `Image.js` hinzu:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Fügen Sie dann `ImageEditConfig` hinzu, um zu bestimmen, wann der Platzhalter in AEM angezeigt werden soll:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   Der Platzhalter zeigt an, ob die Eigenschaft `src` nicht festgelegt ist.

1. Implementieren Sie als Nächstes die Klasse `Image` :

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

1. Fügen Sie den Code `MapTo` hinzu, um die React-Komponente der AEM-Komponente zuzuordnen:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Beachten Sie, dass die Zeichenfolge `wknd-spa-react/components/image` dem Speicherort der AEM-Komponente in `ui.apps` unter: entspricht. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Erstellen Sie eine neue Datei mit dem Namen `Image.css` im selben Verzeichnis und fügen Sie Folgendes hinzu:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Fügen Sie in `Image.js` oben unter den `import` -Anweisungen einen Verweis auf die Datei hinzu:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Öffnen Sie die Datei `ui.frontend/src/components/import-components.js` und fügen Sie der neuen Komponente `Image` einen Verweis hinzu:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Kommentar in `import-components.js` zum Bild der React-Kernkomponente:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Dadurch wird sichergestellt, dass stattdessen unsere benutzerdefinierte Bildkomponente verwendet wird.

1. Stellen Sie aus dem Stammverzeichnis des Projekts den SPA-Code mithilfe von Maven für AEM bereit:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect die SPA in AEM. Alle Bildkomponenten auf der Seite sollten weiterhin funktionieren. Inspect die gerenderte Ausgabe und Sie sollten das Markup für unsere benutzerdefinierte Bildkomponente anstelle der React-Kernkomponente sehen.

   *Markup für benutzerdefinierte Bildkomponenten*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Bild-Markup der React-Kernkomponente*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Dies ist eine gute Einführung in die Erweiterung und Implementierung Ihrer eigenen Komponenten.

