---
title: Client-seitige Bibliotheken und Frontend-Workflow
description: Erfahren Sie, wie Client-seitige Bibliotheken oder Client-Bibliotheken zum Bereitstellen und Verwalten von CSS und JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung verwendet werden. In diesem Tutorial wird auch erläutert, wie das Modul ui.frontend, ein Webpack-Projekt, in den End-to-End-Build-Prozess integriert werden kann.
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Kernkomponenten, AEM-Projektarchetyp
topic: Content Management, Entwicklung
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 4%

---


# Client-seitige Bibliotheken und Frontend-Workflow {#client-side-libraries}

Erfahren Sie, wie Client-seitige Bibliotheken oder Client-Bibliotheken zum Bereitstellen und Verwalten von CSS und JavaScript für eine Adobe Experience Manager (AEM) Sites-Implementierung verwendet werden. In diesem Tutorial wird auch erläutert, wie das [ui.frontend](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/uifrontend.html)-Modul, ein entkoppeltes [webpack](https://webpack.js.org/)-Projekt, in den End-to-End-Build-Prozess integriert werden kann.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

Es wird außerdem empfohlen, das Tutorial [Komponentengrundlagen](component-basics.md#client-side-libraries) zu lesen, um die Grundlagen von Client-seitigen Bibliotheken und AEM zu verstehen.

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

Sehen Sie sich den Basis-Code an, auf dem das Tutorial aufbaut:

1. Sehen Sie sich die Verzweigung `tutorial/client-side-libraries-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

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
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das Profil `classic` an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `tutorial/client-side-libraries-solution` wechseln.

## Ziele

1. Erfahren Sie, wie Client-seitige Bibliotheken über eine bearbeitbare Vorlage in eine Seite eingefügt werden.
1. Erfahren Sie, wie Sie das UI.Frontend-Modul und einen Webpack Development Server für die dedizierte Front-End-Entwicklung verwenden.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf bei der Bereitstellung kompilierter CSS- und JavaScript-Dateien für eine Sites-Implementierung vertraut.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel fügen Sie einige Grundlinien-Stile für die WKND-Site und die Artikelseitenvorlage hinzu, um die Implementierung näher an die [UI-Design-Modelle](assets/pages-templates/wknd-article-design.xd) heranzuführen. Sie verwenden einen erweiterten Frontend-Workflow, um ein Webpack-Projekt in eine AEM Client-Bibliothek zu integrieren.

![Abgeschlossene Stile](assets/client-side-libraries/finished-styles.png)

*Artikelseite mit angewendeten Grundlinienstilen*

## Hintergrund {#background}

Client-seitige Bibliotheken bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Die grundlegenden Ziele für Client-seitige Bibliotheken oder Clientlibs sind:

1. CSS/JS in kleinen separaten Dateien speichern, um die Entwicklung und Wartung zu erleichtern
1. Verwalten von Abhängigkeiten von Drittanbieter-Frameworks auf organisierte Weise
1. Minimieren Sie die Anzahl der clientseitigen Anforderungen, indem Sie CSS/JS in eine oder zwei Anforderungen verketten.

Weitere Informationen zur Verwendung Client-seitiger Bibliotheken [finden Sie hier](https://docs.adobe.com/content/help/de-DE/experience-manager-65/developing/introduction/clientlibs.html).

Clientseitige Bibliotheken haben einige Einschränkungen. Insbesondere ist eine begrenzte Unterstützung für beliebte Frontend-Sprachen wie Sass, LESS und TypeScript. Im Tutorial werden wir sehen, wie das Modul **ui.frontend** dabei helfen kann.

Stellen Sie die Starter-Codebasis in einer lokalen AEM-Instanz bereit und navigieren Sie zu [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Diese Seite ist derzeit nicht formatiert. Als Nächstes implementieren wir Client-seitige Bibliotheken für die WKND-Marke, um CSS und JavaScript zur Seite hinzuzufügen.

## Client-seitige Bibliotheksorganisation {#organization}

Als Nächstes werden wir die Organisation von Client-Bibliotheken untersuchen, die vom [AEM Projektarchetyp](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/overview.html) generiert wurden.

![Allgemeine Client-Bibliotheksorganisation](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Übergeordnetes Diagramm Client-seitige Bibliotheksorganisation und Seiteneinbindung*

>[!NOTE]
>
> Die folgende Client-seitige Bibliotheksorganisation wird vom Projektarchetyp generiert, stellt jedoch lediglich einen Ausgangspunkt dar. Die Art und Weise, wie ein Projekt letztendlich CSS- und JavaScript-Dateien in einer Sites-Implementierung verwaltet und bereitstellt, kann je nach Ressourcen, Fähigkeiten und Anforderungen erheblich variieren.

1. Öffnen Sie mithilfe von VSCode oder einer anderen IDE das Modul **ui.apps**.
1. Erweitern Sie den Pfad `/apps/wknd/clientlibs` , um die vom Archetyp generierten Client-Bibliotheken anzuzeigen.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Wir werden diese clientlibs unten detaillierter untersuchen.

1. Die folgende Tabelle fasst die Client-Bibliotheken zusammen. Weitere Informationen zu [einschließlich Client-Bibliotheken finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Name | Beschreibung | Hinweise |
   |-------------------| ------------| ------|
   | `clientlib-base` | Grundlegende CSS- und JavaScript-Ebene, die für die Funktion der WKND-Site erforderlich sind | bettet Client-Bibliotheken der Kernkomponente ein |
   | `clientlib-grid` | Erzeugt das für die Funktion [Layout-Modus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) erforderliche CSS. | Mobile-/Tablet-Haltepunkte können hier konfiguriert werden |
   | `clientlib-site` | Enthält Site-spezifisches Design für die WKND-Site | Erstellt durch das Modul `ui.frontend` |
   | `clientlib-dependencies` | Bettet alle Drittanbieterabhängigkeiten ein | Erstellt durch das Modul `ui.frontend` |

1. Beachten Sie, dass `clientlib-site` und `clientlib-dependencies` von der Quell-Code-Verwaltung ignoriert werden. Dies ist beabsichtigt, da diese zum Zeitpunkt der Erstellung durch das Modul `ui.frontend` generiert werden.

## Aktualisieren von Basistypen {#base-styles}

Aktualisieren Sie anschließend die im Modul **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** definierten Basisstile. Die Dateien im Modul `ui.frontend` generieren die Bibliotheken `clientlib-site` und `clientlib-dependecies`, die das Site-Design und alle Abhängigkeiten von Drittanbietern enthalten.

Client-seitige Bibliotheken haben einige Einschränkungen bei der Unterstützung von Sprachen wie [Sass](https://sass-lang.com/) oder [TypeScript](https://www.typescriptlang.org/). Es gibt eine Reihe von Open-Source-Tools wie [NPM](https://www.npmjs.com/) und [webpack](https://webpack.js.org/), die die Front-End-Entwicklung beschleunigen und optimieren. Das Ziel des Moduls **ui.frontend** besteht darin, diese Tools zur Verwaltung der meisten Frontend-Quelldateien verwenden zu können.

1. Öffnen Sie das Modul **ui.frontend** und navigieren Sie zu `src/main/webpack/site`.
1. Öffnen Sie die Datei `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` ist der Einstiegspunkt zu allen Sass-Dateien im  `ui.frontend` Modul. Sie enthält die Datei `_variables.scss`, die eine Reihe von Markenvariablen enthält, die in den verschiedenen Sass-Dateien des Projekts verwendet werden sollen. Die `_base.scss`-Datei ist ebenfalls enthalten und definiert einige grundlegende Stile für HTML-Elemente. Ein regulärer Ausdruck enthält alle Stile für einzelne Komponentenstile unter `src/main/webpack/components`. Ein weiterer regulärer Ausdruck enthält alle Dateien unter `src/main/webpack/site/styles`.

1. Prüfen Sie die Datei `main.ts`. `main.ts` enthält  `main.scss` und enthält einen regulären Ausdruck zum Erfassen beliebiger  `.js` oder  `.ts` Dateien im Projekt. Dieser Einstiegspunkt wird von den [webpack-Konfigurationsdateien](https://webpack.js.org/configuration/) als Einstiegspunkt für das gesamte `ui.frontend`-Modul verwendet.

1. Inspect die Dateien unter `src/main/webpack/site/styles`:

   ![Stildateien](assets/client-side-libraries/style-files.png)

   Diese Dateistile für globale Elemente in der Vorlage, wie Kopfzeile, Fußzeile und Hauptinhaltscontainer. Die CSS-Regeln in diesen Dateien zielen auf verschiedene HTML-Elemente `header`, `main` und `footer` ab. Diese HTML-Elemente wurden durch Richtlinien im vorherigen Kapitel [Seiten und Vorlagen](./pages-templates.md) definiert.

1. Erweitern Sie den Ordner `components` unter `src/main/webpack` und überprüfen Sie die Dateien.

   ![Komponenten-Teilungsdateien](assets/client-side-libraries/component-sass-files.png)

   Jede Datei wird einer Kernkomponente wie der [Accordion-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components) zugeordnet. Jede Kernkomponente wird mit der [Block Element Modifier](https://getbem.com/)- oder BEM-Notation erstellt, um das Targeting bestimmter CSS-Klassen mit Stilregeln zu vereinfachen. Die Dateien unter `/components` wurden vom AEM Projektarchetyp mit den verschiedenen BEM-Regeln für jede Komponente herausgestockt.

1. Laden Sie die WKND-Basisstile **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** herunter und entpacken Sie die Datei **entpacken**.

   ![WKND-Basisstile](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Um das Tutorial zu beschleunigen, haben wir die verschiedenen Sass-Dateien bereitgestellt, die die WKND-Marke basierend auf Kernkomponenten und der Struktur der Artikelseitenvorlage implementieren.

1. Überschreiben Sie den Inhalt von `ui.frontend/src` mit Dateien aus dem vorherigen Schritt. Der Inhalt der ZIP-Datei sollte die folgenden Ordner überschreiben:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Geänderte Dateien](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect die geänderten Dateien, um Details zur WKND-Stilimplementierung anzuzeigen.

## Inspect the ui.frontend integration {#ui-frontend-integration}

Ein wichtiges Integrationselement, das in das Modul **ui.frontend** integriert ist, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), nimmt die kompilierten CSS- und JS-Artefakte aus einem webpack/npm-Projekt und wandelt sie in AEM Client-seitige Bibliotheken um.

![Integration der ui.frontend-Architektur](assets/client-side-libraries/ui-frontend-architecture.png)

Der AEM Projektarchetyp richtet diese Integration automatisch ein. Erfahren Sie, wie es funktioniert.


1. Öffnen Sie ein Befehlszeilenterminal und installieren Sie das Modul **ui.frontend** mithilfe des Befehls `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` nur einmal ausgeführt werden, nach einem neuen Klon oder einer neuen Generation des Projekts.

1. Erstellen Sie im selben Terminal das Modul **ui.frontend** und stellen Sie es mithilfe des Befehls `npm run dev` bereit:

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Möglicherweise erhalten Sie einen Fehler wie &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > Dies geschieht normalerweise, weil sich Ihre Umgebung seit der Ausführung von `npm install` geändert hat.
   > Führen Sie `npm rebuild node-sass` aus, um das Problem zu beheben. Dies geschieht, wenn sich die auf Ihrem lokalen Dev-Computer installierte Version von `npm` von der Version unterscheidet, die von Maven `frontend-maven-plugin` in der Datei `aem-guides-wknd/pom.xml` verwendet wird. Sie können dies dauerhaft beheben, indem Sie die Version in der Pom-Datei so ändern, dass sie mit Ihrer lokalen Version übereinstimmt oder umgekehrt.

1. Der Befehl `npm run dev` sollte den Quellcode für das Webpack-Projekt erstellen und kompilieren und letztendlich die **clientlib-site** und **clientlib-dependencies** im Modul **ui.apps** füllen.

   >[!NOTE]
   >
   >Es gibt auch ein `npm run prod`-Profil, das JS und CSS minimiert. Dies ist die Standardkompilierung, sobald der Webpack-Build über Maven ausgelöst wird. Weitere Informationen zum Modul [ui.frontend finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect die Datei `site.css` unter `ui.frontend/dist/clientlib-site/css/site.css`. Dies ist das kompilierte CSS, das auf den Sass-Quelldateien basiert.

   ![Verteilte Site-CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Prüfen Sie die Datei `ui.frontend/clientlib.config.js`. Dies ist die Konfigurationsdatei für ein npm-Plugin, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), das den Inhalt von `/dist` in eine Client-Bibliothek umwandelt und sie in das `ui.apps`-Modul verschiebt.

1. Inspect Sie die Datei `site.css` im Modul **ui.apps** unter `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dies sollte eine identische Kopie der `site.css`-Datei aus dem Modul **ui.frontend** sein. Jetzt, da es sich im Modul **ui.apps** befindet, kann es in AEM bereitgestellt werden.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Da **clientlib-site** während der Build-Zeit mit **npm** oder **maven** kompiliert wird, kann es sicher aus der Quell-Code-Verwaltung im Modul **ui.apps** ignoriert werden. Inspect die `.gitignore`-Datei unter **ui.apps**.

1. Synchronisieren Sie die `clientlib-site`-Bibliothek mit einer lokalen Instanz von AEM mithilfe der Entwicklertools oder Maven-Fähigkeiten.

   ![Synchronisieren der Clientlib-Site](assets/client-side-libraries/sync-clientlib-site.png)

1. Öffnen Sie den Artikel &quot;LA Skatepark&quot;in AEM unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Aktualisierte Basisstile für den Artikel](assets/client-side-libraries/updated-base-styles.png)

   Sie sollten nun die aktualisierten Stile für den Artikel sehen. Möglicherweise müssen Sie eine harte Aktualisierung durchführen, um alle CSS-Dateien zu löschen, die vom Browser zwischengespeichert wurden.

   Es fängt an, den Morgenstürmen viel näher zu sehen!

   >[!NOTE]
   >
   > Die Schritte, die oben zum Erstellen und Bereitstellen des ui.frontend-Codes für AEM ausgeführt werden, werden automatisch ausgeführt, wenn ein Maven-Build vom Stamm des Projekts `mvn clean install -PautoInstallSinglePackage` ausgelöst wird.

>[!CAUTION]
>
> Die Verwendung des Moduls **ui.frontend** ist möglicherweise nicht für alle Projekte erforderlich. Das Modul **ui.frontend** erhöht die Komplexität und ist es nicht erforderlich/wünschenswert, einige dieser erweiterten Frontend-Tools (Sass, Webpack, npm..) zu verwenden, ist dies möglicherweise nicht erforderlich.

## Seiten- und Vorlagenaufnahme {#page-inclusion}

Als Nächstes sehen wir, wie die clientlibs auf der AEM Seite referenziert werden. Eine gängige Best Practice bei der Webentwicklung besteht darin, CSS in die HTML-Kopfzeile `<head>` und JavaScript direkt vor dem Schließen des Tags `</body>` einzuschließen.

1. Navigieren Sie im Modul **ui.apps** zu `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Strukturseitenkomponente](assets/client-side-libraries/customheaderlibs-html.png)

   Dies ist die `page`-Komponente, die zum Rendern aller Seiten in der WKND-Implementierung verwendet wird.

1. Öffnen Sie die Datei `customheaderlibs.html`. Beachten Sie die Zeilen `${clientlib.css @ categories='wknd.base'}`. Dies bedeutet, dass das CSS für die clientlib mit der Kategorie `wknd.base` über diese Datei eingeschlossen wird, einschließlich **clientlib-base** in der Kopfzeile aller unserer Seiten.

1. Aktualisieren Sie `customheaderlibs.html` , um einen Verweis auf Google-Schriftstile einzufügen, den wir zuvor im Modul **ui.frontend** angegeben haben.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Prüfen Sie die Datei `customfooterlibs.html`. Diese Datei, z. B. `customheaderlibs.html`, soll durch die Implementierung von Projekten überschrieben werden. Hier bedeutet die Zeile `${clientlib.js @ categories='wknd.base'}`, dass das JavaScript von **clientlib-base** unten auf allen unseren Seiten enthalten sein wird.

1. Exportieren Sie die Komponente `page` mithilfe der Entwicklertools oder Ihrer Maven-Fähigkeiten auf den AEM-Server.

1. Navigieren Sie zur Vorlage &quot;Artikelseite&quot;unter [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü **Seitenrichtlinie** aus, um das Dialogfeld **Seitenrichtlinie** zu öffnen.

   ![Seitenrichtlinien im Menü &quot;Artikel-Seitenvorlage&quot;](assets/client-side-libraries/template-page-policy.png)

   *Seiteninformationen > Seitenrichtlinie*

1. Beachten Sie, dass die Kategorien für `wknd.dependencies` und `wknd.site` hier aufgeführt sind. Standardmäßig werden Client-Bibliotheken, die über die Seitenrichtlinie konfiguriert werden, aufgeteilt, um das CSS in den Seitenkopf und das JavaScript am Textende einzuschließen. Bei Bedarf können Sie explizit auflisten, dass das clientlib-JavaScript im Seitenkopf geladen wird. Dies gilt für `wknd.dependencies`.

   ![Seitenrichtlinien im Menü &quot;Artikel-Seitenvorlage&quot;](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Es ist auch möglich, über die Seitenkomponente direkt auf `wknd.site` oder `wknd.dependencies` zu verweisen, indem Sie das Skript `customheaderlibs.html` oder `customfooterlibs.html` verwenden, wie wir es bereits für die clientlib `wknd.base` gesehen haben. Die Verwendung der Vorlage bietet eine gewisse Flexibilität, da Sie auswählen können, welche Client-Bibliotheken pro Vorlage verwendet werden. Wenn Sie beispielsweise über eine sehr umfangreiche JavaScript-Bibliothek verfügen, die nur für eine ausgewählte Vorlage verwendet wird.

1. Navigieren Sie zur Seite **LA Skateparks** , die mit der **Artikelseitenvorlage** erstellt wurde: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Sie sollten einen Unterschied in Schriftarten sehen.

1. Klicken Sie auf das Symbol **Seiteninformationen** und wählen Sie im Menü **Als veröffentlicht anzeigen** aus, um die Artikelseite außerhalb des AEM-Editors zu öffnen.

   ![Als veröffentlicht anzeigen](assets/client-side-libraries/view-as-published-article-page.png)

1. Zeigen Sie die Seitenquelle von [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) an und Sie sollten die folgenden clientlib-Verweise in der `<head>` sehen können:

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

   Beachten Sie, dass die Client-Bibliotheken den Proxy-Endpunkt `/etc.clientlibs` verwenden. Unten auf der Seite sollten auch die folgenden clientlib-Includes angezeigt werden:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Wenn Sie auf 6.5/6.4 folgen, werden die clientseitigen Bibliotheken nicht automatisch minimiert. Weitere Informationen finden Sie in der Dokumentation zum [HTML Library Manager zur Aktivierung der Minimierung (empfohlen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Auf der Veröffentlichungsseite ist es wichtig, dass die Client-Bibliotheken **nicht** von **/apps** bereitgestellt werden, da dieser Pfad aus Sicherheitsgründen mithilfe des Abschnitts [Dispatcher-Filter](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section) eingeschränkt werden sollte. Die [allowProxy-Eigenschaft](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) der Client-Bibliothek stellt sicher, dass CSS und JS von **/etc.clientlibs** bereitgestellt werden.

## Webpack DevServer - Statisches Markup {#webpack-dev-static}

In den vorherigen Übungen waren wir in der Lage, mehrere Sass-Dateien im Modul **ui.frontend** zu aktualisieren und durch einen Build-Prozess schließlich diese Änderungen zu sehen, die sich in AEM widerspiegeln. Als Nächstes werden wir uns mit Techniken beschäftigen, die einen [webpack-Dev-Server](https://webpack.js.org/configuration/dev-server/) nutzen, um unsere Frontend-Stile schnell gegen **static** HTML zu entwickeln.

Diese Technik ist praktisch, wenn die meisten Stile und Frontend-Code von einem dedizierten Frontend-Entwickler ausgeführt werden, der möglicherweise keinen einfachen Zugriff auf eine AEM-Umgebung hat. Diese Technik ermöglicht es dem FED auch, Änderungen direkt am HTML-Code vorzunehmen, der dann an einen AEM-Entwickler übergeben werden kann, um ihn als Komponenten zu implementieren.

1. Kopieren Sie die Seitenquelle der LA skatepark-Artikelseite unter [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Öffnen Sie die IDE erneut. Fügen Sie das kopierte Markup aus AEM in `index.html` im Modul **ui.frontend** unter `src/main/webpack/static` ein.
1. Bearbeiten Sie das kopierte Markup und entfernen Sie alle Verweise auf **clientlib-site** und **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Wir können diese Verweise entfernen, da der Webpack Development Server diese Artefakte automatisch generiert.

1. Starten Sie den webpack-Dev-Server von einem neuen Terminal aus, indem Sie den folgenden Befehl im Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dadurch sollte ein neues Browser-Fenster unter [http://localhost:8080/](http://localhost:8080/) mit statischem Markup geöffnet werden.

1. Bearbeiten Sie die Datei `src/main/webpack/site/_variables.scss` . Ersetzen Sie die `$text-color` -Regel durch Folgendes:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Speichern Sie die Änderungen.

1. Sie sollten die Änderungen automatisch im Browser unter [http://localhost:8080](http://localhost:8080) sehen.

   ![Lokale Änderungen am Webpack-Dev-Server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Überprüfen Sie die Datei `/aem-guides-wknd.ui.frontend/webpack.dev.js` . Dies enthält die Webpack-Konfiguration, die zum Starten des webpack-Dev-Servers verwendet wird. Beachten Sie, dass die Pfade `/content` und `/etc.clientlibs` von einer lokal ausgeführten Instanz von AEM verwaltet werden. So werden die Bilder und andere Client-Bibliotheken (die nicht vom **ui.frontend** -Code verwaltet werden) bereitgestellt.

   >[!CAUTION]
   >
   > Die Bild-src des statischen Markups verweist auf eine Live-Bildkomponente in einer lokalen AEM-Instanz. Bilder werden beschädigt angezeigt, wenn sich der Pfad zum Bild ändert, wenn AEM nicht gestartet wurde oder der Browser sich nicht bei der lokalen AEM-Instanz angemeldet hat. Bei Übergabe an eine externe Ressource ist es auch möglich, die Bilder durch statische Referenzen zu ersetzen.

1. Sie können den Webpack-Server über die Befehlszeile **stop** anhalten, indem Sie `CTRL+C` eingeben.

## Webpack DevServer - Watch und aemsync {#webpack-dev-watch}

Eine andere Methode besteht darin, Node.js auf alle Dateiänderungen an den src-Dateien im Modul `ui.frontend` zu überwachen. Bei jeder Dateiänderung wird die Client-Bibliothek schnell kompiliert und das Modul [aemsync](https://www.npmjs.com/package/aemsync) npm verwendet, um die Änderungen mit einem laufenden AEM-Server zu synchronisieren.

1. Starten Sie den webpack-Dev-Server im Modus **watch** von einem neuen Terminal aus, indem Sie den folgenden Befehl im Modul **ui.frontend** ausführen:

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

1. Navigieren Sie zu AEM und zum Artikel LA Skateparks : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![In AEM bereitgestellte Änderungen](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Die Änderungen sollten in AEM bereitgestellt werden. Es gibt eine leichte Verzögerung und Sie müssen den Browser manuell aktualisieren, um die Aktualisierungen anzuzeigen. Die direkte Anzeige der Änderungen in AEM ist jedoch nützlich, wenn Sie mit neuen Komponenten und Dialogfeldern arbeiten.

1. Kehren Sie die Änderung zu `_variables.scss` zurück und speichern Sie die Änderungen. Die Änderungen sollten nach einer leichten Verzögerung erneut mit der lokalen Instanz der AEM synchronisiert werden.

1. Beenden Sie den Webpack Development Server und führen Sie einen vollständigen Maven-Build aus dem Stammverzeichnis des Projekts aus:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Auch hier wird das Modul `ui.frontend` kompiliert, in Client-Bibliotheken umgewandelt und über das Modul `ui.apps` AEM bereitgestellt. Diesmal führt Maven jedoch alles für uns aus.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Die Artikelseite hat jetzt einige konsistente Stile, die mit der WKND-Marke übereinstimmen, und Sie haben sich mit dem Modul **ui.frontend** vertraut gemacht!

### Nächste Schritte {#next-steps}

Erfahren Sie, wie Sie einzelne Stile implementieren und Kernkomponenten mithilfe des Stilsystems von Experience Manager wiederverwenden. [Die Entwicklung mit dem Stilsystem ](style-system.md) umfasst die Verwendung des Stilsystems zum Erweitern von Kernkomponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlageneditors.

Zeigen Sie den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code und stellen Sie ihn lokal in der Git-Klammer `tutorial/client-side-libraries-solution` bereit.

1. Klonen Sie das Repository [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Sehen Sie sich die Verzweigung `tutorial/client-side-libraries-solution` an.

## Zusätzliche Tools und Ressourcen {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis ist ein Open-Source-Befehlszeilen-Tool, mit dem die Front-End-Entwicklung beschleunigt werden kann. Es wird von [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) und [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html) unterstützt.

Auf einer übergeordneten Ebene ist **aemfed** so konzipiert, dass Dateiänderungen im Modul **ui.apps** überwacht und automatisch mit einer laufenden AEM synchronisiert werden. Basierend auf den Änderungen wird ein lokaler Browser automatisch aktualisiert, was die Front-End-Entwicklung beschleunigt. Es ist auch für die Verwendung mit Sling Log Tracker entwickelt, um Server-seitige Fehler automatisch direkt im Terminal anzuzeigen.

Wenn Sie im Modul **ui.apps** viel Arbeit verrichten, HTML-Skripte ändern und benutzerdefinierte Komponenten erstellen, kann **aemfed** ein sehr leistungsstarkes Tool zur Verwendung sein. [Die vollständige Dokumentation finden Sie hier.](https://github.com/abmaonline/aemfed)

### Debuggen Client-seitiger Bibliotheken {#debugging-clientlibs}

Bei verschiedenen Methoden von **categories** und **embetten**, um mehrere Client-Bibliotheken einzuschließen, kann es schwierig sein, eine Fehlerbehebung durchzuführen. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Tools ist **Client-Bibliotheken neu erstellen** , wodurch AEM gezwungen werden, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Listet alle in der AEM-Instanz registrierten Client-Bibliotheken auf.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testausgabe**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : Ermöglicht es einem Benutzer, die erwartete HTML-Ausgabe von clientlib-Includes basierend auf der Kategorie anzuzeigen.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Überprüfung der Abhängigkeiten von Bibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  - hebt alle Abhängigkeiten oder eingebetteten Kategorien hervor, die nicht gefunden werden können.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-Bibliotheken neu erstellen**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  - ermöglicht es einem Benutzer, AEM zu erzwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken zu invalidieren. Dieses Tool ist besonders effektiv bei der Entwicklung mit LESS, da dies AEM zwingen kann, die generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu invalidieren und dann eine Seitenaktualisierung vorzunehmen, anstatt alle Bibliotheken neu zu erstellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![Client-Bibliothek neu erstellen](assets/client-side-libraries/rebuild-clientlibs.png)
