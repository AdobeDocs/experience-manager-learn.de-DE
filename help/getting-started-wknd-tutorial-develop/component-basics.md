---
title: Erste Schritte mit AEM Sites - Grundlagen der Komponenten
description: Machen Sie sich mit der zugrunde liegenden Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch ein einfaches "HelloWorld"-Beispiel vertraut. Themen wie HTML, Sling-Modelle, clientseitige Bibliotheken und Autorendialoge werden erforscht.
sub-product: Sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 4%

---


# Komponenten – Grundlagen {#component-basics}

In diesem Kapitel werden wir die zugrunde liegende Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch ein einfaches `HelloWorld`-Beispiel untersuchen. Es werden kleine Änderungen an einer vorhandenen Komponente vorgenommen, die Themen Authoring, HTL, Sling-Modelle, clientseitige Bibliotheken abdecken.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

Die in den Videos verwendete IDE ist [Visual Studio-Code](https://code.visualstudio.com/) und das [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)-Plugin.

## Vorgabe {#objective}

1. Erfahren Sie, welche Rolle HTML-Vorlagen und Sling-Modelle beim dynamischen Rendern von HTML spielen.
1. Verstehen Sie, wie Dialoge verwendet werden, um das Authoring von Inhalten zu erleichtern.
1. Hier lernen Sie die Grundlagen der clientseitigen Bibliotheken kennen, um CSS und JavaScript zur Unterstützung einer Komponente einzuschließen.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel führen Sie einige Änderungen an einer sehr einfachen `HelloWorld` Komponente durch. Im Zuge der Aktualisierung der Komponente `HelloWorld` erfahren Sie mehr über die wichtigsten Bereiche der Entwicklung AEM Komponenten.

## Chapter Starter Project {#starter-project}

Dieses Kapitel baut auf einem generischen Projekt auf, das vom [AEM Projektarchiv](https://github.com/adobe/aem-project-archetype) generiert wurde. Sehen Sie sich das folgende Video an und überprüfen Sie die [Voraussetzungen](#prerequisites), um loszulegen!

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt erneut verwenden und die Schritte zum Auschecken des Startprojekts überspringen.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Öffnen Sie ein neues Befehlszeilenterminal und führen Sie die folgenden Schritte aus.

1. In einem leeren Verzeichnis klonen Sie das [aem-guides-work](https://github.com/adobe/aem-guides-wknd)-Repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Optional können Sie das im vorherigen Kapitel [Projekt-Setup](./project-setup.md) generierte Projekt weiterhin verwenden.

1. Navigieren Sie zum Ordner `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Erstellen Sie das Projekt und stellen Sie es mit dem folgenden Befehl auf einer lokalen Instanz von AEM bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importieren Sie das Projekt in Ihre bevorzugte IDE, indem Sie die Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment) befolgen.

## Komponenten-Authoring {#component-authoring}

Komponenten können als kleine Bausteine einer Webseite betrachtet werden. Zur Wiederverwendung von Komponenten müssen die Komponenten konfigurierbar sein. Dies erfolgt über den Autorendialog. Als Nächstes erstellen wir eine einfache Komponente und überprüfen, wie Werte aus dem Dialog in AEM beibehalten werden.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Erstellen Sie eine neue Seite mit dem Namen **Komponenten-Grundlagen** unter **WKND-Site** `>` **US** `>` **en**.
1. hinzufügen Sie die **Hello World-Komponente** auf die neu erstellte Seite.
1. Öffnen Sie das Dialogfeld für die Komponente und geben Sie Text ein. Speichern Sie die Änderungen, um die Meldung auf der Seite anzuzeigen.
1. Wechseln Sie in den Entwicklermodus und Ansicht des Inhaltspfads in CRXDE-Lite und überprüfen Sie die Eigenschaften der Komponenteninstanz.
1. Verwenden Sie CRXDE-Lite, um das `cq:dialog`- und `helloworld.html`-Skript unter `/apps/wknd/components/content/helloworld` Ansicht.

## HTML (HTML-Vorlagensprache) und Dialoge {#htl-dialogs}

Die HTML-Vorlagensprache oder **[HTL](https://docs.adobe.com/content/help/de-DE/experience-manager-htl/using/getting-started/getting-started.html)** ist eine leichte, serverseitige Vorlagensprache, die von AEM zum Wiedergeben von Inhalten verwendet wird.

**&quot;** Dialog&quot;definiert die verfügbaren Konfigurationen, die für eine Komponente festgelegt werden können.

Als Nächstes werden wir das `HelloWorld` HTML-Skript aktualisieren, um eine zusätzliche Begrüßung vor der Textnachricht anzuzeigen.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Wechseln Sie zur IDE und öffnen Sie das Projekt zum Modul `ui.apps`.
1. Öffnen Sie die Datei `helloworld.html` und ändern Sie das HTML-Markup.
1. Verwenden Sie die IDE-Werkzeuge wie [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), um die Dateiänderung mit der lokalen AEM zu synchronisieren.
1. Kehren Sie zum Browser zurück und beobachten Sie, dass sich der Komponentenrendering geändert hat.
1. Öffnen Sie die Datei `.content.xml`, die das Dialogfeld für die Komponente `HelloWorld` unter folgender Adresse definiert:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Aktualisieren Sie das Dialogfeld, um ein zusätzliches Textfeld mit dem Namen **title** mit dem Namen `./title` hinzuzufügen:

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

1. Öffnen Sie die Datei `helloworld.html`, die das HTML-Hauptskript darstellt, das für die Wiedergabe der `HelloWorld`-Komponente verantwortlich ist, erneut unter:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Aktualisieren Sie `helloworld.html`, um den Wert des Textfelds **Gruß** als Teil eines `H1`-Tags anzuzeigen:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Sling-Modelle {#sling-models}

Sling-Modelle sind durch Anmerkungen angetriebene Java-&quot;POJOs&quot;(einfache alte Java-Objekte), die die Zuordnung von Daten aus der JCR zu Java-Variablen erleichtern und eine Reihe anderer Schönheiten bei der Entwicklung im Kontext von AEM bieten.

Als Nächstes werden wir einige Aktualisierungen am Sling-Modell `HelloWorldModel` vornehmen, um eine gewisse Geschäftslogik auf die in der JCR gespeicherten Werte anzuwenden, bevor sie an die Seite ausgegeben werden.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Öffnen Sie die Datei `HelloWorldModel.java`, bei der es sich um das Sling-Modell handelt, das mit der Komponente `HelloWorld` verwendet wird.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. hinzufügen die folgenden Importanweisungen:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Aktualisieren Sie die `@Model`-Anmerkung, um eine `DefaultInjectionStrategy` zu verwenden:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. hinzufügen Sie die folgenden Zeilen der `HelloWorldModel`-Klasse zu, um die Werte der JCR-Eigenschaften `title` und `text` der Komponente Java-Variablen zuzuordnen:

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

1. hinzufügen Sie die folgende Methode `getTitle()` zur `HelloWorldModel`-Klasse, die den Wert der Eigenschaft `title` zurückgibt. Diese Methode fügt die zusätzliche Logik hinzu, um den Zeichenfolgenwert &quot;Standardwert hier!&quot;zurückzugeben. wenn die Eigenschaft `title` null oder leer ist:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. hinzufügen Sie die folgende Methode `getText()` zur `HelloWorldModel`-Klasse, die den Wert der Eigenschaft `text` zurückgibt. Diese Methode wandelt den String in alle Großbuchstaben um.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Erstellen und stellen Sie das Bundle über das `core`-Modul bereit:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Verwenden Sie bei Verwendung von AEM 6.4/6.5 `mvn clean install -PautoInstallBundle -Pclassic`

1. Aktualisieren Sie die Datei `helloworld.html` bei `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`, um die neu erstellten Methoden des `HelloWorld`-Modells zu verwenden:

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

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Client-seitige Bibliotheken {#client-side-libraries}

Clientseitige Bibliotheken, kurz clientlibs, bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Clientseitige Bibliotheken sind die Standardmethode, um CSS und JavaScript auf einer Seite in AEM einzuschließen.

Als Nächstes werden wir einige CSS-Stile für die Komponente `HelloWorld` einschließen, um die Grundlagen clientseitiger Bibliotheken zu verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. unter `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` einen neuen Ordner mit dem Namen `clientlib-helloworld` erstellen.
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

1. Aktualisieren Sie die Datei `clientlib-helloworld/.conten.xml`, um die folgenden Eigenschaften einzuschließen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Aktualisieren Sie die `clientlibs/clientlib-base/.content.xml`-Datei auf **embed** die `wknd.helloworld`-Kategorie:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Stellen Sie die Änderungen mithilfe des Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

   >[!NOTE]
   >
   > CSS und JavaScript werden häufig aus Leistungsgründen vom Browser zwischengespeichert. Wenn Sie die Änderung für die Client-Bibliothek nicht sofort sehen, führen Sie eine harte Aktualisierung durch und leeren Sie den Cache des Browsers. Es kann hilfreich sein, ein Inkognito-Fenster zu verwenden, um einen neuen Cache zu gewährleisten.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade die Grundlagen der Komponentenentwicklung in Adobe Experience Manager gelernt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit Adobe Experience Manager-Seiten und -Vorlagen im nächsten Kapitel [Seiten und Vorlagen](pages-templates.md) vertraut. Verstehen Sie, wie Kernkomponenten in das Projekt integriert werden, und lernen Sie erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen kennen, um eine gut strukturierte Artikelseitenvorlage zu erstellen.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes auf der Git-Verzweigung `tutorial/component-basics-solution`.

