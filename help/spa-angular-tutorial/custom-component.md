---
title: Erstellen einer benutzerspezifischen Komponente | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA-Editor verwendet werden soll. Erfahren Sie, wie Sie Authoring-Dialoge und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.
sub-product: Sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 1fd4d31770a4eac37a88a7c6960fd51845601bee
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 3%

---


# Erstellen einer benutzerspezifischen Komponente {#custom-component}

Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA-Editor verwendet werden soll. Erfahren Sie, wie Sie Authoring-Dialoge und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.

## Vorgabe

1. Machen Sie sich mit der Rolle von Sling-Modellen beim Manipulieren der von AEM bereitgestellten JSON-Modell-API vertraut.
2. Erfahren Sie, wie Sie neue Dialogfelder AEM Komponenten erstellen.
3. Hier erfahren Sie, wie Sie eine **benutzerdefinierte** AEM-Komponente erstellen, die mit dem SPA-Editor-Framework kompatibel ist.

## Was Sie erstellen

Der Schwerpunkt früherer Kapitel lag auf der Entwicklung von SPA-Komponenten und deren Zuordnung zu *bestehenden* AEM Kernkomponenten. Dieses Kapitel konzentriert sich auf die Erstellung und Erweiterung *neuer* AEM Komponenten und die Bearbeitung des von AEM bereitgestellten JSON-Modells.

Eine einfache Anleitung `Custom Component` zeigt die Schritte, die zum Erstellen einer neuen AEM-Komponente erforderlich sind.

