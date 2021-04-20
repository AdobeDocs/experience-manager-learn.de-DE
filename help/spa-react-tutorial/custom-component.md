---
title: Erstellen einer benutzerspezifischen Komponente | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA Editor verwendet werden soll. Erfahren Sie, wie Sie Authoring-Dialoge und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.
sub-product: Sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1491'
ht-degree: 4%

---


# Erstellen einer benutzerdefinierten Komponente {#custom-component}

Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA Editor verwendet werden soll. Erfahren Sie, wie Sie Authoring-Dialoge und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.

## Vorgabe

1. Machen Sie sich mit der Rolle von Sling-Modellen beim Manipulieren der von AEM bereitgestellten JSON-Modell-API vertraut.
2. Erfahren Sie, wie Sie neue Dialogfelder AEM Komponenten erstellen.
3. Lernen Sie, eine **benutzerdefinierte** AEM Komponente zu erstellen, die mit dem SPA Editor-Framework kompatibel ist.

## Was Sie erstellen werden

Der Fokus früherer Kapitel lag auf der Entwicklung SPA Komponenten und deren Zuordnung zu *vorhandenen* AEM Kernkomponenten. Dieses Kapitel konzentriert sich auf die Erstellung und Erweiterung von *neuen* AEM Komponenten und die Bearbeitung des von AEM bereitgestellten JSON-Modells.

Ein einfaches `Custom Component` stellt die Schritte dar, die zum Erstellen einer neuen AEM-Komponente erforderlich sind.

![In Großbuchstaben angezeigte Meldung](assets/custom-component/message-displayed.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das `classic`-Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installieren Sie das fertige Paket für die herkömmliche [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest). Die von [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest) bereitgestellten Bilder werden auf der WKND-SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) oder lokal prüfen, indem Sie zur Verzweigung `React/custom-component-solution` wechseln.

## Definieren der AEM Komponente

Eine AEM Komponente wird als Knoten und Eigenschaften definiert. Im Projekt werden diese Knoten und Eigenschaften als XML-Dateien im Modul `ui.apps` dargestellt. Anschließend erstellen Sie die AEM Komponente im Modul `ui.apps`.

>[!NOTE]
>
> Eine schnelle Auffrischung der [Grundlagen der AEM Komponenten kann hilfreich sein](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html).

