---
title: Erstellen einer benutzerdefinierten Komponente | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA Editor verwendet werden kann. Erfahren Sie, wie Sie Bearbeitungsdialogfelder und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1498'
ht-degree: 3%

---

# Erstellen einer benutzerdefinierten Komponente {#custom-component}

Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA Editor verwendet werden kann. Erfahren Sie, wie Sie Bearbeitungsdialogfelder und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.

## Ziele

1. Machen Sie sich mit der Rolle von Sling-Modellen bei der Bearbeitung der von AEM bereitgestellten JSON-Modell-API vertraut.
2. Erfahren Sie, wie Sie neue AEM-Komponentendialogfelder erstellen.
3. Erfahren Sie, wie Sie eine **benutzerdefinierte** AEM-Komponente erstellen, die mit dem SPA Editor-Framework kompatibel ist.

## Was Sie erstellen werden

Der Schwerpunkt früherer Kapitel lag in der Entwicklung SPA Komponenten und deren Zuordnung zu *vorhandenen* AEM Kernkomponenten. Dieses Kapitel konzentriert sich auf die Erstellung und Erweiterung von *neuen* AEM Komponenten und die Bearbeitung des von AEM bereitgestellten JSON-Modells.

Ein einfaches `Custom Component` zeigt die Schritte, die zum Erstellen einer neuen AEM erforderlich sind.

