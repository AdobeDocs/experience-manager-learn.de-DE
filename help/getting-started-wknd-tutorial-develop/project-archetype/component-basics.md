---
title: Erste Schritte mit AEM Sites – Grundlagen zu Komponenten
description: Machen Sie sich mit der zugrunde liegenden Technologie einer Adobe Experience Manager (AEM)-Sites-Komponente durch ein einfaches „HelloWorld“-Beispiel vertraut. HTL, Sling-Modelle, Client-seitige Bibliotheken und Authoring-Dialogfelder werden als Themen behandelt.
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1226'
ht-degree: 100%

---

# Grundlagen zu Komponenten {#component-basics}

In diesem Kapitel erkunden wir die zugrunde liegende Technologie einer Adobe Experience Manager (AEM)-Sites-Komponente durch ein einfaches `HelloWorld`-Beispiel. Es werden kleine Änderungen an einer vorhandenen Komponente vorgenommen und dabei folgende Themen behandelt: Authoring, HTL, Sling-Modelle und Client-seitige Bibliotheken.

## Voraussetzungen {#prerequisites}

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](./overview.md#local-dev-environment).

In den Videos werden [Visual Studio Code](https://code.visualstudio.com/) und das [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)-Plug-in als IDE verwendet.

## Ziel {#objective}

1. Kennenlernen der Rolle von HTL-Vorlagen und Sling-Modellen zum dynamischen Rendern von HTML
1. Verstehen, wie Dialogfelder die Inhaltserstellung vereinfachen
1. Lernen der wichtigsten Grundlagen von Client-seitigen Bibliotheken, um CSS und JavaScript zur Unterstützung von Komponenten einzuschließen

## Was Sie erstellen werden {#what-build}

In diesem Kapitel nehmen Sie verschiedene Änderungen an einer einfachen `HelloWorld`-Komponente vor. Beim Aktualisieren der `HelloWorld`-Komponente lernen Sie die wichtigsten Bereiche der Entwicklung von AEM-Komponenten kennen.

## Ausgangsprojekt für dieses Kapitel {#starter-project}

Dieses Kapitel baut auf einem generischen Projekt auf, erzeugt vom [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype). Sehen Sie sich das folgende Video an und beachten Sie die [Voraussetzungen](#prerequisites), um mit den ersten Schritten beginnen zu können.

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Ausprobieren des Ausgangsprojekts überspringen.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

Öffnen Sie ein neues Befehlszeilen-Terminal und führen Sie die folgenden Aktionen aus.

1. Klonen Sie das Repository [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) in einem leeren Verzeichnis:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Optional können Sie das im vorherigen Kapitel [Projekteinrichtung](./project-setup.md) erstellte Projekt weiter verwenden.

1. Navigieren Sie zum Ordner `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Verwenden Sie den folgenden Befehl, um das Projekt zu erstellen und einer lokalen Instanz von AEM bereitzustellen:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das Profil `classic` an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importieren Sie das Projekt in Ihre bevorzugte IDE, indem Sie die Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment) befolgen.

## Komponenten-Authoring {#component-authoring}

Komponenten können als kleine, modulare Bausteine einer Web-Seite betrachtet werden. Um Komponenten wiederverwenden zu können, müssen die Komponenten konfigurierbar sein. Dies erfolgt über das Authoring-Dialogfeld. Als Nächstes erstellen wir eine einfache Komponente und untersuchen, wie Werte aus dem Dialogfeld in AEM beibehalten werden.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen, im obigen Video ausgeführten Schritte.

1. Erstellen Sie eine Seite mit dem Namen **Komponentengrundlagen** unter **WKND-Website** `>` **DE** `>` **de**.
1. Fügen Sie die **„Hello World“-Komponente** der neu erstellten Seite hinzu.
1. Öffnen Sie das Dialogfeld für die Komponente und geben Sie Text ein. Speichern Sie die Änderungen, um die Nachricht auf der Seite anzuzeigen.
1. Wechseln Sie in den Entwicklermodus, zeigen Sie den Inhaltspfad in CRXDE-Lite an und überprüfen Sie die Eigenschaften der Komponenteninstanz.
1. Verwenden Sie CRXDE-Lite, um das `cq:dialog`- und `helloworld.html`-Skript aus `/apps/wknd/components/content/helloworld` anzuzeigen.

## HTL und Dialogfelder {#htl-dialogs}

**[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=de)** (HTML Template Language) ist eine leichte, Server-seitige Vorlagensprache, die von AEM-Komponenten zum Rendern von Inhalten verwendet wird.

**Dialogfelder** definieren die verfügbaren Konfigurationen, die für eine Komponente erstellt werden können.

Als Nächstes aktualisieren wir das `HelloWorld`-HTL-Skript, um eine zusätzliche Begrüßung vor der Textnachricht anzuzeigen.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen, im obigen Video ausgeführten Schritte.

1. Wechseln Sie zur IDE und öffnen Sie das Projekt im `ui.apps`-Modul.
1. Öffnen Sie die Datei `helloworld.html` und aktualisieren Sie das HTML-Markup.
1. Verwenden Sie IDE-Tools wie [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), um die Dateiänderung mit der lokalen AEM-Instanz zu synchronisieren.
1. Kehren Sie zum Browser zurück und beobachten Sie, wie sich das Komponenten-Rendering geändert hat.
1. Öffnen Sie die `.content.xml`-Datei, die das Dialogfeld für die `HelloWorld`-Komponente definiert, unter:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aktualisieren Sie das Dialogfeld, um ein zusätzliches Textfeld mit der Bezeichnung **Titel** mit dem Namen `./title` hinzuzufügen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Öffnen Sie erneut die Datei `helloworld.html`, die das Haupt-HTL-Skript darstellt, das für das Rendern der `HelloWorld`-Komponente über folgenden Pfad verantwortlich ist:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aktualisieren Sie `helloworld.html`, um den Wert des Textfelds **Grußformel** als Teil eines `H1`-Tags zu rendern:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Entwickler-Plug-ins oder durch Verwendung des Maven-Tools auf einer lokalen Instanz von AEM bereit.

## Sling-Modelle {#sling-models}

Sling-Modelle sind von Anmerkungen gesteuerte Java™-„POJOs“ (Plain Old Java™ Objects), die die Zuordnung von Daten aus JCR zu Java™-Variablen erleichtern. Sie bieten bei der Entwicklung im Kontext von AEM auch mehrere andere Vorteile.

Nehmen wir als Nächstes einige Aktualisierungen am `HelloWorldModel`-Sling-Modell vor, um Business-Logik auf die im JCR gespeicherten Werte anzuwenden, bevor sie auf die Seite ausgegeben werden.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. Öffnen Sie die Datei `HelloWorldModel.java`. Das ist das mit der `HelloWorld`-Komponente verwendete Sling-Modell.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Fügen Sie die folgenden Importanweisungen hinzu:

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aktualisieren Sie die `@Model`-Anmerkung zur Verwendung von `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Fügen Sie die folgenden Zeilen zur `HelloWorldModel`-Klasse hinzu, um die Werte der JCR-Eigenschaften `title` und `text` der Komponente Java™-Variablen zuzuordnen:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. Fügen Sie die folgende Methode `getTitle()` zur `HelloWorldModel`-Klasse hinzu, die den Wert der Eigenschaft namens `title` zurückgibt. Mit dieser Methode wird die zusätzliche Logik hinzugefügt, um den Zeichenfolgenwert „Standardwert hier!“ zurückzugeben. Wenn die Eigenschaft `title` null oder leer ist:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Fügen Sie die folgende Methode `getText()` zur `HelloWorldModel`-Klasse hinzu, die den Wert der Eigenschaft namens `text` zurückgibt. Diese Methode wandelt die Zeichenfolge in Großbuchstaben um.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Erstellen Sie das Paket und stellen Sie es über das `core`-Modul bereit:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Verwenden Sie für AEM 6.4/6.5 `mvn clean install -PautoInstallBundle -Pclassic`

1. Aktualisieren Sie die Datei `helloworld.html` unter `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`, um die neu erstellten Methoden des `HelloWorld`-Modells zu verwenden.

   Das `HelloWorld`-Modell wird für diese Komponenteninstanz über die HTL-Anweisung `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"` instanziiert, wobei die Instanz in der Variablen `model` gespeichert wird.

   Die `HelloWorld`-Modellinstanz ist nun in HTL über die Variable `model` mithilfe von `HelloWord` verfügbar. Diese Methodenaufrufe können beispielsweise eine verkürzte Methodensyntax verwenden: `${model.getTitle()}` kann zu `${model.title}` gekürzt werden.

   Ebenso werden in alle HTL-Skripte [globale Objekte](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=de) eingefügt, auf die mit derselben Syntax wie bei den Sling-Modellojekten zugegriffen werden kann.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld" 
       data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plug-ins oder unter Verwendung von Maven auf einer lokalen Instanz von AEM bereit.

## Client-seitige Bibliotheken {#client-side-libraries}

Client-seitige Bibliotheken, kurz `clientlibs`, bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine Implementierung von AEM Sites erforderlich sind. Client-seitige Bibliotheken sind die Standardmethode zum Einschließen von CSS und JavaScript auf einer Seite in AEM.

Das [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de)-Modul ist ein entkoppeltes [Webpack](https://webpack.js.org/)-Projekt, das in den Build-Prozess integriert ist. Dies ermöglicht die Verwendung beliebter Frontend-Bibliotheken wie Sass, LESS und TypeScript. Das `ui.frontend`-Modul wird im [Kapitel zu Client-seitigen Bibliotheken](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md) eingehender behandelt.

Aktualisieren Sie anschließend die CSS-Stile für die `HelloWorld`-Komponente.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen Schritte, die im obigen Video ausgeführt werden.

1. Öffnen Sie ein Terminal-Fenster und gehen Sie zum `ui.frontend`-Verzeichnis

1. Führen Sie im `ui.frontend`-Verzeichnis den Befehl `npm install npm-run-all --save-dev` zum Installieren des Knotenmoduls [npm-run-all](https://www.npmjs.com/package/npm-run-all) aus. Dieser Schritt ist **für mit Archetype 39 generierte AEM-Projekte erforderlich**. In der nächsten Archetype-Version ist dies nicht mehr erforderlich.

1. Führen Sie als Nächstes den Befehl `npm run watch` aus:

   ```shell
   $ npm run watch
   ```

1. Wechseln Sie zur IDE und öffnen Sie das Projekt zum `ui.frontend`-Modul.
1. Öffnen Sie die Datei `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. Aktualisieren Sie die Datei, sodass ein roter Titel angezeigt wird:

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. Im Terminal sollte eine Aktivität angezeigt werden, die angibt, dass das `ui.frontend`-Modul kompiliert wird und die Änderungen mit der lokalen Instanz von AEM synchronisiert werden.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. Kehren Sie zum Browser zurück und beachten Sie, dass sich die Titelfarbe geändert hat.

   ![Aktualisierung der Komponentengrundlagen](assets/component-basics/color-update.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie kennen jetzt die Grundlagen der Komponentenentwicklung in Adobe Experience Manager!

### Nächste Schritte {#next-steps}

Machen Sie sich im nächsten Kapitel [Seiten und Vorlagen](pages-templates.md) mit den Seiten und Vorlagen von Adobe Experience Manager vertraut. Erfahren Sie, wie Kernkomponenten in das Projekt integriert werden, und lernen Sie erweiterte Richtlinienkonfigurationen für bearbeitbare Vorlagen kennen, um eine gut strukturierte Artikelseitenvorlage zu erstellen.

Sehen Sie sich den fertigen Code in [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code lokal und stellen Sie ihn in der Git-Verzweigung bereit`tutorial/component-basics-solution`.