1. Öffnen Sie in der IDE Ihrer Wahl den Ordner `ui.apps`.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` und erstellen Sie einen neuen Ordner mit dem Namen `custom-component`.
3. Erstellen Sie eine neue Datei mit dem Namen `.content.xml` unter dem Ordner `custom-component`. Füllen Sie die `custom-component/.content.xml` mit folgenden Elementen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Definition benutzerdefinierter Komponenten erstellen](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - erkennt, dass dieser Knoten eine AEM Komponente sein wird.

   `jcr:title` ist der Wert, der Inhaltsautoren angezeigt wird, und der  `componentGroup` bestimmt die Gruppierung von Komponenten in der Authoring-Benutzeroberfläche.

4. Erstellen Sie unter dem Ordner `custom-component` einen weiteren Ordner mit dem Namen `_cq_dialog`.
5. Erstellen Sie unter dem Ordner `_cq_dialog` eine neue Datei mit dem Namen `.content.xml` und füllen Sie sie wie folgt aus:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![Benutzerdefinierte Komponentendefinition](assets/custom-component/dialog-custom-component-defintion.png)

   Die obige XML-Datei generiert einen sehr einfachen Dialog für das `Custom Component`. Der kritische Teil der Datei ist der innere `<message>`-Knoten. Dieses Dialogfeld enthält ein einfaches `textfield` mit dem Namen `Message` und behält den Wert des Textfelds für eine Eigenschaft mit dem Namen `message` bei.

   Neben dem Wert der Eigenschaft `message` wird ein Sling-Modell über das JSON-Modell erstellt.

   >[!NOTE]
   >
   > Sie können viel mehr [Beispiele für Dialoge durch Ansicht der Core-Komponentendefinitionen](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components) anzeigen. Sie können auch zusätzliche Formularfelder wie `select`, `textarea`, `pathfield`, `/libs/granite/ui/components/coral/foundation/form` unter [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form) Ansicht werden.

   Bei einer herkömmlichen AEM ist in der Regel ein Skript [HTL](https://docs.adobe.com/content/help/de-DE/experience-manager-htl/using/overview.html) erforderlich. Da der SPA die Komponente wiedergibt, ist kein HTML-Skript erforderlich.

## Sling-Modell erstellen

Sling-Modelle sind von Anmerkungen angetriebene Java-&quot;POJOs&quot;(einfache alte Java-Objekte), die die Zuordnung von Daten aus der JCR zu Java-Variablen erleichtern. [Sling-](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) Modelle können typischerweise komplexe serverseitige Geschäftslogik für AEM Komponenten enthalten.

Im Kontext des SPA-Editors stellen Sling-Modelle den Inhalt einer Komponente über das JSON-Modell mithilfe einer Funktion unter Verwendung des [Sling Model Exporter](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/foundation/development/develop-sling-model-exporter.html) offen.

1. Öffnen Sie in der IDE Ihrer Wahl das Modul `core`. `CustomComponent.java` und  `CustomComponentImpl.java` wurden bereits als Teil des Kapitelstartercodes erstellt und ausstohlen.

   >[!NOTE]
   >
   > Bei Verwendung der Code-IDE von Visual Studio kann es hilfreich sein, [Erweiterungen für Java](https://code.visualstudio.com/docs/java/extensions) zu installieren.

2. Öffnen Sie die Java-Schnittstelle `CustomComponent.java` bei `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`:

   ![CustomComponent.java-Schnittstelle](assets/custom-component/custom-component-interface.png)

   Dies ist die Java-Schnittstelle, die vom Sling-Modell implementiert wird.

3. Aktualisieren Sie `CustomComponent.java`, damit die `ComponentExporter`-Schnittstelle erweitert wird:

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Die Implementierung der `ComponentExporter`-Schnittstelle ist eine Voraussetzung dafür, dass das Sling-Modell automatisch von der JSON-Modell-API aufgenommen wird.

   Die `CustomComponent`-Schnittstelle enthält eine einzelne get-Methode `getMessage()`. Dies ist die Methode, die den Wert des Autorendialogs über das JSON-Modell verfügbar macht. Nur öffentliche Getter-Methoden mit leeren Parametern `()` werden im JSON-Modell exportiert.

4. Öffnen Sie `CustomComponentImpl.java` bei `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`.

   Dies ist die Implementierung der `CustomComponent`-Schnittstelle. Die `@Model`-Anmerkung identifiziert die Java-Klasse als Sling-Modell. Die `@Exporter`-Anmerkung ermöglicht die Serialisierung und den Export der Java-Klasse über den Sling Model Exporter.

5. Aktualisieren Sie die statische Variable `RESOURCE_TYPE`, um auf die AEM Komponente `wknd-spa-react/components/custom-component` zu verweisen, die in der vorherigen Übung erstellt wurde.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   Der Ressourcentyp der Komponente ist der, der das Sling-Modell an die AEM Komponente bindet und letztendlich der React-Komponente zugeordnet wird.

6. hinzufügen Sie die `getExportedType()`-Methode an die `CustomComponentImpl`-Klasse, um den Komponentenressourcentyp zurückzugeben:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Diese Methode ist bei der Implementierung der `ComponentExporter`-Schnittstelle erforderlich und stellt den Ressourcentyp offen, der die Zuordnung zur React-Komponente ermöglicht.

7. Aktualisieren Sie die `getMessage()`-Methode, um den Wert der `message`-Eigenschaft zurückzugeben, die vom Autorendialogfeld beibehalten wird. Verwenden Sie die Anmerkung `@ValueMap`, um den JCR-Wert `message` einer Java-Variablen zuzuordnen:

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Es wird eine zusätzliche &quot;Geschäftslogik&quot;hinzugefügt, um den Zeichenfolgenwert der Nachricht mit allen Großbuchstaben zurückzugeben. Dadurch können wir den Unterschied zwischen dem Rohwert sehen, der im Autorendialog gespeichert wird, und dem Wert, der vom Sling-Modell offen gelegt wird.

   >[!NOTE]
   >
   > Sie können [fertige CustomComponentImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java) Ansicht durchführen.

## Aktualisieren der React-Komponente

Der React-Code für die benutzerdefinierte Komponente wurde bereits erstellt. Nehmen Sie als Nächstes einige Aktualisierungen vor, um die React-Komponente der AEM Komponente zuzuordnen.

1. Öffnen Sie im Modul `ui.frontend` die Datei `ui.frontend/src/components/Custom/Custom.js`.
2. Beachten Sie die Variable `{this.props.message}` als Teil der `render()`-Methode:

   ```js
   return (
           <div className="CustomComponent">
               <h2 className="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   Es wird erwartet, dass der transformierte Großbuchstabenwert des Sling-Modells dieser `message`-Eigenschaft zugeordnet wird.

3. Importieren Sie das `MapTo`-Objekt aus dem AEM SPA Editor JS SDK und verwenden Sie es, um der AEM Komponente zuzuordnen:

   ```diff
   + import {MapTo} from '@adobe/aem-react-editable-components';
   
    ...
    export default class Custom extends Component {
        ...
    }
   
   + MapTo('wknd-spa-react/components/custom-component')(Custom, CustomEditConfig);
   ```

4. Stellen Sie alle Updates mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen AEM Umgebung aus dem Stammordner des Projektverzeichnisses bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Vorlagenrichtlinie aktualisieren

Navigieren Sie dann zu AEM, um die Aktualisierungen zu überprüfen, und lassen Sie zu, dass `Custom Component` der SPA hinzugefügt wird.

1. Überprüfen Sie die Registrierung des neuen Sling-Modells, indem Sie zu [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels) navigieren.

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Sie sollten die beiden obigen Zeilen sehen, die angeben, dass `CustomComponentImpl` mit der `wknd-spa-react/components/custom-component`-Komponente verknüpft ist und dass sie über den Sling Model Exporter registriert ist.

2. Navigieren Sie zur SPA Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue Komponente `Custom Component` als zulässige Komponente hinzuzufügen:

   ![Richtlinie &quot;Layout Container aktualisieren&quot;](assets/custom-component/custom-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie `Custom Component` als zulässige Komponente:

   ![Benutzerdefinierte Komponente als zulässige Komponente](assets/custom-component/custom-component-allowed-layout-container.png)

## Benutzerdefinierte Komponente erstellen

Als Nächstes erstellen Sie `Custom Component` mit dem AEM SPA Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. Fügen Sie im `Edit`-Modus `Custom Component` dem `Layout Container` hinzu:

   ![Neue Komponente einfügen](assets/custom-component/insert-custom-component.png)

3. Öffnen Sie das Dialogfeld der Komponente und geben Sie eine Meldung ein, die Kleinbuchstaben enthält.

   ![Benutzerdefinierte Komponente konfigurieren](assets/custom-component/enter-dialog-message.png)

   Dies ist das Dialogfeld, das auf der Grundlage der XML-Datei zuvor im Kapitel erstellt wurde.

4. Speichern Sie die Änderungen. Achten Sie darauf, dass die angezeigte Nachricht in allen Schreibrichtungen angezeigt wird.

   ![In Großbuchstaben angezeigte Meldung](assets/custom-component/message-displayed.png)

5. Ansicht des JSON-Modells durch Navigieren zu [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Suchen Sie nach `wknd-spa-react/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   Beachten Sie, dass der JSON-Wert basierend auf der Logik, die dem Sling-Modell hinzugefügt wurde, auf alle Großbuchstaben eingestellt ist.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie Sie eine benutzerdefinierte AEM-Komponente erstellen, die mit dem SPA Editor verwendet werden soll. Sie haben auch gelernt, wie Dialoge, JCR-Eigenschaften und Sling-Modelle interagieren, um das JSON-Modell auszugeben.

Sie können den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) oder den Code lokal prüfen, indem Sie zur Verzweigung `React/custom-component-solution` wechseln.

### Nächste Schritte {#next-steps}

[Erweitern einer Core-Komponente](extend-component.md)  - Erfahren Sie, wie Sie eine bestehende Core-Komponente erweitern, um sie mit dem AEM SPA Editor zu verwenden. Das Verstehen, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern.
