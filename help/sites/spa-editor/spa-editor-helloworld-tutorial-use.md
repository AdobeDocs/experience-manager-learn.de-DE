---
title: Entwickeln mit dem AEM-SPA-Editor - Tutorial "Hello World"
description: AEM SPA Editor unterstützt die kontextbezogene Bearbeitung von Einzelseiten-Apps oder -SPA. Dieses Tutorial ist eine Einführung in SPA Entwicklung, die mit AEM Editor JS SDK verwendet werden SPA. Im Tutorial wird die Anwendung "We.Retail Journal"erweitert, indem eine benutzerdefinierte Komponente "Hello World"hinzugefügt wird. Benutzer können das Tutorial mit React- oder Angular-Frameworks abschließen.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3170'
ht-degree: 3%

---


# Entwickeln mit dem AEM-SPA-Editor - Tutorial &quot;Hello World&quot; {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Dieses Tutorial ist **veraltet**. Es wird empfohlen, Folgendes zu tun: [Erste Schritte mit dem AEM-SPA-Editor und Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) oder [Erste Schritte mit dem AEM-SPA-Editor und React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor unterstützt die kontextbezogene Bearbeitung von Einzelseiten-Apps oder -SPA. Dieses Tutorial ist eine Einführung in SPA Entwicklung, die mit AEM Editor JS SDK verwendet werden SPA. Im Tutorial wird die Anwendung &quot;We.Retail Journal&quot;erweitert, indem eine benutzerdefinierte Komponente &quot;Hello World&quot;hinzugefügt wird. Benutzer können das Tutorial mit React- oder Angular-Frameworks abschließen.

>[!NOTE]
>
> Für die Funktion &quot;Single Page Application (SPA) Editor&quot;ist AEM Service Pack 2 (oder höher) 6.4 erforderlich.
>
> Der SPA Editor ist die empfohlene Lösung für Projekte, die SPA Framework-basiertes Client-seitiges Rendering erfordern (z. B. React oder Angular).

## Vorausgesetztes Lesen {#prereq}

In diesem Tutorial werden die Schritte hervorgehoben, die zum Zuordnen einer SPA Komponente zu einer AEM Komponente erforderlich sind, um die kontextbezogene Bearbeitung zu ermöglichen. Anwender, die mit diesem Tutorial beginnen, sollten sich mit grundlegenden Entwicklungskonzepten in Adobe Experience Manager, AEM sowie mit der Entwicklung von React von Angular-Frameworks vertraut machen. Das Tutorial umfasst sowohl Back-End- als auch Front-End-Entwicklungsaufgaben.

Es wird empfohlen, die folgenden Ressourcen zu überprüfen, bevor Sie mit diesem Tutorial beginnen:

