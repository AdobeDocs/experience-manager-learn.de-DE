---
title: Entwickeln mit dem AEM-SPA-Editor - Tutorial "Hello World"
description: AEM SPA Editor unterstützt die kontextbezogene Bearbeitung von Einzelseiten-Apps oder -SPA. Dieses Tutorial ist eine Einführung in SPA Entwicklung, die mit AEM Editor JS SDK verwendet werden SPA. Im Tutorial wird die Anwendung "We.Retail Journal"erweitert, indem eine benutzerdefinierte Komponente "Hello World"hinzugefügt wird. Benutzer können das Tutorial mit React- oder Angular-Frameworks abschließen.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3144'
ht-degree: 2%

---

# Entwickeln mit dem AEM-SPA-Editor - Tutorial &quot;Hello World&quot; {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Dieses Tutorial **veraltet**. Es wird empfohlen, Folgendes zu tun: [Erste Schritte mit dem AEM SPA Editor und Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) oder [Erste Schritte mit dem AEM SPA Editor und React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor unterstützt die kontextbezogene Bearbeitung von Einzelseiten-Apps oder -SPA. Dieses Tutorial ist eine Einführung in SPA Entwicklung, die mit AEM Editor JS SDK verwendet werden SPA. Im Tutorial wird die Anwendung &quot;We.Retail Journal&quot;erweitert, indem eine benutzerdefinierte Komponente &quot;Hello World&quot;hinzugefügt wird. Benutzer können das Tutorial mit React- oder Angular-Frameworks abschließen.

>[!NOTE]
>
> Für die Funktion &quot;Single Page Application (SPA) Editor&quot;ist AEM Service Pack 2 (oder höher) 6.4 erforderlich.
>
> Der SPA Editor ist die empfohlene Lösung für Projekte, die SPA Framework-basiertes Client-seitiges Rendering erfordern (z. B. React oder Angular).

## Vorausgesetztes Lesen {#prereq}

In diesem Tutorial werden die Schritte hervorgehoben, die zum Zuordnen einer SPA Komponente zu einer AEM Komponente erforderlich sind, um die kontextbezogene Bearbeitung zu ermöglichen. Anwender, die mit diesem Tutorial beginnen, sollten sich mit grundlegenden Entwicklungskonzepten in Adobe Experience Manager, AEM sowie mit der Entwicklung von React von Angular-Frameworks vertraut machen. Das Tutorial umfasst sowohl Back-End- als auch Front-End-Entwicklungsaufgaben.

Es wird empfohlen, die folgenden Ressourcen zu überprüfen, bevor Sie mit diesem Tutorial beginnen:

