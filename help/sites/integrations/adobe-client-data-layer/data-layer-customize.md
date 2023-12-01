---
title: Anpassen der Adobe Client-Datenschicht mit AEM-Komponenten
description: Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM-Komponenten anpassen können. Erfahren Sie, wie Sie mit den von AEM-Kernkomponenten bereitgestellten APIs die Datenschicht erweitern und anpassen können.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2008'
ht-degree: 100%

---

# Anpassen der Adobe Client-Datenschicht mit AEM-Komponenten {#customize-data-layer}

Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM-Komponenten anpassen können. Erfahren Sie, wie Sie mit den von AEM-Kernkomponenten bereitgestellten APIs die Datenschicht [erweitern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=de) und anpassen können.

## Was Sie erstellen werden

![Autorenzeilen-Datenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

In diesem Tutorial werden verschiedene Optionen zur Erweiterung der Adobe Client-Datenschicht durch Aktualisieren der [Autorenzeilen-Komponente](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=de) („Byline“) von WKND beschrieben. Die _Autorenzeilen_-Komponente ist eine **benutzerdefinierte Komponente**. Das im Rahmen dieses Tutorial gewonnene Wissen kann auch auf andere benutzerdefinierte Komponenten angewendet werden.

### Ziele {#objective}

1. Einfügen von Komponentendaten in die Datenschicht, indem Sling-Modell und Komponenten-HTL erweitert werden
1. Verwenden der Datenschicht-Dienstprogramme der Kernkomponenten, um den Aufwand zu reduzieren
1. Verwenden der Datenattribute der Kernkomponente, um sich in vorhandene Datenschichtereignisse sozusagen einzuklinken

## Voraussetzungen {#prerequisites}

Für die Durchführung dieses Tutorials ist eine **lokale Entwicklungsumgebung** erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das hier unter macOS ausgeführt wird. Befehle und Code sind unabhängig vom lokalen Betriebssystem, sofern nicht anders angegeben.

**Neu bei AEM as a Cloud Service?** Sehen Sie sich eine [detaillierte Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) an.

**Neu bei AEM 6.5?** Sehen Sie sich die [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung an](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de).

## Herunterladen und Bereitstellen der WKND-Referenz-Site {#set-up-wknd-site}

In diesem Tutorial wird die Autorenzeilen-Komponente auf die WKND-Referenz-Site ausgedehnt. Klonen und installieren Sie die WKND-Code-Basis in Ihrer lokalen Umgebung.

1. Starten Sie eine lokale **Author**-Schnellstartinstanz von AEM unter [http://localhost:4502](http://localhost:4502).
1. Öffnen Sie ein Terminal-Fenster und klonen Sie die WKND-Code-Basis mithilfe von Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Stellen Sie die WKND-Code-Basis in einer lokalen AEM-Instanz bereit:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Fügen Sie für AEM 6.5 und das neueste Service Pack das `classic`-Profil zum Maven-Befehl hinzu:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Öffnen Sie ein neues Browser-Fenster und melden Sie sich bei AEM an. Öffnen Sie eine **Magazinseite** wie [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Autorenzeilen-Komponente auf der Seite](assets/adobe-client-data-layer/byline-component-onpage.png)

   Es sollte ein Beispiel der Autorenzeilen-Komponente angezeigt werden, die der Seite als Teil eines Experience Fragments hinzugefügt wurde. Sie können das Experience Fragment unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html) anzeigen
1. Öffnen Sie Ihre Entwickler-Tools und geben Sie den folgenden Befehl in die **Konsole** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

   Um den aktuellen Status der Datenschicht auf einer AEM-Site anzuzeigen, überprüfen Sie die Antwort. Sie sollten Informationen über die Seite und einzelne Komponenten sehen.

   ![Adobe Datenschicht-Antwort](assets/data-layer-state-response.png)

   Beachten Sie, dass die Autorenzeile-Komponente nicht auf der Datenschicht aufgeführt ist.

## Aktualisieren des Autorenzeilen-Sling-Modells {#sling-model}

Um Daten über die Komponente in die Datenschicht einzufügen, aktualisieren wir zunächst das Sling-Modell der Komponente. Aktualisieren Sie als Nächstes die Java™-Schnittstelle der Autorenzeile und ihre Sling-Modell-Implementierung, um eine neue Methode `getData()` zu erhalten. Diese Methode enthält die Eigenschaften, die in die Datenschicht eingefügt werden sollen.

1. Öffnen Sie das Projekt `aem-guides-wknd` in der IDE Ihrer Wahl. Navigieren Sie zum `core`-Modul.
1. Öffnen Sie die Datei `Byline.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Autorenzeile-Java-Schnittstelle](assets/adobe-client-data-layer/byline-java-interface.png)

1. Fügen Sie folgende Methode zur Schnittstelle hinzu:

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Öffnen Sie die Datei `BylineImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. Das ist die Implementierung der `Byline`-Schnittstelle, die als Sling-Modell implementiert wird.

1. Fügen Sie am Anfang der Datei die folgenden Importanweisungen hinzu:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Die `fasterxml.jackson`-APIs werden verwendet, um die Daten zu serialisieren, die als JSON bereitgestellt werden sollen. Die `ComponentUtils`-AEM-Kernkomponenten werden verwendet, um zu überprüfen, ob die Datenschicht aktiviert ist.

1. Fügen Sie die nicht implementierte Methode `getData()` zu `BylineImple.java` hinzu:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   In der obigen Methode wird eine neue `HashMap` verwendet, um die Eigenschaften zu erfassen, die als JSON verfügbar gemacht werden sollen. Beachten Sie, dass vorhandene Methoden wie `getName()` und `getOccupations()` verwendet werden. Der `@type` repräsentiert den eindeutigen Ressourcentyp der Komponente und ermöglicht es einem Client, Ereignisse und/oder Trigger anhand des Komponententyps einfach zu identifizieren.

   Der `ObjectMapper` wird verwendet, um die Eigenschaften zu serialisieren und eine JSON-Zeichenfolge zurückzugeben. Diese JSON-Zeichenfolge kann dann in die Datenschicht eingefügt werden.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `core`-Modul mithilfe Ihrer Maven-Kenntnisse:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aktualisieren Sie die Autorenzeilen-HTL. {#htl}

Aktualisieren Sie anschließend die `Byline`-[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=de). HTL (HTML-Vorlagensprache) ist die Vorlage zum Rendern der Komponenten-HTML.

Ein spezielles Datenattribut `data-cmp-data-layer` auf jeder AEM-Komponente wird verwendet, um ihre Datenschicht anzuzeigen. Das von AEM-Kernkomponenten bereitgestellte JavaScript sucht nach diesem Datenattribut. Der Wert dieses Datenattributs wird mit der JSON-Zeichenfolge gefüllt, die von der `getData()`-Methode des Autorenzeilen-Sling-Modells zurückgegeben und in die Adobe Client-Datenschicht eingefügt wird.

1. Öffnen Sie das `aem-guides-wknd`-Projekt in IDE. Navigieren Sie zum `ui.apps`-Modul.
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Autorenzeilen-HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Aktualisieren Sie `byline.html`, um das `data-cmp-data-layer`-Attribut einzuschließen:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Der Wert von `data-cmp-data-layer` wurde auf `"${byline.data}"` gesetzt, wobei `byline` das Sling-Modell ist, das zuvor aktualisiert wurde. `.data` ist die Standardnotation zum Aufrufen einer Java™-Getter-Methode in HTL vom in der vorangegangenen Übung implementierten `getData()`.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `ui.apps`-Modul mithilfe Ihrer Maven-Kenntnisse:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit einer Autorenzeilen-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Öffnen Sie die Entwickler-Tools und überprüfen Sie die HTML-Quelle der Seite für die Autorenzeilen-Komponente:

   ![Autorenzeilen-Datenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Sie sollten sehen, dass die `data-cmp-data-layer` mit der JSON-Zeichenfolge aus dem Sling-Modell gefüllt wurde.

1. Öffnen Sie die Entwickler-Tools des Browsers und geben Sie den folgenden Befehl in die **Konsole** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort zu `component`. Dort sehen Sie, dass die Instanz der `byline`-Komponente zur Datenschicht hinzugefügt wurde:

   ![Autorenzeilenteil der Datenschicht](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Es sollte ein Eintrag wie der folgende angezeigt werden:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Beachten Sie, dass die angezeigten Eigenschaften dieselben sind, die in der `HashMap` im Sling-Modell hinzugefügt wurden.

## Hinzufügen eines Klick-Ereignisses {#click-event}

Die Adobe Client-Datenschicht ist ereignisgesteuert, und eines der häufigsten Ereignisse für das Auslösen einer Aktion ist das `cmp:click`-Ereignis. Die AEM-Kernkomponenten vereinfachen die Registrierung Ihrer Komponente mithilfe des Datenelements: `data-cmp-clickable`.

Klickbare Elemente sind normalerweise eine CTA-Schaltfläche oder ein Navigations-Link. Leider besitzt die Autprenzeilen-Komponente nichts davon, aber wir registrieren sie trotzdem, da dies für andere benutzerdefinierte Komponenten üblich sein könnte.

1. Öffnen Sie das `ui.apps`-Modul in Ihrer IDE
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aktualisieren Sie `byline.html`, um das `data-cmp-clickable`-Attribut im **Namenselement** der Autorenzeile einzuschließen:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Öffnen Sie ein neues Terminal. Erstellen und implementieren Sie nur das `ui.apps`-Modul mithilfe Ihrer Maven-Kenntnisse:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Autorenzeilen-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Zum Testen unseres Ereignisses fügen wir mithilfe von Developer Console manuell JavaScript hinzu. In [Verwenden der Adobe Client-Datenschicht mit AEM-Kernkomponenten](data-layer-overview.md) finden Sie ein Video dazu.

1. Öffnen Sie die Entwickler-Tools des Browsers und geben Sie den folgenden Befehl in die **Konsole** ein:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Diese einfache Methode sollte den Klick auf den Namen der Autorenzeilen-Komponente verarbeiten.

1. Geben Sie die folgende Methode in die **Konsole** ein:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Die obige Methode überträgt einen Ereignis-Listener auf die Datenschicht zum Lauschen auf das Ereignis `cmp:click` und ruft den `bylineClickHandler` auf.

   >[!CAUTION]
   >
   > Es ist wichtig, den Browser während dieser Übung **nicht** zu aktualisieren, da sonst das Konsolen-JavaScript verloren geht.

1. Klicken Sie im Browser mit geöffneter **Konsole** in der Autorenzeilne-Komponente auf den Autorennamen:

   ![Autorenzeilen-Komponente angeklickt](assets/adobe-client-data-layer/byline-component-clicked.png)

   Die Konsolenmeldung `Byline Clicked!` und der Name der Autorenzeile sollten angezeigt werden.

   Das `cmp:click`-Ereignis ist am einfachsten einzubinden. Für komplexere Komponenten und zur Verfolgung anderer Verhaltensweisen ist es möglich, benutzerdefiniertes JavaScript hinzuzufügen, um neue Ereignisse hinzuzufügen und zu registrieren. Ein gutes Beispiel ist die Karussellkomponente, die immer dann ein `cmp:show`-Ereignis auslöst, wenn eine Folie umgeschaltet wird. Siehe den [Quell-Code für weitere Details](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Verwenden des DataLayerBuilder-Dienstprogramms {#data-layer-builder}

Als das Sling-Modell am Anfang des Kapitels [aktualisiert](#sling-model) wurde, haben wir uns dafür entschieden, die JSON-Zeichenfolge mithilfe einer `HashMap` zu erstellen und jede Eigenschaft manuell festzulegen. Diese Methode funktioniert in Bezug auf kleine, einmalige Komponenten einwandfrei. Bei Komponenten, die die AEM-Kernkomponenten erweitern, kann dies jedoch zu viel zusätzlichem Code führen.

Eine Dienstprogrammklasse, `DataLayerBuilder`, dient für den größten Teil der schweren Hebearbeit. Dadurch können Implementierungen nur die Eigenschaften erweitern, die sie benötigen. Aktualisieren wir das Sling-Modell, um den `DataLayerBuilder` zu verwenden.

1. Kehren Sie zur IDE zurück und navigieren Sie zum `core`-Modul.
1. Öffnen Sie die Datei `Byline.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Ändern Sie die `getData()`-Methode, um einen `ComponentData`-Typ zurückzugeben.

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` ist ein Objekt, das von AEM-Kernkomponenten bereitgestellt wird. Dies führt wie im vorherigen Beispiel zu einer JSON-Zeichenfolge, führt aber auch viele zusätzliche Arbeiten durch.

1. Öffnen Sie die Datei `BylineImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Fügen Sie die folgenden Importanweisungen hinzu:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Ersetzen Sie die `getData()`-Methode durch die folgende:

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   Die Autorenzeilen-Komponente verwendet Teile der Bild-Kernkomponente erneut, um ein Bild anzuzeigen, das die Autorin oder den Autor darstellt. Im obigen Snippet wird der [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) verwendet, um die Datenschicht der `Image`-Komponente zu erweitern. Dadurch wird das JSON-Objekt mit allen Daten zum verwendeten Bild vorausgefüllt. Es erfüllt auch einige der Routinefunktionen wie das Festlegen des `@type` und der eindeutigen Kennung der Komponente. Beachten Sie, dass die Methode klein ist!

   Die einzige Eigenschaft erweitert die `withTitle`, die durch den Wert von `getName()` ersetzt wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `core`-Modul mithilfe Ihrer Maven-Kenntnisse:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Kehren Sie zur IDE zurück und öffnen Sie die Datei `byline.html` unter `ui.apps`.
1. Führen Sie eine HTL-Aktualisierung durch, um mithilfe von `byline.data.json` das Attribut `data-cmp-data-layer` aufzufüllen:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Denken Sie daran, dass wir jetzt ein Objekt vom Typ `ComponentData` zurückgeben. Dieses Objekt enthält die Getter-Methode `getJson()`, worüber das Attribut `data-cmp-data-layer` aufgefüllt wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `ui.apps`-Modul mithilfe Ihrer Maven-Kenntnisse:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Autorenzeilen-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Öffnen Sie die Entwickler-Tools des Browsers und geben Sie den folgenden Befehl in die **Konsole** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort unter `component`, um die Instanz der `byline`-Komponente zu finden:

   ![Aktualisierte Autorenzeilen-Datenschicht](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Es sollte ein Eintrag wie der folgende angezeigt werden:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Wie Sie sehen, ist nun ein `image`-Objekt innerhalb des Komponenteneintrags `byline` vorhanden. Dieses enthält wesentlich mehr Informationen zum Asset im DAM. Außerdem wurden `@type`, die eindeutige ID (in diesem Fall `byline-136073cfcb`) und `repo:modifyDate` automatisch aufgefüllt. Letzteres gibt an, wann die Komponente geändert wurde.

## Weitere Beispiele {#additional-examples}

1. Ein weiteres Beispiel für die Erweiterung der Datenschicht ist zu finden, wenn Sie sich die `ImageList`-Komponente in der WKND-Code-Basis ansehen:
   * `ImageList.java` – Java-Schnittstelle im `core`-Modul
   * `ImageListImpl.java` – Sling-Modell im `core`-Modul
   * `image-list.html` – HTL-Vorlage im `ui.apps`-Modul

   >[!NOTE]
   >
   > Es ist etwas schwieriger, benutzerdefinierte Eigenschaften wie `occupation` bei Verwendung von [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) einzuschließen. Wenn Sie jedoch eine Kernkomponente erweitern, die ein Bild oder eine Seite umfasst, spart das Dienstprogramm viel Zeit.

   >[!NOTE]
   >
   > Beim Erstellen einer erweiterten Datenschicht für Objekte, die in einer Implementierung wiederverwendet werden, wird empfohlen, die Datenschichtelemente in ihre eigenen, datenschichtspezifischen Java™-Objekte zu extrahieren. Beispielsweise besitzen die Commerce-Kernkomponenten Schnittstellen für `ProductData` und `CategoryData`, da diese für viele Komponenten innerhalb einer Commerce-Implementierung verwendet werden können. Weitere Informationen finden Sie im [Code im aem-cif-core-components-Repository](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer).

## Herzlichen Glückwunsch! {#congratulations}

Sie haben gerade verschiedene Möglichkeiten kennengelernt, um die Adobe Client-Datenschicht mit AEM-Komponenten zu erweitern und anzupassen.

## Zusätzliche Ressourcen {#additional-resources}

* [Adobe Client-Datenschicht-Dokumentation](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Datenschichtintegration mit den Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=de)
