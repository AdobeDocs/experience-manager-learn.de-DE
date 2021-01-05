---
title: Entwickeln mit dem Stilsystem
seo-title: Entwickeln mit dem Stilsystem
description: Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. Dieses Lernprogramm behandelt die Entwicklung für das Stilsystem, um Core-Komponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors zu erweitern.
sub-product: Sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '3077'
ht-degree: 2%

---


# Entwickeln mit dem Stilsystem {#developing-with-the-style-system}

Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. Dieses Lernprogramm behandelt die Entwicklung für das Stilsystem, um Core-Komponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors zu erweitern.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

Es wird außerdem empfohlen, das Lernprogramm [Clientseitige Bibliotheken und Front-End-Arbeitsablauf](client-side-libraries.md) zu überprüfen, um die Grundlagen clientseitiger Bibliotheken und der verschiedenen Front-End-Tools, die in das AEM Projekt integriert sind, zu verstehen.

### Starterprojekt

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Klonen Sie das [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `style-system/start` an.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout style-system/start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/style-system/solution) oder lokal prüfen, indem Sie zur Verzweigung `style-system/solution` wechseln.

## Vorgabe

1. Erfahren Sie, wie Sie mit dem Stilsystem markenspezifische CSS auf AEM Kernkomponenten anwenden.
1. Erfahren Sie mehr über die BEM-Notation und wie sie zum sorgfältigen Scope von Stilen verwendet werden kann.
1. Wenden Sie erweiterte Richtlinienkonfigurationen mit bearbeitbaren Vorlagen an.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel verwenden wir die Funktion [Stilsystem](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html), um mehrere Varianten von Komponenten zu erstellen, die auf der Artikelseite verwendet werden. Außerdem verwenden wir das Stilsystem, um Varianten für Strukturelemente wie Kopf-/Fußzeile und Layout-Container zu erstellen.

