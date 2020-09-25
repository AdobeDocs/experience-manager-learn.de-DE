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
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 6%

---


# Komponenten – Grundlagen {#component-basics}

In diesem Kapitel werden wir die zugrunde liegende Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch ein einfaches `HelloWorld` Beispiel untersuchen. Es werden kleine Änderungen an einer vorhandenen Komponente vorgenommen, die Themen Authoring, HTL, Sling-Modelle, clientseitige Bibliotheken abdecken.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

## Vorgabe {#objective}

1. Erfahren Sie, welche Rolle HTML-Vorlagen und Sling-Modelle beim dynamischen Rendern von HTML spielen.
1. Verstehen Sie, wie Dialoge verwendet werden, um das Authoring von Inhalten zu erleichtern.
1. Hier lernen Sie die Grundlagen der clientseitigen Bibliotheken kennen, um CSS und JavaScript zur Unterstützung einer Komponente einzuschließen.

## Was Sie erstellen {#what-you-will-build}

In diesem Kapitel führen Sie einige Änderungen an einer sehr einfachen `HelloWorld` Komponente durch. Im Zuge der Aktualisierung der `HelloWorld` Komponente erfahren Sie mehr über die wichtigsten Bereiche der Entwicklung AEM Komponenten.

## Kapitel-Startprojekt {#starter-project}