* [SPA Editor-Funktionsvideo](spa-editor-framework-feature-video-use.md)  - Eine Videoübersicht des SPA-Editors und der We.Retail Journal-App.
* [React.js-Tutorial](https://reactjs.org/tutorial/tutorial.html)  - Eine Einführung in die Entwicklung mit dem React-Framework.
* [Angular-Tutorial](https://angular.io/tutorial)  - Eine Einführung in die Entwicklung mit Angular

## Lokale Entwicklungsumgebung {#local-dev}

Dieses Tutorial wurde für Folgendes entwickelt:

[Adobe Experience Manager 6.5](https://helpx.adobe.com/de/experience-manager/6-5/release-notes.html) oder  [Adobe Experience Manager 6.4](https://helpx.adobe.com/de/experience-manager/6-4/sites/deploying/using/technical-requirements.html) +  [Service Pack 5](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html)

In diesem Tutorial sollten die folgenden Technologien und Tools installiert werden:

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) und npm 5.6.0+ (npm wird mit node.js installiert)

Überprüfen Sie die Installation der oben genannten Tools, indem Sie ein neues Terminal öffnen und Folgendes ausführen:

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## Übersicht {#overview}

Das grundlegende Konzept besteht darin, eine SPA Komponente einer AEM Komponente zuzuordnen. AEM Komponenten, die serverseitig ausgeführt werden, exportieren Inhalte in Form von JSON. Der JSON-Inhalt wird vom SPA verwendet, der clientseitig im Browser ausgeführt wird. Es wird eine 1:1-Zuordnung zwischen SPA Komponenten und einer AEM Komponente erstellt.

![SPA Komponentenzuordnung](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Beliebte Frameworks [React JS](https://reactjs.org/) und [Angular](https://angular.io/) werden standardmäßig unterstützt. Benutzer können dieses Tutorial entweder in Angular oder in React absolvieren, mit welchem Framework sie am besten vertraut sind.

## Projekt-Setup {#project-setup}

SPA Entwicklung hat einen Fuß in AEM Entwicklung und den anderen draußen. Das Ziel besteht darin, SPA Entwicklung unabhängig und (meist) AEM agnostisch zu ermöglichen.

* SPA Projekte können während der Frontend-Entwicklung unabhängig vom AEM-Projekt arbeiten.
* Frontend-Build-Tools und -Technologien wie Webpack, NPM, [!DNL Grunt] und [!DNL Gulp]werden weiterhin verwendet.
* Um für AEM zu erstellen, wird das SPA-Projekt kompiliert und automatisch in das AEM Projekt aufgenommen.
* Standardpakete AEM , die zur Bereitstellung der SPA in AEM verwendet werden.

![Übersicht über Artefakte und Bereitstellung](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA Entwicklung hat einen Fuß in AEM Entwicklung, und der andere draußen - damit SPA Entwicklung unabhängig und (meist) AEM erfolgt.*

Ziel dieses Tutorials ist es, die We.Retail Journal-App um eine neue Komponente zu erweitern. Laden Sie zunächst den Quellcode für die App &quot;We.Retail Journal&quot;herunter und stellen Sie ihn auf einem lokalen AEM bereit.

1. **** Laden Sie den neuesten  [We.Retail Journal-Code von GitHub herunter](https://github.com/adobe/aem-sample-we-retail-journal).

   Oder klonen Sie das Repository über die Befehlszeile:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >Das Tutorial funktioniert mit der Verzweigung **Übergeordnet** mit **1.2.1-SNAPSHOT** des Projekts.

1. Die folgende Struktur sollte sichtbar sein:

   ![Ordnerstruktur des Projekts](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Das Projekt enthält die folgenden Maven-Module:

   * `all`: Bettet das gesamte Projekt in ein einzelnes Paket ein und installiert es.
   * `bundles`: Enthält zwei OSGi-Bundles: und Core, die Java-Code  [!DNL Sling Models] und anderen Java-Code enthalten.
   * `ui.apps`: enthält die /apps-Teile des Projekts, d. h. JS- und CSS-Clientlibs, Komponenten, Runmode-spezifische Konfigurationen.
   * `ui.content`: enthält Strukturinhalte und Konfigurationen (`/content`,  `/conf`)
   * `react-app`: We.Retail Journal React-Anwendung. Dies ist sowohl ein Maven-Modul als auch ein Webpack-Projekt.
   * `angular-app`: Angular-Anwendung &quot;We.Retail Journal&quot;. Dies ist sowohl ein [!DNL Maven]-Modul als auch ein Webpack-Projekt.

1. Öffnen Sie ein neues Terminal-Fenster und führen Sie den folgenden Befehl aus, um die gesamte App in einer lokalen AEM-Instanz zu erstellen und bereitzustellen, die unter [http://localhost:4502](http://localhost:4502) ausgeführt wird.

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > In diesem Projekt lautet das Maven-Profil zum Erstellen und Verpacken des gesamten Projekts `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > Wenn beim Build ein Fehler auftritt, [stellen Sie sicher, dass die Maven-Datei settings.xml das Maven-Artefakt-Repository](https://helpx.adobe.com/de/experience-manager/kb/SetUpTheAdobeMavenRepository.html) der Adobe enthält.

1. Gehen Sie zu:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   Die We.Retail Journal-App sollte im AEM Sites-Editor angezeigt werden.

1. Wählen Sie im Modus [!UICONTROL Bearbeiten] eine Komponente aus, die Sie bearbeiten möchten, und aktualisieren Sie den Inhalt.

   ![Bearbeiten einer Komponente](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Wählen Sie das Symbol [!UICONTROL Seiteneigenschaften] aus, um die [!UICONTROL Seiteneigenschaften] zu öffnen. Wählen Sie [!UICONTROL Vorlage bearbeiten] aus, um die Vorlage der Seite zu öffnen.

   ![Menü &quot;Seiteneigenschaften&quot;](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. In der neuesten Version des SPA-Editors können [bearbeitbare Vorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) auf dieselbe Weise wie bei herkömmlichen Sites-Implementierungen verwendet werden. Diese wird später mit unserer benutzerdefinierten Komponente erneut besucht.

   >[!NOTE]
   >
   > Nur AEM 6.5 und AEM 6.4 + **Service Pack 5** unterstützen bearbeitbare Vorlagen.

## Entwicklungsübersicht {#development-overview}

![Überblicksentwicklung](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA Entwicklungsdurchläufe erfolgen unabhängig von AEM. Wenn die SPA bereit für die Bereitstellung in AEM ist, werden die folgenden allgemeinen Schritte ausgeführt (wie oben dargestellt).

1. Der AEM Projekterstellung wird aufgerufen, wodurch wiederum ein Build des SPA-Projekts Trigger wird. Das We.Retail Journal verwendet das [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. Der [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) des SPA-Projekts bettet die kompilierte SPA als AEM Client-Bibliothek in das AEM Projekt ein.
1. Das AEM-Projekt generiert ein AEM-Paket, einschließlich des kompilierten SPA, sowie weiteren unterstützenden AEM-Code.

## AEM Komponente erstellen {#aem-component}

**Persona: AEM Entwickler**

Zuerst wird eine AEM Komponente erstellt. Die AEM-Komponente ist für das Rendern der JSON-Eigenschaften verantwortlich, die von der React-Komponente gelesen werden. Die AEM Komponente ist auch für die Bereitstellung eines Dialogfelds für alle bearbeitbaren Eigenschaften der Komponente verantwortlich.

Importieren Sie mithilfe von [!DNL Eclipse] oder anderen [!DNL IDE] das Projekt &quot;We.Retail Journal Maven&quot;.

1. Aktualisieren Sie den Reaktor **pom.xml**, um das Plug-in [!DNL Apache Rat] zu entfernen. Dieses Plug-in überprüft jede Datei, um sicherzustellen, dass ein Lizenzheader vorhanden ist. Für unsere Zwecke müssen wir uns nicht um diese Funktionalität kümmern.

   Entfernen Sie in **aem-sample-we-retail-journal/pom.xml** **apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. Erstellen Sie im Modul **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) einen neuen Knoten unter `ui.apps/jcr_root/apps/we-retail-journal/components` mit dem Namen **helloworld** des Typs **cq:Component**.
1. Fügen Sie die folgenden Eigenschaften zur **helloworld**-Komponente hinzu, dargestellt in XML (`/helloworld/.content.xml`):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World-Komponente](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > Um die Funktion &quot;Bearbeitbare Vorlagen&quot;zu veranschaulichen, haben wir absichtlich `componentGroup="Custom Components"` festgelegt. In einem realen Projekt ist es am besten, die Anzahl der Komponentengruppen zu minimieren, sodass eine bessere Gruppe &quot;[!DNL We.Retail Journal]&quot;wäre, um sie mit den anderen Inhaltskomponenten abzugleichen.
   >
   > Nur AEM 6.5 und AEM 6.4 + **Service Pack 5** unterstützen bearbeitbare Vorlagen.

1. Als Nächstes wird ein Dialogfeld erstellt, in dem eine benutzerdefinierte Nachricht für die Komponente **Hello World** konfiguriert werden kann. Fügen Sie unter `/apps/we-retail-journal/components/helloworld` einen Knotennamen **cq:dialog** von **nt:unstructured** hinzu.
1. Das **cq:dialog** zeigt ein einzelnes Textfeld an, das Text in einer Eigenschaft namens **[!DNL message]** beibehält. Fügen Sie unter dem neu erstellten **cq:dialog** die folgenden Knoten und Eigenschaften hinzu, die in XML unten dargestellt werden (`helloworld/_cq_dialog/.content.xml`):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![Dateistruktur](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   Die obige XML-Knotendefinition erstellt ein Dialogfeld mit einem einzelnen Textfeld, in das ein Benutzer eine &quot;Meldung&quot;eingeben kann. Beachten Sie die Eigenschaft `name="./message"` im Knoten `<message />` . Dies ist der Name der Eigenschaft, die in AEM im JCR gespeichert wird.

1. Als Nächstes wird ein leeres Dialogfeld für Richtlinien erstellt (`cq:design_dialog`). Das Dialogfeld &quot;Richtlinie&quot;ist erforderlich, um die Komponente im Vorlageneditor anzuzeigen. Für diesen einfachen Anwendungsfall ist es ein leeres Dialogfeld.

   Fügen Sie unter `/apps/we-retail-journal/components/helloworld` einen Knotennamen `cq:design_dialog` von `nt:unstructured` hinzu.

   Die Konfiguration wird in XML unten dargestellt (`helloworld/_cq_design_dialog/.content.xml`).

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. Stellen Sie die Codebasis in der Befehlszeile AEM bereit:

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   Überprüfen Sie in [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) , ob die Komponente bereitgestellt wurde, indem Sie den Ordner unter `/apps/we-retail-journal/components:` überprüfen.

   ![Bereitgestellte Komponentenstruktur in CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Sling-Modell erstellen {#create-sling-model}

**Persona: AEM Entwickler**

Als Nächstes wird ein [!DNL Sling Model] erstellt, um die [!DNL Hello World]-Komponente zu sichern. In einem herkömmlichen WCM-Anwendungsfall implementiert das [!DNL Sling Model] eine Geschäftslogik und ein serverseitiges Rendering-Skript (HTL) führt einen Aufruf an das [!DNL Sling Model] durch. Dadurch bleibt das Rendering-Skript relativ einfach.

[!DNL Sling Models] werden auch im SPA-Anwendungsfall verwendet, um serverseitige Geschäftslogik zu implementieren. Der Unterschied besteht darin, dass im Anwendungsfall [!DNL SPA] die [!DNL Sling Models] ihre Methoden als serialisierte JSON verfügbar macht.

>[!NOTE]
>
>Als Best Practice sollten Entwickler nach Möglichkeit [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) verwenden. Neben anderen Funktionen bieten Kernkomponenten [!DNL Sling Models] eine JSON-Ausgabe, die &quot;SPA-bereit&quot;ist, sodass sich Entwickler mehr auf die Frontend-Präsentation konzentrieren können.

1. Öffnen Sie im Editor Ihrer Wahl das Projekt **we-retail-journal-commons** ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. Im Paket `com.adobe.cq.sample.spa.commons.impl.models`:
   * Erstellen Sie eine neue Klasse mit dem Namen `HelloWorld`.
   * Hinzufügen einer Implementierungsschnittstelle für `com.adobe.cq.export.json.ComponentExporter.`

   ![Neuer Java-Klassenassistent](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   Die `ComponentExporter`-Schnittstelle muss implementiert werden, damit [!DNL Sling Model] mit AEM Content Services kompatibel ist.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. Fügen Sie eine statische Variable mit dem Namen `RESOURCE_TYPE` hinzu, um den Ressourcentyp der Komponente [!DNL HelloWorld] zu identifizieren:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Fügen Sie die OSGi-Anmerkungen für `@Model` und `@Exporter` hinzu. Die `@Model`-Anmerkung registriert die Klasse als [!DNL Sling Model]. Die Anmerkung `@Exporter` stellt die Methoden als serialisierte JSON-Datei mit dem Framework [!DNL Jackson Exporter] bereit.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. Implementieren Sie die Methode `getDisplayMessage()`, um die JCR-Eigenschaft `message` zurückzugeben. Verwenden Sie die [!DNL Sling Model]-Anmerkung von `@ValueMapValue`, um die unter der Komponente gespeicherte Eigenschaft `message` einfach abzurufen. Die Anmerkung `@Optional` ist wichtig, da `message` beim ersten Hinzufügen der Komponente zur Seite nicht aufgefüllt wird.

   Im Rahmen der Geschäftslogik wird der Nachricht eine Zeichenfolge &quot;**Hallo**&quot;vorangestellt.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > Der Methodenname `getDisplayMessage` ist wichtig. Wenn [!DNL Sling Model] mit [!DNL Jackson Exporter] serialisiert wird, wird dies als JSON-Eigenschaft bereitgestellt: `displayMessage`. Die [!DNL Jackson Exporter] serialisiert und stellt alle `getter`-Methoden bereit, die keinen Parameter verwenden (es sei denn, sie sind explizit zum Ignorieren markiert). Später in der React-/Angular-App werden wir diesen Eigenschaftswert lesen und ihn als Teil der Anwendung anzeigen.

   Die Methode `getExportedType` ist ebenfalls wichtig. Der Wert der Komponente `resourceType` wird verwendet, um die JSON-Daten der Frontend-Komponente (Angular/React) zuzuordnen. Wir werden dies im nächsten Abschnitt untersuchen.

1. Implementieren Sie die Methode `getExportedType()` , um den Ressourcentyp der Komponente `HelloWorld` zurückzugeben.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   Den vollständigen Code für [**HelloWorld.java** finden Sie hier.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Stellen Sie den Code AEM mithilfe von Apache Maven bereit:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Überprüfen Sie die Bereitstellung und Registrierung von [!DNL Sling Model], indem Sie in der OSGi-Konsole zu [[!UICONTROL Status] > [!UICONTROL Sling-Modelle]](http://localhost:4502/system/console/status-slingmodels) navigieren.

   Sie sollten sehen, dass das Sling-Modell `HelloWorld` an den Sling-Ressourcentyp `we-retail-journal/components/helloworld` gebunden ist und als [!DNL Sling Model Exporter Servlet] registriert ist:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Erstellen einer React-Komponente {#react-component}

**Persona: Frontend-Entwickler**

Als Nächstes wird die React-Komponente erstellt. Öffnen Sie das Modul **react-app** ( `<src>/aem-sample-we-retail-journal/react-app`) mit dem Editor Ihrer Wahl.

>[!NOTE]
>
> Sie können diesen Abschnitt überspringen, wenn Sie nur an der [Angular-Entwicklung](#angular-component) interessiert sind.

1. Navigieren Sie im Ordner `react-app` zum Ordner src . Erweitern Sie den Komponentenordner, um die vorhandenen React-Komponentendateien anzuzeigen.

   ![Struktur der React-Komponentendatei](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Fügen Sie eine neue Datei unter dem Komponentenordner `HelloWorld.js` hinzu.
1. Öffnen Sie `HelloWorld.js`. Fügen Sie eine Importanweisung hinzu, um die React-Komponentenbibliothek zu importieren. Fügen Sie eine zweite Importanweisung hinzu, um den von Adobe bereitgestellten `MapTo`-Helfer zu importieren. Der `MapTo`-Helfer stellt eine Zuordnung der React-Komponente zur JSON der AEM-Komponente bereit.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Erstellen Sie unter den Importen eine neue Klasse mit dem Namen `HelloWorld` , die die React `Component` -Schnittstelle erweitert. Fügen Sie die erforderliche `render()`-Methode zur `HelloWorld`-Klasse hinzu.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. Der `MapTo`-Helfer enthält automatisch ein Objekt mit dem Namen `cqModel` als Teil der Props der React-Komponente. Das `cqModel` enthält alle Eigenschaften, die von [!DNL Sling Model] verfügbar gemacht werden.

   Denken Sie daran, dass das zuvor erstellte [!DNL Sling Model] eine Methode `getDisplayMessage()` enthält. `getDisplayMessage()` wird als JSON-Schlüssel mit dem Namen  `displayMessage` bei der Ausgabe übersetzt.

   Implementieren Sie die `render()`-Methode, um ein `h1`-Tag auszugeben, das den Wert von `displayMessage` enthält. [JSX](https://reactjs.org/docs/introducing-jsx.html), eine Syntaxerweiterung für JavaScript, wird verwendet, um das endgültige Markup der Komponente zurückzugeben.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. Implementieren Sie eine Konfigurationsmethode zum Bearbeiten. Diese Methode wird über den `MapTo` -Helfer übergeben und stellt dem AEM-Editor Informationen bereit, um einen Platzhalter anzuzeigen, falls die Komponente leer ist. Dies tritt auf, wenn die Komponente der SPA hinzugefügt, aber noch nicht erstellt wurde. Fügen Sie unter der Klasse `HelloWorld` Folgendes hinzu:

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. Rufen Sie am Ende der Datei den Helper `MapTo` auf und übergeben Sie die Klasse `HelloWorld` und die Klasse `HelloWorldEditConfig`. Dadurch wird die React-Komponente basierend auf dem Ressourcentyp der AEM Komponente der AEM Komponente zugeordnet: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   Den vollständigen Code für [**HelloWorld.js** finden Sie hier.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Öffnen Sie die Datei `ImportComponents.js`. Sie finden sie unter `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Fügen Sie eine Zeile hinzu, um die `HelloWorld.js` mit den anderen Komponenten im kompilierten JavaScript-Bundle anzufordern:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. Erstellen Sie im Ordner `components` eine neue Datei mit dem Namen `HelloWorld.css` als Geschwister von `HelloWorld.js.` Füllen Sie die Datei mit dem folgenden Element, um einige grundlegende Stile für die Komponente `HelloWorld` zu erstellen:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Öffnen Sie `HelloWorld.js` erneut und aktualisieren Sie unter den Importanweisungen, um `HelloWorld.css` zu benötigen:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Stellen Sie den Code AEM mithilfe von Apache Maven bereit:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Öffnen Sie in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Suchen Sie in app.js schnell nach &quot;HelloWorld&quot;, um sicherzustellen, dass die React-Komponente in der kompilierten App enthalten ist.

   >[!NOTE]
   >
   > **app.** js ist die gebündelte React-App. Der Code ist nicht mehr für Menschen lesbar. Der Befehl `npm run build` hat einen optimierten Build ausgelöst, der kompiliertes JavaScript ausgibt, das von modernen Browsern interpretiert werden kann.


## Angular-Komponente erstellen {#angular-component}

**Persona: Frontend-Entwickler**

>[!NOTE]
>
> Sie können diesen Abschnitt überspringen, wenn Sie nur an der React-Entwicklung interessiert sind.

Als Nächstes wird die Angular-Komponente erstellt. Öffnen Sie das Modul **angular-app** (`<src>/aem-sample-we-retail-journal/angular-app`) mit dem Editor Ihrer Wahl.

1. Navigieren Sie im Ordner `angular-app` zum Ordner `src` . Erweitern Sie den Komponentenordner, um die vorhandenen Angular-Komponentendateien anzuzeigen.

   ![Angular-Dateistruktur](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Fügen Sie unter dem Komponentenordner `helloworld` einen neuen Ordner hinzu. Fügen Sie unter dem Ordner `helloworld` neue Dateien mit dem Namen `helloworld.component.css, helloworld.component.html, helloworld.component.ts` hinzu.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. Öffnen Sie `helloworld.component.ts`. Fügen Sie eine Importanweisung hinzu, um die Klassen Angular `Component` und `Input` zu importieren. Erstellen Sie eine neue Komponente mit dem Verweis auf `styleUrls` und `templateUrl` auf `helloworld.component.css` und `helloworld.component.html`. Exportieren Sie schließlich die Klasse `HelloWorldComponent` mit der erwarteten Eingabe von `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > Wenn Sie sich an die zuvor erstellte [!DNL Sling Model] erinnern, gab es eine Methode **getDisplayMessage()**. Die serialisierte JSON dieser Methode lautet **displayMessage**, die wir jetzt in der Angular-App lesen.

1. Öffnen Sie `helloworld.component.html` , um ein `h1` -Tag einzufügen, das die `displayMessage` -Eigenschaft druckt:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Aktualisieren Sie `helloworld.component.css` , um einige grundlegende Stile für die Komponente einzuschließen.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Aktualisieren Sie `helloworld.component.spec.ts` mit dem folgenden Testbett:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. Nächstes Update `src/components/mapping.ts` , um `HelloWorldComponent` einzuschließen. Fügen Sie einen `HelloWorldEditConfig` hinzu, der den Platzhalter im AEM-Editor markiert, bevor die Komponente konfiguriert wurde. Fügen Sie schließlich eine Zeile hinzu, um die AEM Komponente der Angular-Komponente mit dem Helfer `MapTo` zuzuordnen.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   Den vollständigen Code für [**mapping.ts** finden Sie hier.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Aktualisieren Sie `src/app.module.ts` , um das **NgModule** zu aktualisieren. Fügen Sie **`HelloWorldComponent`** als **Deklaration** hinzu, die zum **AppModule** gehört. Fügen Sie `HelloWorldComponent` auch als **entryComponent** hinzu, damit sie kompiliert und dynamisch in die App eingefügt wird, während das JSON-Modell verarbeitet wird.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   Den vollständigen Code für [**app.module.ts** finden Sie hier.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Stellen Sie den Code mithilfe von Maven für AEM bereit:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Öffnen Sie in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Führen Sie eine Schnellsuche nach **HelloWorld** in `main.js` durch, um sicherzustellen, dass die Angular-Komponente eingeschlossen wurde.

   >[!NOTE]
   >
   > **main.** js ist die gebündelte Angular-App. Der Code ist nicht mehr für Menschen lesbar. Der Befehl npm run build hat einen optimierten Build ausgelöst, der kompiliertes JavaScript ausgibt, das von modernen Browsern interpretiert werden kann.

## Aktualisieren der Vorlage {#template-update}

1. Navigieren Sie zur bearbeitbaren Vorlage für die React- und/oder Angular-Versionen:

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Wählen Sie den [!UICONTROL Layout-Container] aus und klicken Sie auf das Symbol [!UICONTROL Richtlinie] , um die Richtlinie zu öffnen:

   ![Layout-Richtlinie auswählen](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Führen Sie unter **[!UICONTROL Properties]** > **[!UICONTROL Zulässige Komponenten]** eine Suche nach **[!DNL Custom Components]** durch. Sie sollten die Komponente **[!DNL Hello World]** sehen und sie auswählen. Speichern Sie Ihre Änderungen, indem Sie auf das Kontrollkästchen oben rechts klicken.

   ![Richtlinienkonfiguration für Layout-Container](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Nach dem Speichern sollte die Komponente **[!DNL HelloWorld]** als zulässige Komponente im [!UICONTROL Layout-Container] angezeigt werden.

   ![Aktualisierte zulässige Komponenten](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Nur AEM 6.5 und AEM 6.4.5 unterstützen die Funktion &quot;Bearbeitbare Vorlage&quot;des SPA-Editors. Bei Verwendung von AEM 6.4 müssen Sie die Richtlinie für zulässige Komponenten manuell über die CRXDE Lite konfigurieren: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` oder `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite mit den aktualisierten Richtlinienkonfigurationen für [!UICONTROL Zulässige Komponenten] im [!UICONTROL Layout-Container]:

   ![CRXDE Lite mit den aktualisierten Richtlinienkonfigurationen für zulässige Komponenten im Layout-Container](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Alles zusammenbringen {#putting-together}

1. Navigieren Sie zu den Angular- oder React-Seiten:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Suchen Sie die Komponente **[!DNL Hello World]** und ziehen Sie die Komponente **[!DNL Hello World]** auf die Seite.

   ![Hallo Welt ziehen und ablegen](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   Der Platzhalter sollte angezeigt werden.

   ![Hallo, Platzhalter](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Wählen Sie die Komponente aus und fügen Sie eine Meldung im Dialogfeld hinzu, z. B. &quot;Welt&quot;oder &quot;Ihr Name&quot;. Speichern Sie die Änderungen.

   ![gerenderte Komponente](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Beachten Sie, dass der Nachricht immer die Zeichenfolge &quot;Hello &quot;vorangestellt wird. Dies ist das Ergebnis der Logik in `HelloWorld.java` [!DNL Sling Model].

## Nächste Schritte {#next-steps}

[Abgeschlossene Lösung für die Komponente &quot;HelloWorld&quot;](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Vollständiger Quellcode für [[!DNL We.Retail Journal] auf GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Sehen Sie sich ein detaillierteres Tutorial zur Entwicklung von React mit [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/de/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html) an.

## Fehlerbehebung {#troubleshooting}

### Projekt kann nicht in Eclipse erstellt werden {#unable-to-build-project-in-eclipse}

**Fehler:** Fehler beim Import des  [!DNL We.Retail Journal] Projekts in Eclipse bei nicht erkannten Zielausführungen:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![Eclipse-Fehlerassistent](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Auflösung**: Klicken Sie auf Beenden , um diese später zu beheben. Dies sollte den Abschluss des Tutorials nicht verhindern.

**Fehler**: Das React-Modul  `react-app`wird während eines Maven-Builds nicht erfolgreich erstellt.

**Lösung:** Versuchen Sie, den  `node_modules` Ordner unter der  **React-App zu löschen**. Führen Sie den Apache Maven-Befehl `mvn  clean install -PautoInstallSinglePackage` im Stammverzeichnis des Projekts erneut aus.

### Nicht zufrieden stellende Abhängigkeiten in AEM {#unsatisfied-dependencies-in-aem}

![Fehler bei Package Manager-Abhängigkeit](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Wenn eine AEM Abhängigkeit nicht erfüllt ist, entweder im **[!UICONTROL AEM Package Manager]** oder in der **[!UICONTROL AEM Web Console]** (Felix Console), deutet dies darauf hin, dass SPA Editor-Funktion nicht verfügbar ist.

### Komponente wird nicht angezeigt

**Fehler**: Selbst nach einer erfolgreichen Bereitstellung und der Überprüfung, ob die kompilierten Versionen von React-/Angular-Apps über die aktualisierte  `helloworld` Komponente verfügen, wird meine Komponente nicht angezeigt, wenn ich sie auf die Seite ziehe. Ich kann die Komponente in der AEM Benutzeroberfläche sehen.

**Auflösung**: Löschen Sie den Verlauf/Cache Ihres Browsers und/oder öffnen Sie einen neuen Browser oder verwenden Sie den Inkognito-Modus. Wenn dies nicht funktioniert, machen Sie den Client-Bibliotheks-Cache auf der lokalen AEM ungültig. AEM versucht, große Client-Bibliotheken zwischenzuspeichern, um effizient zu sein. Manchmal ist eine manuelle Invalidierung des Caches erforderlich, um Probleme zu beheben, bei denen veralteter Code zwischengespeichert wird.

Navigieren Sie zu: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) und klicken Sie auf &quot;Cache invalidieren&quot;. Kehren Sie zu Ihrer React-/Angular-Seite zurück und aktualisieren Sie die Seite.

![Client-Bibliothek neu erstellen](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
