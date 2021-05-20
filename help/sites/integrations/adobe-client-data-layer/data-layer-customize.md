---
title: Anpassen der Adobe Client-Datenschicht mit AEM Komponenten
description: Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM anpassen. Erfahren Sie, wie Sie mit den von AEM Kernkomponenten bereitgestellten APIs die Datenschicht erweitern und anpassen können.
feature: Adobe Client-Datenschicht, Kernkomponente
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: Integrationen
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 4%

---


# Anpassen der Adobe Client-Datenschicht mit AEM Komponenten {#customize-data-layer}

Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM anpassen. Erfahren Sie, wie Sie mit den von [AEM Kernkomponenten bereitgestellten APIs die Datenschicht erweitern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) und anpassen können.

## Was Sie erstellen werden

![Byte-Datenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

In diesem Tutorial werden Sie verschiedene Optionen zum Erweitern der Adobe Client-Datenschicht untersuchen, indem Sie die WKND [Byline-Komponente](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html) aktualisieren. Dies ist eine benutzerdefinierte Komponente. Die in diesem Tutorial gelernten Lektionen können auf andere benutzerdefinierte Komponenten angewendet werden.

### Ziele {#objective}

1. Fügen Sie Komponentendaten in die Datenschicht ein, indem Sie ein Sling-Modell und eine Komponenten-HTL erweitern.
1. Verwenden Sie Datenschichtprogramme der Kernkomponenten, um den Aufwand zu reduzieren
1. Verwenden Sie die Datenattribute der Kernkomponente , um in vorhandene Datenschichtereignisse einzubinden

## Voraussetzungen {#prerequisites}

Um dieses Tutorial abzuschließen, ist eine **lokale Entwicklungsumgebung** erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das auf einem macOS ausgeführt wird. Befehle und Code sind unabhängig vom lokalen Betriebssystem, sofern nicht anders angegeben.

**Neu bei AEM as a Cloud Service?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) an.

**Neu bei AEM 6.5?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) an.

## Herunterladen und Bereitstellen der WKND-Referenz-Website {#set-up-wknd-site}

In diesem Tutorial wird die Byline-Komponente auf der WKND-Referenz-Site erweitert. Klonen und installieren Sie die WKND-Codebasis in Ihrer lokalen Umgebung.

1. Starten Sie eine lokale Schnellstart-Instanz **author** von AEM, die unter [http://localhost:4502](http://localhost:4502) ausgeführt wird.
1. Öffnen Sie ein Terminal-Fenster und klonen Sie die WKND-Codebasis mithilfe von Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Stellen Sie die WKND-Codebasis in einer lokalen Instanz von AEM bereit:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 und das neueste Service Pack verwenden, fügen Sie das Profil `classic` zum Maven-Befehl hinzu:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Öffnen Sie ein neues Browser-Fenster und melden Sie sich bei AEM an. Öffnen Sie eine Seite **Magazine** wie: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Autorenkomponente auf Seite](assets/adobe-client-data-layer/byline-component-onpage.png)

   Es sollte ein Beispiel der Byline-Komponente angezeigt werden, die der Seite als Teil eines Experience Fragment hinzugefügt wurde. Sie können das Experience Fragment unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html) anzeigen.
1. Öffnen Sie Ihre Entwicklertools und geben Sie den folgenden Befehl in die **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect die Antwort, um den aktuellen Status der Datenschicht auf einer AEM Site anzuzeigen. Sie sollten Informationen über die Seite und einzelne Komponenten sehen.

   ![Adobe Data Layer Response](assets/data-layer-state-response.png)

   Beachten Sie, dass die Byline-Komponente nicht auf der Datenschicht aufgeführt ist.

## Aktualisieren Sie das Byline-Sling-Modell {#sling-model}

Um Daten über die Komponente in die Datenschicht einzufügen, müssen wir zunächst das Sling-Modell der Komponente aktualisieren. Aktualisieren Sie anschließend die Java-Schnittstelle von Byline und die Sling-Modell-Implementierung, um eine neue Methode `getData()` hinzuzufügen. Diese Methode enthält die Eigenschaften, die wir in die Datenschicht einfügen möchten.