![Meldung in Großbuchstaben](assets/custom-component/message-displayed.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Stellen Sie die Codebasis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installieren Sie das fertige Paket für die traditionelle Referenz-Site [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Die von [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest) bereitgestellten Bilder werden auf der WKND-SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/custom-component-solution` wechseln.

## Definieren der AEM-Komponente

Eine AEM Komponente ist als Knoten und Eigenschaften definiert. Im Projekt werden diese Knoten und Eigenschaften als XML-Dateien im Modul `ui.apps` dargestellt. Erstellen Sie anschließend die AEM-Komponente im Modul `ui.apps` .

>[!NOTE]
>
> Ein kurzer Auffrischungskurs zu den [Grundlagen AEM Komponenten kann hilfreich sein](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Öffnen Sie in der IDE Ihrer Wahl den Ordner `ui.apps` .
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` und erstellen Sie einen neuen Ordner mit dem Namen `custom-component`.
3. Erstellen Sie eine neue Datei mit dem Namen `.content.xml` unter dem Ordner `custom-component` . Füllen Sie die `custom-component/.content.xml` mit folgenden Elementen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Erstellen einer benutzerdefinierten Komponentendefinition](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - gibt an, dass dieser Knoten eine AEM Komponente sein wird.

   `jcr:title` ist der Wert, der den Inhaltsautoren angezeigt wird, und der  `componentGroup` bestimmt die Gruppierung von Komponenten in der Authoring-Benutzeroberfläche.

4. Erstellen Sie unter dem Ordner `custom-component` einen weiteren Ordner mit dem Namen `_cq_dialog`.
5. Erstellen Sie unter dem Ordner `_cq_dialog` eine neue Datei mit dem Namen `.content.xml` und fügen Sie sie wie folgt ein:

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

   ![Definition benutzerdefinierter Komponenten](assets/custom-component/dialog-custom-component-defintion.png)

   Die obige XML-Datei generiert ein sehr einfaches Dialogfeld für `Custom Component`. Der kritische Teil der Datei ist der innere `<message>` -Knoten. Dieses Dialogfeld enthält ein einfaches `textfield` mit dem Namen `Message` und behält den Wert des Textfelds für eine Eigenschaft mit dem Namen `message` bei.

   Neben dem Anzeigen des Werts der `message`-Eigenschaft über das JSON-Modell wird ein Sling-Modell erstellt.

   >[!NOTE]
   >
   > Sie können viel mehr [Beispiele für Dialogfelder anzeigen, indem Sie die Kernkomponentendefinitionen](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components) anzeigen. Sie können auch zusätzliche Formularfelder anzeigen, wie `select`, `textarea`, `pathfield`, die unter `/libs/granite/ui/components/coral/foundation/form` unter [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form) verfügbar sind.

   Bei einer herkömmlichen AEM-Komponente ist in der Regel ein [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=de)-Skript erforderlich. Da das SPA die Komponente rendert, ist kein HTL-Skript erforderlich.

## Sling-Modell erstellen

Sling-Modelle sind von Anmerkungen gesteuerte Java-&quot;POJOs&quot;(Plain Old Java Objects), die die Zuordnung von Daten aus dem JCR zu Java-Variablen erleichtern. [Sling-](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) Modelle funktionieren normalerweise, um komplexe serverseitige Geschäftslogik für AEM Komponenten zu verkapseln.

Im Kontext des SPA-Editors stellen Sling-Modelle den Inhalt einer Komponente über das JSON-Modell mithilfe einer Funktion mit dem [Sling Model Exporter](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html) bereit.

1. Öffnen Sie in der IDE Ihrer Wahl das Modul `core` . `CustomComponent.java` und  `CustomComponentImpl.java` wurden bereits als Teil des Kapitel-Starter-Codes erstellt und ausgelagert.

   >[!NOTE]
   >
   > Bei Verwendung von Visual Studio Code IDE kann es hilfreich sein, [Erweiterungen für Java](https://code.visualstudio.com/docs/java/extensions) zu installieren.

2. Öffnen Sie die Java-Schnittstelle `CustomComponent.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java-Schnittstelle](assets/custom-component/custom-component-interface.png)

   Dies ist die Java-Schnittstelle, die vom Sling-Modell implementiert wird.

3. Aktualisieren Sie `CustomComponent.java` so, dass die `ComponentExporter`-Schnittstelle erweitert wird:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Die Implementierung der `ComponentExporter`-Schnittstelle ist eine Voraussetzung dafür, dass das Sling-Modell automatisch von der JSON-Modell-API erfasst wird.

   Die `CustomComponent`-Schnittstelle enthält eine einzelne Getter-Methode `getMessage()`. Dies ist die Methode, die den Wert des Autorendialogfelds über das JSON-Modell verfügbar macht. Nur Getter-Methoden mit leeren Parametern `()` werden in das JSON-Modell exportiert.

4. Öffnen Sie `CustomComponentImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Dies ist die Implementierung der `CustomComponent`-Schnittstelle. Die Anmerkung `@Model` identifiziert die Java-Klasse als Sling-Modell. Mit der Anmerkung `@Exporter` kann die Java-Klasse über den Sling Model Exporter serialisiert und exportiert werden.

5. Aktualisieren Sie die statische Variable `RESOURCE_TYPE`, um auf die in der vorherigen Übung erstellte AEM `wknd-spa-angular/components/custom-component` zu verweisen.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Der Ressourcentyp der Komponente ist das, was das Sling-Modell an die AEM-Komponente bindet und letztendlich der Angular-Komponente zugeordnet wird.

6. Fügen Sie die `getExportedType()`-Methode zur `CustomComponentImpl`-Klasse hinzu, um den Komponenten-Ressourcentyp zurückzugeben:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Diese Methode ist bei der Implementierung der `ComponentExporter`-Schnittstelle erforderlich und stellt den Ressourcentyp bereit, der die Zuordnung zur Angular-Komponente ermöglicht.

7. Aktualisieren Sie die `getMessage()`-Methode, um den Wert der `message`-Eigenschaft zurückzugeben, die vom Autorendialogfeld beibehalten wird. Verwenden Sie die `@ValueMap` -Anmerkung, um den JCR-Wert `message` einer Java-Variablen zuzuordnen:

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

   Es wird eine zusätzliche &quot;Geschäftslogik&quot;hinzugefügt, um den Wert der Nachricht in Großbuchstaben zurückzugeben. Auf diese Weise können wir den Unterschied zwischen dem im Autorendialogfeld gespeicherten Rohwert und dem vom Sling-Modell bereitgestellten Wert sehen.

   >[!NOTE]
   >
   > Sie können [fertige CustomComponentImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java) anzeigen.

## Aktualisieren der Angular-Komponente

Der Angular-Code für die benutzerdefinierte Komponente wurde bereits erstellt. Nehmen Sie als Nächstes einige Aktualisierungen vor, um die Angular-Komponente der AEM-Komponente zuzuordnen.

1. Öffnen Sie im Modul `ui.frontend` die Datei `ui.frontend/src/app/components/custom/custom.component.ts` .
2. Beachten Sie die Zeile `@Input() message: string;` . Es wird erwartet, dass der umgewandelte Wert in Großbuchstaben dieser Variablen zugeordnet wird.
3. Importieren Sie das `MapTo`-Objekt aus dem AEM SPA Editor JS SDK und verwenden Sie es, um es der AEM-Komponente zuzuordnen:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Öffnen Sie `cutom.component.html` und beobachten Sie, dass der Wert von `{{message}}` neben einem `<h2>`-Tag angezeigt wird.
5. Öffnen Sie `custom.component.css` und fügen Sie die folgende Regel hinzu:

   ```css
   :host-context {
       display: block;
   }
   ```

   Damit der Platzhalter für den AEM-Editor richtig angezeigt wird, wenn die Komponente leer ist, muss `:host-context` oder ein anderes `<div>` auf `display: block;` gesetzt werden.

6. Stellen Sie mithilfe Ihrer Maven-Kenntnisse alle Updates in einer lokalen AEM-Umgebung aus dem Stammverzeichnis des Projektverzeichnisses bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Vorlagenrichtlinie aktualisieren

Navigieren Sie anschließend zu AEM , um die Aktualisierungen zu überprüfen und zu erlauben, dass der SPA `Custom Component` hinzugefügt wird.

1. Überprüfen Sie die Registrierung des neuen Sling-Modells, indem Sie zu [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels) navigieren.

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Die beiden oben genannten Zeilen zeigen an, dass `CustomComponentImpl` mit der Komponente `wknd-spa-angular/components/custom-component` verknüpft ist und über den Sling Model Exporter registriert wird.

2. Navigieren Sie zur SPA Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue `Custom Component` als zulässige Komponente hinzuzufügen:

   ![Aktualisieren der Layout-Container-Richtlinie](assets/custom-component/custom-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie `Custom Component` als zulässige Komponente:

   ![Benutzerdefinierte Komponente als zulässige Komponente](assets/custom-component/custom-component-allowed-layout-container.png)

## Erstellen der benutzerdefinierten Komponente

Als Nächstes erstellen Sie `Custom Component` mit dem AEM SPA Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Fügen Sie im `Edit` -Modus `Custom Component` zum `Layout Container` hinzu:

   ![Neue Komponente einfügen](assets/custom-component/insert-custom-component.png)

3. Öffnen Sie das Dialogfeld der Komponente und geben Sie eine Meldung ein, die Kleinbuchstaben enthält.

   ![Benutzerdefinierte Komponente konfigurieren](assets/custom-component/enter-dialog-message.png)

   Dies ist das Dialogfeld, das basierend auf der zuvor im Kapitel enthaltenen XML-Datei erstellt wurde.

4. Speichern Sie die Änderungen. Beachten Sie, dass die angezeigte Nachricht in allen Groß- und Kleinschreibung enthalten ist.

   ![Meldung in Großbuchstaben](assets/custom-component/message-displayed.png)

5. Zeigen Sie das JSON-Modell an, indem Sie zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) navigieren. Suchen Sie nach `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Beachten Sie, dass der JSON-Wert auf alle Großbuchstaben basierend auf der Logik festgelegt ist, die zum Sling-Modell hinzugefügt wurde.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie eine benutzerdefinierte AEM-Komponente erstellt wird und wie Sling-Modelle und -Dialogfelder mit dem JSON-Modell funktionieren.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/custom-component-solution` wechseln.

### Nächste Schritte {#next-steps}

[Erweitern einer Kernkomponente](extend-component.md)  - Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM SPA Editor zu verwenden. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern.
