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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# Erstellen einer benutzerdefinierten Komponente {#custom-component}

Erfahren Sie, wie Sie eine benutzerdefinierte Komponente erstellen, die mit dem AEM SPA Editor verwendet werden kann. Erfahren Sie, wie Sie Bearbeitungsdialogfelder und Sling-Modelle entwickeln, um das JSON-Modell zu erweitern und eine benutzerdefinierte Komponente zu füllen.

## Ziel

1. Machen Sie sich mit der Rolle von Sling-Modellen bei der Bearbeitung der von AEM bereitgestellten JSON-Modell-API vertraut.
2. Erfahren Sie, wie Sie AEM Komponentendialogfelder erstellen.
3. Erfahren Sie, wie Sie eine **custom** AEM Komponente, die mit dem SPA Editor-Framework kompatibel ist.

## Was Sie erstellen werden

Der Schwerpunkt früherer Kapitel lag in der Entwicklung SPA Komponenten und deren Zuordnung zu *vorhandene* AEM Kernkomponenten. In diesem Kapitel wird beschrieben, wie Sie *new* AEM Komponenten und bearbeiten Sie das von AEM bereitgestellte JSON-Modell.

Einfach `Custom Component` zeigt die Schritte, die zum Erstellen einer neuen AEM erforderlich sind.

