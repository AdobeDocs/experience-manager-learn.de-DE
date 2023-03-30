---
title: Client-Bibliotheken und Front-End-Workflow
description: Erfahren Sie, wie Sie mit Client-Bibliotheken CSS und JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung bereitstellen und verwalten können. Erfahren Sie, wie das Modul ui.frontend, ein Webpack-Projekt, in den End-to-End-Build-Prozess integriert werden kann.
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '2799'
ht-degree: 4%

---

# Client-Bibliotheken und Front-End-Workflow {#client-side-libraries}

Erfahren Sie, wie Client-seitige Bibliotheken oder Client-Bibliotheken zum Bereitstellen und Verwalten von CSS und JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung verwendet werden. In diesem Tutorial wird auch erläutert, wie die [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de) -Modul, ein entkoppeltes [Webpack](https://webpack.js.org/) -Projekt in den End-to-End-Build-Prozess integriert werden können.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment).

Außerdem wird empfohlen, die [Komponentengrundlagen](component-basics.md#client-side-libraries) Tutorial zum Verständnis der Grundlagen von Client-seitigen Bibliotheken und AEM.

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

Sehen Sie sich den Basis-Code an, auf dem das Tutorial aufbaut:

1. Sehen Sie sich die `tutorial/client-side-libraries-start` Verzweigung aus [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Stellen Sie die Codebasis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie die `classic` Profile zu beliebigen Maven-Befehlen hinzufügen.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer in [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) oder den Code lokal auschecken, indem Sie zu der Verzweigung wechseln `tutorial/client-side-libraries-solution`.

## Ziele

1. Erfahren Sie, wie Client-seitige Bibliotheken über eine bearbeitbare Vorlage in eine Seite eingefügt werden.
1. Erfahren Sie, wie Sie die `ui.frontend` und einen Webpack Development Server für die dedizierte Front-End-Entwicklung.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf bei der Bereitstellung kompilierter CSS- und JavaScript-Dateien für eine Sites-Implementierung vertraut.

## Was Sie erstellen werden {#what-build}

In diesem Kapitel fügen Sie einige Grundlinien-Stile für die WKND-Site und die Artikelseitenvorlage hinzu, um die Implementierung näher an die [Designmodelle der Benutzeroberfläche](assets/pages-templates/wknd-article-design.xd). Sie verwenden einen erweiterten Front-End-Workflow, um ein Webpack-Projekt in eine AEM Client-Bibliothek zu integrieren.

![Abgeschlossene Stile](assets/client-side-libraries/finished-styles.png)

*Artikelseite mit angewendeten Grundlinienstilen*

## Hintergrund {#background}

Client-seitige Bibliotheken bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Die grundlegenden Ziele für Client-seitige Bibliotheken oder Clientlibs sind:

1. CSS/JS in kleinen separaten Dateien speichern, um die Entwicklung und Wartung zu erleichtern
1. Verwalten von Abhängigkeiten von Drittanbieter-Frameworks auf organisierte Weise
1. Minimieren Sie die Anzahl der clientseitigen Anforderungen, indem Sie CSS/JS in eine oder zwei Anforderungen verketten.

Weitere Informationen zur Verwendung Client-seitiger Bibliotheken [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de).

Clientseitige Bibliotheken haben einige Einschränkungen. Insbesondere ist eine begrenzte Unterstützung für beliebte Frontend-Sprachen wie Sass, LESS und TypeScript. Im Tutorial zeigen wir Ihnen, wie die **ui.frontend** -Modul kann dabei helfen, dies zu lösen.

Stellen Sie die Starter-Codebasis in einer lokalen AEM-Instanz bereit und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Diese Seite ist nicht formatiert. Implementieren wir Client-seitige Bibliotheken für die WKND-Marke, um CSS und JavaScript zur Seite hinzuzufügen.

## Client-seitige Bibliotheksorganisation {#organization}

Als Nächstes untersuchen wir die Organisation von Client-Bibliotheken, die von der [AEM Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de).

![Allgemeine Client-Bibliotheksorganisation](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Übergeordnetes Diagramm Client-seitige Bibliotheksorganisation und Seiteneinbindung*

>[!NOTE]
>
> Die folgende Client-seitige Bibliotheksorganisation wird vom Projektarchetyp generiert, stellt jedoch lediglich einen Ausgangspunkt dar. Die Art und Weise, wie ein Projekt CSS und JavaScript letztendlich in einer Sites-Implementierung verwaltet und bereitstellt, kann je nach Ressourcen, Fachkenntnissen und Anforderungen erheblich variieren.

1. Wenn Sie VSCode oder eine andere IDE verwenden, öffnen Sie die **ui.apps** -Modul.
1. Erweitern des Pfads `/apps/wknd/clientlibs` , um die vom Archetyp generierten Client-Bibliotheken anzuzeigen.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Im folgenden Abschnitt werden diese Client-seitigen Bibliotheken ausführlicher erläutert.

1. Die folgende Tabelle fasst die Client-Bibliotheken zusammen. Weitere Informationen [einschließlich Client-Bibliotheken finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Name | Beschreibung | Anmerkungen |
   |-------------------| ------------| ------|
   | `clientlib-base` | Grundlegende CSS- und JavaScript-Ebene, die für die Funktion der WKND-Site erforderlich sind | bettet Client-Bibliotheken der Kernkomponente ein |
   | `clientlib-grid` | Generiert das erforderliche CSS für [Layout-Modus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) arbeiten. | Mobile-/Tablet-Haltepunkte können hier konfiguriert werden |
   | `clientlib-site` | Enthält Site-spezifisches Design für die WKND-Site | Erstellt von der `ui.frontend` Modul |
   | `clientlib-dependencies` | Bettet alle Drittanbieterabhängigkeiten ein | Erstellt von der `ui.frontend` Modul |

1. Beachten Sie, dass `clientlib-site` und `clientlib-dependencies` werden von der Quell-Code-Verwaltung ignoriert. Dies ist beabsichtigt, da diese zum Zeitpunkt der Erstellung durch die `ui.frontend` -Modul.

## Basistypen aktualisieren {#base-styles}

Aktualisieren Sie anschließend die in der Variablen **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de)** -Modul. Die Dateien in der `ui.frontend` -Modul generieren `clientlib-site` und `clientlib-dependecies` -Bibliotheken, die das Site-Design und alle Drittanbieter-Abhängigkeiten enthalten.

Clientseitige Bibliotheken unterstützen keine erweiterten Sprachen wie [Sass](https://sass-lang.com/) oder [TypeScript](https://www.typescriptlang.org/). Es gibt mehrere Open-Source-Tools wie [NPM](https://www.npmjs.com/) und [Webpack](https://webpack.js.org/) die die Front-End-Entwicklung beschleunigen und optimieren. Das Ziel der **ui.frontend** -Modul kann diese Tools verwenden, um die meisten Frontend-Quelldateien zu verwalten.

1. Öffnen Sie die **ui.frontend** -Modul und navigieren Sie zu `src/main/webpack/site`.
1. Öffnen Sie die Datei `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` ist der Einstiegspunkt zu den Sass-Dateien im `ui.frontend` -Modul. Er enthält die `_variables.scss` -Datei, die eine Reihe von Markenvariablen enthält, die in den verschiedenen Sass-Dateien des Projekts verwendet werden sollen. Die `_base.scss` -Datei ist ebenfalls enthalten und definiert einige grundlegende Stile für HTML-Elemente. Ein regulärer Ausdruck enthält die Stile für einzelne Komponentenstile unter `src/main/webpack/components`. Ein weiterer regulärer Ausdruck enthält die Dateien unter `src/main/webpack/site/styles`.

1. Prüfen Sie die Datei `main.ts`. Er umfasst `main.scss` und einen regulären Ausdruck, um alle `.js` oder `.ts` -Dateien im Projekt. Dieser Einstiegspunkt wird von der [Webpack-Konfigurationsdateien](https://webpack.js.org/configuration/) als Einstiegspunkt für die gesamte `ui.frontend` -Modul.

1. Inspect der Dateien unter `src/main/webpack/site/styles`:

   ![Stildateien](assets/client-side-libraries/style-files.png)

   Diese Dateistile für globale Elemente in der Vorlage, wie Kopfzeile, Fußzeile und Hauptinhaltscontainer. Die CSS-Regeln in diesen Dateien zielen auf verschiedene HTML-Elemente ab `header`, `main`und  `footer`. Diese HTML-Elemente wurden durch Richtlinien im vorherigen Kapitel definiert [Seiten und Vorlagen](./pages-templates.md).

1. Erweitern Sie die `components` Ordner unter `src/main/webpack` und überprüfen Sie die Dateien.

   ![Komponenten-Teilungsdateien](assets/client-side-libraries/component-sass-files.png)

   Jede Datei wird einer Kernkomponente wie der [Accordion-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Jede Kernkomponente erstellt mit [Block Element Modifier](https://getbem.com/) oder BEM-Notation, um das Targeting bestimmter CSS-Klassen mit Stilregeln zu erleichtern. Die Dateien unter `/components` wurden vom AEM Projektarchetyp mit den verschiedenen BEM-Regeln für die einzelnen Komponenten herausgestochen.

1. WKND-Basisstile herunterladen **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** und **unzip** die Datei.

   ![WKND-Basisstile](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Um das Tutorial zu beschleunigen, werden mehrere Sass-Dateien bereitgestellt, die die WKND-Marke basierend auf Kernkomponenten und der Struktur der Artikelseitenvorlage implementieren.

1. Überschreiben der Inhalte von `ui.frontend/src` mit Dateien aus dem vorherigen Schritt. Der Inhalt der ZIP-Datei sollte die folgenden Ordner überschreiben:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Geänderte Dateien](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect die geänderten Dateien, um Details zur WKND-Stilimplementierung anzuzeigen.

## Inspect: ui.frontend-Integration {#ui-frontend-integration}

Ein wichtiges Integrationselement, das in **ui.frontend** Modul, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) nimmt die kompilierten CSS- und JS-Artefakte aus einem webpack/npm-Projekt und wandelt sie in AEM Client-seitige Bibliotheken um.

![Integration der ui.frontend-Architektur](assets/client-side-libraries/ui-frontend-architecture.png)

Der AEM Projektarchetyp richtet diese Integration automatisch ein. Erfahren Sie, wie es funktioniert.


1. Öffnen Sie ein Befehlszeilen-Terminal und installieren Sie die **ui.frontend** -Modul, das `npm install` command:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` Ausführung ist nur einmal erforderlich, z. B. nach einem neuen Klon oder einer Generation des Projekts.

1. Starten Sie den Webpack Development Server in **watch** -Modus, indem Sie den folgenden Befehl ausführen:

   ```shell
   $ npm run watch
   ```

1. Dadurch werden die Quelldateien aus dem `ui.frontend` -Modul und synchronisiert die Änderungen mit AEM unter [http://localhost:4502](http://localhost:4502)

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

1. Der Befehl `npm run watch` füllt schließlich die **clientlib-site** und **clientlib-dependencies** im **ui.apps** -Modul, das dann automatisch mit AEM synchronisiert wird.

   >[!NOTE]
   >
   >Es gibt auch eine `npm run prod` -Profil, das JS und CSS minimiert. Dies ist die Standardkompilierung, sobald der Webpack-Build über Maven ausgelöst wird. Weitere Informationen zum [ui.frontend-Modul finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de).

1. Inspect der Datei `site.css` unter `ui.frontend/dist/clientlib-site/site.css`. Dies ist das kompilierte CSS, das auf den Sass-Quelldateien basiert.

   ![Verteilte Site-CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Prüfen Sie die Datei `ui.frontend/clientlib.config.js`. Dies ist die Konfigurationsdatei für ein npm-Plug-in, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) die den Inhalt von `/dist` in eine Client-Bibliothek ein und verschiebt sie in die `ui.apps` -Modul.

1. Inspect der Datei `site.css` im **ui.apps** -Modul an `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dies sollte eine identische Kopie der `site.css` aus der **ui.frontend** -Modul. Jetzt ist es in **ui.apps** -Modul kann es in AEM bereitgestellt werden.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Seit **clientlib-site** wird während der Build-Zeit kompiliert, indem entweder **npm** oder **Maven**, kann sie aus der Quell-Code-Verwaltung im **ui.apps** -Modul. Inspect `.gitignore` Datei unter **ui.apps**.

1. Öffnen Sie den Artikel &quot;LA Skatepark&quot;in AEM unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Aktualisierte Basisstile für den Artikel](assets/client-side-libraries/updated-base-styles.png)

   Sie sollten nun die aktualisierten Stile für den Artikel sehen. Möglicherweise müssen Sie eine harte Aktualisierung durchführen, um alle CSS-Dateien zu löschen, die vom Browser zwischengespeichert wurden.

   Es fängt an, sich den Mücken viel näher zu sehen!

   >[!NOTE]
   >
   > Die Schritte, die oben zum Erstellen und Bereitstellen des ui.frontend-Codes für AEM ausgeführt werden, werden automatisch ausgeführt, wenn ein Maven-Build vom Stamm des Projekts ausgelöst wird `mvn clean install -PautoInstallSinglePackage`.

## Vornehmen einer Stiländerung

Nehmen Sie als Nächstes eine kleine Änderung an der `ui.frontend` -Modul, um die `npm run watch` stellt die Stile automatisch für die lokale AEM bereit.

1. Von `ui.frontend` -Modul öffnen Sie die Datei: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Aktualisieren Sie die `$brand-primary` Farbvariable:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Speichern Sie die Änderungen.

1. Kehren Sie zum Browser zurück und aktualisieren Sie die AEM Seite, um die Aktualisierungen anzuzeigen:

   ![Client-seitige Bibliotheken](assets/client-side-libraries/style-update-brand-primary.png)

1. Ändern Sie die `$brand-primary` Farbe verwenden und den Webpack-Build mithilfe des Befehls stoppen `CTRL+C`.

>[!CAUTION]
>
> Die Verwendung der **ui.frontend** nicht für alle Projekte erforderlich sein. Die **ui.frontend** fügt zusätzliche Komplexität hinzu. Wenn es nicht notwendig/wünschenswert ist, einige dieser erweiterten Frontend-Tools zu verwenden (Sass, Webpack, npm..), ist dies möglicherweise nicht erforderlich.

## Seiten- und Vorlageneinbindung {#page-inclusion}

Als Nächstes sehen wir, wie die clientlibs auf der AEM Seite referenziert werden. Eine gängige Best Practice bei der Webentwicklung ist die Einbeziehung von CSS in die HTML-Kopfzeile. `<head>` und JavaScript direkt vor dem Schließen `</body>` -Tag.

1. Navigieren Sie zur Vorlage &quot;Artikelseite&quot;unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klicken Sie auf **Seiteninformationen** und wählen Sie im Menü **Seitenrichtlinie** , um **Seitenrichtlinie** angezeigt.

   ![Seitenrichtlinien im Menü &quot;Artikel-Seitenvorlage&quot;](assets/client-side-libraries/template-page-policy.png)

   *Seiteninformationen > Seitenrichtlinie*

1. Beachten Sie, dass die Kategorien für `wknd.dependencies` und `wknd.site` sind hier aufgeführt. Standardmäßig werden Client-Bibliotheken, die über die Seitenrichtlinie konfiguriert werden, aufgeteilt, um das CSS in den Seitenkopf und das JavaScript am Textende einzuschließen. Sie können das clientlib-JavaScript, das im Seitenkopf geladen wird, explizit auflisten. Dies gilt für `wknd.dependencies`.

   ![Seitenrichtlinien im Menü &quot;Artikel-Seitenvorlage&quot;](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Es ist auch möglich, die `wknd.site` oder `wknd.dependencies` direkt aus der Seitenkomponente mithilfe der `customheaderlibs.html` oder `customfooterlibs.html` Skript. Die Verwendung der Vorlage bietet Ihnen Flexibilität, da Sie auswählen können, welche Client-Bibliotheken pro Vorlage verwendet werden. Beispiel: Sie verfügen über eine umfangreiche JavaScript-Bibliothek, die nur für eine Vorlage zum Auswählen verwendet wird.

1. Navigieren Sie zum **LA Skateparks** mit der **Artikelseitenvorlage**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Klicken Sie auf **Seiteninformationen** und wählen Sie im Menü **Als veröffentlicht anzeigen** , um die Artikelseite außerhalb des AEM-Editors zu öffnen.

   ![Als veröffentlicht anzeigen](assets/client-side-libraries/view-as-published-article-page.png)

1. Anzeigen der Seitenquelle von [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) und Sie sollten die folgenden clientlib-Verweise in der `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Beachten Sie, dass die Client-Bibliotheken den Proxy verwenden `/etc.clientlibs` -Endpunkt. Sie sollten auch sehen, dass die folgende clientlib unten auf der Seite Folgendes enthält:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Für AEM 6.5/6.4 werden die clientseitigen Bibliotheken nicht automatisch minimiert. Weitere Informationen finden Sie in der Dokumentation unter [HTML Library Manager zum Aktivieren der Minimierung (empfohlen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de#using-preprocessors).

   >[!WARNING]
   >
   >Auf der Veröffentlichungsseite ist es wichtig, dass die Client-Bibliotheken **not** bereitgestellt von **/apps** da dieser Pfad aus Sicherheitsgründen mit der [Dispatcher-Filterabschnitt](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). Die [allowProxy-Eigenschaft](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) der Client-Bibliothek stellt sicher, dass CSS und JS von bereitgestellt werden **/etc.clientlibs**.

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie einzelne Stile implementieren und Kernkomponenten mithilfe des Stilsystems von Experience Manager wiederverwenden. [Entwickeln mit dem Stilsystem](style-system.md) behandelt die Verwendung des Stilsystems zum Erweitern von Kernkomponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors.

Anzeigen des fertigen Codes unter [GitHub](https://github.com/adobe/aem-guides-wknd) oder den Code lokal in der Git-Verzweigung überprüfen und bereitstellen `tutorial/client-side-libraries-solution`.

1. Klonen Sie die [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) Repository.
1. Sehen Sie sich die `tutorial/client-side-libraries-solution` -Verzweigung.

## Zusätzliche Tools und Ressourcen {#additional-resources}

### Webpack DevServer - Statisches Markup {#webpack-dev-static}

In den vorherigen Übungen werden mehrere Sass-Dateien im **ui.frontend** -Modul aktualisiert wurde und durch einen Build-Prozess, sehen Sie schließlich, dass diese Änderungen sich in AEM widerspiegeln. Als Nächstes sehen wir uns eine Technik an, die eine [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) die schnelle Entwicklung der Frontend-Stile gegen **statisch** HTML.

Diese Technik ist praktisch, wenn die meisten Stile und Frontend-Code von einem dedizierten Frontend-Entwickler ausgeführt werden, der möglicherweise keinen einfachen Zugriff auf eine AEM-Umgebung hat. Diese Technik ermöglicht es dem FED auch, Änderungen direkt an der HTML vorzunehmen, die dann an einen AEM Entwickler übergeben werden können, um sie als Komponenten zu implementieren.

1. Kopieren Sie die Seitenquelle der LA skatepark-Artikelseite unter [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Öffnen Sie Ihre IDE erneut. Fügen Sie das kopierte Markup aus AEM in die `index.html` im **ui.frontend** Modul unter `src/main/webpack/static`.
1. Bearbeiten Sie das kopierte Markup und entfernen Sie alle Verweise auf **clientlib-site** und **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Entfernen Sie diese Verweise, da der Webpack Development Server diese Artefakte automatisch generiert.

1. Starten Sie den webpack-Dev-Server von einem neuen Terminal aus, indem Sie den folgenden Befehl in der **ui.frontend** -Modul:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dadurch sollte ein neues Browser-Fenster unter [http://localhost:8080/](http://localhost:8080/) mit statischem Markup.

1. Datei bearbeiten `src/main/webpack/site/_variables.scss` -Datei. Ersetzen Sie die `$text-color` Regel mit folgendem Inhalt verwenden:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Speichern Sie die Änderungen.

1. Sie sollten die Änderungen automatisch im Browser auf [http://localhost:8080](http://localhost:8080).

   ![Lokale Änderungen am Webpack-Dev-Server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Überprüfen Sie die `/aem-guides-wknd.ui.frontend/webpack.dev.js` -Datei. Dies enthält die Webpack-Konfiguration, die zum Starten des webpack-Dev-Servers verwendet wird. Die Pfade werden hierdurch gesteuert `/content` und `/etc.clientlibs` von einer lokal ausgeführten Instanz von AEM. So werden die Bilder und andere Client-Bibliotheken (nicht von der **ui.frontend** -Code) bereitgestellt werden.

   >[!CAUTION]
   >
   > Die Bild-src des statischen Markups verweist auf eine Live-Bildkomponente in einer lokalen AEM-Instanz. Bilder erscheinen beschädigt, wenn sich der Pfad zum Bild ändert, wenn AEM nicht gestartet wird oder der Browser sich nicht bei der lokalen AEM-Instanz angemeldet hat. Bei Übergabe an eine externe Ressource ist es auch möglich, die Bilder durch statische Referenzen zu ersetzen.

1. Sie können **stop** den Webpack-Server über die Befehlszeile durch Eingabe von `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** ist ein Open-Source-Befehlszeilen-Tool, mit dem die Front-End-Entwicklung beschleunigt werden kann. Es wird durch [aemsync](https://www.npmjs.com/package/aemsync), [BrowserSync](https://browsersync.io/)und [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Auf hoher Ebene wird die `aemfed`ist so konzipiert, dass Dateiänderungen in der **ui.apps** und synchronisieren sie automatisch direkt mit einer laufenden AEM-Instanz. Basierend auf den Änderungen wird ein lokaler Browser automatisch aktualisiert, wodurch die Front-End-Entwicklung beschleunigt wird. Es ist auch für die Verwendung mit Sling Log Tracker entwickelt, um Server-seitige Fehler automatisch direkt im Terminal anzuzeigen.

Wenn Sie viel Arbeit im **ui.apps** -Modul, Ändern von HTL-Skripten und Erstellen benutzerdefinierter Komponenten, **aemfed** kann ein leistungsstarkes Tool sein. [Die vollständige Dokumentation finden Sie hier .](https://github.com/abmaonline/aemfed).

### Debuggen Client-seitiger Bibliotheken {#debugging-clientlibs}

Verwenden verschiedener Methoden **categories** und **Einbettungen** um mehrere Client-Bibliotheken einzuschließen, kann die Fehlerbehebung schwerfällig sein. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Instrumente ist **Client-Bibliotheken neu erstellen** was AEM zwingt, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [**Sprunglippen**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Listet die in der AEM-Instanz registrierten Client-Bibliotheken auf. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testausgabe**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - ermöglicht es einem Benutzer, die erwartete HTML-Ausgabe von clientlib-Includes basierend auf der Kategorie anzuzeigen. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Überprüfung von Bibliotheksabhängigkeiten**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - markiert alle Abhängigkeiten oder eingebetteten Kategorien, die nicht gefunden werden können. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-Bibliotheken neu erstellen**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - ermöglicht es einem Benutzer, AEM zu erzwingen, die Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken ungültig zu machen. Dieses Tool ist bei der Entwicklung mit LESS effektiv, da dies AEM zwingen kann, die generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu invalidieren und dann eine Seitenaktualisierung vorzunehmen, anstatt die Bibliotheken neu zu erstellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Client-Bibliothek neu erstellen](assets/client-side-libraries/rebuild-clientlibs.png)
