---
title: Client-Bibliotheken und Frontend-Workflow
description: Erfahren Sie, wie Sie mit Client-Bibliotheken CSS und JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung bereitstellen und verwalten können. Erfahren Sie, wie das Modul „ui.frontend“, ein webpack-Projekt, in den End-to-End-Build-Prozess integriert werden kann.
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 680
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 100%

---

# Client-Bibliotheken und Frontend-Workflow {#client-side-libraries}

Erfahren Sie, wie Sie mit Client-seitigen Bibliotheken oder Clientlibs CSS and JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung bereitstellen und verwalten können. In diesem Tutorial wird auch erläutert, wie das Modul [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de), ein entkoppeltes [webpack](https://webpack.js.org/)-Projekt, in den End-to-End-Build-Prozess integriert werden kann.

## Voraussetzungen {#prerequisites}

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

Außerdem wird das Tutorial zu [Komponentengrundlagen](component-basics.md#client-side-libraries) empfohlen, um sich mit den Grundlagen von Client-seitigen Bibliotheken und AEM vertraut zu machen.

### Ausgangsprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Ausprobieren des Ausgangsprojekts überspringen.

Checken Sie den Basis-Code aus, auf dem das Tutorial aufbaut:

1. Checken Sie die Verzweigung `tutorial/client-side-libraries-start` aus [GitHub](https://github.com/adobe/aem-guides-wknd) aus.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Stellen Sie die Code-Basis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, fügen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) anzeigen oder den Code lokal auschecken, indem Sie zur Verzweigung `tutorial/client-side-libraries-solution` wechseln.

## Ziele

1. Verstehen, wie Client-seitige Bibliotheken über eine bearbeitbare Vorlage in eine Seite eingebunden werden.
1. Lernen, wie das Modul `ui.frontend` und ein webpack-Entwicklungs-Server zur dedizierten Frontend-Entwicklung verwendet werden.
1. Verstehen des kompletten Workflows zur Bereitstellung von kompiliertem CSS und JavaScript für eine Site-Implementierung.

## Was Sie erstellen werden {#what-build}

In diesem Kapitel fügen Sie einige allgemeine Stile zur WKND-Website und zur Artikelseiten-Vorlage hinzu, um die Implementierung den [Benutzeroberflächen-Mockups](assets/pages-templates/wknd-article-design.xd) anzugleichen. Sie verwenden einen erweiterten Frontend-Workflow, um ein webpack-Projekt in eine AEM-Client-Bibliothek zu integrieren.

![Fertiggestellte Stile](assets/client-side-libraries/finished-styles.png)

*Artikelseite mit angewendeten allgemeinen Stilen*

## Hintergrund {#background}

Client-seitige Bibliotheken bieten eine Möglichkeit zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Die allgemeinen Ziele für Client-seitige Bibliotheken (Clientlibs) sind:

1. Speichern von CSS/JS in kleinen diskreten Dateien, um die Entwicklung und Wartung zu vereinfachen
1. Organisiertes Verwalten der Abhängigkeiten von Drittanbieter-Frameworks
1. Minimieren der Anzahl Client-seitiger Anfragen durch CSS-/JS-Verkettung zu einer oder zwei Anfragen

Weitere Informationen zur Verwendung Client-seitiger Bibliotheken [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de).

Client-seitige Bibliotheken sind mit einigen Einschränkungen verbunden. Insbesondere werden gängige Frontend-Sprachen wie Sass, LESS und TypeScript nur in begrenztem Umfang unterstützt. Im Tutorial zeigen wir Ihnen, wie hier das Modul **ui.frontend** helfen kann.

Stellen Sie die anfängliche Code-Basis in einer lokalen AEM-Instanz bereit und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Diese Seite ist ungestylt. Implementieren wir nun Client-seitige Bibliotheken für die WKND-Marke, um CSS und JavaScript zur Seite hinzuzufügen.

## Client-seitige Bibliotheksorganisation {#organization}

Als Nächstes untersuchen wir die vom [AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) generierte Clientlibs-Organisation.

![Allgemeine Organisation der Client-Bibliotheken](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Allgemeines Diagramm zur Organisation Client-seitiger Bibliothen und Seiteneinbindung*

>[!NOTE]
>
> Die folgende Client-seitige Bibliotheksorganisation wird vom AEM-Projektarchetyp generiert, ist jedoch nur ein Ausgangspunkt. Die Art und Weise, wie in einem Projekt CSS und JavaScript letztendlich verwaltet und in einer Sites-Implementierung bereitgestellt werden, kann je nach Ressourcen, Fachkenntnissen und Anforderungen erheblich variieren.

1. Wenn Sie VSCode oder eine andere IDE verwenden, öffnen Sie das Modul **ui.apps**.
1. Erweitern Sie den Pfad `/apps/wknd/clientlibs`, um die vom Archetyp generierten Clientlibs anzuzeigen.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Im folgenden Abschnitt werden diese Clientlibs ausführlicher erläutert.

1. In der folgenden Tabelle sind die Client-Bibliotheken zusammengefasst. Weitere Informationen zum Einschließen von Client-Bibliotheken [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=de#developing).

   | Name | Beschreibung | Anmerkungen |
   |-------------------| ------------| ------|
   | `clientlib-base` | CSS- und JavaScript-Basis, die für die Funktionalität der WKND-Website erforderlich ist. | Bettet Client-Bibliotheken der Kernkomponente ein. |
   | `clientlib-grid` | Generiert das erforderliche CSS, damit der [Layout-Modus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=de) funktioniert. | Mobile-/Tablet-Haltepunkte können hier konfiguriert werden. |
   | `clientlib-site` | Enthält ein Website-spezifisches Design für die WKND-Website. | Generiert vom Modul `ui.frontend`. |
   | `clientlib-dependencies` | Bettet etwaige Drittanbieter-Abhängigkeiten ein. | Generiert vom Modul `ui.frontend`. |

1. Beachten Sie, dass `clientlib-site` und `clientlib-dependencies` von der Quell-Code-Verwaltung ignoriert werden. Dies ist beabsichtigt, da diese zum Zeitpunkt der Erstellung durch das Modul `ui.frontend` generiert werden.

## Aktualisieren von Basisstilen {#base-styles}

Aktualisieren Sie als Nächstes die im Modul **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de)** definierten Basisstile. Die Dateien im Modul `ui.frontend` generieren die `clientlib-site`- und `clientlib-dependecies`-Bibliotheken, die das Website-Design und alle Drittanbieter-Abhängigkeiten enthalten.

Client-seitige Bibliotheken unterstützen keine höheren Sprachen wie [Sass](https://sass-lang.com/) oder [TypeScript](https://www.typescriptlang.org/). Es gibt mehrere Open-Source-Tools wie [NPM](https://www.npmjs.com/) und [webpack](https://webpack.js.org/), die die Frontend-Entwicklung beschleunigen und optimieren. Das Ziel des Moduls **ui.frontend** besteht darin, diese Tools verwenden zu können, um die meisten Frontend-Quelldateien zu verwalten.

1. Öffnen Sie das Modul **ui.frontend** und navigieren Sie zu `src/main/webpack/site`.
1. Öffnen Sie die Datei `main.scss`

   ![main.scss – Einstiegspunkt](assets/client-side-libraries/main-scss.png)

   Die Datei `main.scss` ist der Einstiegspunkt zu den Sass-Dateien im Modul `ui.frontend`. Sie enthält die Datei `_variables.scss` mit einer Reihe von Markenvariablen, die in den verschiedenen Sass-Dateien des Projekts verwendet werden sollen. Die Datei `_base.scss` ist ebenfalls enthalten und definiert einige grundlegende Stile für HTML-Elemente. Ein regulärer Ausdruck enthält die Stile für einzelne Komponentenstile unter `src/main/webpack/components`. Ein weiterer regulärer Ausdruck enthält die Dateien unter `src/main/webpack/site/styles`.

1. Überprüfen Sie die Datei `main.ts`. Sie umfasst `main.scss` sowie einen regulären Ausdruck, um alle `.js`- oder `.ts`-Dateien im Projekt zu erfassen. Dieser Einstiegspunkt wird von den [webpack-Konfigurationsdateien](https://webpack.js.org/configuration/) als Einstiegspunkt für das gesamte Modul `ui.frontend` verwendet.

1. Überprüfen Sie die Dateien unter `src/main/webpack/site/styles`:

   ![Stildateien](assets/client-side-libraries/style-files.png)

   Diese Dateistile betreffen globale Elemente in der Vorlage, z. B. Header, Footer und Hauptinhalts-Container. Die CSS-Regeln in diesen Dateien zielen auf verschiedene HTML-Elemente (`header`, `main` und `footer`) ab. Diese HTML-Elemente wurden durch Richtlinien im vorherigen Kapitel [Seiten und Vorlagen](./pages-templates.md) definiert.

1. Erweitern Sie den Ordner `components` unter `src/main/webpack` und überprüfen Sie die Dateien.

   ![Komponenten-Sass-Dateien](assets/client-side-libraries/component-sass-files.png)

   Jede Datei wird einer Kernkomponente wie der [Akkordeon-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=de) zugeordnet. Jede Kernkomponente wird mit [Block Element Modifier](https://getbem.com/)(BEM)-Notation erstellt, um das Targeting bestimmter CSS-Klassen mit Stilregeln zu erleichtern. Die Dateien unter `/components` wurden vom AEM-Projektarchetyp mit den verschiedenen BEM-Regeln für die einzelnen Komponenten ignoriert.

1. Laden Sie die WKND-Basisstile **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** herunter und **entzippen** Sie die Datei.

   ![WKND-Basisstile](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Um während des Tutorials Zeit zu sparen, werden mehrere Sass-Dateien, die die WKND-Marke basierend auf Kernkomponenten und der Struktur der Artikelseiten-Vorlage implementieren, bereitgestellt.

1. Überschreiben Sie den Inhalt von `ui.frontend/src` mit Dateien aus dem vorherigen Schritt. Der Inhalt der ZIP-Datei sollte die folgenden Ordner überschreiben:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Geänderte Dateien](assets/client-side-libraries/changed-files-uifrontend.png)

   Überprüfen Sie die geänderten Dateien, um Details zur WKND-Stilimplementierung anzuzeigen.

## Überprüfen der ui.frontend-Integration {#ui-frontend-integration}

Ein wichtiges Integrationselement, das im Modul **ui.frontend** integriert ist, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), wandelt die kompilierten CSS- und JS-Artefakte aus einem webpack/npm-Projekt in AEM Client-seitige Bibliotheken um.

![Integration der ui.frontend-Architektur](assets/client-side-libraries/ui-frontend-architecture.png)

Der AEM-Projektarchetyp richtet diese Integration automatisch ein. Erfahren Sie als Nächstes, wie dies funktioniert.


1. Öffnen Sie ein Befehlszeilen-Terminal und installieren Sie das Modul **ui.frontend** mit dem Befehl `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` muss nur einmal ausgeführt werden, z. B. nach einem neuen Klon oder Projekterstellung.

1. Starten Sie den webpack-Dev-Server im Modus **watch**, indem Sie den folgenden Befehl ausführen:

   ```shell
   $ npm run watch
   ```

1. Dadurch werden die Quelldateien aus dem Modul `ui.frontend` kompiliert und die Änderungen mit AEM unter [http://localhost:4502](http://localhost:4502) synchronisiert.

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

1. Der Befehl `npm run watch` füllt schließlich **clientlib-site** und **clientlib-dependencies** im Modul **ui.apps** auf, das dann automatisch mit AEM synchronisiert wird.

   >[!NOTE]
   >
   >Es gibt auch ein Profil `npm run prod`, das JS und CSS minimiert. Dies ist die Standardkompilierung, sobald der webpack-Build über Maven ausgelöst wird. Weitere Informationen zum ui.frontend-Modul [finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de).

1. Überprüfen Sie die Datei `site.css` unter `ui.frontend/dist/clientlib-site/site.css`. Dies ist die kompilierte CSS-Datei, die auf den Sass-Quelldateien basiert.

   ![Verteilte site.css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Überprüfen Sie die Datei `ui.frontend/clientlib.config.js`. Dies ist die Konfigurationsdatei für das npm-Plug-in [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), das den Inhalt von `/dist` in eine Client-Bibliothek umwandelt und in das Modul `ui.apps` verschiebt.

1. Überprüfen Sie die Datei `site.css` im Modul **ui.apps** unter `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Hierbei sollte es sich um eine identische Kopie der Datei `site.css` aus dem Modul **ui.frontend** handeln. Da sie sich nun im Modul **ui.apps** befindet, kann sie für AEM bereitgestellt werden.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** zum Zeitpunkt der Erstellung mit **npm** oder **Maven** kompiliert wird, kann sie seitens der Quell-Code-Verwaltung im Modul **ui.apps** problemlos ignoriert werden. Überprüfen Sie die Datei `.gitignore` unter **ui.apps**.

1. Öffnen Sie den Artikel „LA Skatepark“ in AEM unter [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Aktualisierte Basisstile für den Artikel](assets/client-side-libraries/updated-base-styles.png)

   Sie sollten nun die aktualisierten Stile für den Artikel sehen. Möglicherweise müssen Sie einen Hard Refresh (harte Aktualisierung) durchführen, um etwaige vom Browser zwischengespeicherte CSS-Dateien zu löschen.

   Das ist den Mockups doch schon viel ähnlicher!

   >[!NOTE]
   >
   > Die Schritte, die oben zum Erstellen und Bereitstellen des ui.frontend-Codes für AEM ausgeführt wurden, werden automatisch ausgeführt, wenn ein Maven-Build vom Stamm des Projekts `mvn clean install -PautoInstallSinglePackage` ausgelöst wird.

## Vornehmen einer Stiländerung

Nehmen Sie als Nächstes eine kleine Änderung am Modul `ui.frontend` vor, damit `npm run watch` die Stile automatisch für die lokale AEM-Instanz bereitstellt.

1. Öffnen Sie vom Modul `ui.frontend` aus die Datei `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Aktualisieren Sie die Farbvariable `$brand-primary`:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Speichern Sie die Änderungen.

1. Kehren Sie zum Browser zurück und aktualisieren Sie die AEM-Seite, um die Aktualisierungen anzuzeigen:

   ![Client-seitige Bibliotheken](assets/client-side-libraries/style-update-brand-primary.png)

1. Setzen Sie die Änderung an der Farbe `$brand-primary` zurück und stoppen Sie den webpack-Build mit dem Befehl `CTRL+C`.

>[!CAUTION]
>
> Das Modul **ui.frontend** ist unter Umständen nicht für alle Projekte erforderlich. Das Modul **ui.frontend** sorgt für zusätzliche Komplexität. Wenn es nicht notwendig/wünschenswert ist, einige dieser erweiterten Frontend-Tools (Sass, webpack, npm …) zu verwenden, ist es möglicherweise gar nicht erforderlich.

## Einbinden von Seiten und Vorlagen {#page-inclusion}

Als Nächstes sehen wir uns an, wie die Clientlibs auf der AEM-Seite referenziert werden. Eine gängige Best Practice bei der Web-Entwicklung ist die Einbindung von CSS in den HTML-Header `<head>` und von JavaScript direkt vor dem schließenden `</body>`-Tag.

1. Navigieren Sie zur Artikelseiten-Vorlage unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü die Option **Seitenrichtlinie** aus, um das Dialogfeld **Seitenrichtlinie** anzuzeigen.

   ![Menüoption „Seitenrichtlinie“ der Artikelseiten-Vorlage](assets/client-side-libraries/template-page-policy.png)

   *Seiteninformationen > Seitenrichtlinie*

1. Beachten Sie, dass die Kategorien für `wknd.dependencies` und `wknd.site` hier aufgeführt sind. Standardmäßig werden Clientlibs, die über die Seitenrichtlinie konfiguriert werden, aufgeteilt, um CSS in den Seitenkopf und JavaScript am Textende einzuschließen. Sie können das Clientlib-JavaScript, das im Seitenkopf geladen wird, explizit auflisten. Dies ist der Fall für `wknd.dependencies`.

   ![Menüoption „Seitenrichtlinie“ der Artikelseiten-Vorlage](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Es ist auch möglich, `wknd.site` oder `wknd.dependencies` mit dem Skript `customheaderlibs.html` oder `customfooterlibs.html` direkt von der Seitenkomponente aus zu referenzieren. Die Verwendung der Vorlage bietet Ihnen Flexibilität, da Sie auswählen können, welche Client-Bibliotheken pro Vorlage verwendet werden, wenn Sie beispielsweise über eine umfangreiche JavaScript-Bibliothek verfügen, die nur für eine ausgewählte Vorlage verwendet wird.

1. Navigieren Sie zur Seite **LA Skateparks** über die **Artikelseiten-Vorlage**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü die Option **Als veröffentlicht anzeigen** aus, um die Artikelseite außerhalb des AEM-Editors zu öffnen.

   ![Als veröffentlicht anzeigen](assets/client-side-libraries/view-as-published-article-page.png)

1. Zeigen Sie die Seitenquelle von [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) an. Sie sollten die folgenden Clientlib-Verweise in `<head>` sehen:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Beachten Sie, dass die Clientlibs den `/etc.clientlibs`-Proxy-Endpunkt verwenden. Sie sollten auch sehen, dass die folgende Clientlib unten auf der Seite Folgendes enthält:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Für AEM 6.5/6.4 werden die Client-seitigen Bibliotheken nicht automatisch minimiert. Weitere Informationen finden Sie in der [HTML Library Manager-Dokumentation zum Aktivieren der Minimierung (empfohlen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de#using-preprocessors).

   >[!WARNING]
   >
   >Auf der Veröffentlichungsseite ist es wichtig, dass die Client-Bibliotheken **nicht** von **/apps** bereitgestellt werden, da dieser Pfad aus Sicherheitsgründen mithilfe des [Dispatcher-Filterabschnitts](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#example-filter-section) beschränkt werden sollte. Die [allowProxy-Eigenschaft](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) der Client-Bibliothek stellt sicher, dass CSS und JS von **/etc.clientlibs** bereitgestellt werden.

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie einzelne Stile implementieren und Kernkomponenten mithilfe des Stilsystems von Experience Manager wiederverwenden. Unter [Entwickeln mit dem Stilsystem](style-system.md) finden Sie Informationen zur Verwendung des Stilsystems, um Kernkomponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors zu erweitern.

Sehen Sie sich den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code und stellen Sie ihn lokal auf der Git-Verzweigung `tutorial/client-side-libraries-solution` bereit.

1. Klonen Sie das Repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Checken Sie die Verzweigung `tutorial/client-side-libraries-solution` aus.

## Zusätzliche Tools und Ressourcen {#additional-resources}

### webpack-Dev-Server – Statisches Markup {#webpack-dev-static}

In den vorherigen Übungen wurden mehrere Sass-Dateien im Modul **ui.frontend** aktualisiert, wobei diese Änderungen durch einen Build-Prozess schließlich in AEM widergespiegelt werden. Als Nächstes sehen wir uns eine Methode an, die über einen [webpack-Dev-Server](https://webpack.js.org/configuration/dev-server/) die schnelle Entwicklung von Frontend-Stilen in **statischem** HTML-Code ermöglicht.

Diese Methode ist praktisch, wenn der Großteil der Stile und des Frontend-Codes von einem einzelnen Frontend-Entwickler bzw. einer einzelnen Frontend-Entwicklerin ausgeführt wird, der bzw. die unter Umständen nicht problemlos auf eine AEM-Umgebung zugreifen kann. Mit dieser Methode kann der Frontend-Entwickler bzw. die Frontend-Entwicklerin auch direkte Änderungen am HTML-Code vornehmen, der dann an einen AEM-Entwickler bzw. eine AEM-Entwicklerin übergeben und als Komponenten implementiert werden kann.

1. Kopieren Sie die Seitenquelle der LA Skatepark-Artikelseite unter [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Öffnen Sie Ihre IDE erneut. Fügen Sie das kopierte Markup aus AEM unter `src/main/webpack/static` in `index.html` im Modul **ui.frontend** ein.
1. Bearbeiten Sie das kopierte Markup und entfernen Sie alle Verweise auf **clientlib-site** und **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Entfernen Sie diese Verweise, da der webpack-Entwicklungs-Server diese Artefakte automatisch generiert.

1. Starten Sie den webpack-Dev-Server von einem neuen Terminal aus, indem Sie den folgenden Befehl im Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dadurch sollte ein neues Browser-Fenster unter [http://localhost:8080/](http://localhost:8080/) mit statischem Markup geöffnet werden.

1. Bearbeiten Sie die Datei `src/main/webpack/site/_variables.scss`. Ersetzen Sie die Regel `$text-color` durch Folgendes:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Speichern Sie die Änderungen.

1. Die Änderungen sollten automatisch im Browser auf [http://localhost:8080](http://localhost:8080) zu sehen sein.

   ![Lokale Änderungen am webpack-Dev-Server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Überprüfen Sie die Datei `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Diese enthält die webpack-Konfiguration, die zum Starten des webpack-Dev-Servers verwendet wird. Die Pfade `/content` und `/etc.clientlibs` werden dabei von einer lokal ausgeführten AEM-Instanz weitergeleitet. So werden die Bilder und andere (nicht vom **ui.frontend**-Code verwaltete) Client-Bibliotheken bereitgestellt.

   >[!CAUTION]
   >
   > Die Bild-src des statischen Markups verweist auf eine Live-Bildkomponente in einer lokalen AEM-Instanz. Bilder erscheinen fehlerhaft, wenn sich der Pfad zum Bild ändert, wenn AEM nicht gestartet wird oder der Browser sich nicht bei der lokalen AEM-Instanz angemeldet hat. Bei Übergabe an eine externe Ressource ist es auch möglich, die Bilder durch statische Referenzen zu ersetzen.

1. Sie können den Webpack-Server **stoppen**, indem Sie `CTRL+C` in die Befehlszeile eingeben.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** ist ein Open-Source-Befehlszeilen-Tool, mit dem die Frontend-Entwicklung beschleunigt werden kann. Es wird unterstützt durch [aemsync](https://www.npmjs.com/package/aemsync), [BrowserSync](https://browsersync.io/) und [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

`aemfed` ist so konzipiert, dass es Dateiänderungen im **ui.apps**-Modul erfasst und diese direkt mit einer laufenden AEM-Instanz automatisch synchronisiert. Basierend auf den Änderungen wird ein lokaler Browser automatisch aktualisiert, wodurch die Frontend-Entwicklung beschleunigt wird. Es wurde auch für die Verwendung mit Sling Log Tracker entwickelt, um Server-seitige Fehler automatisch direkt im Terminal anzuzeigen.

Wenn Sie im **ui.apps**-Modul häufig Aufgaben ausführen müssen, wie etwa HTL-Skripts ändern und benutzerdefinierte Komponenten erstellen, kann **aemfed** ein sehr nützliches Tool sein. [Die vollständige Dokumentation finden Sie hier](https://github.com/abmaonline/aemfed).

### Debuggen Client-seitiger Bibliotheken {#debugging-clientlibs}

Bei Verwendung verschiedener Methoden, um mit **Kategorien** und **Einbettungen** mehrere Client-Bibliotheken einzuschließen, kann die Fehlerbehebung mühsam sein. AEM stellt mehrere Tools als Hilfe zur Verfügung. Eines der wichtigsten Tools ist **Rebuild Client Libraries**, das AEM zwingt, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) – Listet die in der AEM-Instanz registrierten Client-Bibliotheken auf. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testausgabe**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) – ermöglicht es einem Benutzenden, die erwartete HTML-Ausgabe von clientlib-Einfügungen basierend auf der Kategorie anzuzeigen. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Gültigkeitsprüfung von Bibliotheksabhängigkeiten**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) – markiert alle Abhängigkeiten oder eingebetteten Kategorien, die nicht gefunden werden können. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-Bibliotheken neu erstellen**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) – ermöglicht es einem Benutzenden, AEM zu zwingen, die Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken zu invalidieren. Dieses Tool ist bei der Entwicklung mit LESS nützlich, da dies AEM zwingen kann, das generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu invalidieren und dann eine Seitenaktualisierung vorzunehmen, anstatt die Bibliotheken neu zu erstellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Client-Bibliothek neu erstellen](assets/client-side-libraries/rebuild-clientlibs.png)
