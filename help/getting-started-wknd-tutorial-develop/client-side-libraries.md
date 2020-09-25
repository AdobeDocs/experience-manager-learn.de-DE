---
title: Clientseitige Bibliotheken und Front-End-Workflow
description: Erfahren Sie, wie clientlibs und clientlibs zum Bereitstellen und Verwalten von CSS und JavaScript für eine Implementierung von Adobe Experience Manager (AEM) Sites verwendet werden. In diesem Lernprogramm wird auch erläutert, wie das Modul ui.frontend, ein Webpack-Projekt, in den End-to-End-Build-Prozess integriert werden kann.
sub-product: Sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 6%

---


# Clientseitige Bibliotheken und Front-End-Workflow {#client-side-libraries}

Erfahren Sie, wie clientlibs und clientlibs zum Bereitstellen und Verwalten von CSS und JavaScript für eine Implementierung von Adobe Experience Manager (AEM) Sites verwendet werden. In diesem Lernprogramm wird auch erläutert, wie das [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) -Modul, ein entkoppeltes [Webpack](https://webpack.js.org/) -Projekt, in den End-to-End-Build-Prozess integriert werden kann.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

Es wird außerdem empfohlen, das Tutorial [Komponentengrundlagen](component-basics.md#client-side-libraries) zu lesen, um die Grundlagen clientseitiger Bibliotheken und AEM zu verstehen.

### Starterprojekt

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Klonen Sie das [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) -Repository.
1. Sehen Sie sich die `client-side-libraries/start` Verzweigung an

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `client-side-libraries/solution`.

## Vorgabe

1. Verstehen Sie, wie clientseitige Bibliotheken über eine bearbeitbare Vorlage auf eine Seite eingefügt werden.
1. Erfahren Sie, wie Sie das UI.Frontend-Modul und einen Webpack-Entwicklungsserver für die dedizierte Front-End-Entwicklung verwenden.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf für die Bereitstellung von kompiliertem CSS und JavaScript in einer Sites-Implementierung vertraut.

## Was Sie erstellen {#what-you-will-build}

In diesem Kapitel fügen Sie einige grundlegende Stile für die WKND-Site und die Artikelseitenvorlage hinzu, um die Implementierung näher an die [UI-Designmodelle](assets/pages-templates/wknd-article-design.xd)heranzuführen. Sie verwenden einen erweiterten Front-End-Arbeitsablauf, um ein Webpack-Projekt in eine AEM Client-Bibliothek zu integrieren.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Hintergrund {#background}

Clientseitige Bibliotheken bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Die grundlegenden Ziele für clientseitige Bibliotheken oder clientlibs sind:

1. CSS/JS in kleinen, diskreten Dateien speichern, um die Entwicklung und Wartung zu erleichtern
1. Verwalten von Abhängigkeiten von Drittanbieter-Frameworks auf organisierte Weise
1. Minimieren Sie die Anzahl der clientseitigen Anforderungen, indem Sie CSS/JS in eine oder zwei Anforderungen verketten.

Weitere Informationen zur Verwendung Client-seitiger Bibliotheken [finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html).

Clientseitige Bibliotheken haben einige Einschränkungen. Vor allem ist die Unterstützung für gängige Front-End-Sprachen wie Sass, LESS und TypeScript eingeschränkt. Im Tutorial werden wir sehen, wie das **ui.frontend** Modul helfen kann, dies zu lösen.

Stellen Sie die Startercodebasis in einer lokalen AEM bereit und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Diese Seite ist derzeit nicht formatiert. Als Nächstes implementieren wir clientseitige Bibliotheken für die Marke WKND, um CSS und Javascript zur Seite hinzuzufügen.

## Clientseitige Bibliotheksorganisation {#organization}

Als Nächstes werden wir die Organisation der clientlibs durch das [AEM Projekt Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html).

![Allgemeine clientlibrary-Organisation](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramm auf hoher Ebene Client-seitige Bibliotheksorganisation und Seiteninklusion*

>[!NOTE]
>
> Die folgende clientseitige Bibliotheksorganisation wird von AEM Project Archetype erzeugt, stellt aber lediglich einen Ausgangspunkt dar. Die Art und Weise, wie ein Projekt letztendlich eine Implementierung von CSS und JavaScript auf Sites verwaltet und bereitstellt, kann je nach Ressourcen, Fertigkeiten und Anforderungen drastisch variieren.

1. Öffnen Sie mithilfe von Eclipse oder einer anderen IDE das Modul **ui.apps** .
1. Erweitern Sie den Pfad `/apps/wknd/clientlibs` zur Ansicht der vom Archetyp generierten clientlibs.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Wir werden diese clientlibs im Folgenden genauer untersuchen.

1. Inspect die Eigenschaften von `clientlibs/clientlib-base`.

   **clientlib-base** stellt die Basisebene von CSS und JavaScript dar, die für die Funktion der WKND-Site erforderlich sind. Beachten Sie die Eigenschaft, `categories` die auf `wknd.base`. `categories` ist ein Tag-Mechanismus für clientlibs und ist, wie sie referenziert werden können.

   Beachten Sie die `embed` Eigenschaft und den `String[]` Wert. Die `embed` Eigenschaft bettet andere clientlibs je nach Kategorie ein. **clientlib-base** enthält alle benötigten AEM Core Component-Client-Bibliotheken. Dazu gehören Artefakte wie Javascript für das Karussell, Schnellsuchkomponenten, die funktionieren sollen. **clientlib-base** wird keine eigenen CSS- und JavaScript-Dateien einschließen, sondern lediglich andere Client-Bibliotheken einbetten. **clientlib-base** bettet clientlib- **grid** clientlib mit der Kategorie von `wknd.grid`ein.

   Beachten Sie, dass die `allowProxy` Eigenschaft auf `true`. Es ist eine Best Practice, immer `allowProxy=true` auf clientlibs einzustellen. Die `allowProxy` Eigenschaft ermöglicht es uns, die clientlibs mit unserem Anwendungscode unter zu speichern, liefert dann die clientlibs über einen Pfad, der mit vorangestellt wird, `/apps` aber **** `/etc.clientlibs` , um zu vermeiden, dass Anwendungscode an Endbenutzer offen gelegt wird. Weitere Informationen über die Eigenschaft „allowProxy“ [finden Sie hier.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect die Eigenschaften von `clientlibs/clientlib-grid`.

   **clientlib-grid** ist für das Einschließen/Generieren des CSS verantwortlich, das für die Verwendung des [Layoutmodus](https://docs.adobe.com/content/help/de-DE/experience-manager-65/authoring/siteandpage/responsive-layout.html) mit dem AEM Sites Editor erforderlich ist. **clientlib-grid** hatte eine Kategorie auf `wknd.grid` eingestellt und wird über **clientlib-base** eingebettet.

   Das Raster kann angepasst werden, um verschiedene Spaltenmengen und Haltepunkte zu verwenden. Als Nächstes werden die generierten Standard-Haltepunkte aktualisiert.

1. Aktualisieren Sie die Datei `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   Dadurch werden die Haltepunkte geändert, um unseren in `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`&quot;Vorlage&quot;festgelegten Haltepunkten zu entsprechen.

   Beachten Sie, dass diese Datei tatsächlich auf eine `grid_base.less` Datei verweist, unter `/libs` der ein benutzerdefiniertes Mixin zum Generieren des Rasters enthalten ist.

1. Inspect die Eigenschaften für `clientlibs/clientlib-site`.

   **clientlib-site** enthält alle Site-spezifischen Stile für die Marke WKND. Notieren Sie die Kategorie von `wknd.site`. Die CSS und Javascript, die diese clientlib generiert, werden tatsächlich im `ui.frontend` Modul gepflegt. Wir werden diese Integration als Nächstes untersuchen.

1. Inspect die Eigenschaften für `clientlibs/clientlib-dependencies`.

   **clientlib-Abhängigkeiten** sollen alle Drittanbieter-Abhängigkeiten einbetten. Es handelt sich um eine separate clientlib, sodass sie bei Bedarf oben auf der HTML-Seite geladen werden kann. Notieren Sie die Kategorie von `wknd.dependencies`. Die CSS und Javascript, die diese clientlib generiert, werden tatsächlich im `ui.frontend` Modul gepflegt. Wir werden diese Integration später im Tutorial untersuchen.

## Using the ui.frontend module {#ui-frontend}

Als Nächstes werden wir die Verwendung des Moduls **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** untersuchen.

### Motivation

Clientseitige Bibliotheken haben einige Einschränkungen bei der Unterstützung von Sprachen wie [Sass](https://sass-lang.com/) oder [TypeScript](https://www.typescriptlang.org/). Es gab auch eine Explosion von Open-Source-Tools wie [NPM](https://www.npmjs.com/) und [Webpack](https://webpack.js.org/) , die die Front-End-Entwicklung beschleunigen und optimieren.

Die Grundidee hinter dem Modul **ui.frontend** besteht darin, großartige Werkzeuge wie NPM und Webpack zu verwenden, um einen Großteil der Front-End-Entwicklung zu verwalten. Ein wichtiges Integrationselement, das in das **ui.frontend** -Modul integriert ist, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) nimmt die kompilierten CSS- und JS-Artefakte eines Webpack/npm-Projekts und wandelt sie in AEM clientseitigen Bibliotheken um. Dies gibt einem Front-End-Entwickler mehr Freiheit, verschiedene Werkzeuge und Technologien auszuwählen.

![Integration der Frontend-Architektur](assets/client-side-libraries/ui-frontend-architecture.png)

### Verwenden

Jetzt fügen wir einige Basisstile für die Marke WKND hinzu, indem wir einige Sass-Dateien (`.scss` Erweiterung) über das Modul **ui.frontend** hinzufügen.

1. Öffnen Sie das Modul **ui.frontend** und navigieren Sie zu `src/main/webpack/base/sass`.

   ![ui.frontend-Modul](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Erstellen Sie eine neue Datei mit dem Namen `_variables.scss` unter dem Ordner `src/main/webpack/base/sass`.
1. Füllen Sie `_variables.scss` wie folgt:

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Mit Sass können wir Variablen erstellen, die dann in verschiedenen Dateien verwendet werden können, um Konsistenz zu gewährleisten. Beachten Sie die Schriftfamilien. Später in der Übung werden wir sehen, wie wir einen Aufruf an Google Web-Schriftarten, um diese Schriftarten verwenden können.

1. Erstellen Sie eine weitere Datei mit dem Namen `_elements.scss` darunter `src/main/webpack/base/sass` und füllen Sie sie wie folgt aus:

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Beachten Sie, dass die `_elements.scss` Datei die Variablen in der `_variables.scss`Datei verwendet.

1. Aktualisieren Sie `_shared.scss` unten, `src/main/webpack/base/sass` um die `_elements.scss` und `_variables.scss` Dateien einzuschließen.

   ```css
   @import './variables';
   @import './elements';
   ```

1. Öffnen Sie ein Befehlszeilenterminal und installieren Sie das **ui.frontend** -Modul mithilfe des `npm install` Befehls:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` muss nur einmal ausgeführt werden, nach einem neuen Klon oder einer Generation des Projekts.

1. Erstellen Sie im selben Terminal das **Modul ui.frontend** und stellen Sie es mithilfe des `npm run dev` Befehls bereit:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   Der Befehl `npm run dev` sollte den Quellcode für das Webpack-Projekt erstellen und kompilieren und schließlich die **clientlib-site** - und **clientlib-Abhängigkeiten** im Modul **ui.apps** füllen.

   >[!NOTE]
   >
   >Es gibt auch ein `npm run prod` Profil, das die JS und CSS minimiert. Dies ist die Standardkompilierung, wenn der Webpack-Build über Maven ausgelöst wird. Weitere Informationen zum Modul [ui.frontend finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect die Datei `site.css` darunter `ui.frontend/dist/clientlib-site/css/site.css`. Beachten Sie, dass das CSS hauptsächlich aus Inhalten der zuvor erstellten `_elements.scss` Datei besteht, die Variablen jedoch durch tatsächliche Werte ersetzt wurden.

   ![Verteilte Site-CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect die Datei `ui.frontend/clientlib.config.js`. Dies ist die Konfigurationsdatei für ein npm-Plugin, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-generator** ist das Tool, das für die Transformation des kompilierten CSS/JavaScript und das Kopieren in das **ui.apps** Modul verantwortlich ist.

1. Inspect Sie die Datei `site.css` im **Modul ui.apps** unter `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dies sollte eine identische Kopie der `site.css` Datei aus dem **ui.frontend** Modul sein. Nun, da es sich im **ui.apps** -Modul befindet, kann es für AEM bereitgestellt werden.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** während der Buildzeit entweder mit **npm** oder maven **kompiliert wird, kann es tatsächlich aus der Quellcodeverwaltung im Modul** ui.apps **** ignoriert werden. Inspect Sie die `.gitignore` Datei unter **ui.apps**.

>[!CAUTION]
>
> Die Verwendung des **ui.frontend** Moduls ist möglicherweise nicht für alle Projekte erforderlich. Das Modul **ui.frontend** erhöht die Komplexität und wenn es nicht notwendig/wünschenswert ist, einige dieser erweiterten Front-End-Werkzeuge zu verwenden (Sass, Webpack, npm...), kann es zu übertönen sein. Aus diesem Grund wird es als optionaler Teil des AEM Project Archetype betrachtet und die Verwendung von standardmäßigen clientseitigen Bibliotheken und Vanille CSS und JavaScript wird weiterhin vollständig unterstützt.

## Aufnahme von Seiten und Vorlagen {#page-inclusion}

Als Nächstes werden wir prüfen, wie das Projekt so eingerichtet ist, dass die clientlibs in AEM Vorlagen/Seiten aufgenommen werden. Eine gängige Best Practice bei der Webentwicklung ist es, CSS vor dem Schließen des `<head>` Tags in den HTML-Header `</body>` und JavaScript einzuschließen.

1. Navigieren Sie im **Modul ui.apps** zu `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Komponente &quot;Strukturseite&quot;](assets/client-side-libraries/customheaderlibs-html.png)

   Dies ist die `page` Komponente, die zum Rendern aller Seiten in der WKND-Implementierung verwendet wird.

1. Öffnen Sie die Datei `customheaderlibs.html`. Beachten Sie die Linien `${clientlib.css @ categories='wknd.base'}`. Dies bedeutet, dass das CSS für die clientlib mit einer Kategorie von über diese Datei eingeschlossen `wknd.base` wird, inklusive **clientlib-base** in der Kopfzeile aller unserer Seiten.

1. Aktualisieren Sie, `customheaderlibs.html` um einen Verweis auf Google-Schriftstile aufzunehmen, den wir zuvor im Modul **ui.frontend** angegeben haben. Auch ContextHub werden wir vorerst kommentieren...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect die Datei `customfooterlibs.html`. Diese Datei, wie `customheaderlibs.html` soll durch die Implementierung von Projekten überschrieben werden. Hier `${clientlib.js @ categories='wknd.base'}` bedeutet die Zeile, dass das JavaScript von **clientlib-base** unten auf allen Seiten eingeschlossen wird.

1. Erstellen und Bereitstellen des Projekts auf einer lokalen AEM mit Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Gehen Sie zu den WKND-Vorlagen unter [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Wählen Sie die **Artikelseitenvorlage** aus und öffnen Sie sie im Vorlageneditor.

   ![Artikelseitenvorlage auswählen](assets/client-side-libraries/open-article-page-template.png)

1. Klicken Sie auf das Symbol &quot; **Seiteninformationen** &quot;und wählen Sie im Menü die Option &quot; **Seitenrichtlinie** &quot;aus, um das Dialogfeld &quot; **Seitenrichtlinie** &quot;zu öffnen.

   ![Seitenrichtlinien für das Menü &quot;Artikelvorlage&quot;](assets/client-side-libraries/template-page-policy.png)

   *Seiteninformationen > Seitenrichtlinie*

1. Beachten Sie, dass die Kategorien für `wknd.dependencies` und `wknd.site` hier aufgeführt sind. Standardmäßig werden clientlibs, die über die Seitenrichtlinie konfiguriert wurden, aufgeteilt, um die CSS in den Seitenkopf und das JavaScript am Textende einzuschließen. Auf Wunsch können Sie explizit Listen zum Laden des clientlib JavaScript im Seitenkopf vornehmen. Dies ist der Fall `wknd.dependencies`.

   ![Seitenrichtlinien für das Menü &quot;Artikelvorlage&quot;](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Es ist auch möglich, direkt auf die `wknd.site` oder `wknd.dependencies` von der Seitenkomponente zu verweisen, indem Sie das `customheaderlibs.html` oder `customfooterlibs.html` Skript verwenden, wie wir früher für die `wknd.base` clientlib gesehen haben. Die Verwendung der Vorlage bietet eine gewisse Flexibilität, da Sie auswählen können, welche clientlibs pro Vorlage verwendet werden. Beispiel: Sie haben eine sehr schwere JavaScript-Bibliothek, die nur für eine ausgewählte Vorlage verwendet wird.

1. Navigieren Sie zur Seite **LA Skateparks** , die mithilfe der **Artikelseitenvorlage** erstellt wurde: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Sie sollten einen Unterschied bei den Schriftarten und einigen grundlegenden Stilen sehen, die angewendet werden, um anzuzeigen, dass das im Modul **ui.frontend** erstellte CSS funktioniert.

1. Klicken Sie auf das Symbol &quot; **Seiteninformationen** &quot;und wählen Sie im Menü die Option &quot; **Ansicht als veröffentlicht** &quot;aus, um die Artikelseite außerhalb des AEM-Editors zu öffnen.

   ![Als veröffentlicht anzeigen](assets/client-side-libraries/view-as-published-article-page.png)

1. Ansicht der Seitenquelle von [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) und Sie sollten die folgenden clientlib-Verweise im sehen `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Beachten Sie, dass clientlibs den Proxy- `/etc.clientlibs` Endpunkt verwenden. Am unteren Seitenrand sollte außerdem die folgende clientlib-Datei angezeigt werden:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Es ist auf der Veröffentlichungsseite wichtig, dass die Client-Bibliotheken **nicht** von **/Apps** bereitgestellt werden, da dieser Pfad aus Sicherheitsgründen mithilfe des [Dispatcher-Filterabschnitts](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)eingeschränkt werden sollte. Die [allowProxy-Eigenschaft](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) der Client-Bibliothek stellt sicher, dass die CSS- und JS-Dienste von **/etc.clientlibs** bereitgestellt werden.

## Webpack DevServer {#webpack-dev-server}

In den vorherigen Übungen konnten wir mehrere Sass-Dateien im **ui.frontend** Modul aktualisieren und durch einen Build-Prozess sehen, dass diese Änderungen letztendlich in AEM reflektiert werden. Als Nächstes werden wir uns mit einem [Webpack-dev-server](https://webpack.js.org/configuration/dev-server/) beschäftigen, um unsere Front-End-Stile schnell zu entwickeln.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Nachfolgend sind die Schritte auf hoher Ebene aufgeführt, die im Video gezeigt werden:

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem **ui.frontend** -Modul ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dadurch sollte ein neues Browserfenster unter [http://localhost:8080/](http://localhost:8080/) mit statischem Markup geöffnet werden.
1. Kopieren Sie die Seitenquelle der Artikelseite &quot;LA skatepark&quot;unter [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Fügen Sie das kopierte Markup aus AEM in das `index.html` Modul **ui.frontend** unten ein `src/main/webpack/static`.
1. Bearbeiten Sie das kopierte Markup und entfernen Sie alle Verweise auf **clientlib-site** - und **clientlib-Abhängigkeiten**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Wir können diese Verweise entfernen, da der Webpack-Dev-Server diese Artefakte automatisch generiert.

1. Bearbeiten Sie die `.scss` Dateien und sehen Sie die Änderungen, die automatisch im Browser angezeigt werden.
1. Überprüfen Sie die `/aem-guides-wknd.ui.frontend/webpack.dev.js` Datei. Dies enthält die Webpack-Konfiguration, die zum Beginn des Webpack-dev-Servers verwendet wird. Beachten Sie, dass es die Pfade `/content` und `/etc.clientlibs` von einer lokal ausgeführten Instanz von AEM proximiert. So werden die Bilder und andere clientlibs (die nicht vom **ui.frontend** -Code verwaltet werden) zur Verfügung gestellt.

   >[!CAUTION]
   >
   > Die Image-src des statischen Markups verweist auf eine Live-Bildkomponente auf einer lokalen AEM. Bilder werden beschädigt angezeigt, wenn sich der Pfad zum Bild ändert, wenn AEM nicht gestartet wird oder wenn der Browser sich nicht bei der lokalen AEM angemeldet hat.
1. Sie können den Webpack-Server über die Befehlszeile **stoppen** , indem Sie `CTRL+C`.

## Putting It Together {#putting-it-together}

Der Schwerpunkt dieses Tutorials liegt auf clientseitigen Bibliotheken und potenziellen Front-End-Workflows, die in AEM integriert werden können. Vor diesem Hintergrund beschleunigen wir die Implementierung durch die Installation von [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), das einige Standardstile für Core-Komponenten bereitstellt, die in der Artikelseitenvorlage verwendet werden:

* [Breadcrumb](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/breadcrumb.html)
* [Download](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Bild](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html)
* [Liste](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/list.html)
* [Navigation](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/navigation.html)
* [Schnellsuche](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/components/quick-search.html)
* [Trennzeichen](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Nachfolgend sind die Schritte auf hoher Ebene aufgeführt, die im Video gezeigt werden:

1. Laden Sie [clientseitige Bibliotheken-final-styles.zip herunter und entpacken Sie die unten stehenden Inhalte aus dem ZIP-](assets/client-side-libraries/client-side-libraries-final-styles.zip) Archiv `ui.frontend/src/main/webpack`. Der Inhalt der ZIP-Datei sollte die folgenden Ordner überschreiben:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Vorschau der neuen Stile mithilfe des Webpack-Dev-Servers:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Stellen Sie die Code-Basis in einer lokalen AEM-Instanz bereit, um die neuen Stile anzuzeigen, die auf den Artikel des LA-Skate-Parks angewendet wurden:

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, die Artikelseite hat jetzt einige konsistente Stile, die mit der Marke WKND übereinstimmen und Sie sind mit dem **ui.frontend** Modul vertraut!

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. [Die Entwicklung mit dem Stilsystem](style-system.md) umfasst die Verwendung des Stilsystems zum Erweitern der Kernkomponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlageneditors.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder Überprüfung und Bereitstellung des Codes lokal auf der Git-Klammer `client-side-libraries/solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) -Repository.
1. Schau dir die `client-side-libraries/solution` Verzweigung an.

## Zusätzliche Werkzeuge und Ressourcen {#additional-resources}

### aemed {#develop-aemfed}

[**aemfeed**](https://aemfed.io/) ist ein Open-Source-Befehlszeilenwerkzeug, das zur Beschleunigung der Front-End-Entwicklung verwendet werden kann. Es wird mit [aemsync](https://www.npmjs.com/package/aemsync), [BrowserSync](https://www.npmjs.com/package/browser-sync) und [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)betrieben.

Auf einer hohen Ebene **aemfeed** ist so konzipiert, dass Dateiänderungen im Modul **ui.apps** überwacht und automatisch direkt mit einer laufenden AEM synchronisiert werden. Auf der Grundlage der Änderungen wird ein lokaler Browser automatisch aktualisiert, was die Entwicklung des Front-End beschleunigt. Es wurde auch für die Verwendung mit Sling Log Tracker entwickelt, um serverseitige Fehler automatisch direkt im Terminal anzuzeigen.

Wenn Sie eine Menge Arbeit im **ui.apps** -Modul machen, HTML-Skripte ändern und benutzerdefinierte Komponenten erstellen, kann **aemfeed** ein sehr leistungsfähiges Werkzeug sein. [Die vollständige Dokumentation finden Sie hier.](https://github.com/abmaonline/aemfed).

### Debuggen clientseitiger Bibliotheken {#debugging-clientlibs}

Bei verschiedenen Methoden der **Kategorien** und **Einbettung** , um mehrere Client-Bibliotheken einzuschließen, kann es schwerfällig sein, Fehler zu beheben. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Werkzeuge ist die **Neuerstellung von Client-Bibliotheken** , die AEM zwingen, alle LESS-Dateien neu zu kompilieren und die CSS zu generieren.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Liste aller in der AEM Instanz registrierten Clientbibliotheken. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testausgabe**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : Ermöglicht dem Benutzer, die erwartete HTML-Ausgabe von clientlib auf der Grundlage der Kategorie anzuzeigen. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Überprüfung**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) von Abhängigkeiten von Bibliotheken - hebt alle Abhängigkeiten oder eingebetteten Kategorien hervor, die nicht gefunden werden können. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-Bibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) neu erstellen: Ermöglicht es einem Benutzer, AEM zu erzwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache der Clientbibliotheken zu ungültigen. Dieses Tool ist besonders effektiv, wenn es mit LESS entwickelt wird, da dies AEM zwingen kann, die generierte CSS erneut zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu ungültigen und dann eine Seitenaktualisierung durchzuführen, anstatt alle Bibliotheken neu zu erstellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Client-Bibliothek neu erstellen](assets/client-side-libraries/rebuild-clientlibs.png)