>[!VIDEO](https://video.tv.adobe.com/v/30386/?quality=12&learn=on)

## Hintergrund {#background}

Das [Stilsystem](https://docs.adobe.com/content/help/de/experience-manager-65/developing/components/style-system.html) ermöglicht es Entwicklern und Vorlageneditoren, mehrere visuelle Varianten einer Komponente zu erstellen. Autoren können dann wiederum entscheiden, welcher Stil beim Erstellen einer Seite verwendet werden soll. Wir werden das Stilsystem während des gesamten Tutorials nutzen, um mehrere einzigartige Stile zu erzielen und gleichzeitig die Kernkomponenten in einem Low-Code-Ansatz zu nutzen.

Die allgemeine Idee beim Stilsystem ist, dass Autoren verschiedene Stile für das Aussehen einer Komponente wählen können. Die &quot;Stile&quot;werden durch zusätzliche CSS-Klassen unterstützt, die in das äußere div einer Komponente injiziert werden. In den Client-Bibliotheken werden CSS-Regeln auf Grundlage dieser Stilklassen hinzugefügt, sodass sich das Erscheinungsbild der Komponente ändert.

Die detaillierte Dokumentation für das Stilsystem finden Sie hier. [](https://docs.adobe.com/content/help/en/experience-manager-65/developing/components/style-system.html) Es gibt auch ein großartiges [technisches Video zum Verständnis des Stilsystems](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Stil der Titelkomponente {#title-component}

An dieser Stelle wurde die Komponente [Titel](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/title.html) im Rahmen des Moduls **ui.apps** in das Projekt unter `/apps/wknd/components/content/title` kopiert. Die Standardstile von Überschriftenelementen (`H1`, `H2`, `H3`...) wurden bereits im Modul **ui.frontend** in der Datei `_elements.scss` unter `ui.frontend/src/main/webpack/base/sass/_elements.scss` implementiert.

### Unterstrichstil

Die [WKND-Artikelentwürfe](assets/pages-templates/wknd-article-design.xd) enthalten einen eindeutigen Stil für die Titelkomponente mit einer Unterstreichung. Statt zwei Komponenten zu erstellen oder das Komponentendialogfeld zu ändern, kann das Stilsystem verwendet werden, um Autoren die Möglichkeit zu geben, einen Unterstrichstil hinzuzufügen.

![Unterstreichungsstil - Titelkomponente](assets/style-system/title-underline-style.png)

### Inspect Title Component Markup

Als Front-End-Entwickler besteht der erste Schritt zum Formatieren einer Core-Komponente darin, das von der Komponente generierte Markup zu verstehen.

Im Rahmen des erstellten Projekts wurde der Archetyp in das Projekt **Core Component Examples** eingebettet. Für Entwickler und Inhaltsersteller enthält dies eine einfache Referenz, um alle Funktionen zu verstehen, die mit Core-Komponenten verfügbar sind. Eine Live-Version ist auch [verfügbar](https://opensource.adobe.com/aem-core-wcm-components/library.html).

1. Öffnen Sie einen neuen Browser und Ansicht der Titelkomponente:

   Lokale AEM: [http://localhost:4502/editor.html/content/core-components-examples/library/title.html](http://localhost:4502/editor.html/content/core-components-examples/library/title.html)

   Live-Beispiel: [https://opensource.adobe.com/aem-core-wcm-components/library/title.html](https://opensource.adobe.com/aem-core-wcm-components/library/title.html)

1. Nachstehend finden Sie das Markup für die Komponente &quot;Titel&quot;:

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   BEM-Notation der Title-Komponente:

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. Das Style-System fügt dem äußeren div, das die Komponente umgibt, eine CSS-Klasse hinzu. Daher ähnelt das Markup, das wir als Targeting einsetzen werden, in etwa dem Folgenden:

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### Implementierung des Underline-Stils - ui.frontend

Als Nächstes implementieren wir den Underline-Stil mit dem Modul **ui.frontend** unseres Projekts. Wir verwenden den Webpack-Entwicklungsserver, der zum Lieferumfang des Moduls **ui.frontend** gehört, um die Stile *vor der Bereitstellung auf einer lokalen Instanz von AEM zu Vorschau.*

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   Dadurch sollte ein Browser unter [http://localhost:8080](http://localhost:8080) geöffnet werden.

   >[!NOTE]
   >
   > Wenn Bilder beschädigt angezeigt werden, stellen Sie sicher, dass das Startprojekt auf einer lokalen Instanz von AEM bereitgestellt wurde (auf Port 4502 ausgeführt) und der verwendete Browser sich auch bei der lokalen AEM angemeldet hat.

   ![Webpack-Entwicklungsserver](assets/style-system/static-webpack-server.png)

1. Öffnen Sie in Eclipse oder der IDE Ihrer Wahl die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`. Dies ist das statische Markup, das vom Webpack-Entwicklungsserver verwendet wird.
1. Suchen Sie in `index.html` eine Instanz der Titellomponente, der der Unterstrichstil hinzugefügt werden soll, indem Sie im Dokument nach *cmp-title* suchen. Wählen Sie die Komponente &quot;Title&quot;mit dem Text *&quot;Vans off the Wall Skatepark&quot;* (Zeile 218). hinzufügen Sie die Klasse `cmp-title--underline` in das umliegende div:

   ```html
    <!-- before -->
    <div class="title aem-GridColumn aem-GridColumn--default--8">
        <div class="cmp-title">
            <h2 class="cmp-title__text">Vans off the Wall Skatepark</h2>
        </div>
    </div>
   ```

   ```html
    <!-- After -->
    <div class="cmp-title--underline title aem-GridColumn aem-GridColumn--default--8">
        <div class="cmp-title">
            <h2 class="cmp-title__text">Vans off the Wall Skatepark</h2>
        </div>
    </div>
   ```

1. Kehren Sie zum Browser zurück und vergewissern Sie sich, dass die zusätzliche Klasse im Markup angezeigt wird.
1. Kehren Sie zum Modul **ui.frontend** zurück und aktualisieren Sie die Datei `title.scss` unter: `ui.frontend/src/main/webpack/components/content/title/scss/title.scss`:

   ```css
   /* Add Title Underline Style */
   .cmp-title--underline {
   
       .cmp-title {
       }
   
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >Es wird als Best Practice erachtet, die Zielgruppe immer eng zu unterteilen. Dadurch wird sichergestellt, dass zusätzliche Stile keine Auswirkungen auf andere Bereiche der Seite haben.
   >
   >Alle Kernkomponenten halten die **[BEM-Notation](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)** ein. Es empfiehlt sich, beim Erstellen eines Standardstils für eine Komponente die äußere CSS-Klasse Zielgruppe. Eine weitere Best Practice ist die Verwendung von Klassennamen, die von der BEM-Notation der Kernkomponente anstelle von HTML-Elementen angegeben werden.

1. Kehren Sie erneut zum Browser zurück und Sie sollten den Stil &quot;Unterstreichen&quot;sehen:

   ![Unterstreichen Sie den Stil, der auf dem Webpack-Dev-Server sichtbar ist](assets/style-system/underline-implemented-webpack.png)

1. Beenden Sie den Webpack-Entwicklungsserver.

### hinzufügen einer Titelrichtlinie

Als Nächstes müssen wir eine neue Richtlinie für Titel-Komponenten hinzufügen, damit Inhaltsersteller den Stil &quot;Unterstreichen&quot;wählen können, der auf bestimmte Komponenten angewendet werden soll. Dies erfolgt mithilfe des Vorlageneditors in AEM.

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zum Ordner **Artikelseitenvorlage** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html).

1. Wählen Sie im Modus **Struktur** im Container **Layout** das Symbol **Richtlinie** neben der unter *Zulässige Komponenten* aufgelisteten Komponente **Titel** aus:

   ![Richtlinie für Titel konfigurieren](assets/style-system/article-template-title-policy-icon.png)

1. Erstellen Sie eine neue Richtlinie für die Komponente &quot;Titel&quot;mit den folgenden Werten:

   *Richtlinientitel **:  **WKND-Titel**

   *Eigenschaften* > Registerkarte &quot; *Stile&quot;* >  *Hinzufügen neuen Stils*

   **Unterstreichen** :  `cmp-title--underline`

   ![Stilrichtlinien-Konfiguration für Titel](assets/style-system/title-style-policy.gif)

   Klicken Sie auf **Fertig**, um die Änderungen an der Titelrichtlinie zu speichern.

   >[!NOTE]
   >
   > Der Wert `cmp-title--underline` stimmt mit der CSS-Klasse überein, die wir früher bei der Entwicklung im Modul **ui.frontend** ausgewählt haben.

### Unterstreichen-Stil anwenden

Als Autor können wir schließlich den Stil der Unterstreichung auf bestimmte Komponenten des Titels anwenden.

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Wählen Sie im Modus **Bearbeiten** eine Titelkomponente. Klicken Sie auf das Symbol **Paintbrush** und wählen Sie den Stil **Unterstreichen** aus:

   ![Unterstreichen-Stil anwenden](assets/style-system/apply-underline-style-title.png)

   Als Autor sollten Sie den Stil ein-/ausschalten können.

1. Klicken Sie auf das Symbol **Seiteninformationen** > **Ansicht als Veröffentlicht**, um die Seite außerhalb AEM Editors zu überprüfen.

   ![Als veröffentlicht anzeigen](assets/style-system/view-as-published.png)

   Verwenden Sie Ihre Browser-Entwicklerwerkzeuge, um zu überprüfen, ob die CSS-Klasse `cmp-title--underline` auf das äußere div angewendet wurde.

## Stil der Textkomponente {#text-component}

Als Nächstes wiederholen wir ähnliche Schritte, um einen eindeutigen Stil auf die [Textkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/text.html) anzuwenden. Die Textkomponente wurde im Rahmen des Moduls `/apps/wknd/components/content/text` in das Projekt unter **ui.apps** kopiert. Die Standardstile von Absatzelementen wurden bereits im Modul **ui.frontend** in der Datei `_elements.scss` unter `ui.frontend/src/main/webpack/base/sass/_elements.scss` implementiert.

### Stil des Anführungsblocks

Die [WKND-Artikelentwürfe](assets/pages-templates/wknd-article-design.xd) enthalten einen eindeutigen Stil für die Textkomponente mit einem Anführungsblock:

![Stil des Anführungsblocks - Textkomponente](assets/style-system/quote-block-style.png)

### Inspect Text Component Markup

Wieder einmal werden wir das Markup der Textkomponente überprüfen.

1. Öffnen Sie einen neuen Browser und Ansicht der Textkomponente als Teil der Core Component Library:
Lokale AEM: [http://localhost:4502/editor.html/content/core-components-examples/library/text.html](http://localhost:4502/editor.html/content/core-components-examples/library/text.html)

   Live-Beispiel: [https://opensource.adobe.com/aem-core-wcm-components/library/text.html](https://opensource.adobe.com/aem-core-wcm-components/library/text.html)

1. Nachstehend finden Sie das Markup für die Komponente Text:

   ```html
   <div class="cmp-text">
       <p><b>Bold </b>can be used to emphasize a word or phrase, as can <u>underline</u> and <i>italics.&nbsp;</i><sup>Superscript</sup> and <sub>subscript</sub> are useful for mathematical (E = mc<sup>2</sup>) or scientific (h<sub>2</sub>O) expressions. Paragraph styles can provide alternative renderings, such as quote sections:</p>
       <blockquote>"<i>Be yourself; everyone else is already taken"</i></blockquote>
       <b>- Oscar Wilde</b>
   </div>
   ```

   BEM-Notation der Titel-Komponente:

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. Das Style-System fügt dem äußeren div, das die Komponente umgibt, eine CSS-Klasse hinzu. Daher ähnelt das Markup, das wir als Targeting einsetzen werden, in etwa dem Folgenden:

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text">
           <p><b>Bold </b>can be used to emphasize a word or phrase, as can <u>underline</u> and <i>italics.&nbsp;</i><sup>Superscript</sup> and <sub>subscript</sub> are useful for mathematical (E = mc<sup>2</sup>) or scientific (h<sub>2</sub>O) expressions. Paragraph styles can provide alternative renderings, such as quote sections:</p>
           <blockquote>"<i>Be yourself; everyone else is already taken"</i></blockquote>
           <b>- Oscar Wilde</b>
       </div>
   </div>
   ```

### Implementieren des Anführungsblock-Stils - ui.frontend

Als Nächstes implementieren wir den Stil des Angebotsblocks mit dem Modul **ui.frontend** unseres Projekts.

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Öffnen Sie in Eclipse oder der IDE Ihrer Wahl die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`. Dies ist das statische Markup, das vom Webpack-Entwicklungsserver verwendet wird.
1. Suchen Sie in `index.html` nach einer Instanz der Textkomponente, indem Sie nach dem Text *&quot;Jacob Wester&quot;* (Zeile 210) suchen. hinzufügen Sie die Klasse `cmp-text--quote` in das umliegende div:

   ```html
    <!-- before -->
    <div class="text aem-GridColumn aem-GridColumn--default--8">
        <div class="cmp-text">
            <blockquote>"There is no better place to shred then Los Angeles"</blockquote>
            <p>Jacob Wester - Pro Skater</p>
        </div>
    </div>
   ```

   ```html
    <!-- After -->
    <div class="cmp-text--quote text aem-GridColumn aem-GridColumn--default--8">
        <div class="cmp-text">
            <blockquote>"There is no better place to shred then Los Angeles"</blockquote>
            <p>Jacob Wester - Pro Skater</p>
        </div>
    </div>
   ```

1. Kehren Sie zum Browser zurück und vergewissern Sie sich, dass die zusätzliche Klasse im Markup angezeigt wird.
1. Kehren Sie zum Modul **ui.frontend** zurück und aktualisieren Sie die Datei `text.scss` unter: `ui.frontend/src/main/webpack/components/content/text/scss/text.scss`:

   ```css
   /* WKND Text Quote style */
   
   .cmp-text--quote {
   
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-h2;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
   
           p {
               font-size:    $font-size-large;
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > In diesem Fall werden rohe HTML-Elemente von den Stilen als Ziel ausgewählt. Dies liegt daran, dass die Komponente Text einen Rich-Text-Editor für Inhaltsersteller bereitstellt. Die Erstellung von Stilen direkt gegen RTE-Inhalte sollte mit Sorgfalt erfolgen, und es ist umso wichtiger, die Stile genau zu definieren.

1. Kehren Sie erneut zum Browser zurück und Sie sollten den Stil des Angebotsblocks sehen:

   ![Stil des Anführungsblocks im Webpack-Dev-Server sichtbar](assets/style-system/quoteblock-implemented-webpack.png)

1. Beenden Sie den Webpack-Entwicklungsserver.

### hinzufügen einer Textrichtlinie

Fügen Sie dann eine neue Richtlinie für die Textkomponenten hinzu.

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zum Ordner **Artikelseitenvorlage** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html).

1. Wählen Sie im Modus **Struktur** im Container **Layout** das Symbol **Richtlinie** neben der unter *Zulässige Komponenten* aufgelisteten Komponente **aus:**

   ![Textrichtlinie konfigurieren](assets/style-system/article-template-text-policy-icon.png)

1. Erstellen Sie eine neue Richtlinie für die Textkomponente mit den folgenden Werten:

   *Richtlinientitel **:  **WKND-Text**

   *Plugins* >  *Absatzformate* > Absatzformate  *aktivieren*

   *Stile, Registerkarte* >  *Hinzufügen neuen Stil*

   **Anführungsblock** :  `cmp-text--quote`

   ![Richtlinie für Textkomponenten](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Textkomponentenrichtlinie 2](assets/style-system/text-policy-enable-quotestyle.png)

   Klicken Sie auf **Fertig**, um die Änderungen an der Textrichtlinie zu speichern.

### Anwenden des Anführungsblock-Stils

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Wählen Sie im Modus **Bearbeiten** eine Textkomponente. Bearbeiten Sie die Komponente, um ein Anführungselement einzuschließen:

   ![Konfiguration der Textkomponente](assets/style-system/configure-text-component.png)

1. Wählen Sie die Textkomponente aus und klicken Sie auf das Symbol **Paintbrush** und wählen Sie den Stil **Anführungsblock** aus:

   ![Anwenden des Anführungsblock-Stils](assets/style-system/quote-block-style-applied.png)

   Als Autor sollten Sie den Stil ein-/ausschalten können.

## Layout-Container {#layout-container}

Mithilfe von Layout-Containern können Sie die Grundstruktur der Artikelseitenvorlage erstellen und die Ablagebereiche festlegen, in denen Autoren Inhalte auf einer Seite hinzufügen können. Layout-Container können auch das Stilsystem nutzen und Autoren noch mehr Optionen zum Entwerfen von Layouts bereitstellen.

Derzeit wird eine CSS-Regel auf die gesamte Seite angewendet, die eine feste Breite erzwingt. Stattdessen besteht ein flexiblerer Ansatz darin, einen Stil **Feste Breite** zu erstellen, den Inhaltsersteller aktivieren/deaktivieren können.

### Implementieren des Formats &quot;Feste Breite&quot;- ui.frontend

Wir werden Beginn bei der Implementierung des Formats &quot;Feste Breite&quot;im Modul **ui.frontend** unseres Projektes haben.

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. Öffnen Sie die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`.
1. Wir möchten den Hauptteil unserer Artikelseitenvorlage mit fester Breite füllen, sodass die Kopf- und Fußzeile weiter erweitert werden kann. Daher möchten wir den zweiten `<div class='responsivegrid aem-GridColumn aem-GridColumn--default--12'` (Layout-Container) zwischen den beiden Erlebnisfragmenten (Zeile 136) Zielgruppe haben.

   ![Haupttextlayout Container Div](assets/style-system/main-body-layoutContainer.png)

1. hinzufügen Sie die Klasse `cmp-layout-container--fixed` auf die `div`, die im vorherigen Schritt identifiziert wurde.

   ```html
   <!-- Experience Fragment Header -->
   <div class="experiencefragment aem-GridColumn aem-GridColumn--default--12">
       ...
   </div>
   <!-- Main body Layout Container -->
   <div class="responsivegrid cmp-layout-container--fixed aem-GridColumn aem-GridColumn--default--12">
       ...
   </div>
   <!-- Experience Fragment Footer -->
   <div class="experiencefragment aem-GridColumn aem-GridColumn--default--12">
       ...
   </div>
   ```

1. Aktualisieren Sie die Datei `container.scss` unter: `ui.frontend/src/main/webpack/components/content/container/scss/container.scss`:

   ```css
   /* WKND Layout Container - Fixed Width */
   
   .cmp-layout-container--fixed {
       @media (min-width: $screen-medium + 1) {
           display:block;
           max-width:  $max-width !important;
           float: unset !important;
           margin: 0 auto !important;
           padding: 0 $gutter-padding;
           clear: both !important;
       }
   }
   ```

1. Aktualisieren Sie die Datei `_elements.scss` unter: `ui.frontend/src/main/webpack/base/sass/_elements.scss` und ändern Sie die `.root`-Regel, damit die maximale Breite auf die Variable `$max-body-width` eingestellt wird.

   ```css
    /* Before */
    body {
        ...
   
        .root {
            max-width: $max-width;
            margin: 0 auto;
            padding-top: 12px;
        }
    }
   ```

   ```css
    /* After */
    body {
        ...
   
        .root {
            max-width: $max-body-width;
            margin: 0 auto;
            padding-top: 12px;
        }
    }
   ```

   >[!NOTE]
   >
   > Die vollständige Liste der Variablen und Werte finden Sie unter: `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

1. Zurück zum Browser sollten Sie sehen, dass der Hauptinhalt der Seite gleich aussieht, die Kopf- und Fußzeile jedoch viel breiter ist. Dies ist zu erwarten.

   ![Container mit festem Layout - Webpack-Server](assets/style-system/fixed-layout-container-webpack-server.png)

### Container-Richtlinie für Layout aktualisieren

Als Nächstes fügen wir den Stil für die feste Breite hinzu, indem wir die Container-Richtlinien in AEM aktualisieren.

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zum Ordner **Artikelseitenvorlage** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html).

1. Wählen Sie im Modus **Struktur** den Container **Layout** (zwischen Erlebnisfragment-Kopf und Fußzeile) und klicken Sie auf das Symbol **Richtlinie**.

   ![Container des Haupttextlayouts konfigurieren](assets/style-system/layout-container-article-template-policy-icon.png)

1. Aktualisieren Sie die **WKND-Site-Standard**-Richtlinie, um einen zusätzlichen Stil für **Feste Breite** mit dem Wert `cmp-layout-container--fixed` einzuschließen:

   ![Aktualisierung der WKND-Site-Standardrichtlinie  ](assets/style-system/wknd-site-default-policy-update-fixed-width.png)

   Speichern Sie Ihre Änderungen und verweisen Sie auf die Seite &quot;Artikelseitenvorlage&quot;.

1. Wählen Sie erneut den Container **Layout** (zwischen der Kopf- und Fußzeile des Erlebnisfragments) aus. Diesmal sollte das Symbol **Paintbrush** angezeigt werden und Sie können **Feste Breite** aus der Dropdownliste Stil auswählen.

   ![Container für Layout mit fester Breite anwenden](assets/style-system/apply-fixed-width-layout-container.png)

   Sie sollten die Stile ein-/ausschalten können.

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Sie sollten den Container mit fester Breite in Aktion sehen.

## Kopf-/Fußzeile - Erlebnisfragment {#experience-fragment}

Als Nächstes fügen wir der Kopf- und Fußzeile Stile hinzu, um die Artikelseitenvorlage fertigzustellen. Sowohl die Kopf- als auch die Fußzeile wurden als Erlebnisfragment implementiert, bei dem es sich um eine Gruppierung von Komponenten innerhalb eines Containers handelt. Wir können eine eindeutige CSS-Klasse auf Experience Fragment-Komponenten anwenden, genau wie andere Core-Komponenten mit dem Style System.

### Kopfzeilenstil implementieren - ui.frontend

Die Komponenten in der Header-Komponente sind bereits so gestaltet, dass sie mit den [AdobeXD-Designs](assets/pages-templates/wknd-article-design.xd) übereinstimmen. Es sind nur einige kleine Layout-Änderungen erforderlich.

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. Öffnen Sie die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`.
1. Suchen Sie die Instanz **first** der Komponente &quot;Erlebnisfragment&quot;, indem Sie nach *class=&quot;experiencefragment* (Zeile 48) suchen.
1. hinzufügen Sie die Klasse `cmp-experiencefragment--header` auf die `div`, die im vorherigen Schritt identifiziert wurde.

   ```html
       ...
       <div class="root responsivegrid">
           <div class="aem-Grid aem-Grid--12 aem-Grid--default--12 ">
   
           <!-- add cmp-experiencefragment--header -->
           <div class="experiencefragment cmp-experiencefragment--header aem-GridColumn aem-GridColumn--default--12">
               ...
   ```

1. Öffnen Sie die Datei `experiencefragment.scss` unter: `ui.frontend/src/main/webpack/components/content/experiencefragment/scss/experiencefragment.scss`. Hängen Sie die folgenden Stile an die Datei an:

   ```css
   /* Header Style */
   .cmp-experiencefragment--header {
   
       .cmp-experiencefragment {
           max-width: $max-width;
           margin: 0 auto;
       }
   
       /* Logo Image */
       .cmp-image__image {
           max-width: 8rem;
           margin-top: $gutter-padding / 2;
           margin-bottom: $gutter-padding / 2;
       }
   
       @media (max-width: $screen-medium) {
   
           .cmp-experiencefragment {
               padding-top: 1rem;
               padding-bottom: 1rem;
           }
           /* Logo Image */
           .cmp-image__image {
               max-width: 6rem;
               margin-top: .75rem;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > Wir nehmen hier eine Abkürzung, um das Logo in der Kopfzeile zu gestalten. Das Logo ist eigentlich nur eine Bildkomponente, die sich zufällig im Erlebnisfragment befindet. Sagen wir später, wir mussten ein weiteres Bild zur Kopfzeile hinzufügen, wir wären nicht in der Lage, zwischen den beiden unterscheiden. Bei Bedarf kann der Image-Komponente immer eine &quot;logo&quot;-Klasse hinzugefügt werden.

1. Kehren Sie zum Browser zurück und Ansicht des Webpack-Dev-Servers. Die Kopfzeilenstile sollten aktualisiert werden, damit sie besser an den Rest des Inhalts ausgerichtet werden. Wenn Sie den Browser auf die Breite eines Tablet-/Mobilgeräts verkleinern, sollten Sie auch bemerken, dass das Logo besser in der Größe angezeigt wird.

   ![Erlebnisfragment-Kopfzeile](assets/style-system/header-experience-fragment-webpack.png)

### Implementieren des Fußzeilenstils - ui.frontend

Die Fußzeile in [AdobeXD-Entwürfen](assets/pages-templates/wknd-article-design.xd) enthält einen schwarzen Hintergrund mit hellem Text. Wir müssen den Inhalt in unserer Experience Fragment-Fußzeile gestalten, um dies widerzuspiegeln.

1. Öffnen Sie die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`.

1. Suchen Sie die Instanz **second** der Komponente &quot;Erlebnisfragment&quot;, indem Sie nach *class=&quot;experiencefragment* (Zeile 385) suchen.

1. hinzufügen Sie die Klasse `cmp-experiencefragment--footer` auf die `div`, die im vorherigen Schritt identifiziert wurde.

   ```html
   <!-- add cmp-experiencefragment--footer -->
   <div class="experiencefragment cmp-experiencefragment--footer aem-GridColumn aem-GridColumn--default--12">
   ```

1. Öffnen Sie die Datei `experiencefragment.scss` erneut unter: `ui.frontend/src/main/webpack/components/content/experiencefragment/scss/experiencefragment.scss`. **Fügen Sie der Datei die folgenden Stile** hinzu:

   ```css
   /* Footer Style */
   .cmp-experiencefragment--footer {
   
       background-color: $black;
       color: $gray-light;
       margin-top: 5rem;
   
       p {
           font-size: $font-size-small;
       }
   
       .cmp-experiencefragment {
           max-width: $max-width;
           margin: 0 auto;
           padding-bottom: 0rem;
       }
   
       /* Separator */
       .cmp-separator {
           margin-top: 2rem;
           margin-bottom: 2rem;
       }
   
       .cmp-separator__horizontal-rule {
           border: 0;
       }
   
       /* Navigation */
       .cmp-navigation__item-link {
           color: $nav-link-inverse;
           &:hover,
           &:focus {
               background-color: unset;
               text-decoration: underline;
           }
       }
   
       .cmp-navigation__item--level-1.cmp-navigation__item--active .cmp-navigation__item-link {
           background-color: unset;
           color: $gray-lighter;
           text-decoration: underline;
       }
   
   }
   ```

   >[!CAUTION]
   >
   > Wieder nehmen wir eine Abkürzung, indem wir die Standardstile der Navigationskomponente aus unserer Experience Fragment-Fußzeile CSS überschreiben. Es ist unwahrscheinlich, dass es je mehrere Navigationskomponenten in der Fußzeile geben würde, und ebenso unwahrscheinlich, dass ein Inhaltsersteller einen Navigationsstil umschalten möchte. Eine bessere Vorgehensweise wäre, einen Fußzeilenstil nur für die Navigationskomponente zu erstellen.

1. Kehren Sie zum Browser und Webpack Dev-Server zurück. Die Fußzeilenstile sollten aktualisiert werden, damit sie den XD näher entsprechen.

   ![Fußzeile](assets/style-system/footer-webpack-style.png)

1. Beenden Sie den Webpack-Entwicklungsserver.

### Erlebnisfragment-Richtlinie aktualisieren

Als Nächstes fügen wir die Kopf- und Fußzeilenstile hinzu, indem wir die Komponentenrichtlinie für Erlebnisfragmente in AEM aktualisieren.

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zum Ordner **Artikelseitenvorlage** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page-template/structure.html).

1. Wählen Sie im Modus **Struktur** die Option Überschrift **Erlebnisfragment** und klicken Sie dann auf das Symbol **Richtlinie**.

   ![Erlebnisfragment-Richtlinie konfigurieren](assets/style-system/experience-fragment-click-policy.png)

1. Aktualisieren Sie die **WKND Site Experience Fragment - Header**-Richtlinie, um eine **Standard-CSS-Klasse** mit dem Wert `cmp-experiencefragment--header` hinzuzufügen:

   ![WKND Site Experience Fragment - Header-Update](assets/style-system/experience-fragment-header-policy-configure.png)

   Speichern Sie Ihre Änderungen, und Sie sollten nun sehen, wie die richtigen CSS-Stile für Kopfzeilen angewendet werden.

   >[!NOTE]
   >
   > Da es nicht nötig ist, den Header-Stil anders als in der Vorlage umzuschalten, können wir ihn einfach als Standard-CSS-Stil festlegen.

1. Wählen Sie anschließend die Fußzeile **Erlebnisfragment** und klicken Sie auf das Symbol **Richtlinie**, um die Richtlinienkonfiguration zu öffnen.

1. Aktualisieren Sie die **WKND Site Experience Fragment - Footer**-Richtlinie, um eine **Standard-CSS-Klasse** mit dem Wert `cmp-experiencefragment--footer` hinzuzufügen:

   ![WKND-Site-Erlebnisfragment - Fußzeilenaktualisierung](assets/style-system/experience-fragment-footer-policy-configure.png)

   Speichern Sie Ihre Änderungen, und Sie sollten sehen, wie die CSS-Stile der Fußzeile angewendet werden.

   ![WKND-Artikelvorlage - Endgültige Stile](assets/style-system/final-header-footer-applied.png)

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Es sollte die aktualisierte Kopf- und Fußzeile angewendet werden.

## Überprüfen {#review}

Überprüfen Sie die Stile und Funktionen, die als Teil des Kapitels implementiert wurden.

>[!VIDEO](https://video.tv.adobe.com/v/30378/?quality=12&learn=on)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, die Artikelseite ist fast vollständig formatiert und Sie haben praktische Erfahrungen mit dem AEM Style System gesammelt.

### Nächste Schritte {#next-steps}

Lernen Sie die Schritte von Ende zu Ende zu erstellen, um eine [benutzerdefinierte AEM-Komponente](custom-component.md) zu erstellen, die in einem Dialog verfasste Inhalte anzeigt, und untersuchen Sie die Entwicklung eines Sling-Modells, um eine Geschäftslogik zu kapseln, die die HTML der Komponente ausfüllt.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `style-system/solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `style-system/solution` an.