![Meldung in Großbuchstaben](assets/custom-component/message-displayed.png)

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](overview.md#local-dev-environment).

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

   Wenn Sie [AEM 6.x](overview.md#compatibility) Fügen Sie die `classic` profile:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installieren Sie das fertige Paket für das herkömmliche [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest). Die von [WKND-Referenz-Site](https://github.com/adobe/aem-guides-wknd/releases/latest) werden auf der WKND-SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code immer in [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) oder den Code lokal auschecken, indem Sie zu der Verzweigung wechseln `Angular/custom-component-solution`.

## Definieren der AEM-Komponente

Eine AEM Komponente ist als Knoten und Eigenschaften definiert. Im Projekt werden diese Knoten und Eigenschaften als XML-Dateien im `ui.apps` -Modul. Erstellen Sie anschließend die AEM-Komponente im `ui.apps` -Modul.

>[!NOTE]
>
> Ein kurzer Auffrischungswert für die [Die Grundlagen AEM Komponenten können hilfreich sein](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Öffnen Sie die `ui.apps` in der IDE Ihrer Wahl.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` und erstellen Sie einen Ordner mit dem Namen `custom-component`.
3. Erstellen Sie eine Datei mit dem Namen `.content.xml` unterhalb der `custom-component` Ordner. Füllen Sie die `custom-component/.content.xml` durch Folgendes ersetzen:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Erstellen einer benutzerdefinierten Komponentendefinition](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - gibt an, dass dieser Knoten eine AEM Komponente ist.

   `jcr:title` ist der Wert, der den Inhaltsautoren angezeigt wird, und der `componentGroup` bestimmt die Gruppierung von Komponenten in der Authoring-Benutzeroberfläche.

4. Unter dem `custom-component` Ordner erstellen Sie einen weiteren Ordner mit dem Namen `_cq_dialog`.
5. Unter dem `_cq_dialog` Ordner erstellen Sie eine Datei mit dem Namen `.content.xml` und fügen Sie Folgendes hinzu:

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

   Die obige XML-Datei generiert ein einfaches Dialogfeld für die `Custom Component`. Der kritische Teil der Datei ist der innere `<message>` Knoten. Dieses Dialogfeld enthält eine einfache `textfield` benannt `Message` und behält den Wert des Textfelds in einer Eigenschaft mit dem Namen bei `message`.

   Ein Sling-Modell wird erstellt, neben dem der Wert der `message` -Eigenschaft über das JSON-Modell.

   >[!NOTE]
   >
   > Sie können viel mehr anzeigen [Beispiele für Dialogfelder durch Anzeigen der Kernkomponenten-Definitionen](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Sie können auch zusätzliche Formularfelder anzeigen, z. B. `select`, `textarea`, `pathfield`, verfügbar unter `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Mit einer herkömmlichen AEM-Komponente [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) -Skript ist normalerweise erforderlich. Da das SPA die Komponente rendert, ist kein HTL-Skript erforderlich.

## Sling-Modell erstellen

Sling-Modelle sind von Anmerkungen gesteuerte Java™-&quot;POJOs&quot;(Plain Old Java™ Objects), die die Zuordnung von Daten aus JCR zu Java™-Variablen erleichtern. [Sling-Modelle](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) typischerweise Funktion zum Einkapseln komplexer serverseitiger Geschäftslogik für AEM Komponenten.

Im Kontext des SPA-Editors stellen Sling-Modelle den Inhalt einer Komponente über das JSON-Modell mithilfe einer Funktion mit dem [Sling Model Exporter](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=de).

1. Öffnen Sie in der IDE Ihrer Wahl die `core` -Modul. `CustomComponent.java` und `CustomComponentImpl.java` wurden bereits als Teil des Kapitel-Starter-Codes erstellt und ausgelagert.

   >[!NOTE]
   >
   > Bei Verwendung der Visual Studio Code IDE kann es hilfreich sein, die [Erweiterungen für Java™](https://code.visualstudio.com/docs/java/extensions).

2. Öffnen Sie die Java™-Benutzeroberfläche. `CustomComponent.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java-Schnittstelle](assets/custom-component/custom-component-interface.png)

   Dies ist die Java™-Schnittstelle, die vom Sling-Modell implementiert wird.

3. Aktualisieren `CustomComponent.java` damit die `ComponentExporter` -Schnittstelle:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Implementieren der `ComponentExporter` -Schnittstelle ist eine Voraussetzung dafür, dass das Sling-Modell automatisch von der JSON-Modell-API erfasst wird.

   Die `CustomComponent` -Schnittstelle enthält eine einzige Getter-Methode `getMessage()`. Dies ist die Methode, die den Wert des Autorendialogfelds über das JSON-Modell verfügbar macht. Nur Getter-Methoden mit leeren Parametern `()` werden im JSON-Modell exportiert.

4. Öffnen `CustomComponentImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Dies ist die Implementierung der `CustomComponent` -Schnittstelle. Die `@Model` -Anmerkung identifiziert die Java™-Klasse als Sling-Modell. Die `@Exporter` -Anmerkung ermöglicht die Serialisierung und den Export der Java™-Klasse über den Sling Model Exporter.

5. Aktualisieren der statischen Variablen `RESOURCE_TYPE` , um auf die AEM-Komponente zu verweisen `wknd-spa-angular/components/custom-component` , die in der vorherigen Übung erstellt wurden.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Der Ressourcentyp der Komponente ist das, was das Sling-Modell an die AEM-Komponente bindet und letztendlich der Angular-Komponente zuordnet.

6. Fügen Sie die `getExportedType()` -Methode `CustomComponentImpl` -Klasse zum Zurückgeben des Komponenten-Ressourcentyps:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Diese Methode ist bei der Implementierung der `ComponentExporter` -Schnittstelle und stellt den Ressourcentyp bereit, der die Zuordnung zur Angular-Komponente ermöglicht.

7. Aktualisieren Sie die `getMessage()` -Methode, um den Wert der `message` -Eigenschaft beibehalten, die im Dialogfeld &quot;Autor&quot;beibehalten wird. Verwenden Sie die `@ValueMap` -Anmerkung wird dem JCR-Wert zugeordnet `message` auf eine Java™-Variable:

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
   Sie können die [Fertige CustomComponentImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Aktualisieren der Angular-Komponente

Der Angular-Code für die benutzerdefinierte Komponente wurde bereits erstellt. Nehmen Sie als Nächstes einige Aktualisierungen vor, um die Angular-Komponente der AEM-Komponente zuzuordnen.

1. Im `ui.frontend` -Modul öffnen Sie die -Datei `ui.frontend/src/app/components/custom/custom.component.ts`
2. Beobachten Sie die `@Input() message: string;` Linie. Es wird erwartet, dass der umgewandelte Wert in Großbuchstaben dieser Variablen zugeordnet wird.
3. Importieren Sie die `MapTo` -Objekt vom AEM SPA Editor JS SDK und verwenden Sie es, um es der AEM Komponente zuzuordnen:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Öffnen `cutom.component.html` und beachten Sie, dass der Wert von `{{message}}` wird in einer `<h2>` -Tag.
5. Öffnen `custom.component.css` und fügen Sie die folgende Regel hinzu:

   ```css
   :host-context {
       display: block;
   }
   ```

   Damit der AEM Editor-Platzhalter richtig angezeigt wird, wenn die Komponente leer ist, muss `:host-context` oder andere `<div>` muss auf `display: block;`.

6. Stellen Sie die Aktualisierungen mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Umgebung aus dem Stammverzeichnis des Projektverzeichnisses bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Vorlagenrichtlinie aktualisieren

Navigieren Sie als Nächstes zu AEM , um die Aktualisierungen zu überprüfen und die `Custom Component` zum SPA hinzugefügt werden.

1. Überprüfen Sie die Registrierung des neuen Sling-Modells, indem Sie zu [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Sie sollten die beiden obigen Zeilen sehen, die die `CustomComponentImpl` ist mit dem `wknd-spa-angular/components/custom-component` und dass sie über den Sling Model Exporter registriert ist.

2. Navigieren Sie zur SPA Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue `Custom Component` als zulässige Komponente:

   ![Aktualisieren der Layout-Container-Richtlinie](assets/custom-component/custom-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie die `Custom Component` als zulässige Komponente:

   ![Benutzerdefinierte Komponente als zulässige Komponente](assets/custom-component/custom-component-allowed-layout-container.png)

## Erstellen der benutzerdefinierten Komponente

Als Nächstes erstellen Sie die `Custom Component` mit dem AEM-SPA-Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` -Modus, fügen Sie die `Custom Component` der `Layout Container`:

   ![Neue Komponente einfügen](assets/custom-component/insert-custom-component.png)

3. Öffnen Sie das Dialogfeld der Komponente und geben Sie eine Meldung ein, die Kleinbuchstaben enthält.

   ![Benutzerdefinierte Komponente konfigurieren](assets/custom-component/enter-dialog-message.png)

   Dies ist das Dialogfeld, das basierend auf der zuvor im Kapitel enthaltenen XML-Datei erstellt wurde.

4. Speichern Sie die Änderungen. Beachten Sie, dass die angezeigte Nachricht in allen Groß- und Kleinschreibung enthalten ist.

   ![Meldung in Großbuchstaben](assets/custom-component/message-displayed.png)

5. Anzeigen des JSON-Modells durch Navigieren zu [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Suchen Sie nach `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Beachten Sie, dass der JSON-Wert auf alle Großbuchstaben basierend auf der Logik festgelegt ist, die zum Sling-Modell hinzugefügt wurde.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie eine benutzerdefinierte AEM-Komponente erstellt wird und wie Sling-Modelle und -Dialogfelder mit dem JSON-Modell funktionieren.

Sie können den fertigen Code immer in [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) oder den Code lokal auschecken, indem Sie zu der Verzweigung wechseln `Angular/custom-component-solution`.

### Nächste Schritte {#next-steps}

[Erweitern einer Kernkomponente](extend-component.md) - Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM SPA Editor zu verwenden. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern.