* [SPA Editor-Funktionsvideo](spa-editor-framework-feature-video-use.md) - Eine Videoübersicht über den SPA Editor und die We.Retail Journal-App.
* [Tutorial zu React.js](https://reactjs.org/tutorial/tutorial.html) - Einführung in die Entwicklung mit dem Rahmen für React.
* [Angular-Tutorial](https://angular.io/tutorial) - Einführung in die Entwicklung mit Angular

## Lokale Entwicklungsumgebung {#local-dev}

Dieses Tutorial wurde für Folgendes entwickelt:

[Adobe Experience Manager 6.5](https://helpx.adobe.com/de/experience-manager/6-5/release-notes.html) oder [Adobe Experience Manager 6.4](https://helpx.adobe.com/de/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html)

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
* Frontend-Build-Tools und -Technologien wie Webpack, NPM, [!DNL Grunt] und [!DNL Gulp]weiterhin verwendet werden.
* Um für AEM zu erstellen, wird das SPA-Projekt kompiliert und automatisch in das AEM Projekt aufgenommen.
* Standardpakete AEM , die zur Bereitstellung der SPA in AEM verwendet werden.

![Übersicht über Artefakte und Bereitstellung](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA Entwicklung hat einen Fuß in AEM Entwicklung, und der andere draußen - damit SPA Entwicklung unabhängig und (meist) AEM erfolgt.*

Ziel dieses Tutorials ist es, die We.Retail Journal-App um eine neue Komponente zu erweitern. Laden Sie zunächst den Quellcode für die App &quot;We.Retail Journal&quot;herunter und stellen Sie ihn auf einem lokalen AEM bereit.

1. **Download** der neuesten [We.Retail Journal-Code von GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   Oder klonen Sie das Repository über die Befehlszeile:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >Das Tutorial funktioniert mit dem **Übergeordnet** Zweig mit **1.2.1-SNAPSHOT** -Version des Projekts.

1. Die folgende Struktur sollte sichtbar sein:

   ![Ordnerstruktur des Projekts](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Das Projekt enthält die folgenden Maven-Module:

   * `all`: Bettet das gesamte Projekt in ein einzelnes Paket ein und installiert es.
   * `bundles`: Enthält zwei OSGi-Bundles: Kommas und Core, die [!DNL Sling Models] und anderen Java-Code.
   * `ui.apps`: enthält die /apps-Teile des Projekts, d. h. JS- und CSS-Clientlibs, Komponenten, Runmode-spezifische Konfigurationen.
   * `ui.content`: enthält strukturellen Inhalt und Konfigurationen (`/content`, `/conf`)
   * `react-app`: We.Retail Journal React-Anwendung. Dies ist sowohl ein Maven-Modul als auch ein Webpack-Projekt.
   * `angular-app`: Angular-Anwendung &quot;We.Retail Journal&quot;. Dies ist sowohl ein [!DNL Maven] und ein Webpack-Projekt.

1. Öffnen Sie ein neues Terminal-Fenster und führen Sie den folgenden Befehl aus, um die gesamte App zu erstellen und in einer lokalen AEM-Instanz bereitzustellen, die auf ausgeführt wird. [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > In diesem Projekt ist das Maven-Profil zum Erstellen und Verpacken des gesamten Projekts `autoInstallSinglePackage`

1. Gehen Sie zu:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   Die We.Retail Journal-App sollte im AEM Sites-Editor angezeigt werden.

1. In [!UICONTROL Bearbeiten] -Modus wählen Sie eine Komponente aus, die Sie bearbeiten möchten, und aktualisieren Sie den Inhalt.

   ![Bearbeiten einer Komponente](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Wählen Sie die [!UICONTROL Seiteneigenschaften] Symbol zum Öffnen [!UICONTROL Seiteneigenschaften]. Auswählen [!UICONTROL Vorlage bearbeiten] , um die Vorlage der Seite zu öffnen.

   ![Menü &quot;Seiteneigenschaften&quot;](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. In der neuesten Version des SPA-Editors [Bearbeitbare Vorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) kann auf dieselbe Weise wie bei herkömmlichen Sites-Implementierungen verwendet werden. Diese wird später mit unserer benutzerdefinierten Komponente erneut besucht.

   >[!NOTE]
   >
   > Nur AEM 6.5 und AEM 6.4 + **Service Pack 5** unterstützt bearbeitbare Vorlagen.

## Entwicklungsübersicht {#development-overview}

![Überblicksentwicklung](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA Entwicklungsdurchläufe erfolgen unabhängig von AEM. Wenn die SPA bereit für die Bereitstellung in AEM ist, werden die folgenden allgemeinen Schritte ausgeführt (wie oben dargestellt).

1. Der AEM Projekterstellung wird aufgerufen, wodurch wiederum ein Build des SPA-Projekts Trigger wird. Das We.Retail-Journal verwendet die [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. Das SPA [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) bettet die kompilierte SPA als AEM Client-Bibliothek in das AEM Projekt ein.
1. Das AEM-Projekt generiert ein AEM-Paket, einschließlich des kompilierten SPA, sowie weiteren unterstützenden AEM-Code.

## AEM Komponente erstellen {#aem-component}

**Persona: AEM Entwickler**

Zuerst wird eine AEM Komponente erstellt. Die AEM-Komponente ist für das Rendern der JSON-Eigenschaften verantwortlich, die von der React-Komponente gelesen werden. Die AEM Komponente ist auch für die Bereitstellung eines Dialogfelds für alle bearbeitbaren Eigenschaften der Komponente verantwortlich.

Verwenden [!DNL Eclipse]oder anderen [!DNL IDE], importieren Sie das Projekt &quot;We.Retail Journal Maven&quot;.

1. Aktualisieren des Reaktors **pom.xml** , um [!DNL Apache Rat] Plug-in. Dieses Plug-in überprüft jede Datei, um sicherzustellen, dass ein Lizenzheader vorhanden ist. Für unsere Zwecke müssen wir uns nicht um diese Funktionalität kümmern.

   In **aem-sample-we-retail-journal/pom.xml** remove **apache-rate-plugin**:

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

1. Im **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) erstellen Sie einen neuen Knoten unter `ui.apps/jcr_root/apps/we-retail-journal/components` benannt **helloworld** des Typs **cq:Component**.
1. Fügen Sie die folgenden Eigenschaften zum **helloworld** -Komponente, dargestellt in XML (`/helloworld/.content.xml`) unten:

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
   > Zur Veranschaulichung der Funktion &quot;Bearbeitbare Vorlagen&quot;haben wir die Variable `componentGroup="Custom Components"`. In einem realen Projekt ist es am besten, die Anzahl der Komponentengruppen zu minimieren, sodass eine bessere Gruppe &quot;[!DNL We.Retail Journal]&quot;, um mit den anderen Inhaltskomponenten abzugleichen.
   >
   > Nur AEM 6.5 und AEM 6.4 + **Service Pack 5** unterstützt bearbeitbare Vorlagen.

1. Als Nächstes wird ein Dialogfeld erstellt, in dem eine benutzerdefinierte Nachricht für die **Hello World** -Komponente. darunter `/apps/we-retail-journal/components/helloworld` Knotennamen hinzufügen **cq:dialog** von **nt:unstructured**.
1. Die **cq:dialog** zeigt ein einzelnes Textfeld an, das Text in einer Eigenschaft mit dem Namen **[!DNL message]**. Unter dem neu erstellten **cq:dialog** Fügen Sie die folgenden Knoten und Eigenschaften hinzu, die in XML unten dargestellt sind (`helloworld/_cq_dialog/.content.xml`) :

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

   Die obige XML-Knotendefinition erstellt ein Dialogfeld mit einem einzelnen Textfeld, in das ein Benutzer eine &quot;Meldung&quot;eingeben kann. Notieren Sie die Eigenschaft . `name="./message"` innerhalb der `<message />` Knoten. Dies ist der Name der Eigenschaft, die in AEM im JCR gespeichert wird.

1. Als Nächstes wird ein leeres Dialogfeld für Richtlinien erstellt (`cq:design_dialog`). Das Dialogfeld &quot;Richtlinie&quot;ist erforderlich, um die Komponente im Vorlageneditor anzuzeigen. Für diesen einfachen Anwendungsfall ist es ein leeres Dialogfeld.

   darunter `/apps/we-retail-journal/components/helloworld` Knotennamen hinzufügen `cq:design_dialog` von `nt:unstructured`.

   Die Konfiguration wird in XML unten dargestellt (`helloworld/_cq_design_dialog/.content.xml`)

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

   In [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) überprüfen, ob die Komponente bereitgestellt wurde, indem der Ordner unter `/apps/we-retail-journal/components:`

   ![Bereitgestellte Komponentenstruktur in CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Sling-Modell erstellen {#create-sling-model}

**Persona: AEM Entwickler**

Weiter mit [!DNL Sling Model] wird erstellt, um die [!DNL Hello World] -Komponente. In einem herkömmlichen WCM-Anwendungsfall sollte die [!DNL Sling Model] implementiert eine Geschäftslogik, und ein serverseitiges Rendering-Skript (HTL) führt einen Aufruf an die [!DNL Sling Model]. Dadurch bleibt das Rendering-Skript relativ einfach.

[!DNL Sling Models] werden auch im SPA-Anwendungsfall verwendet, um serverseitige Geschäftslogik zu implementieren. Der Unterschied besteht darin, dass im [!DNL SPA] Anwendungsfall, [!DNL Sling Models] stellt seine Methoden als serialisierte JSON bereit.

>[!NOTE]
>
>Als Best Practice sollten Entwickler [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) nach Möglichkeit. Neben anderen Funktionen bieten Kernkomponenten [!DNL Sling Models] mit JSON-Ausgabe, die &quot;SPA-bereit&quot;ist, sodass sich Entwickler mehr auf die Front-End-Präsentation konzentrieren können.

1. Öffnen Sie im Editor Ihrer Wahl die **we-retail-journal-commons** Projekt ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. Im Paket `com.adobe.cq.sample.spa.commons.impl.models`:
   * Erstellen Sie eine neue Klasse mit dem Namen `HelloWorld`.
   * Hinzufügen einer Implementierungsschnittstelle für `com.adobe.cq.export.json.ComponentExporter.`

   ![Neuer Java-Klassenassistent](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   Die `ComponentExporter` muss implementiert werden, damit die [!DNL Sling Model] , um mit AEM Content Services kompatibel zu sein.

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

1. Hinzufügen einer statischen Variablen mit dem Namen `RESOURCE_TYPE` zur Identifizierung der [!DNL HelloWorld] Ressourcentyp der Komponente:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Hinzufügen der OSGi-Anmerkungen für `@Model` und `@Exporter`. Die `@Model` -Anmerkung registriert die Klasse als [!DNL Sling Model]. Die `@Exporter` -Anmerkung zeigt die Methoden als serialisiertes JSON mit der [!DNL Jackson Exporter] Framework.

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

1. Implementieren der -Methode `getDisplayMessage()` , um die JCR-Eigenschaft zurückzugeben `message`. Verwenden Sie die [!DNL Sling Model] Anmerkung von `@ValueMapValue` um das Abrufen der Eigenschaft zu vereinfachen `message` unter der Komponente gespeichert. Die `@Optional` -Anmerkung ist wichtig, da sie beim ersten Hinzufügen der Komponente zur Seite  `message`  nicht aufgefüllt werden.

   Als Teil der Geschäftslogik bezeichnet eine Zeichenfolge Folgendes: &quot;**Hallo**&quot;, wird der Nachricht vorangestellt.

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
   > Der Name der Methode `getDisplayMessage` ist wichtig. Wenn die [!DNL Sling Model] mit der [!DNL Jackson Exporter] wird sie als JSON-Eigenschaft bereitgestellt: `displayMessage`. Die [!DNL Jackson Exporter] serialisiert und stellt alle `getter` -Methoden verwenden, die keinen -Parameter annehmen (es sei denn, sie sind explizit zum Ignorieren markiert). Später in der React-/Angular-App werden wir diesen Eigenschaftswert lesen und ihn als Teil der Anwendung anzeigen.

   Die Methode `getExportedType` ist auch wichtig. Der -Wert der Komponente `resourceType` wird verwendet, um die JSON-Daten der Frontend-Komponente (Angular/React) zuzuordnen. Wir werden dies im nächsten Abschnitt untersuchen.

1. Implementieren der -Methode `getExportedType()` , um den Ressourcentyp der `HelloWorld` -Komponente.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   Der vollständige Code für [**HelloWorld.java** finden Sie hier .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Stellen Sie den Code AEM mithilfe von Apache Maven bereit:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Überprüfen der Bereitstellung und Registrierung des [!DNL Sling Model] durch Navigation zu [[!UICONTROL Status] > [!UICONTROL Sling-Modelle]](http://localhost:4502/system/console/status-slingmodels) in der OSGi-Konsole.

   Sie sollten sehen, dass die Variable `HelloWorld` Das Sling-Modell ist an die `we-retail-journal/components/helloworld` Sling-Ressourcentyp und dass er als [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Erstellen einer React-Komponente {#react-component}

**Persona: Frontend-Entwickler**

Als Nächstes wird die React-Komponente erstellt. Öffnen Sie die **response-app** module ( `<src>/aem-sample-we-retail-journal/react-app`) unter Verwendung des Editors Ihrer Wahl.

>[!NOTE]
>
> Sie können diesen Abschnitt überspringen, wenn Sie nur an [Angular-Entwicklung](#angular-component).

1. Innerhalb des `react-app` Ordner navigieren Sie zum Ordner src . Erweitern Sie den Komponentenordner, um die vorhandenen React-Komponentendateien anzuzeigen.

   ![Struktur der React-Komponentendatei](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Fügen Sie eine neue Datei unter dem Komponentenordner hinzu mit dem Namen `HelloWorld.js`.
1. Öffnen Sie `HelloWorld.js`. Fügen Sie eine Importanweisung hinzu, um die React-Komponentenbibliothek zu importieren. Fügen Sie eine zweite Importanweisung hinzu, um die `MapTo` Helper, bereitgestellt von der Adobe. Die `MapTo` Helper stellt eine Zuordnung der React-Komponente zur JSON der AEM Komponente bereit.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Unter den Importen wird eine neue Klasse mit dem Namen `HelloWorld` , die React erweitert `Component` -Schnittstelle. Fügen Sie die erforderlichen hinzu `render()` -Methode `HelloWorld` -Klasse.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. Die `MapTo` Helper enthält automatisch ein Objekt namens `cqModel` als Teil der Eigenschaften der React-Komponente. Die `cqModel` enthält alle Eigenschaften, die von der [!DNL Sling Model].

   Speichern Sie die [!DNL Sling Model] früher erstellt wurde, enthält eine Methode `getDisplayMessage()`. `getDisplayMessage()` wird als JSON-Schlüssel mit dem Namen `displayMessage` wenn Ausgabe.

   Implementieren des `render()` -Methode zur Ausgabe einer `h1` -Tag, das den Wert von `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), eine Syntaxerweiterung für JavaScript, wird verwendet, um das endgültige Markup der Komponente zurückzugeben.

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

1. Implementieren Sie eine Konfigurationsmethode zum Bearbeiten. Diese Methode wird über die `MapTo` unterstützt und stellt dem AEM Editor Informationen bereit, um einen Platzhalter anzuzeigen, falls die Komponente leer ist. Dies tritt auf, wenn die Komponente der SPA hinzugefügt, aber noch nicht erstellt wurde. Fügen Sie unter dem `HelloWorld` -Klasse:

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

1. Rufen Sie am Ende der Datei die `MapTo` Helper, Übergeben der `HelloWorld` und `HelloWorldEditConfig`. Dadurch wird die React-Komponente basierend auf dem Ressourcentyp der AEM Komponente der AEM Komponente zugeordnet: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   Der fertige Code für [**HelloWorld.js** finden Sie hier .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Öffnen Sie die Datei `ImportComponents.js`. Sie finden sie unter `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Fügen Sie eine Zeile hinzu, um die `HelloWorld.js` mit den anderen Komponenten im kompilierten JavaScript-Bundle:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. Im  `components`  Ordner erstellen Sie eine neue Datei mit dem Namen `HelloWorld.css` als Geschwister `HelloWorld.js.` Füllen Sie die Datei mit folgendem Inhalt, um einige grundlegende Stile für die `HelloWorld` component:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Erneutes Öffnen `HelloWorld.js` und aktualisieren Sie unterhalb der Importanweisungen, um `HelloWorld.css`:

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

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Suchen Sie in app.js schnell nach &quot;HelloWorld&quot;, um sicherzustellen, dass die React-Komponente in der kompilierten App enthalten ist.

   >[!NOTE]
   >
   > **app.js** ist die gebündelte React-App. Der Code ist nicht mehr für Menschen lesbar. Die `npm run build` -Befehl hat einen optimierten Build ausgelöst, der kompiliertes JavaScript ausgibt, das von modernen Browsern interpretiert werden kann.


## Angular-Komponente erstellen {#angular-component}

**Persona: Frontend-Entwickler**

>[!NOTE]
>
> Sie können diesen Abschnitt überspringen, wenn Sie nur an der React-Entwicklung interessiert sind.

Als Nächstes wird die Angular-Komponente erstellt. Öffnen Sie die **angular-app** module (`<src>/aem-sample-we-retail-journal/angular-app`) unter Verwendung des Editors Ihrer Wahl.

1. Innerhalb des `angular-app` Ordner navigieren zu seiner `src` Ordner. Erweitern Sie den Komponentenordner, um die vorhandenen Angular-Komponentendateien anzuzeigen.

   ![Angular-Dateistruktur](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Fügen Sie einen neuen Ordner unter dem Komponentenordner hinzu mit dem Namen `helloworld`. Unter dem `helloworld` Ordner fügen neue Dateien hinzu namens `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

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

1. Öffnen Sie `helloworld.component.ts`. Fügen Sie eine Importanweisung hinzu, um die Angular zu importieren. `Component` und `Input` Klassen. Erstellen Sie eine neue Komponente, die auf die `styleUrls` und `templateUrl` nach `helloworld.component.css` und `helloworld.component.html`. Schließlich die Klasse exportieren `HelloWorldComponent` mit dem erwarteten Input von `displayMessage`.

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
   > Wenn Sie sich an die [!DNL Sling Model] zuvor erstellt wurde, gab es eine Methode **getDisplayMessage()**. Die serialisierte JSON-Datei dieser Methode lautet **displayMessage**, die wir jetzt in der Angular-App lesen.

1. Öffnen `helloworld.component.html` , um `h1` -Tag, das die `displayMessage` Eigenschaft:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Aktualisieren `helloworld.component.css` , um einige grundlegende Stile für die Komponente einzuschließen.

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

1. Aktualisieren `helloworld.component.spec.ts` mit dem folgenden Prüfstand:

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

1. Nächstes Update `src/components/mapping.ts` , um `HelloWorldComponent`. Hinzufügen einer `HelloWorldEditConfig` markiert den Platzhalter im AEM-Editor, bevor die Komponente konfiguriert wurde. Fügen Sie schließlich eine Zeile hinzu, um die AEM Komponente der Angular-Komponente mit der `MapTo` Helper.

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

   Der vollständige Code für [**mapping.ts** finden Sie hier .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Aktualisieren `src/app.module.ts` , um die **NgModule**. Fügen Sie die **`HelloWorldComponent`** as a **Erklärung** , der zu **AppModule**. Fügen Sie außerdem `HelloWorldComponent` als **entryComponent** sodass sie kompiliert und dynamisch in die App integriert wird, während das JSON-Modell verarbeitet wird.

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

   Der fertige Code für [**app.module.ts** finden Sie hier .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Stellen Sie den Code mithilfe von Maven für AEM bereit:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Schnellsuche nach **HelloWorld** in `main.js` , um zu überprüfen, ob die Angular-Komponente eingeschlossen wurde.

   >[!NOTE]
   >
   > **main.js** ist die gebündelte Angular-App. Der Code ist nicht mehr für Menschen lesbar. Der Befehl npm run build hat einen optimierten Build ausgelöst, der kompiliertes JavaScript ausgibt, das von modernen Browsern interpretiert werden kann.

## Aktualisieren der Vorlage {#template-update}

1. Navigieren Sie zur bearbeitbaren Vorlage für die React- und/oder Angular-Versionen:

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Wählen Sie den Hauptteil aus [!UICONTROL Layout-Container] und wählen Sie die [!UICONTROL Politik] Symbol, um die Richtlinie zu öffnen:

   ![Layout-Richtlinie auswählen](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   under **[!UICONTROL Eigenschaften]** > **[!UICONTROL Zugelassene Komponenten]**, führen Sie eine Suche nach **[!DNL Custom Components]**. Sie sollten die **[!DNL Hello World]** -Komponente auswählen. Speichern Sie Ihre Änderungen, indem Sie auf das Kontrollkästchen oben rechts klicken.

   ![Richtlinienkonfiguration für Layout-Container](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Nach dem Speichern sollte die **[!DNL HelloWorld]** -Komponente als zulässige Komponente in [!UICONTROL Layout-Container].

   ![Aktualisierte zulässige Komponenten](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Nur AEM 6.5 und AEM 6.4.5 unterstützen die Funktion &quot;Bearbeitbare Vorlage&quot;des SPA-Editors. Bei Verwendung von AEM 6.4 müssen Sie die Richtlinie für zulässige Komponenten manuell über die CRXDE Lite konfigurieren: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` oder `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite mit den aktualisierten Richtlinienkonfigurationen für [!UICONTROL Zugelassene Komponenten] im [!UICONTROL Layout-Container]:

   ![CRXDE Lite mit den aktualisierten Richtlinienkonfigurationen für zulässige Komponenten im Layout-Container](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Zusammenfassung {#putting-together}

1. Navigieren Sie zu den Angular- oder React-Seiten:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Suchen Sie die **[!DNL Hello World]** Komponente hinzufügen und per Drag-and-Drop **[!DNL Hello World]** -Komponente auf der Seite.

   ![Hallo Welt ziehen und ablegen](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   Der Platzhalter sollte angezeigt werden.

   ![Hallo, Platzhalter](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Wählen Sie die Komponente aus und fügen Sie eine Meldung im Dialogfeld hinzu, z. B. &quot;Welt&quot;oder &quot;Ihr Name&quot;. Speichern Sie die Änderungen.

   ![gerenderte Komponente](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Beachten Sie, dass der Nachricht immer die Zeichenfolge &quot;Hello &quot;vorangestellt wird. Dies ist das Ergebnis der Logik in der `HelloWorld.java` [!DNL Sling Model].

## Nächste Schritte {#next-steps}

[Abgeschlossene Lösung für die Komponente &quot;HelloWorld&quot;](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Vollständiger Quellcode für [[!DNL We.Retail Journal] auf GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Sehen Sie sich ein tiefergehendes Tutorial zur Entwicklung von React mit [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/de/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Fehlerbehebung {#troubleshooting}

### Projekt kann nicht in Eclipse erstellt werden {#unable-to-build-project-in-eclipse}

**Fehler:** Fehler beim Import der [!DNL We.Retail Journal] Projekt in Eclipse für nicht erkannte Zielausführungen:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![Eclipse-Fehlerassistent](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Auflösung**: Klicken Sie auf Beenden , um diese später zu beheben. Dies sollte den Abschluss des Tutorials nicht verhindern.

**Fehler**: das React-Modul, `react-app`, wird während eines Maven-Builds nicht erfolgreich erstellt.

**Auflösung:** Löschen Sie die `node_modules` Ordner unter **response-app**. Führen Sie den Befehl &quot;Apache Maven&quot;erneut aus `mvn  clean install -PautoInstallSinglePackage` aus dem Stammverzeichnis des Projekts.

### Nicht zufrieden stellende Abhängigkeiten in AEM {#unsatisfied-dependencies-in-aem}

![Fehler bei Package Manager-Abhängigkeit](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Wenn eine AEM Abhängigkeit nicht erfüllt ist, wird in der **[!UICONTROL AEM Package Manager]** oder in **[!UICONTROL AEM Web-Konsole]** (Felix-Konsole), deutet dies darauf hin, dass SPA Editor-Funktion nicht verfügbar ist.

### Komponente wird nicht angezeigt

**Fehler**: Selbst nach erfolgreicher Bereitstellung und Überprüfung, ob die kompilierten Versionen von React/Angular-Apps die aktualisierten `helloworld` Komponente Meine Komponente wird nicht angezeigt, wenn ich sie auf die Seite ziehe. Ich kann die Komponente in der AEM Benutzeroberfläche sehen.

**Auflösung**: Löschen Sie den Verlauf/Cache Ihres Browsers und/oder öffnen Sie einen neuen Browser oder verwenden Sie den Inkognito-Modus. Wenn dies nicht funktioniert, machen Sie den Client-Bibliotheks-Cache auf der lokalen AEM ungültig. AEM versucht, große Client-Bibliotheken zwischenzuspeichern, um effizient zu sein. Manchmal ist eine manuelle Invalidierung des Caches erforderlich, um Probleme zu beheben, bei denen veralteter Code zwischengespeichert wird.

Navigieren Sie zu: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) und klicken Sie auf Cache invalidieren . Kehren Sie zu Ihrer React-/Angular-Seite zurück und aktualisieren Sie die Seite.

![Client-Bibliothek neu erstellen](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
