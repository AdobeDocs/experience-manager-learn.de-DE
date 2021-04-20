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
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3301'
ht-degree: 4%

---


# Clientseitige Bibliotheken und Front-End-Workflow {#client-side-libraries}

Erfahren Sie, wie clientlibs und clientlibs zum Bereitstellen und Verwalten von CSS und JavaScript für eine Implementierung von Adobe Experience Manager (AEM) Sites verwendet werden. In diesem Lernprogramm wird auch erläutert, wie das [ui.frontend](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/uifrontend.html)-Modul, ein entkoppeltes [Webpack](https://webpack.js.org/)-Projekt, in den End-to-End-Build-Prozess integriert werden kann.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

Es wird außerdem empfohlen, das Tutorial [Komponentengrundlagen](component-basics.md#client-side-libraries) zu lesen, um die Grundlagen clientseitiger Bibliotheken und AEM zu verstehen.

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt erneut verwenden und die Schritte zum Auschecken des Startprojekts überspringen.

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Sehen Sie sich die Verzweigung `tutorial/client-side-libraries-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) oder lokal prüfen, indem Sie zur Verzweigung `tutorial/client-side-libraries-solution` wechseln.

## Vorgabe

1. Verstehen Sie, wie clientseitige Bibliotheken über eine bearbeitbare Vorlage auf eine Seite eingefügt werden.
1. Erfahren Sie, wie Sie das UI.Frontend-Modul und einen Webpack-Entwicklungsserver für die dedizierte Front-End-Entwicklung verwenden.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf für die Bereitstellung von kompiliertem CSS und JavaScript in einer Sites-Implementierung vertraut.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel fügen Sie einige Baseline-Stile für die WKND-Site und die Artikelseitenvorlage hinzu, um die Implementierung näher an die [UI-Designmodelle](assets/pages-templates/wknd-article-design.xd) heranzuführen. Sie verwenden einen erweiterten Front-End-Arbeitsablauf, um ein Webpack-Projekt in eine AEM Client-Bibliothek zu integrieren.

![Fertige Stile](assets/client-side-libraries/finished-styles.png)

*Artikelseite mit angewendeten Basisstilen*

## Hintergrund {#background}

Clientseitige Bibliotheken bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Die grundlegenden Ziele für clientseitige Bibliotheken oder clientlibs sind:

1. CSS/JS in kleinen, diskreten Dateien speichern, um die Entwicklung und Wartung zu erleichtern
1. Verwalten von Abhängigkeiten von Drittanbieter-Frameworks auf organisierte Weise
1. Minimieren Sie die Anzahl der clientseitigen Anforderungen, indem Sie CSS/JS in eine oder zwei Anforderungen verketten.

Weitere Informationen zur Verwendung Client-seitiger Bibliotheken [finden Sie hier](https://docs.adobe.com/content/help/de-DE/experience-manager-65/developing/introduction/clientlibs.html).

Clientseitige Bibliotheken haben einige Einschränkungen. Vor allem ist die Unterstützung für gängige Front-End-Sprachen wie Sass, LESS und TypeScript eingeschränkt. Im Tutorial werden wir sehen, wie das **ui.frontend** Modul helfen kann, dies zu lösen.

Stellen Sie die Startercodebasis in einer lokalen AEM-Instanz bereit und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Diese Seite ist derzeit nicht formatiert. Als Nächstes implementieren wir clientseitige Bibliotheken für die Marke WKND, um CSS und Javascript zur Seite hinzuzufügen.

## Clientseitige Bibliotheksorganisation {#organization}

Anschließend werden wir die Organisation der clientlibs durch das [AEM Projekt Archetype](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/overview.html) generiert.

![Allgemeine clientlibrary-Organisation](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagramm auf hoher Ebene Client-seitige Bibliotheksorganisation und Seiteninklusion*

>[!NOTE]
>
> Die folgende clientseitige Bibliotheksorganisation wird von AEM Project Archetype erzeugt, stellt aber lediglich einen Ausgangspunkt dar. Die Art und Weise, wie ein Projekt letztendlich eine Implementierung von CSS und JavaScript auf Sites verwaltet und bereitstellt, kann je nach Ressourcen, Fertigkeiten und Anforderungen drastisch variieren.

1. Mit VSCode oder einer anderen IDE öffnen Sie das Modul **ui.apps**.
1. Erweitern Sie den Pfad `/apps/wknd/clientlibs`, um die vom Archetyp generierten clientlibs Ansicht.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Wir werden diese clientlibs im Folgenden genauer untersuchen.

1. Die folgende Tabelle fasst die Clientbibliotheken zusammen. Weitere Informationen zu [einschließlich Client-Bibliotheken finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Name | Beschreibung | Hinweise |
   |-------------------| ------------| ------|
   | `clientlib-base` | Basisebene von CSS und JavaScript, die für die Funktion der WKND-Site erforderlich sind | bettet Core Component client libs ein |
   | `clientlib-grid` | Generiert das für [Layoutmodus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) erforderliche CSS. | Hier können Haltepunkte für Mobilgeräte und Tablets konfiguriert werden |
   | `clientlib-site` | Enthält Site-spezifisches Design für die WKND-Site | Generiert durch das Modul `ui.frontend` |
   | `clientlib-dependencies` | Bettet alle Drittanbieter-Abhängigkeiten ein | Generiert durch das Modul `ui.frontend` |

1. Beachten Sie, dass `clientlib-site` und `clientlib-dependencies` von der Quellcodeverwaltung ignoriert werden. Dies geschieht standardmäßig, da diese während der Buildzeit vom Modul `ui.frontend` generiert werden.

## Basisstile {#base-styles} aktualisieren

Aktualisieren Sie anschließend die im Modul **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** definierten Basisstile. Die Dateien im Modul `ui.frontend` generieren die Bibliotheken `clientlib-site` und `clientlib-dependecies`, die das Site-Design und alle Abhängigkeiten von Drittanbietern enthalten.

Clientseitige Bibliotheken haben einige Einschränkungen bei der Unterstützung von Sprachen wie [Sass](https://sass-lang.com/) oder [TypeScript](https://www.typescriptlang.org/). Es gibt eine Reihe von Open-Source-Tools wie [NPM](https://www.npmjs.com/) und [webpack](https://webpack.js.org/), die die Front-End-Entwicklung beschleunigen und optimieren. Das Ziel des Moduls **ui.frontend** ist es, diese Tools zur Verwaltung der meisten Front-End-Quelldateien verwenden zu können.

1. Öffnen Sie das Modul **ui.frontend** und navigieren Sie zu `src/main/webpack/site`.
1. Öffnen Sie die Datei `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` ist der Einstiegspunkt für alle Sass-Dateien im  `ui.frontend` Modul. Es enthält die Datei `_variables.scss`, die eine Reihe von Markenvariablen enthält, die in verschiedenen Sass-Dateien im Projekt verwendet werden. Die Datei `_base.scss` ist ebenfalls enthalten und definiert einige grundlegende Stile für HTML-Elemente. Ein regulärer Ausdruck enthält alle Stile für einzelne Komponentenstile unter `src/main/webpack/components`. Ein anderer regulärer Ausdruck enthält alle Dateien unter `src/main/webpack/site/styles`.

1. Prüfen Sie die Datei `main.ts`. `main.ts` enthält  `main.scss` und enthält einen regulären Ausdruck, um alle  `.js` oder  `.ts` Dateien im Projekt zu sammeln. Dieser Einstiegspunkt wird von den Konfigurationsdateien [webpack](https://webpack.js.org/configuration/) als Einstiegspunkt für das gesamte `ui.frontend`-Modul verwendet.

1. Inspect Sie die Dateien unter `src/main/webpack/site/styles`:

   ![Stildateien](assets/client-side-libraries/style-files.png)

   Diese Dateistile für globale Elemente in der Vorlage, z. B. Kopf- und Fußzeile sowie Hauptinhalt-Container. Die CSS-Regeln in diesen Dateien Zielgruppe verschiedener HTML-Elemente `header`, `main` und `footer`. Diese HTML-Elemente wurden durch Richtlinien im vorherigen Kapitel [Seiten und Vorlagen](./pages-templates.md) definiert.

1. Erweitern Sie den Ordner `components` unter `src/main/webpack` und überprüfen Sie die Dateien.

   ![Komponentenauswertungsdateien](assets/client-side-libraries/component-sass-files.png)

   Jede Datei ist einer Kernkomponente wie der [Akkordeon-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components) zugeordnet. Jede Core-Komponente wird mit [Block-Elementmodifikator](https://getbem.com/) oder BEM-Notation erstellt, um die Zielgruppe bestimmter CSS-Klassen mit Stilregeln zu vereinfachen. Die Dateien unter `/components` wurden vom AEM Project Archetype mit den verschiedenen BEM-Regeln für jede Komponente gestubelt.

1. Laden Sie die WKND-Basisformate **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** und **unzip** herunter.

   ![WKND-Basisstile](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Um das Tutorial zu beschleunigen, haben wir die verschiedenen Sass-Dateien bereitgestellt, die die Marke WKND auf Basis der Kernkomponenten und der Struktur der Artikelseitenvorlage implementieren.

1. Überschreiben Sie den Inhalt von `ui.frontend/src` mit Dateien aus dem vorherigen Schritt. Der Inhalt der ZIP-Datei sollte die folgenden Ordner überschreiben:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Geänderte Dateien](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect Sie die geänderten Dateien, um Details zur Implementierung des WKND-Stils anzuzeigen.

## Inspect the ui.frontend integration {#ui-frontend-integration}

Ein wichtiges Integrationselement, das in das Modul **ui.frontend** integriert ist, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) nimmt die kompilierten CSS- und JS-Artefakte eines Webpack/npm-Projekts und wandelt sie in AEM clientseitigen Bibliotheken um.

![Integration der Frontend-Architektur](assets/client-side-libraries/ui-frontend-architecture.png)

Diese Integration wird vom AEM Project Archetype automatisch eingerichtet. Erfahren Sie, wie es funktioniert.


1. Öffnen Sie ein Befehlszeilenterminal und installieren Sie das Modul **ui.frontend** mithilfe des Befehls `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` muss nur einmal ausgeführt werden, nach einem neuen Klon oder einer Generation des Projekts.

1. Erstellen Sie im selben Terminal das Modul **ui.frontend** und stellen Sie es mithilfe des Befehls `npm run dev` bereit:

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Sie erhalten möglicherweise einen Fehler wie &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > Dies geschieht normalerweise, weil sich Ihre Umgebung seit der Ausführung von `npm install` geändert hat.
   > Führen Sie `npm rebuild node-sass` aus, um das Problem zu beheben. Dies geschieht, wenn sich die auf Ihrem lokalen Dev-Computer installierte Version von `npm` von der Version unterscheidet, die von der Maven `frontend-maven-plugin` in der Datei `aem-guides-wknd/pom.xml` verwendet wird. Sie können dies dauerhaft beheben, indem Sie die Version in der Pom-Datei so ändern, dass sie Ihrer lokalen Version entspricht oder umgekehrt.

1. Der Befehl `npm run dev` sollte den Quellcode für das Webpack-Projekt erstellen und kompilieren und letztendlich die **clientlib-site** und **clientlib-Abhängigkeiten** im Modul **ui.apps** füllen.

   >[!NOTE]
   >
   >Es gibt auch ein `npm run prod`-Profil, das JS und CSS minimiert. Dies ist die Standardkompilierung, wenn der Webpack-Build über Maven ausgelöst wird. Weitere Informationen zum Modul [ui.frontend finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect Sie die Datei `site.css` unter `ui.frontend/dist/clientlib-site/css/site.css`. Dies ist die kompilierte CSS basierend auf den Sass-Quelldateien.

   ![Verteilte Site-CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Prüfen Sie die Datei `ui.frontend/clientlib.config.js`. Dies ist die Konfigurationsdatei für ein npm-Plugin, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), das den Inhalt von `/dist` in eine Client-Bibliothek transformiert und in das `ui.apps`-Modul verschiebt.

1. Inspect Sie die Datei `site.css` im Modul **ui.apps** unter `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dies sollte eine identische Kopie der Datei `site.css` aus dem Modul **ui.frontend** sein. Nun, da es sich im Modul **ui.apps** befindet, kann es in AEM bereitgestellt werden.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** während der Buildzeit entweder mit **npm** oder **maven** kompiliert wird, kann es sicher aus der Quellcodeverwaltung im Modul **ui.apps** ignoriert werden. Inspect Sie die Datei `.gitignore` unter **ui.apps**.

1. Synchronisieren Sie die `clientlib-site`-Bibliothek mit einer lokalen Instanz von AEM mithilfe der Entwicklerwerkzeuge oder Maven-Fähigkeiten.

   ![Clientlib-Site synchronisieren](assets/client-side-libraries/sync-clientlib-site.png)

1. Öffnen Sie den Artikel &quot;LA Skatepark&quot;in AEM unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Aktualisierte Basisstile für den Artikel](assets/client-side-libraries/updated-base-styles.png)

   Sie sollten nun die aktualisierten Stile für den Artikel sehen. Möglicherweise müssen Sie eine harte Aktualisierung durchführen, um alle CSS-Dateien zu löschen, die im Browser zwischengespeichert werden.

   Es fängt an, den Mockups viel näher zu sehen!

   >[!NOTE]
   >
   > Die oben ausgeführten Schritte zum Erstellen und Bereitstellen des ui.frontend-Codes für AEM werden automatisch ausgeführt, wenn ein Maven-Build vom Stamm des Projekts `mvn clean install -PautoInstallSinglePackage` ausgelöst wird.

>[!CAUTION]
>
> Die Verwendung des Moduls **ui.frontend** ist möglicherweise nicht für alle Projekte erforderlich. Das Modul **ui.frontend** fügt zusätzliche Komplexität hinzu und wenn es nicht notwendig/wünschenswert ist, einige dieser erweiterten Front-End-Tools (Sass, Webpack, npm...) zu verwenden, ist es möglicherweise nicht erforderlich.

## Seiten- und Vorlageninklusion {#page-inclusion}

Als Nächstes sollten wir überprüfen, wie auf die clientlibs auf der AEM Seite verwiesen wird. Eine gängige Best Practice bei der Webentwicklung ist, CSS in die HTML-Kopfzeile `<head>` und JavaScript direkt einzuschließen, bevor das `</body>`-Tag geschlossen wird.

1. Navigieren Sie im Modul **ui.apps** zu `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Komponente &quot;Strukturseite&quot;](assets/client-side-libraries/customheaderlibs-html.png)

   Dies ist die Komponente `page`, die zum Rendern aller Seiten in der WKND-Implementierung verwendet wird.

1. Öffnen Sie die Datei `customheaderlibs.html`. Beachten Sie die Zeilen `${clientlib.css @ categories='wknd.base'}`. Dies bedeutet, dass das CSS für die clientlib mit der Kategorie `wknd.base` über diese Datei eingeschlossen wird, einschließlich **clientlib-base** in der Kopfzeile aller Seiten.

1. Aktualisieren Sie `customheaderlibs.html`, um einen Verweis auf Google-Schriftstile einzuschließen, den wir zuvor im Modul **ui.frontend** angegeben haben.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Prüfen Sie die Datei `customfooterlibs.html`. Diese Datei, z. B. `customheaderlibs.html` soll durch die Implementierung von Projekten überschrieben werden. Hier bedeutet die Zeile `${clientlib.js @ categories='wknd.base'}`, dass das JavaScript von **clientlib-base** unten auf allen Seiten eingeschlossen wird.

1. Exportieren Sie die Komponente `page` mithilfe der Entwicklerwerkzeuge oder Ihrer Maven-Fähigkeiten auf den AEM-Server.

1. Navigieren Sie zur Vorlage &quot;Artikelseite&quot;unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü **Seitenrichtlinie** aus, um das Dialogfeld **Seitenrichtlinie** zu öffnen.

   ![Seitenrichtlinien für das Menü &quot;Artikelvorlage&quot;](assets/client-side-libraries/template-page-policy.png)

   *Seiteninformationen > Seitenrichtlinie*

1. Beachten Sie, dass die Kategorien für `wknd.dependencies` und `wknd.site` hier aufgeführt sind. Standardmäßig werden clientlibs, die über die Seitenrichtlinie konfiguriert wurden, aufgeteilt, um die CSS in den Seitenkopf und das JavaScript am Textende einzuschließen. Auf Wunsch können Sie explizit Listen zum Laden des clientlib JavaScript im Seitenkopf vornehmen. Dies ist bei `wknd.dependencies` der Fall.

   ![Seitenrichtlinien für das Menü &quot;Artikelvorlage&quot;](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Es ist auch möglich, direkt auf das `wknd.site`- oder `wknd.dependencies`-Skript von der Seitenkomponente zu verweisen, indem das `customheaderlibs.html`- oder `customfooterlibs.html`-Skript verwendet wird, wie wir es zuvor für das `wknd.base`-clientlib gesehen haben. Die Verwendung der Vorlage bietet eine gewisse Flexibilität, da Sie auswählen können, welche clientlibs pro Vorlage verwendet werden. Beispiel: Sie haben eine sehr schwere JavaScript-Bibliothek, die nur für eine ausgewählte Vorlage verwendet wird.

1. Navigieren Sie zur Seite **LA Skateparks**, die mit der **Article Page Template** erstellt wurde: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Sie sollten einen Unterschied in den Schriftarten sehen.

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü **Ansicht als veröffentlicht** aus, um die Artikelseite außerhalb des AEM-Editors zu öffnen.

   ![Als veröffentlicht anzeigen](assets/client-side-libraries/view-as-published-article-page.png)

1. Ansicht der Seitenquelle von [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) und Sie sollten die folgenden clientlib-Verweise in `<head>` sehen können:

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Beachten Sie, dass clientlibs den Proxy `/etc.clientlibs`-Endpunkt verwenden. Am unteren Seitenrand sollte außerdem die folgende clientlib-Datei angezeigt werden:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Wenn Sie auf 6.5/6.4 folgen, werden die clientseitigen Bibliotheken nicht automatisch minimiert. Lesen Sie die Dokumentation zum [HTML-Bibliotheksmanager, um minfication (empfohlen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors) zu aktivieren.

   >[!WARNING]
   >
   >Auf der Seite &quot;Veröffentlichen&quot;ist es wichtig, dass die Client-Bibliotheken von **nicht** von **/apps** bereitgestellt werden, da dieser Pfad aus Sicherheitsgründen mithilfe des [Dispatcher-Filterabschnitts](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section) eingeschränkt werden sollte. Die [allowProxy-Eigenschaft](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) der Client-Bibliothek stellt sicher, dass CSS und JS von **/etc.clientlibs** bereitgestellt werden.

## Webpack DevServer - Statisches Markup {#webpack-dev-static}

In den vorherigen Übungen waren wir in der Lage, mehrere Sass-Dateien im Modul **ui.frontend** zu aktualisieren und durch einen Build-Prozess sehen Sie schließlich diese Änderungen in AEM. Als Nächstes werden wir uns mit Techniken befassen, die einen [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) nutzen, um unsere Front-End-Stile schnell gegen **static** HTML zu entwickeln.

Diese Technik ist praktisch, wenn die meisten Stile und der Front-End-Code von einem eigenen Front-End-Entwickler ausgeführt werden, der möglicherweise keinen einfachen Zugriff auf eine AEM Umgebung hat. Diese Technik ermöglicht es dem FED auch, Änderungen direkt am HTML vorzunehmen, die dann an einen AEM Entwickler übergeben werden können, um sie als Komponenten zu implementieren.

1. Kopieren Sie die Seitenquelle der Artikelseite LA skatepark unter [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Öffnen Sie die IDE erneut. Fügen Sie das kopierte Markup aus AEM in das `index.html`-Modul im **ui.frontend**-Modul unter `src/main/webpack/static` ein.
1. Bearbeiten Sie das kopierte Markup und entfernen Sie alle Verweise auf **clientlib-site** und **clientlib-Abhängigkeiten**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Wir können diese Verweise entfernen, da der Webpack-Dev-Server diese Artefakte automatisch generiert.

1. Beginn des Webpack-Dev-Servers von einem neuen Terminal, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dadurch sollte unter [http://localhost:8080/](http://localhost:8080/) ein neues Browserfenster mit statischem Markup geöffnet werden.

1. Bearbeiten Sie die Datei `src/main/webpack/site/_variables.scss`. Ersetzen Sie die `$text-color`-Regel durch Folgendes:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Speichern Sie die Änderungen.

1. Die Änderungen sollten automatisch im Browser unter [http://localhost:8080](http://localhost:8080) angezeigt werden.

   ![Änderungen am lokalen Webpack-Dev-Server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Überprüfen Sie die Datei `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Dies enthält die Webpack-Konfiguration, die zum Beginn des Webpack-dev-Servers verwendet wird. Beachten Sie, dass es die Pfade `/content` und `/etc.clientlibs` aus einer lokal ausgeführten Instanz von AEM proximiert. So werden die Bilder und andere clientlibs (die nicht vom **ui.frontend**-Code verwaltet werden) verfügbar gemacht.

   >[!CAUTION]
   >
   > Die Image-src des statischen Markups verweist auf eine Live-Bildkomponente auf einer lokalen AEM. Bilder werden beschädigt angezeigt, wenn sich der Pfad zum Bild ändert, wenn AEM nicht gestartet wurde oder wenn der Browser sich nicht bei der lokalen AEM angemeldet hat. Bei der Übergabe an eine externe Ressource ist es auch möglich, die Bilder durch statische Referenzen zu ersetzen.

1. Sie können **stop** den Webpack-Server über die Befehlszeile anhalten, indem Sie `CTRL+C` eingeben.

## Webpack DevServer - Watch und aemsync {#webpack-dev-watch}

Eine andere Methode besteht darin, Node.js auf Dateiänderungen an den src-Dateien im Modul `ui.frontend` zu achten. Bei jeder Dateiänderung wird die Client-Bibliothek schnell kompiliert und mit dem Modul [aemsync](https://www.npmjs.com/package/aemsync) npm synchronisiert, um die Änderungen mit einem laufenden AEM Server zu synchronisieren.

1. Beginn Sie den Webpack-Dev-Server im Modus **watch** von einem neuen Terminal, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Dadurch werden die `src`-Dateien kompiliert und die Änderungen mit AEM unter [http://localhost:4502](http://localhost:4502) synchronisiert.

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. Navigieren Sie zu AEM und zum Artikel LA Skateparks: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![An AEM bereitgestellte Änderungen](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Die Änderungen sollten in AEM bereitgestellt werden. Es gibt eine leichte Verzögerung, und Sie müssen den Browser manuell aktualisieren, um die Updates anzuzeigen. Die direkte Anzeige der Änderungen in AEM ist jedoch nützlich, wenn Sie mit neuen Komponenten und dem Erstellen von Dialogfeldern arbeiten.

1. Kehren Sie die Änderung zu `_variables.scss` zurück und speichern Sie die Änderungen. Die Änderungen sollten nach einer leichten Verzögerung erneut mit der lokalen Instanz der AEM synchronisiert werden.

1. Beenden Sie den Webpack-Dev-Server und führen Sie einen vollständigen Maven-Build aus dem Stammordner des Projekts aus:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wieder wird das Modul `ui.frontend` kompiliert, in clientlibraries transformiert und über das Modul `ui.apps` AEM bereitgestellt. Diesmal aber macht Maven alles für uns.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, die Artikelseite hat jetzt einige konsistente Stile, die mit der Marke WKND übereinstimmen und Sie sind mit dem **ui.frontend** Modul vertraut!

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. [Entwickeln mit dem Style ](style-system.md) SystemCovers unter Verwendung des Style Systems zur Erweiterung der Core-Komponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `tutorial/client-side-libraries-solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `tutorial/client-side-libraries-solution` an.

## Zusätzliche Tools und Ressourcen {#additional-resources}

### aemed {#develop-aemfed}

[**aemfedis**](https://aemfed.io/) ist ein Open-Source-Befehlszeilenwerkzeug, das zur Beschleunigung der Front-End-Entwicklung verwendet werden kann. Es wird mit [aemsync](https://www.npmjs.com/package/aemsync), [BrowserSync](https://www.npmjs.com/package/browser-sync) und [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html) betrieben.

Auf hoher Ebene ist **aemfeed** so ausgelegt, dass Dateiänderungen im **ui.apps**-Modul überwacht und automatisch direkt mit einer laufenden AEM synchronisiert werden. Auf der Grundlage der Änderungen wird ein lokaler Browser automatisch aktualisiert, was die Entwicklung des Front-End beschleunigt. Es wurde auch für die Verwendung mit Sling Log Tracker entwickelt, um serverseitige Fehler automatisch direkt im Terminal anzuzeigen.

Wenn Sie im Modul **ui.apps** viel Arbeit verrichten, HTML-Skripte ändern und benutzerdefinierte Komponenten erstellen, kann **aemfeed** ein sehr leistungsstarkes Werkzeug sein, das Sie verwenden können. [Die vollständige Dokumentation finden Sie hier.](https://github.com/abmaonline/aemfed).

### Debuggen clientseitiger Bibliotheken {#debugging-clientlibs}

Bei verschiedenen Methoden von **Kategorien** und **embetten**, um mehrere Client-Bibliotheken einzuschließen, kann es schwerfällig sein, eine Fehlerbehebung durchzuführen. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Werkzeuge ist **Client-Bibliotheken neu erstellen**, was AEM zwingt, alle LESS-Dateien neu zu kompilieren und die CSS zu generieren.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Liste aller in der AEM Instanz registrierten Client-Bibliotheken.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testausgabe**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  - ermöglicht es dem Benutzer, die erwartete HTML-Ausgabe von clientlib auf der Grundlage der Kategorie anzuzeigen.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Überprüfung**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  der Abhängigkeiten von Bibliotheken: hebt alle Abhängigkeiten oder eingebetteten Kategorien hervor, die nicht gefunden werden können.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-Bibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  neu erstellen: Ermöglicht es einem Benutzer, AEM zu erzwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache der Client-Bibliotheken zu ungültigen. Dieses Tool ist besonders effektiv, wenn es mit LESS entwickelt wird, da dies AEM zwingen kann, die generierte CSS erneut zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu ungültigen und dann eine Seitenaktualisierung durchzuführen, anstatt alle Bibliotheken neu zu erstellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Client-Bibliothek neu erstellen](assets/client-side-libraries/rebuild-clientlibs.png)