1. Öffnen Sie in der IDE Ihrer Wahl das Projekt `aem-guides-wknd` . Navigieren Sie zum Modul `core` .
1. Öffnen Sie die Datei `Byline.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Byte Java-Schnittstelle](assets/adobe-client-data-layer/byline-java-interface.png)

1. Fügen Sie der -Oberfläche eine neue Methode hinzu:

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

1. Öffnen Sie die Datei `BylineImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Dies ist die Implementierung der `Byline`-Schnittstelle und wird als Sling-Modell implementiert.

1. Fügen Sie am Anfang der Datei die folgenden Importanweisungen hinzu:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Die `fasterxml.jackson`-APIs werden verwendet, um die Daten zu serialisieren, die wir als JSON verfügbar machen möchten. Die `ComponentUtils` AEM Kernkomponenten werden verwendet, um zu überprüfen, ob die Datenschicht aktiviert ist.

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

   In der obigen Methode wird ein neuer `HashMap` verwendet, um die Eigenschaften zu erfassen, die wir als JSON verfügbar machen möchten. Beachten Sie, dass vorhandene Methoden wie `getName()` und `getOccupations()` verwendet werden. `@type` den eindeutigen Ressourcentyp der Komponente darstellt, sodass ein Client Ereignisse und/oder Trigger anhand des Komponententyps leicht identifizieren kann.

   `ObjectMapper` wird verwendet, um die Eigenschaften zu serialisieren und eine JSON-Zeichenfolge zurückzugeben. Diese JSON-Zeichenfolge kann dann in die Datenschicht eingefügt werden.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und stellen Sie mithilfe Ihrer Maven-Fähigkeiten nur das Modul `core` bereit:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aktualisieren der Byline-HTL {#htl}

Aktualisieren Sie als Nächstes `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (HTML-Vorlagensprache) ist die Vorlage zum Rendern der HTML-Datei der Komponente.

Ein spezielles Datenattribut `data-cmp-data-layer` für jede AEM Komponente wird verwendet, um seine Datenschicht anzuzeigen.  JavaScript, das von AEM Kernkomponenten bereitgestellt wird, sucht nach diesem Datenattribut, dessen Wert mit der JSON-Zeichenfolge gefüllt wird, die von der `getData()`-Methode des Byline Sling-Modells zurückgegeben wird, und injiziert die Werte in die Adobe Client-Datenschicht.

1. Öffnen Sie in der IDE das Projekt `aem-guides-wknd` . Navigieren Sie zum Modul `ui.apps` .
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byte HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Aktualisieren Sie `byline.html`, um das Attribut `data-cmp-data-layer` einzuschließen:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Der Wert von `data-cmp-data-layer` wurde auf `"${byline.data}"` festgelegt, wobei `byline` das zuvor aktualisierte Sling-Modell ist. `.data` ist die Standardnotation zum Aufrufen einer Java Getter-Methode in HTL von , die in der vorherigen Übung  `getData()` implementiert wurde.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und stellen Sie mithilfe Ihrer Maven-Fähigkeiten nur das Modul `ui.apps` bereit:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit einer Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Öffnen Sie die Entwickler-Tools und überprüfen Sie die HTML-Quelle der Seite für die Byline-Komponente:

   ![Byte-Datenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Sie sollten sehen, dass `data-cmp-data-layer` mit der JSON-Zeichenfolge aus dem Sling-Modell gefüllt wurde.

1. Öffnen Sie die Entwicklertools des Browsers und geben Sie den folgenden Befehl in die **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort unter `component`, um die Instanz der Komponente `byline` zu finden, die der Datenschicht hinzugefügt wurde:

   ![Byline-Teil der Datenschicht](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Es sollte ein Eintrag wie der folgende angezeigt werden:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Beachten Sie, dass die angezeigten Eigenschaften dieselben sind, die im Sling-Modell im `HashMap` hinzugefügt werden.

## Hinzufügen eines Klickereignisses {#click-event}

Die Adobe Client-Datenschicht ist ereignisgesteuert und eines der häufigsten Ereignisse zum Trigger einer Aktion ist das `cmp:click`-Ereignis. Die AEM Kernkomponenten vereinfachen die Registrierung Ihrer Komponente mithilfe des Datenelements: `data-cmp-clickable`.

Klickbare Elemente sind normalerweise eine CTA-Schaltfläche oder ein Navigations-Link. Leider hat die Komponente &quot;Byline&quot;keine dieser Funktionen, aber wir werden sie trotzdem registrieren, da dies für andere benutzerdefinierte Komponenten üblich sein könnte.

1. Öffnen Sie das Modul `ui.apps` in Ihrer IDE.
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aktualisieren Sie `byline.html` , um das Attribut `data-cmp-clickable` im Element **name** der Zeile  einzuschließen:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Öffnen Sie ein neues Terminal. Erstellen und stellen Sie mithilfe Ihrer Maven-Fähigkeiten nur das Modul `ui.apps` bereit:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Zum Testen unseres Ereignisses fügen wir mithilfe der Entwicklerkonsole manuell JavaScript hinzu. Ein Video dazu finden Sie unter [Verwenden der Adobe Client-Datenschicht mit AEM Kernkomponenten](data-layer-overview.md) .

1. Öffnen Sie die Entwicklertools des Browsers und geben Sie die folgende Methode in die **Console** ein:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Diese einfache Methode sollte den Klick auf den Namen der Byline-Komponente verarbeiten.

1. Geben Sie die folgende Methode in die **Console** ein:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Die obige Methode pusht einen Ereignis-Listener auf die Datenschicht, um auf das `cmp:click`-Ereignis zu warten, und ruft `bylineClickHandler` auf.

   >[!CAUTION]
   >
   > Es ist wichtig **nicht**, den Browser während dieser Übung zu aktualisieren. Andernfalls geht das Konsolen-JavaScript verloren.

1. Klicken Sie im Browser bei geöffneter **Console** auf den Namen des Autors in der Komponente &quot;Byline&quot;:

   ![Byline-Komponente angeklickt](assets/adobe-client-data-layer/byline-component-clicked.png)

   Sie sollten die Konsolenmeldung `Byline Clicked!` und den Namen der Byline sehen.

   Das `cmp:click`-Ereignis ist am einfachsten einzubinden. Für komplexere Komponenten und zur Verfolgung anderer Verhaltensweisen ist es möglich, benutzerdefiniertes JavaScript hinzuzufügen, um neue Ereignisse hinzuzufügen und zu registrieren. Ein gutes Beispiel ist die Karussellkomponente, bei der jedes Umschalten einer Folie ein `cmp:show`-Ereignis Trigger wird. Weitere Informationen finden Sie im [Quellcode](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Verwenden des DataLayerBuilder-Dienstprogramms {#data-layer-builder}

Wenn das Sling-Modell zuvor im Kapitel [aktualisiert](#sling-model) war, haben wir uns dafür entschieden, die JSON-Zeichenfolge mithilfe eines `HashMap` zu erstellen und jede der Eigenschaften manuell festzulegen. Diese Methode funktioniert in Bezug auf kleine einmalige Komponenten einwandfrei. Bei Komponenten, die die AEM Kernkomponenten erweitern, kann dies jedoch zu viel zusätzlichem Code führen.

Eine Dienstprogrammklasse, `DataLayerBuilder`, ist vorhanden, um den Großteil der Schwerarbeit zu erledigen. Dadurch können Implementierungen nur die Eigenschaften erweitern, die sie benötigen. Aktualisieren wir das Sling-Modell, um das `DataLayerBuilder` zu verwenden.

1. Kehren Sie zur IDE zurück und navigieren Sie zum Modul `core` .
1. Öffnen Sie die Datei `Byline.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Ändern Sie die `getData()`-Methode, um einen Typ von `ComponentData` zurückzugeben.

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

   `ComponentData` ist ein Objekt, das von AEM Kernkomponenten bereitgestellt wird. Dies führt wie im vorherigen Beispiel zu einer JSON-Zeichenfolge, führt aber auch viele zusätzliche Arbeiten durch.

1. Öffnen Sie die Datei `BylineImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Fügen Sie die folgenden Importanweisungen hinzu:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Ersetzen Sie die `getData()`-Methode durch Folgendes:

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

   Die Byline-Komponente verwendet Teile der Bild-Kernkomponente erneut, um ein Bild anzuzeigen, das den Autor darstellt. Im obigen Ausschnitt wird [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) verwendet, um die Datenschicht der Komponente `Image` zu erweitern. Dadurch wird das JSON-Objekt mit allen Daten zum verwendeten Bild vorausgefüllt. Es erfüllt auch einige der Routinefunktionen wie das Festlegen der `@type` und der eindeutigen Kennung der Komponente. Beachten Sie, dass die Methode wirklich klein ist!

   Die einzige Eigenschaft erweiterte die `withTitle` , die durch den Wert `getName()` ersetzt wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und stellen Sie mithilfe Ihrer Maven-Fähigkeiten nur das Modul `core` bereit:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Kehren Sie zur IDE zurück und öffnen Sie die Datei `byline.html` unter `ui.apps` .
1. Aktualisieren Sie die HTL, um `byline.data.json` zum Ausfüllen des Attributs `data-cmp-data-layer` zu verwenden:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Denken Sie daran, dass wir jetzt ein Objekt vom Typ `ComponentData` zurückgeben. Dieses Objekt enthält eine Getter-Methode `getJson()`, die zum Ausfüllen des Attributs `data-cmp-data-layer` verwendet wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und stellen Sie mithilfe Ihrer Maven-Fähigkeiten nur das Modul `ui.apps` bereit:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Öffnen Sie die Entwicklertools des Browsers und geben Sie den folgenden Befehl in die **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort unter `component` , um die Instanz der Komponente `byline` zu suchen:

   ![Byline-Datenschicht aktualisiert](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   Beachten Sie, dass es jetzt ein `image`-Objekt innerhalb des `byline`-Komponenteneintrags gibt. Dies enthält viel mehr Informationen über das Asset im DAM. Beachten Sie außerdem, dass die `@type` und die eindeutige ID (in diesem Fall `byline-136073cfcb`) automatisch ausgefüllt wurden, sowie die `repo:modifyDate`, die angibt, wann die Komponente geändert wurde.

## Zusätzliche Beispiele {#additional-examples}

1. Ein weiteres Beispiel für die Erweiterung der Datenschicht kann durch Überprüfen der Komponente `ImageList` in der WKND-Codebasis angezeigt werden:
   * `ImageList.java` - Java-Schnittstelle im - `core` Modul.
   * `ImageListImpl.java` - Sling-Modell im - `core` Modul.
   * `image-list.html` - HTL-Vorlage im  `ui.apps` Modul.

   >[!NOTE]
   >
   > Es ist etwas schwieriger, benutzerdefinierte Eigenschaften wie `occupation` bei der Verwendung von [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) einzuschließen. Wenn Sie jedoch eine Kernkomponente erweitern, die ein Bild oder eine Seite enthält, spart das Dienstprogramm viel Zeit.

   >[!NOTE]
   >
   > Beim Erstellen einer erweiterten Datenschicht für Objekte, die in einer Implementierung wiederverwendet werden, wird empfohlen, die Datenschichtelemente in eigene Datenschichtspezifische Java-Objekte zu extrahieren. Beispielsweise haben die Commerce-Kernkomponenten Schnittstellen für `ProductData` und `CategoryData` hinzugefügt, da diese in einer Commerce-Implementierung für viele Komponenten verwendet werden können. Weitere Informationen finden Sie unter [dem Code im aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) .

## Herzlichen Glückwunsch! {#congratulations}

Sie haben gerade einige Möglichkeiten untersucht, um die Adobe Client-Datenschicht mit AEM Komponenten zu erweitern und anzupassen!

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Datenschichtintegration mit den Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Verwenden der Dokumentation zur Adobe Client-Datenschicht und zu Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/data-layer/overview.html)
