---
title: Benutzerdefinierte Komponente
description: Behandelt die End-to-End-Erstellung einer benutzerdefinierten Byline-Komponente, die erstellten Inhalt anzeigt. Dazu gehört die Entwicklung eines Sling-Modells zum Einkapseln der Geschäftslogik zum Ausfüllen der byline-Komponente und der entsprechenden HTL zum Rendern der Komponente.
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 79d41d833ab0659f26f988678e124daa18b857f3
workflow-type: tm+mt
source-wordcount: '4138'
ht-degree: 1%

---

# Benutzerdefinierte Komponente {#custom-component}

In diesem Tutorial wird die End-to-End-Erstellung einer benutzerdefinierten AEM-Byline-Komponente beschrieben, die in einem Dialogfeld verfasste Inhalte anzeigt. Außerdem wird untersucht, wie ein Sling-Modell entwickelt wird, um eine Geschäftslogik einzukapseln, die die HTL der Komponente füllt.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment).

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

Sehen Sie sich den Basis-Code an, auf dem das Tutorial aufbaut:

1. Sehen Sie sich die `tutorial/custom-component-start` Verzweigung aus [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
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

Sie können den fertigen Code immer in [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) oder den Code lokal auszuchecken, indem Sie zu der Verzweigung wechseln `tutorial/custom-component-solution`.

## Ziel

1. Informationen zum Erstellen einer benutzerdefinierten AEM-Komponente
1. Erfahren Sie, wie Sie Geschäftslogik mit Sling-Modellen einbinden können.
1. Grundlegendes zur Verwendung eines Sling-Modells aus einem HTL-Skript

## Was Sie erstellen werden {#byline-component}

In diesem Teil des WKND-Tutorials wird eine Byline-Komponente erstellt, mit der verfasste Informationen über den Beitragenden eines Artikels angezeigt werden.

![Beispiel für eine byline-Komponente](assets/custom-component/byline-design.png)

*Byte-Komponente*

Die Implementierung der Byline-Komponente umfasst ein Dialogfeld, das den byline-Inhalt erfasst, sowie ein benutzerdefiniertes Sling-Modell, das die von der Byline abruft:

* Name
* Bild
* Berufe

## Erstellen einer Byte-Komponente {#create-byline-component}

Erstellen Sie zunächst die Knotenstruktur der Byline-Komponente und definieren Sie ein Dialogfeld. Dies stellt die Komponente in AEM dar und definiert implizit den Ressourcentyp der Komponente anhand ihres Speicherorts im JCR.

Das Dialogfeld stellt die Benutzeroberfläche bereit, die Autoren von Inhalten bereitstellen können. Für diese Implementierung wird die AEM WCM-Kernkomponente **Bild** -Komponente wird genutzt, um das Authoring und Rendering des Byline-Bildes zu handhaben, sodass es als die `sling:resourceSuperType`.

### Komponentendefinition erstellen {#create-component-definition}

1. Im **ui.apps** -Modul, navigieren Sie zu `/apps/wknd/components` und erstellen Sie einen neuen Ordner mit dem Namen `byline`.
1. Unter dem `byline` Ordner fügen Sie eine neue Datei mit dem Namen `.content.xml`

   ![Dialogfeld zum Erstellen eines Knotens](assets/custom-component/byline-node-creation.png)

1. Füllen Sie die `.content.xml` -Datei mit der folgenden Zeichenfolge:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   Die obige XML-Datei stellt die Definition für die Komponente bereit, einschließlich Titel, Beschreibung und Gruppe. Die `sling:resourceSuperType` weist auf `core/wcm/components/image/v2/image`, wobei [Kernbildkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=de).

### HTML-Skript erstellen {#create-the-htl-script}

1. Unter dem `byline` Ordner, neue Datei hinzufügen `byline.html`, der für die HTML-Präsentation der Komponente verantwortlich ist. Es ist wichtig, die Datei mit dem Ordner zu benennen, da sie zum Standardskript wird, das Sling zum Rendern dieses Ressourcentyps verwendet.

1. Fügen Sie den folgenden Code zum `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` is [später erneut aufrufen](#byline-htl), sobald das Sling-Modell erstellt wurde. Der aktuelle Status der HTL-Datei ermöglicht die Anzeige der Komponente in einem leeren Status im Seiten-Editor von AEM Sites, wenn sie per Drag-and-Drop auf die Seite gezogen wird.

### Erstellen der Dialogfelddefinition {#create-the-dialog-definition}

Definieren Sie als Nächstes ein Dialogfeld für die Komponente Byline mit den folgenden Feldern:

* **Name**: ein Textfeld, in dem der Name des Beitragenden angegeben wird.
* **Bild**: einen Verweis auf das Biobild des Mitwirkenden.
* **Berufe**: eine Liste der dem Beitragenden zugeschriebenen Berufe. Berufe sollten alphabetisch in aufsteigender Reihenfolge sortiert werden (a bis z).

1. Unter dem `byline` Ordner erstellen, erstellen Sie einen neuen Ordner mit dem Namen `_cq_dialog`.
1. darunter `byline/_cq_dialog` eine neue Datei mit dem Namen `.content.xml`. Dies ist die XML-Definition für das Dialogfeld. Fügen Sie die folgende XML hinzu:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   Diese Knotendefinitionen des Dialogfelds verwenden [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) , um zu steuern, welche Dialogfeldregisterkarten von der `sling:resourceSuperType` -Komponente, in diesem Fall die **Bildkomponente der Kernkomponenten**.

   ![abgeschlossenes Dialogfeld für Byline](assets/custom-component/byline-dialog-created.png)

### Dialogfeld &quot;Richtlinie erstellen&quot; {#create-the-policy-dialog}

Erstellen Sie nach demselben Ansatz wie bei der Dialogfelderstellung ein Dialogfeld &quot;Richtlinie&quot;(ehemals &quot;Design Dialog&quot;), um unerwünschte Felder in der Richtlinienkonfiguration auszublenden, die von der Bildkomponente der Kernkomponenten übernommen wurde.

1. Unter dem `byline` Ordner erstellen, erstellen Sie einen neuen Ordner mit dem Namen `_cq_design_dialog`.
1. darunter `byline/_cq_design_dialog` eine neue Datei mit dem Namen `.content.xml`. Aktualisieren Sie die Datei mit folgendem Code: mit der folgenden XML-Datei. Es ist am einfachsten, die `.content.xml` und kopieren/fügen Sie die unten stehende XML-Datei hinzu.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   Die Grundlage für die **Politikdialog** XML wurde aus der [Bildkomponente der Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Wie in der Dialogfeldkonfiguration, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) wird verwendet, um irrelevante Felder auszublenden, die ansonsten von der `sling:resourceSuperType`, wie durch die Knotendefinitionen mit `sling:hideResource="{Boolean}true"` -Eigenschaft.

### Bereitstellen des Codes {#deploy-the-code}

1. Synchronisieren Sie die Änderungen in `ui.apps` mit Ihrer IDE oder mit Ihren Maven-Fähigkeiten.

   ![Exportieren in AEM Server-Byte-Komponente](assets/custom-component/export-byline-component-aem.png)

## Komponente zu einer Seite hinzufügen {#add-the-component-to-a-page}

Um die Dinge einfach zu halten und uns auf AEM Komponentenentwicklung zu konzentrieren, fügen wir die Byline-Komponente in ihrem aktuellen Status einer Artikelseite hinzu, um die `cq:Component` Die Knotendefinition ist bereitgestellt und korrekt, AEM erkennt die neue Komponentendefinition und das Dialogfeld der Komponente funktioniert für das Authoring.

### Hinzufügen eines Bildes zur AEM Assets

Laden Sie zunächst einen Beispielkopfzeilenshot in AEM Assets hoch, um das Bild in der Byline-Komponente zu füllen.

1. Navigieren Sie zum Ordner LA Skateparks in AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Hochladen des Head-Shots für  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** in den Ordner.

   ![Headshot in AEM Assets hochgeladen](assets/custom-component/stacey-roswell-headshot-assets.png)

### Komponente erstellen {#author-the-component}

Als Nächstes fügen Sie die Komponente &quot;Byline&quot;einer Seite in AEM hinzu. Da wir die Byline-Komponente zur **WKND Sites-Projekt - Inhalt** Komponentengruppe über die `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` definiert ist, ist es automatisch für jede **Container** which **Politik** erlaubt die **WKND Sites-Projekt - Inhalt** Komponentengruppe, die der Layout-Container der Artikelseite ist.

1. Navigieren Sie zum Artikel LA Skatepark unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Ziehen Sie aus der linken Seitenleiste eine **Byte-Komponente** auf **bottom** des Layout-Containers der geöffneten Artikelseite.

   ![Hinzufügen einer byline-Komponente zur Seite](assets/custom-component/add-to-page.png)

1. Stellen Sie sicher, dass **Die linke Seitenleiste ist geöffnet** und sichtbar sind, und die **Asset Finder** ausgewählt ist.

1. Wählen Sie die **Platzhalter für Byline-Komponenten**, wodurch wiederum die Aktionsleiste angezeigt wird, und tippen Sie auf **Schraubenschlüssel** -Symbol, um das Dialogfeld zu öffnen.

1. Öffnen Sie das Dialogfeld und öffnen Sie die erste Registerkarte (Asset), öffnen Sie die linke Seitenleiste und ziehen Sie aus der Asset-Suche ein Bild in die Dropzone Bild . Suchen Sie nach &quot;stacey&quot;, um das Bio-Bild von Stacey Roswells zu finden, das im WKND ui.content-Paket bereitgestellt wird.

   ![Bild zum Dialogfeld hinzufügen](assets/custom-component/add-image.png)

1. Nachdem Sie ein Bild hinzugefügt haben, klicken Sie auf die **Eigenschaften** Registerkarte, um die **Name** und **Berufe**.

   Geben Sie bei der Aufnahme in den Beruf **umgekehrt, alphabetisch** -Reihenfolge, sodass die alphabetisierende Geschäftslogik, die wir im Sling-Modell implementieren werden, sofort sichtbar ist.

   Tippen Sie auf **Fertig** unten rechts, um die Änderungen zu speichern.

   ![Füllen von Eigenschaften der byline-Komponente](assets/custom-component/add-properties.png)

   AEM Autoren konfigurieren und erstellen Komponenten über die Dialogfelder. Zu diesem Zeitpunkt in der Entwicklung der Byline-Komponente sind die Dialogfelder zur Datenerfassung enthalten, die Logik zum Rendern des erstellten Inhalts wurde jedoch noch nicht hinzugefügt. Daher wird nur der Platzhalter angezeigt.

1. Navigieren Sie nach dem Speichern des Dialogfelds zu [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) und überprüfen Sie, wie der Inhalt der Komponente im Inhaltsknoten der byline-Komponente unter der AEM gespeichert wird.

   Suchen Sie den Inhaltsknoten der Byline-Komponente unter der Seite &quot;LA Skate Parks&quot;, d. h. `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Beachten Sie die Eigenschaftsnamen `name`, `occupations`und `fileReference` werden auf der **byline-Knoten**.

   Beachten Sie außerdem die `sling:resourceType` des Knotens auf `wknd/components/content/byline` bindet diesen Inhaltsknoten an die Byline-Komponentenimplementierung.

   ![Autoreneigenschaften in CRXDE](assets/custom-component/byline-properties-crxde.png)

## Erstellen eines Byline Sling-Modells {#create-sling-model}

Als Nächstes erstellen wir ein Sling-Modell, das als Datenmodell fungiert und die Geschäftslogik für die Byline-Komponente speichert.

Sling-Modelle sind von Anmerkungen gesteuerte Java-&quot;POJOs&quot;(Plain Old Java Objects), die die Zuordnung von Daten aus dem JCR zu Java-Variablen erleichtern und eine Reihe anderer Eigenschaften bei der Entwicklung im Kontext von AEM bieten.

### Überprüfen von Maven-Abhängigkeiten {#maven-dependency}

Das Byline-Sling-Modell stützt sich auf mehrere von AEM bereitgestellte Java-APIs. Diese APIs werden über die `dependencies` im `core` POM-Datei des Moduls. Das für dieses Tutorial verwendete Projekt wurde für AEM as a Cloud Service entwickelt. Es ist jedoch insofern einzigartig, als es mit AEM 6.5/6.4 abwärtskompatibel ist. Daher sind beide Abhängigkeiten für Cloud Service und AEM 6.x enthalten.

1. Öffnen Sie die `pom.xml` Datei unter `<src>/aem-guides-wknd/core/pom.xml`.
1. Suchen der Abhängigkeit für `aem-sdk-api` - **Nur as a Cloud Service AEM**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   Die [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=de#building-for-the-sdk) enthält alle öffentlichen Java-APIs, die von AEM bereitgestellt werden. Die `aem-sdk-api` wird beim Erstellen dieses Projekts standardmäßig verwendet. Die Version wird im übergeordneten Reaktorpom beibehalten, das sich im Stammverzeichnis des Projekts unter `aem-guides-wknd/pom.xml`.

1. Suchen Sie die Abhängigkeit für die `uber-jar` - **Nur AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   Die `uber-jar` ist nur enthalten, wenn `classic` Profil aufgerufen wird, d. h. `mvn clean install -PautoInstallSinglePackage -Pclassic`. Auch dies ist für dieses Projekt einzigartig. In einem realen Projekt, das mithilfe des AEM Projektarchetyps generiert wurde, `uber-jar` ist der Standardwert, wenn die angegebene Version von AEM 6.5 oder 6.4 ist.

   Die [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) enthält alle öffentlichen Java-APIs, die von AEM 6.x verfügbar gemacht werden. Die Version wird im übergeordneten Reaktorpom verwaltet, das sich im Stammverzeichnis des Projekts befindet. `aem-guides-wknd/pom.xml`.

1. Suchen der Abhängigkeit für `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Dies ist die Gesamtheit der öffentlichen Java-APIs, die von AEM Kernkomponenten bereitgestellt werden. AEM Kernkomponenten ist ein Projekt, das außerhalb von AEM verwaltet wird und daher einen separaten Versionszyklus aufweist. Aus diesem Grund ist es eine Abhängigkeit, die separat eingeschlossen werden muss und **not** im `uber-jar` oder `aem-sdk-api`.

   Wie die uber-jar wird auch die Version für diese Abhängigkeit in der übergeordneten Reaktor-POM-Datei unter `aem-guides-wknd/pom.xml`.

   Später in diesem Tutorial verwenden wir die Image-Klasse der Kernkomponente, um das Bild in der Byline-Komponente anzuzeigen. Die Abhängigkeit der Kernkomponente muss vorhanden sein, damit unser Sling-Modell erstellt und kompiliert werden kann.

### Byte-Schnittstelle {#byline-interface}

Erstellen Sie eine öffentliche Java-Schnittstelle für die Byline. `Byline.java` definiert die öffentlichen Methoden, die erforderlich sind, um die `byline.html` HTL-Skript.

1. Innerhalb der `aem-guides-wknd.core` Modul unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models` eine neue Datei mit dem Namen `Byline.java`

   ![Erstellen einer Byline-Oberfläche](assets/custom-component/create-byline-interface.png)

1. Aktualisieren `Byline.java` mit den folgenden Methoden:

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   Die ersten beiden Methoden legen die Werte für die **name** und **Berufe** für die Komponente &quot;Byline&quot;.

   Die `isEmpty()` -Methode wird verwendet, um festzustellen, ob die Komponente über Inhalte verfügt, die gerendert werden sollen, oder ob sie darauf wartet, konfiguriert zu werden.

   Beachten Sie, dass es keine Methode für das Bild gibt. [Wir werden uns ansehen, warum das später so ist](#tackling-the-image-problem).

1. Java-Pakete, die öffentliche Java-Klassen enthalten (in diesem Fall ein Sling-Modell), müssen mithilfe der  `package-info.java` -Datei.

   Seit dem Java-Paket der WKND-Quelle `com.adobe.aem.guides.wknd.core.models` deklariert sind Version von `1.0.0`, und wir fügen eine innovative öffentliche Oberfläche und Methoden hinzu, muss die Version auf `1.1.0`. Öffnen Sie die Datei unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` und aktualisieren `@Version("1.0.0")` nach `@Version("1.1.0")`.

       &quot;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       import org.osgi.annotation.versioning.Version;
       &quot;
   
Wenn Änderungen an den Dateien in diesem Paket vorgenommen werden, wird die [Paketversion muss semantisch angepasst werden](https://semver.org/). Wenn nicht, wird die [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) erkennt eine ungültige Paketversion und beschädigt die erstellte. Glücklicherweise meldet das Maven-Plug-in bei Fehlern die ungültige Java-Paketversion sowie die Version, die es sein sollte. Aktualisieren Sie einfach die `@Version("...")` Deklaration im verletzenden Java-Paket `package-info.java` auf die vom Plug-in empfohlene Version zu aktualisieren.

### Byte-Implementierung {#byline-implementation}

`BylineImpl.java` ist die Implementierung des Sling-Modells, das die `Byline.java` -Schnittstelle, die zuvor definiert wurde. Der vollständige Code für `BylineImpl.java` finden Sie unten in diesem Abschnitt.

1. Erstellen Sie einen neuen Ordner mit dem Namen `impl` unter `core/src/main/java/com/adobe/aem/guides/core/models`.
1. Im `impl` Ordner erstellen Sie eine neue Datei `BylineImpl.java`.

   ![Byline-Impl-Datei](assets/custom-component/byline-impl-file.png)

1. Öffnen Sie `BylineImpl.java`. Geben Sie an, dass die Variable `Byline` -Schnittstelle. Verwenden Sie die automatischen Vervollständigungsfunktionen der IDE oder aktualisieren Sie die Datei manuell, um die zur Implementierung der `Byline` -Schnittstelle:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Hinzufügen der Anmerkungen des Sling-Modells durch Aktualisieren `BylineImpl.java` mit den folgenden Anmerkungen auf Klassenebene. Diese `@Model(..)`-Anmerkung ist das, was die Klasse in ein Sling-Modell umwandelt.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Sehen wir uns diese Anmerkung und ihre Parameter an:

   * Die `@Model` annotation registriert BylineImpl als Sling-Modell, wenn es in AEM bereitgestellt wird.
   * Die `adaptables` gibt an, dass dieses Modell durch die Anfrage angepasst werden kann.
   * Die `adapters` -Parameter ermöglicht die Registrierung der Implementierungsklasse unter der Byline-Schnittstelle. Dadurch kann das HTL-Skript das Sling-Modell über die -Schnittstelle aufrufen (anstatt direkt über die impl-Schnittstelle). [Weitere Informationen zu Adaptern finden Sie hier .](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * Die `resourceType` verweist auf den (zuvor erstellten) Byline-Komponentenkomponenten-Ressourcentyp und hilft beim Auflösen des richtigen Modells, wenn mehrere Implementierungen vorhanden sind. [Weitere Informationen zum Verknüpfen einer Modellklasse mit einem Ressourcentyp finden Sie hier .](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementieren der Sling-Modell-Methoden {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

Die erste Methode, die wir angehen werden, ist `getName()` , der einfach den Wert zurückgibt, der unter der Eigenschaft im JCR-Inhaltsknoten der Byline gespeichert ist `name`.

Dazu muss die Variable `@ValueMapValue` Die Sling-Modellannotation wird verwendet, um den Wert mithilfe der ValueMap der Anforderungsressource in ein Java-Feld einzufügen.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Da die JCR-Eigenschaft denselben Namen wie das Java-Feld hat (beide sind &quot;name&quot;), `@ValueMapValue` löst diese Zuordnung automatisch auf und fügt den Wert der Eigenschaft in das Java-Feld ein.

#### getOccupations() {#implementing-get-occupations}

Die nächste zu implementierende Methode lautet `getOccupations()`. Diese Methode erfasst alle Berufe, die in der JCR-Eigenschaft gespeichert sind `occupations` und eine sortierte (alphabetisch) Kollektion zurückgeben.

Verwenden Sie dieselbe Technik, die in `getName()` Der Eigenschaftswert kann in das Feld des Sling-Modells eingefügt werden.

Sobald die JCR-Eigenschaftswerte im Sling-Modell über das eingefügte Java-Feld verfügbar sind `occupations`, kann die Geschäftslogik zur Sortierung in der `getOccupations()` -Methode.


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

Die letzte öffentliche Methode ist `isEmpty()` , der bestimmt, wann die Komponente sich selbst zum Rendern als &quot;ausreichend erstellt&quot;betrachten soll.

Für diese Komponente verfügen wir über Geschäftsanforderungen, denen zufolge alle drei Felder, Name, Bild und Berufe ausgefüllt werden müssen *before* die Komponente kann gerendert werden.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### Behebung des Bildproblems {#tackling-the-image-problem}

Die Überprüfung der Namen und Arbeitsbedingungen ist trivial (und der Apache Commons Lang3 bietet die immer praktische [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) -Klasse), es ist jedoch unklar, wie die **Vorhandensein des Bildes** kann validiert werden, da die Kernkomponenten-Bildkomponente zum Aufdecken des Bildes verwendet wird.

Es gibt zwei Möglichkeiten, dies anzugehen:

Überprüfen Sie, ob die `fileReference` Die JCR-Eigenschaft wird zu einem Asset aufgelöst. *ODER* Konvertieren Sie diese Ressource in ein Bildsling-Modell der Kernkomponente und stellen Sie sicher, dass die `getSrc()` -Methode nicht leer ist.

Wir werden uns für die **second** Ansatz. Der erste Ansatz ist wahrscheinlich ausreichend, aber in diesem Tutorial wird dieser verwendet werden, um uns zu ermöglichen, andere Funktionen von Sling-Modellen zu untersuchen.

1. Erstellen Sie eine private Methode, die das Bild abruft. Diese Methode ist privat, da das Bildobjekt nicht in der HTL selbst verfügbar gemacht werden muss und nur zum Antrieb von `isEmpty().`

   Fügen Sie die folgende private Methode für `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Wie oben erwähnt, gibt es zwei weitere Ansätze, um die **Bild-Sling-Modell**:

   Die erste verwendet die `@Self` Anmerkung, um die aktuelle Anforderung automatisch an die Kernkomponente anzupassen `Image.class`

   Die zweite verwendet die [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi-Dienst, ein sehr praktischer Dienst, der uns beim Erstellen von Sling-Modellen anderer Typen in Java-Code unterstützt.

   Wir werden uns für den zweiten Ansatz entscheiden.

   >[!NOTE]
   >
   >In einer realen Implementierung sollten Sie &quot;One&quot;verwenden, indem Sie `@Self` wird bevorzugt, da es die einfachere, elegantere Lösung ist. In diesem Tutorial verwenden wir den zweiten Ansatz, da wir dazu gezwungen sind, mehr Facetten von Sling-Modellen zu untersuchen, die äußerst nützlich sind, sind komplexere Komponenten!

   Da Sling-Modelle Java POJOs sind und nicht OSGi-Dienste, sind die üblichen OSGi-Injektionsanmerkungen `@Reference` **cannot** verwendet werden, anstatt dass Sling-Modelle eine spezielle **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** -Anmerkung, die ähnliche Funktionen bietet.

1. Aktualisieren `BylineImpl.java` , um `OSGiService` Anmerkung zum Einfügen der `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Mit dem `ModelFactory` verfügbar ist, kann ein Kernkomponente Bild-Sling-Modell erstellt werden, indem Folgendes verwendet wird:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Diese Methode erfordert jedoch sowohl eine Anforderung als auch eine Ressource, die noch nicht im Sling-Modell verfügbar sind. Um diese zu erhalten, werden weitere Anmerkungen des Sling-Modells verwendet!

   So rufen Sie die aktuelle Anfrage ab: **[@self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** -Anmerkung kann verwendet werden, um die `adaptable` (die in der Variablen `@Model(..)` as `SlingHttpServletRequest.class`in ein Java-Klassenfeld ein.

1. Fügen Sie die **@self** Anmerkung zum Abrufen der **SlingHttpServletRequest-Anfrage**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Denken Sie daran, mithilfe von `@Self Image image` das Einfügen des Bild-Sling-Modells der Kernkomponente war eine Option oben - die `@Self` annotation versucht, das adaptive Objekt einzufügen (in diesem Fall ein SlingHttpServletRequest) und sich an den Anmerkungsfeldtyp anzupassen. Da das Kernkomponente Image Sling-Modell von SlingHttpServletRequest-Objekten angepasst werden kann, hätte dies funktioniert und ist weniger Code als unser forschenderer Ansatz.

   Jetzt haben wir die Variablen eingefügt, die erforderlich sind, um unser Bildmodell über die ModelFactory-API zu instanziieren. Wir werden das Sling-Modell **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** -Anmerkung hinzufügen, um dieses Objekt nach der Instanziierung des Sling-Modells abzurufen.

   `@PostConstruct` ist unglaublich nützlich und fungiert in ähnlicher Kapazität als Konstruktor. Es wird jedoch aufgerufen, nachdem die Klasse instanziiert und alle kommentierten Java-Felder eingefügt wurden. Andere Anmerkungen des Sling-Modells hingegen kommentieren Java-Klassenfelder (Variablen), `@PostConstruct` kommentiert eine void-, zero -Parametermethode, die normalerweise `init()` (kann jedoch beliebig benannt werden).

1. Hinzufügen **@PostConstruct** -Methode:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Beachten Sie, dass Sling-Modelle **NOT** OSGi-Dienste, sodass der Klassenstatus sicher aufrechterhalten werden kann. Häufig `@PostConstruct` leitet den Sling-Modell-Klassenstatus ab und richtet ihn für die spätere Verwendung ein, ähnlich dem, was ein einfacher Konstruktor tut.

   Beachten Sie Folgendes: Wenn die Variable `@PostConstruct` -Methode eine Ausnahme auslöst, instanziiert das Sling-Modell nicht (es ist null).

1. **getImage()** kann jetzt aktualisiert werden, um einfach das Bildobjekt zurückzugeben.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Gehen wir zurück zu `isEmpty()` und schließen Sie die Implementierung ab:

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Beachten Sie mehrere Aufrufe von `getImage()` ist nicht problematisch, da die initialisierte `image` Klassenvariable und ruft nicht auf `modelFactory.getModelFromWrappedRequest(...)` was nicht zu teuer ist, aber es lohnt sich, unnötigerweise zu vermeiden.

1. Das endgültige `BylineImpl.java` sollte wie folgt aussehen:


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that will be used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## Byline HTL {#byline-htl}

Im `ui.apps` Modul öffnen `/apps/wknd/components/byline/byline.html` haben wir in der vorherigen Einrichtung der AEM-Komponente erstellt.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Sehen wir uns an, was dieses HTL-Skript bisher bewirkt:

* Die `placeholderTemplate` verweist auf den Platzhalter der Kernkomponenten, der angezeigt wird, wenn die Komponente nicht vollständig konfiguriert ist. Dadurch wird im AEM Sites-Seiteneditor ein Feld mit dem Komponententitel gerendert, wie oben im Abschnitt `cq:Component`s  `jcr:title` -Eigenschaft.

* Die `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` lädt die `placeholderTemplate` definiert oben und übergibt einen booleschen Wert (derzeit hartcodiert an `false`) in die Platzhaltervorlage ein. Wann `isEmpty` auf &quot;true&quot;gesetzt ist, rendert die Platzhaltervorlage das graue Feld, andernfalls wird nichts gerendert.

### Aktualisieren der Byline-HTL

1. Aktualisieren **byline.html** mit der folgenden skelettalen HTML-Struktur:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Beachten Sie, dass die CSS-Klassen dem [BEM-Namenskonvention](https://getbem.com/naming/). Obwohl die Verwendung von BEM-Konventionen nicht obligatorisch ist, wird BEM empfohlen, da es in CSS-Klassen der Kernkomponenten verwendet wird und im Allgemeinen zu sauberen, lesbaren CSS-Regeln führt.

### Instanziieren von Sling-Modellobjekten in HTL {#instantiating-sling-model-objects-in-htl}

Die [Blockanweisung verwenden](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) wird verwendet, um Sling-Modell-Objekte im HTL-Skript zu instanziieren und sie einer HTL-Variablen zuzuweisen.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` verwendet die von BylineImpl implementierte Byline-Schnittstelle (com.adobe.aem.guides.wknd.models.Byline) und passt die aktuelle SlingHttpServletRequest an. Das Ergebnis wird in einer HTL-Variablennamen-Zeile ( `data-sly-use.<variable-name>`).

1. Aktualisieren des äußeren `div` auf die **Byte** Sling-Modell über seine öffentliche Oberfläche:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Zugreifen auf Sling-Modellmethoden {#accessing-sling-model-methods}

HTL leiht von JSTL und verwendet dieselbe Verkürzung der Namen von Java-Getter-Methoden.

Beispielsweise Aufrufen der `getName()` -Methode gekürzt werden kann auf `byline.name`, ähnlich anstelle von `byline.isEmpty`, können Sie dies in `byline.empty`. Verwenden vollständiger Methodennamen, `byline.getName` oder `byline.isEmpty`, funktioniert auch. Beachten Sie die `()` werden nie zum Aufrufen von Methoden in HTL verwendet (ähnlich wie JSTL).

Java-Methoden, die einen Parameter erfordern **cannot** in HTL verwendet werden. Dies ist beabsichtigt, um die Logik in HTL einfach zu halten.

1. Der Byline-Name kann der Komponente hinzugefügt werden, indem Sie die `getName()` -Methode im Byline-Sling-Modell oder in HTL: `${byline.name}`.

   Aktualisieren Sie die `h2` Tag:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Verwendung der HTL-Ausdrucksoptionen {#using-htl-expression-options}

[Optionen für HTL-Ausdrücke](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) agieren als Modifikator für Inhalte in HTL und reichen von der Datumsformatierung bis zur i18n-Übersetzung. Ausdrücke können auch verwendet werden, um Listen oder Arrays von Werten zu verknüpfen. Dies ist es, was erforderlich ist, um die Berufe in einem kommagetrennten Format anzuzeigen.

Ausdrücke werden über die `@` -Operator im HTL-Ausdruck.

1. Um der Liste der Berufe mit &quot;, &quot;beizutreten, wird der folgende Code verwendet:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Bedingte Anzeige des Platzhalters {#conditionally-displaying-the-placeholder}

Die meisten HTL-Skripte für AEM Komponenten nutzen die **Platzhalter-Paradigma** Bereitstellen eines visuellen Hinweises für Autoren **anzeigen, dass eine Komponente falsch erstellt wurde und nicht in der AEM-Veröffentlichungsinstanz angezeigt wird**. Die Konvention, um diese Entscheidung zu fördern, besteht darin, eine Methode auf dem unterstützenden Sling-Modell der Komponente zu implementieren, in unserem Fall: `Byline.isEmpty()`.

`isEmpty()` wird im Byline Sling-Modell aufgerufen und das Ergebnis (oder besser sein Negativ, über die `!` Operator) in einer HTL-Variablen namens `hasContent`:

1. Aktualisieren des äußeren `div` zum Speichern einer HTL-Variablen mit dem Namen `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Beachten Sie die Verwendung von `data-sly-test`, die HTL `test` -Block ist insofern interessant, als er eine HTL-Variable festlegt UND das HTML-Element, auf dem sie sich befindet, rendert/nicht rendert, basierend darauf, ob das Ergebnis des HTL-Ausdrucks wahr ist oder nicht. Wenn &quot;true&quot;, wird das HTML-Element gerendert, andernfalls wird es nicht gerendert.

   Diese HTL-Variable `hasContent` kann jetzt wiederverwendet werden, um den Platzhalter bedingt ein-/auszublenden.

1. Aktualisieren Sie den bedingten Aufruf an die `placeholderTemplate` unten in der Datei mit folgendem Inhalt:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Anzeigen des Bildes mit Kernkomponenten {#using-the-core-components-image}

Das HTL-Skript für `byline.html` ist nun fast vollständig und fehlt nur noch das Bild.

Da wir `sling:resourceSuperType` Wenn Sie die Kernkomponenten-Bildkomponente verwenden, um das Authoring des Bildes bereitzustellen, können wir auch die Kernkomponente &quot;Bild&quot;verwenden, um das Bild zu rendern!

Dazu müssen wir die aktuelle Byline-Ressource einbeziehen, aber den Ressourcentyp der Kernkomponenten-Bildkomponente erzwingen, indem der Ressourcentyp verwendet wird. `core/wcm/components/image/v2/image`. Dies ist ein leistungsstarkes Muster für die Wiederverwendung von Komponenten. Zu diesem Zweck verwendet HTL `data-sly-resource` verwendet.

1. Ersetzen Sie die `div` mit einer Klasse von `cmp-byline__image` durch Folgendes ersetzen:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Diese `data-sly-resource`, die aktuelle Ressource über den relativen Pfad eingeschlossen `'.'`und erzwingt die Einbeziehung der aktuellen Ressource (oder der byline-Inhaltsressource) in den Ressourcentyp von `core/wcm/components/image/v2/image`.

   Der Ressourcentyp &quot;Kernkomponente&quot;wird direkt und nicht über einen Proxy verwendet, da es sich hierbei um eine Verwendung im Skript handelt und niemals in unserem Inhalt beibehalten wird.

2. Abgeschlossen `byline.html` unten:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Stellen Sie die Codebasis in einer lokalen AEM-Instanz bereit. Da Änderungen an `core` und `ui.apps` beide Module müssen bereitgestellt werden.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Bei der Bereitstellung auf AEM 6.5/6.4 rufen Sie die `classic` profile:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Sie können auch das gesamte Projekt aus dem Stammverzeichnis mithilfe des Maven-Profils erstellen `autoInstallSinglePackage` Dies kann jedoch die Inhaltsänderungen auf der Seite überschreiben. Dies liegt daran, dass die Variable `ui.content/src/main/content/META-INF/vault/filter.xml` wurde für den Tutorial-Starter-Code geändert, um den vorhandenen AEM Inhalt sauber zu überschreiben. In einem realen Szenario wird dies kein Problem sein.

### Überprüfen der nicht formatierten Byline-Komponente {#reviewing-the-unstyled-byline-component}

1. Navigieren Sie nach der Bereitstellung der Aktualisierung zum [Ultimate Guide to LA Skateparks ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) -Seite oder wo immer Sie die Byline-Komponente zuvor im Kapitel hinzugefügt haben.

1. Die **image**, **name** und **Berufe** erscheint nun und wir haben eine nicht formatierte, aber funktionierende Byline-Komponente.

   ![Unformatierte Byline-Komponente](assets/custom-component/unstyled.png)

### Überprüfen der Sling-Modellregistrierung {#reviewing-the-sling-model-registration}

Die [Statusansicht der Sling-Modelle AEM Web-Konsole](http://localhost:4502/system/console/status-slingmodels) zeigt alle registrierten Sling-Modelle in AEM an. Das Byline Sling-Modell kann durch Überprüfen dieser Liste als installiert und erkannt validiert werden.

Wenn die Variable **BylineImpl** nicht in dieser Liste angezeigt wird, liegt wahrscheinlich ein Problem mit den Anmerkungen des Sling-Modells vor oder das Sling-Modell wurde nicht zum registrierten Sling-Modelle-Paket hinzugefügt (`com.adobe.aem.guides.wknd.core.models`) im Kernprojekt.

![Byte Sling Model registriert](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Byte-Stile {#byline-styles}

Die Byline-Komponente muss so formatiert sein, dass sie mit dem kreativen Design für die Byline-Komponente ausgerichtet wird. Dies wird mithilfe von SCSS erreicht, AEM Unterstützung für über die **ui.frontend** Maven-Unterprojekt.

### Standardstil hinzufügen

Fügen Sie Standardstile für die Byline-Komponente hinzu.

1. Kehren Sie zur IDE zurück und **ui.frontend** Projekt `/src/main/webpack/components`:
1. Erstellen Sie eine neue Datei mit dem Namen `_byline.scss`.

   ![Inline-Projekt-Explorer](assets/custom-component/byline-style-project-explorer.png)

1. Fügen Sie die CSS-Datei für die Byline-Implementierung (in Form von SCSS geschrieben) in die `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```
1. Öffnen Sie ein Terminal und navigieren Sie zum `ui.frontend` -Modul.
1. Starten Sie die `watch` verarbeiten mit dem folgenden npm-Befehl:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Kehren Sie zum Browser zurück und navigieren Sie zum [Artikel zu LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Sie sollten die aktualisierten Stile für die Komponente sehen.

   ![fertige Autorenkomponente](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Möglicherweise müssen Sie den Browser-Cache löschen, um sicherzustellen, dass kein veraltetes CSS bereitgestellt wird, und die Seite mit der Byline-Komponente aktualisieren, um den vollen Stil zu erhalten.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade eine benutzerdefinierte Komponente von Grund auf neu mit Adobe Experience Manager erstellt!

### Nächste Schritte {#next-steps}

Erfahren Sie mehr über AEM Komponentenentwicklung, indem Sie lernen, wie Sie JUnit-Tests für den Byline-Java-Code schreiben, um sicherzustellen, dass alles ordnungsgemäß entwickelt und implementierte Geschäftslogik korrekt und vollständig ist.

* [Schreiben von Komponententests oder AEM](unit-testing.md)

Anzeigen des fertigen Codes unter [GitHub](https://github.com/adobe/aem-guides-wknd) oder den Code lokal in der Git-Klammer überprüfen und bereitstellen `tutorial/custom-component-solution`.

1. Klonen Sie die [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) Repository.
1. Sehen Sie sich die `tutorial/custom-component-solution` Verzweigung
