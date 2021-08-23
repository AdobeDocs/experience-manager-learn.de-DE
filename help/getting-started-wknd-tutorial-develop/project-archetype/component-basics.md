---
title: Erste Schritte mit AEM Sites - Komponentengrundlagen
description: Machen Sie sich mit der zugrunde liegenden Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch ein einfaches "HelloWorld"-Beispiel vertraut. Themen zu HTL, Sling-Modellen, Client-seitigen Bibliotheken und Autorendialogfeldern werden untersucht.
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Kernkomponenten, Entwicklertools
topic: Content Management, Entwicklung
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 3%

---


# Komponenten – Grundlagen {#component-basics}

In diesem Kapitel werden wir die zugrunde liegende Technologie einer Adobe Experience Manager (AEM) Sites-Komponente anhand eines einfachen `HelloWorld`-Beispiels untersuchen. Es werden kleine Änderungen an einer vorhandenen Komponente vorgenommen, die Themen wie Authoring, HTL, Sling-Modelle, Client-seitige Bibliotheken abdecken.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](./overview.md#local-dev-environment).

Die in den Videos verwendete IDE ist [Visual Studio Code](https://code.visualstudio.com/) und das Plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) .

## Ziele {#objective}

1. Erfahren Sie mehr über die Rolle von HTL-Vorlagen und Sling-Modellen zum dynamischen Rendern von HTML.
1. Erfahren Sie, wie Dialogfelder zur Erleichterung der Inhaltserstellung verwendet werden.
1. Lernen Sie die Grundlagen von Client-seitigen Bibliotheken kennen, um CSS und JavaScript einzuschließen und eine Komponente zu unterstützen.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel nehmen Sie einige Änderungen an einer sehr einfachen `HelloWorld`-Komponente vor. Im Zuge der Aktualisierung der Komponente `HelloWorld` erfahren Sie mehr über die wichtigsten Bereiche der Entwicklung AEM Komponenten.

## Chapter Starter Project {#starter-project}

Dieses Kapitel baut auf einem generischen Projekt auf, das vom [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) generiert wurde. Sehen Sie sich das folgende Video an und sehen Sie sich die [Voraussetzungen](#prerequisites) an, um loszulegen!

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Öffnen Sie ein neues Befehlszeilen-Terminal und führen Sie die folgenden Aktionen aus.

1. Klonen Sie in einem leeren Verzeichnis das Repository [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Optional können Sie das im vorherigen Kapitel generierte Projekt [Projekt-Setup](./project-setup.md) weiterhin verwenden.

1. Navigieren Sie zum Ordner `aem-guides-wknd` .

   ```shell
   $ cd aem-guides-wknd
   ```

1. Erstellen Sie das Projekt und stellen Sie es mit dem folgenden Befehl in einer lokalen Instanz von AEM bereit:

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

## Komponentenbearbeitung {#component-authoring}

Komponenten können als kleine modulare Bausteine einer Webseite betrachtet werden. Um Komponenten wiederzuverwenden, müssen die Komponenten konfigurierbar sein. Dies erfolgt über das Dialogfeld &quot;Autor&quot;. Als Nächstes erstellen wir eine einfache Komponente und überprüfen, wie Werte aus dem Dialogfeld in AEM beibehalten werden.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen Schritte, die im obigen Video ausgeführt werden.

1. Erstellen Sie eine neue Seite mit dem Namen **Komponentengrundlagen** unter **WKND-Site** `>` **US** `>` **en**.
1. Fügen Sie der neu erstellten Seite die Komponente **Hello World** hinzu.
1. Öffnen Sie das Dialogfeld für die Komponente und geben Sie Text ein. Speichern Sie die Änderungen, um die Meldung auf der Seite anzuzeigen.
1. Wechseln Sie in den Entwicklermodus, zeigen Sie den Inhaltspfad in CRXDE-Lite an und überprüfen Sie die Eigenschaften der Komponenteninstanz.
1. Verwenden Sie CRXDE-Lite, um das Skript `cq:dialog` und `helloworld.html` anzuzeigen, das sich unter `/apps/wknd/components/content/helloworld` befindet.

## HTL (HTML-Vorlagensprache) und Dialogfelder {#htl-dialogs}

HTML-Vorlagensprache oder **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)** ist eine leichte, serverseitige Vorlagensprache, die von AEM Komponenten zum Rendern von Inhalten verwendet wird.

**** Dialogfelder definieren die verfügbaren Konfigurationen, die für eine Komponente vorgenommen werden können.

Als Nächstes wird das HTML-Skript `HelloWorld` aktualisiert, um vor der Textnachricht einen zusätzlichen Gruß anzuzeigen.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen Schritte, die im obigen Video ausgeführt werden.

1. Wechseln Sie zur IDE und öffnen Sie das Projekt zum Modul `ui.apps` .
1. Öffnen Sie die Datei `helloworld.html` und nehmen Sie eine Änderung am HTML-Markup vor.
1. Verwenden Sie die IDE-Tools wie [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), um die Dateiänderung mit der lokalen AEM-Instanz zu synchronisieren.
1. Kehren Sie zum Browser zurück und beobachten Sie, dass sich der Komponenten-Renderer geändert hat.
1. Öffnen Sie die Datei `.content.xml` , die das Dialogfeld für die Komponente `HelloWorld` definiert, unter:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aktualisieren Sie das Dialogfeld, um ein zusätzliches Textfeld mit dem Namen **Titel** mit dem Namen `./title` hinzuzufügen:

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

1. Öffnen Sie die Datei `helloworld.html` erneut, die das HTL-Haupt darstellt, das für die Wiedergabe der `HelloWorld`-Komponente verantwortlich ist. Sie befindet sich unter:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aktualisieren Sie `helloworld.html` , um den Wert des Textfelds **Grußformel** als Teil eines `H1` -Tags anzuzeigen:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Entwickler-Plug-ins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Sling-Modelle {#sling-models}

Sling-Modelle sind von Anmerkungen gesteuerte Java-&quot;POJOs&quot;(Plain Old Java Objects), die die Zuordnung von Daten aus dem JCR zu Java-Variablen erleichtern und eine Reihe anderer Eigenschaften bei der Entwicklung im Kontext von AEM bieten.

Als Nächstes nehmen wir einige Aktualisierungen am Sling-Modell `HelloWorldModel` vor, um eine gewisse Geschäftslogik auf die im JCR gespeicherten Werte anzuwenden, bevor sie an die Seite ausgegeben werden.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Öffnen Sie die Datei `HelloWorldModel.java`, die das Sling-Modell ist, das mit der Komponente `HelloWorld` verwendet wird.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Fügen Sie die folgenden Importanweisungen hinzu:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aktualisieren Sie die `@Model` -Anmerkung, um eine `DefaultInjectionStrategy` zu verwenden:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Fügen Sie der Klasse `HelloWorldModel` die folgenden Zeilen hinzu, um die Werte der JCR-Eigenschaften der Komponente `title` und `text` Java-Variablen zuzuordnen:

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

1. Fügen Sie der `HelloWorldModel`-Klasse die folgende Methode `getTitle()` hinzu, die den Wert der Eigenschaft `title` zurückgibt. Diese Methode fügt die zusätzliche Logik hinzu, um den Zeichenfolgenwert &quot;Default Value here&quot;zurückzugeben. wenn die Eigenschaft `title` null oder leer ist:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Fügen Sie der `HelloWorldModel`-Klasse die folgende Methode `getText()` hinzu, die den Wert der Eigenschaft `text` zurückgibt. Diese Methode wandelt den String in alle Großbuchstaben um.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Erstellen und stellen Sie das Bundle über das Modul `core` bereit:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Verwenden Sie bei Verwendung von AEM 6.4/6.5 `mvn clean install -PautoInstallBundle -Pclassic`

1. Aktualisieren Sie die Datei `helloworld.html` unter `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`, um die neu erstellten Methoden des `HelloWorld`-Modells zu verwenden:

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
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plug-ins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Client-seitige Bibliotheken {#client-side-libraries}

Client-seitige Bibliotheken, kurz clientlibs , bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Clientseitige Bibliotheken sind die Standardmethode zum Einschließen von CSS und JavaScript in eine Seite in AEM.

Als Nächstes werden wir einige CSS-Stile für die Komponente `HelloWorld` einfügen, um die Grundlagen der clientseitigen Bibliotheken zu verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Im Folgenden finden Sie die allgemeinen Schritte, die im obigen Video ausgeführt werden.

1. Erstellen Sie unter `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` einen neuen Ordner mit dem Namen `clientlib-helloworld`.
1. Erstellen Sie einen Ordner und eine Dateistruktur wie die folgende unter `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Füllen Sie `helloworld.css` wie folgt:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. Füllen Sie `helloworld/clientlibs/css.txt` wie folgt:

   ```plain
   #base=css
   helloworld.css
   ```

1. Füllen Sie `helloworld/clientlibs/js/helloworld.js` wie folgt:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. Füllen Sie `helloworld/clientlibs/js.txt` wie folgt:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aktualisieren Sie die Datei `clientlib-helloworld/.content.xml` , um die folgenden Eigenschaften einzuschließen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Aktualisieren Sie die `clientlibs/clientlib-base/.content.xml`-Datei auf **embed** die Kategorie `wknd.helloworld` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Stellen Sie die Änderungen mithilfe des Entwickler-Plug-ins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

   >[!NOTE]
   >
   > CSS und JavaScript werden aus Leistungsgründen häufig vom Browser zwischengespeichert. Wenn Sie die Änderung für die Client-Bibliothek nicht sofort sehen, führen Sie eine harte Aktualisierung durch und löschen Sie den Cache des Browsers. Es kann hilfreich sein, ein Inkognito-Fenster zu verwenden, um einen neuen Cache sicherzustellen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade die Grundlagen der Komponentenentwicklung in Adobe Experience Manager gelernt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit Adobe Experience Manager-Seiten und -Vorlagen im nächsten Kapitel [Seiten und Vorlagen](pages-templates.md) vertraut. Erfahren Sie, wie Kernkomponenten in das Projekt integriert werden, und lernen Sie erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen kennen, um eine gut strukturierte Artikelseitenvorlage zu erstellen.

Zeigen Sie den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code und stellen Sie ihn lokal auf der Git-Verzweigung `tutorial/component-basics-solution` bereit.