Dieses Kapitel baut auf einem generischen Projekt auf, das vom [AEM Projektarchiv](https://github.com/adobe/aem-project-archetype)generiert wurde. Sehen Sie sich das folgende Video an und überprüfen Sie die [Voraussetzungen](#prerequisites) für den Einstieg!

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Öffnen Sie ein neues Befehlszeilenterminal und führen Sie die folgenden Schritte aus.

1. In einem leeren Ordner klonen Sie das [Repository aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Optional können Sie die [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) Verzweigung direkt herunterladen.

1. Navigieren Sie zum `aem-guides-wknd` Ordner:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Zu `component-basics/start` Zweig wechseln:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Erstellen Sie das Projekt und stellen Sie es mit dem folgenden Befehl auf einer lokalen Instanz von AEM bereit:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importieren Sie das Projekt in Ihre bevorzugte IDE, indem Sie die Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment)befolgen.

## Komponenten-Authoring {#component-authoring}

Komponenten können als kleine Bausteine einer Webseite betrachtet werden. Zur Wiederverwendung von Komponenten müssen die Komponenten konfigurierbar sein. Dies erfolgt über den Autorendialog. Als Nächstes erstellen wir eine einfache Komponente und überprüfen, wie Werte aus dem Dialog in AEM beibehalten werden.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Erstellen Sie eine neue Seite mit dem Namen **Komponentengrundlagen** unter der **WKND-Site** `>` US **** en `>` ****.
1. hinzufügen Sie die **Komponente** &quot;Hello World&quot;auf die neu erstellte Seite.
1. Öffnen Sie das Dialogfeld für die Komponente und geben Sie Text ein. Speichern Sie die Änderungen, um die Meldung auf der Seite anzuzeigen.
1. Wechseln Sie in den Entwicklermodus und Ansicht des Inhaltspfads in CRXDE-Lite und überprüfen Sie die Eigenschaften der Komponenteninstanz.
1. Verwenden Sie CRXDE-Lite, um das `cq:dialog` und das `helloworld.html` Skript unter `/apps/wknd/components/content/helloworld`.

## HTML-Vorlagensprache (HTL) {#htl-templates}

HTML-Vorlagensprache oder [HTML](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) ist eine leichte, serverseitige Vorlagensprache, die von AEM zur Inhaltswiedergabe verwendet wird.

Als Nächstes wird das `HelloWorld` HTML-Skript aktualisiert, um eine zusätzliche Begrüßung vor der Textnachricht anzuzeigen.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. Wechseln Sie zur Eclipse IDE und öffnen Sie das Projekt zum `ui.apps` Modul.
1. Öffnen Sie die `.content.xml` Datei, die das Dialogfeld für die `HelloWorld` Komponente definiert unter:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Aktualisieren Sie das Dialogfeld, um ein zusätzliches Textfeld mit dem Namen **Gruß** mit dem Namen `./greeting`:

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
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
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

1. Öffnen Sie die Datei `helloworld.html`, die das wichtigste HTML-Skript darstellt, das für die Wiedergabe der `HelloWorld` Komponente verantwortlich ist. Sie befindet sich unter:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Aktualisieren Sie, `helloworld.html` um den Wert des Textfelds &quot; **Gruß** &quot;als Teil eines `H1` -Tags wiederzugeben:

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Sling-Modelle {#sling-models}

Sling-Modelle sind durch Anmerkungen angetriebene Java-&quot;POJOs&quot;(einfache alte Java-Objekte), die die Zuordnung von Daten aus der JCR zu Java-Variablen erleichtern und eine Reihe anderer Schönheiten bei der Entwicklung im Kontext von AEM bieten.

Als Nächstes werden wir einige Aktualisierungen am `HelloWorldModel` Sling-Modell vornehmen, um eine gewisse Geschäftslogik auf die in der JCR gespeicherten Werte anzuwenden, bevor sie an die Seite ausgegeben werden.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Öffnen Sie die Datei `HelloWorldModel.java`, bei der es sich um das mit der `HelloWorld` Komponente verwendete Sling-Modell handelt.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. hinzufügen Sie die folgenden Zeilen zur `HelloWorldModel` Klasse, um die Werte der JCR-Eigenschaften der Komponente `greeting` und `text` den Java-Variablen zuzuordnen:

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. hinzufügen die folgende Methode `getGreeting()` der `HelloWorldModel` Klasse, die den Wert der Eigenschaft namens `greeting`zurückgibt. Diese Methode fügt zusätzliche Logik hinzu, um den Zeichenfolgenwert &quot;Hello&quot;zurückzugeben, wenn die Eigenschaft null oder leer `greeting` ist:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. hinzufügen die folgende Methode `getTextUpperCase()` der `HelloWorldModel` Klasse, die den Wert der Eigenschaft namens `text`zurückgibt. Diese Methode transformiert den String in alle Zeichen des Typs &quot;upperCase&quot;.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Aktualisieren Sie die Datei `helloworld.html` auf `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` die neu erstellten Methoden des `HelloWorld` Modells:

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Client-seitige Bibliotheken {#client-side-libraries}

Clientseitige Bibliotheken, kurz clientlibs, bieten einen Mechanismus zum Organisieren und Verwalten von CSS- und JavaScript-Dateien, die für eine AEM Sites-Implementierung erforderlich sind. Clientseitige Bibliotheken sind die Standardmethode, um CSS und JavaScript auf einer Seite in AEM einzuschließen.

Als Nächstes werden wir einige CSS-Stile für die `HelloWorld` Komponente einschließen, um die Grundlagen clientseitiger Bibliotheken zu verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Im Folgenden finden Sie die Schritte auf hoher Ebene, die im obigen Video ausgeführt werden.

1. darunter `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` erstellen Sie einen neuen Knoten mit dem Namen `clientlibs` eines Knotentyps `cq:ClientLibraryFolder`.
1. Erstellen Sie einen Ordner und eine Dateistruktur wie die folgende `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Füllen Sie `helloworld/clientlibs/css/helloworld.css` wie folgt:

   ```css
   .cmp-helloworld h1 {
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
   console.log("hello world!");
   ```

1. Füllen Sie `helloworld/clientlibs/js.txt` wie folgt:

   ```plain
   #base=js
   helloworld.js
   ```

1. Aktualisieren Sie die `clientlibs` Knoteneigenschaften, um die folgenden beiden Eigenschaften einzuschließen:

   | Name | Typ | Wert |
   |------|------|-------|
   | categories | Zeichenfolge | wknd.base |
   | allowProxy | Boolesch | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Stellen Sie die Änderungen mithilfe des Eclipse Developer-Plugins oder mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen Instanz von AEM bereit.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade die Grundlagen der Komponentenentwicklung in Adobe Experience Manager gelernt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit Adobe Experience Manager-Seiten und -Vorlagen im nächsten Kapitel [Seiten und Vorlagen](pages-templates.md)vertraut. Verstehen Sie, wie Kernkomponenten in das Projekt integriert werden, und lernen Sie erweiterte Richtlinienkonfigurationen bearbeitbarer Vorlagen kennen, um eine gut strukturierte Artikelseitenvorlage zu erstellen.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder Überprüfung und Bereitstellung des Codes lokal auf der Git-Klammer `component-basics/solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) -Repository.
1. Sehen Sie sich die `component-basics/solution` Verzweigung an