![In Großbuchstaben angezeigte Meldung](assets/custom-component/message-displayed.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Bei Verwendung von [AEM 6.x](overview.md#compatibility) fügen Sie das `classic` Profil hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installieren Sie das fertige Paket für die herkömmliche [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest). Die von der [WKND-Referenzseite](https://github.com/adobe/aem-guides-wknd/releases/latest) bereitgestellten Bilder werden im WKND SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `Angular/custom-component-solution`.

## Definieren der AEM Komponente

Eine AEM Komponente wird als Knoten und Eigenschaften definiert. Im Projekt werden diese Knoten und Eigenschaften als XML-Dateien im `ui.apps` Modul dargestellt. Anschließend erstellen Sie die AEM Komponente im `ui.apps` Modul.

>[!NOTE]
>
> Eine schnelle Auffrischung der [Grundlagen AEM Komponenten kann hilfreich](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)sein.

1. Öffnen Sie in der IDE Ihrer Wahl den `ui.apps` Ordner.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` einem neuen Ordner mit dem Namen `custom-component`.
3. Create a new file named `.content.xml` beneath the `custom-component` folder. Populate the `custom-component/.content.xml` with the following:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Definition benutzerdefinierter Komponenten erstellen](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - erkennt, dass dieser Knoten eine AEM Komponente sein wird.

   `jcr:title` ist der Wert, der Inhaltsautoren angezeigt wird, und der `componentGroup` bestimmt die Gruppierung von Komponenten in der Authoring-Benutzeroberfläche.

4. Erstellen Sie unter dem `custom-component` Ordner einen weiteren Ordner mit dem Namen `_cq_dialog`.
5. Erstellen Sie unterhalb des `_cq_dialog` Ordners eine neue Datei mit dem Namen `.content.xml` und füllen Sie sie wie folgt aus:

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

   Die obige XML-Datei erzeugt einen sehr einfachen Dialog für die `Custom Component`. Der kritische Teil der Datei ist der innere `<message>` Knoten. Dieser Dialog enthält einen einfachen `textfield` Namen `Message` und behält den Wert des textifelds bei einer Eigenschaft mit dem Namen `message`.

   Neben dem Wert der `message` Eigenschaft wird ein Sling-Modell über das JSON-Modell erstellt.

   >[!NOTE]
   >
   > Sie können noch viel mehr [Beispiele für Dialoge in den Core-Komponentendefinitionen](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)anzeigen. Sie können auch weitere Formularfelder wie `select`, `textarea`, `pathfield`, unterhalb `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)verfügbar machen.

   Bei einer herkömmlichen AEM ist in der Regel ein [HTML](https://docs.adobe.com/content/help/de-DE/experience-manager-htl/using/overview.html) -Skript erforderlich. Da die SPA die Komponente wiedergibt, ist kein HTML-Skript erforderlich.

## Sling-Modell erstellen

Sling-Modelle sind von Anmerkungen angetriebene Java-&quot;POJOs&quot;(einfache alte Java-Objekte), die die Zuordnung von Daten aus der JCR zu Java-Variablen erleichtern. [Sling-Modelle](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) können in der Regel komplexe serverseitige Geschäftslogik für AEM Komponenten enthalten.

Im Kontext des SPA-Editors stellen Sling-Modelle den Inhalt einer Komponente über das JSON-Modell mithilfe einer Funktion mit dem [Sling Model Exporter](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)offen.

1. Öffnen Sie in der IDE Ihrer Wahl das `core` Modul. `CustomComponent.java` und `CustomComponentImpl.java` wurden bereits als Teil des Kapitelstartercodes erstellt und ausgelagert.

   >[!NOTE]
   >
   > Bei Verwendung der Code-IDE von Visual Studio kann es hilfreich sein, [Erweiterungen für Java](https://code.visualstudio.com/docs/java/extensions)zu installieren.

2. Öffnen Sie die Java-Oberfläche `CustomComponent.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java-Schnittstelle](assets/custom-component/custom-component-interface.png)

   Dies ist die Java-Schnittstelle, die vom Sling-Modell implementiert wird.

3. Aktualisieren Sie, `CustomComponent.java` sodass die `ComponentExporter` Oberfläche erweitert wird:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Die Implementierung der `ComponentExporter` Schnittstelle ist eine Voraussetzung dafür, dass das Sling-Modell automatisch von der JSON-Modell-API aufgenommen wird.

   Die `CustomComponent` Oberfläche enthält eine einzige Getter-Methode `getMessage()`. Dies ist die Methode, die den Wert des Autorendialogs über das JSON-Modell verfügbar macht. Nur Getter-Methoden mit leeren Parametern `()` werden im JSON-Modell exportiert.

4. Öffnen Sie `CustomComponentImpl.java` bei `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Dies ist die Implementierung der `CustomComponent` Schnittstelle. Die `@Model` Anmerkung identifiziert die Java-Klasse als Sling-Modell. Die `@Exporter` Anmerkung ermöglicht die Serialisierung und den Export der Java-Klasse über den Sling Model Exporter.

5. Aktualisieren Sie die statische Variable `RESOURCE_TYPE` so, dass sie auf die in der vorherigen Übung `wknd-spa-angular/components/custom-component` erstellte AEM verweist.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Der Ressourcentyp der Komponente ist der, der das Sling-Modell an die AEM Komponente bindet und letztendlich der Angular-Komponente zugeordnet wird.

6. hinzufügen Sie die `getExportedType()` Methode an die `CustomComponentImpl` -Klasse, um den Komponentenressourcentyp zurückzugeben:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Diese Methode ist bei der Implementierung der `ComponentExporter` Schnittstelle erforderlich und stellt den Ressourcentyp offen, der die Zuordnung zur Angular-Komponente ermöglicht.

7. Aktualisieren Sie die `getMessage()` Methode, um den Wert der Eigenschaft zurückzugeben, der vom Autorendialogfeld beibehalten wird `message` . Verwenden Sie die `@ValueMap` Anmerkung, um den JCR-Wert einer Java-Variablen zuzuordnen `message` :

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

   Es wird eine zusätzliche &quot;Geschäftslogik&quot;hinzugefügt, um den Wert der Nachricht als Großbuchstabe zurückzugeben. Dadurch können wir den Unterschied zwischen dem Rohwert sehen, der im Autorendialog gespeichert wird, und dem Wert, der vom Sling-Modell offen gelegt wird.

   >[!NOTE]
   >
   > Sie können die [fertige Datei CustomComponentImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java)Ansicht haben.

## Angular-Komponente aktualisieren

Der Angular-Code für die benutzerdefinierte Komponente wurde bereits erstellt. Nehmen Sie als Nächstes einige Aktualisierungen vor, um die Angular-Komponente der AEM Komponente zuzuordnen.

1. Öffnen Sie die Datei im `ui.frontend` Modul `ui.frontend/src/app/components/custom/custom.component.ts`
2. Beobachten Sie die `@Input() message: string;` Linie. Es wird erwartet, dass der transformierte Großbuchstabenwert dieser Variablen zugeordnet wird.
3. Importieren Sie das `MapTo` Objekt aus dem JS-SDK des AEM SPA Editor und verwenden Sie es, um es der AEM Komponente zuzuordnen:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Öffnen Sie `cutom.component.html` und beachten Sie, dass der Wert von `{{message}}` neben einem `<h2>` -Tag angezeigt wird.
5. Öffnen `custom.component.css` und fügen Sie die folgende Regel hinzu:

   ```css
   :host-context {
       display: block;
   }
   ```

   Damit der Platzhalter für den AEM-Editor korrekt angezeigt wird, wenn die Komponente leer ist, muss der `:host-context` oder ein anderer `<div>` auf `display: block;`eingestellt werden.

6. Stellen Sie alle Updates mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen AEM Umgebung aus dem Stammordner des Projektverzeichnisses bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Vorlagenrichtlinie aktualisieren

Navigieren Sie anschließend zu AEM, um die Aktualisierungen zu überprüfen und das Hinzufügen des Updates zur SPA `Custom Component` zu ermöglichen.

1. Überprüfen Sie die Registrierung des neuen Sling-Modells, indem Sie zu [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)navigieren.

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Sie sollten die beiden obigen Zeilen sehen, die angeben, dass die Komponente mit der `CustomComponentImpl` `wknd-spa-angular/components/custom-component` Komponente verknüpft ist und dass sie über den Sling Model Exporter registriert ist.

2. Navigieren Sie zur Vorlage der SPA-Seite unter [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue Komponente `Custom Component` als zulässige Komponente hinzuzufügen:

   ![Richtlinie &quot;Layout Container aktualisieren&quot;](assets/custom-component/custom-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie die `Custom Component` Komponente als zulässige Komponente:

   ![Benutzerdefinierte Komponente als zulässige Komponente](assets/custom-component/custom-component-allowed-layout-container.png)

## Benutzerdefinierte Komponente erstellen

Als Nächstes erstellen Sie die Datei `Custom Component` mit dem AEM SPA-Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Fügen Sie im `Edit` Modus `Custom Component` Folgendes hinzu `Layout Container`:

   ![Neue Komponente einfügen](assets/custom-component/insert-custom-component.png)

3. Öffnen Sie das Dialogfeld der Komponente und geben Sie eine Meldung ein, die Kleinbuchstaben enthält.

   ![Benutzerdefinierte Komponente konfigurieren](assets/custom-component/enter-dialog-message.png)

   Dies ist das Dialogfeld, das auf der Grundlage der XML-Datei zuvor im Kapitel erstellt wurde.

4. Speichern Sie die Änderungen. Achten Sie darauf, dass die angezeigte Nachricht in allen Schreibrichtungen angezeigt wird.

   ![In Großbuchstaben angezeigte Meldung](assets/custom-component/message-displayed.png)

5. Ansicht des JSON-Modells durch Navigieren zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Suchen Sie nach `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Beachten Sie, dass der JSON-Wert basierend auf der Logik, die dem Sling-Modell hinzugefügt wurde, auf alle Großbuchstaben eingestellt ist.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie Sie eine benutzerdefinierte AEM-Komponente erstellen und wie Sling-Modelle und Dialoge mit dem JSON-Modell funktionieren.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `Angular/custom-component-solution`.

### Nächste Schritte {#next-steps}

[Erweitern einer Core-Komponente](extend-component.md) - Erfahren Sie, wie Sie eine bestehende Core-Komponente für den AEM SPA-Editor erweitern. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern.
