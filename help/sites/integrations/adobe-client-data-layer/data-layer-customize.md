---
title: Anpassen der Adobe Client-Datenschicht mit AEM
description: Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM anpassen. Erfahren Sie, wie Sie mithilfe der von AEM Core-Komponenten bereitgestellten APIs die Datenschicht erweitern und anpassen können.
feature: Adobe Client Data Layer, Core Component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: Integrations
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 5%

---


# Anpassen der Adobe Client-Datenschicht mit AEM Komponenten {#customize-data-layer}

Erfahren Sie, wie Sie die Adobe Client-Datenschicht mit Inhalten aus benutzerdefinierten AEM anpassen. Erfahren Sie, wie Sie mithilfe der von [AEM Kernkomponenten bereitgestellten APIs die Datenschicht erweitern und anpassen können.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)

## Was Sie erstellen werden

![Autorendatenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

In diesem Lernprogramm werden Sie verschiedene Optionen zum Erweitern der Adobe Client Data Layer durch Aktualisieren der WKND [Byline-Komponente](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html) untersuchen. Dies ist eine benutzerdefinierte Komponente und die in diesem Lernprogramm gewonnenen Erfahrungen können auf andere benutzerdefinierte Komponenten angewendet werden.

### Ziele {#objective}

1. Komponentendaten durch Erweitern eines Sling-Modells und einer Komponente HTL in die Datenschicht injizieren
1. Verwenden Sie Core Component-Datenschichtprogramme, um den Aufwand zu reduzieren
1. Verwenden Sie die Datenattribute der Hauptkomponenten, um eine Verknüpfung mit bestehenden Ereignissen der Datenschicht herzustellen.

## Voraussetzungen {#prerequisites}

Eine **lokale Entwicklungs-Umgebung** ist erforderlich, um dieses Lernprogramm abzuschließen. Screenshots und Videos werden mit dem AEM als Cloud Service-SDK erfasst, das auf einem macOS ausgeführt wird. Befehle und Code sind unabhängig vom lokalen Betriebssystem, sofern nicht anders angegeben.

**Neu bei AEM as a Cloud Service?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung mit dem AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) einzurichten.

**Neu zu AEM 6.5?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) einzurichten.

## Herunterladen und Bereitstellen der WKND-Referenz-Website {#set-up-wknd-site}

Dieses Lernprogramm erweitert die Komponente &quot;Byline&quot;auf der WKND-Referenz-Website. Klonen und installieren Sie die WKND-Codebasis auf Ihrer lokalen Umgebung.

1. Beginn einer lokalen QuickStart-Instanz **author** der AEM, die unter [http://localhost:4502](http://localhost:4502) ausgeführt wird.
1. Öffnen Sie ein Terminalfenster und klonen Sie die WKND-Codebasis mithilfe von Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Stellen Sie die WKND-Codebasis auf einer lokalen Instanz von AEM bereit:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 und das neueste Service Pack verwenden, fügen Sie dem Maven-Befehl das Profil `classic` hinzu:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Öffnen Sie ein neues Browserfenster und melden Sie sich bei AEM an. Öffnen Sie eine **Zeitschrift**-Seite wie: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Autorenkomponente auf Seite](assets/adobe-client-data-layer/byline-component-onpage.png)

   Sie sollten ein Beispiel der Byline-Komponente sehen, die der Seite als Teil eines Erlebnisfragments hinzugefügt wurde. Sie können das Erlebnisfragment unter [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html) Ansicht haben.
1. Öffnen Sie Ihre Entwicklerwerkzeuge und geben Sie in **Console** den folgenden Befehl ein:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect Sie die Antwort, um den aktuellen Status der Datenschicht auf einer AEM Site anzuzeigen. Sie sollten Informationen zur Seite und zu den einzelnen Komponenten anzeigen.

   ![Adobe Data Layer Response](assets/data-layer-state-response.png)

   Beachten Sie, dass die Komponente &quot;Byline&quot;nicht in der Datenschicht aufgeführt ist.

## Byline-Sling-Modell {#sling-model} aktualisieren

Um Daten über die Komponente in die Datenschicht einzufügen, müssen wir zunächst das Sling-Modell der Komponente aktualisieren. Aktualisieren Sie anschließend die Java-Schnittstelle und die Sling-Modellimplementierung von Byline, um eine neue Methode `getData()` hinzuzufügen. Diese Methode enthält die Eigenschaften, die wir in die Datenschicht injizieren möchten.

1. Öffnen Sie in der IDE Ihrer Wahl das Projekt `aem-guides-wknd`. Navigieren Sie zum Modul `core`.
1. Öffnen Sie die Datei `Byline.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Byte-Java-Schnittstelle](assets/adobe-client-data-layer/byline-java-interface.png)

1. hinzufügen einer neuen Methode an der Schnittstelle:

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

1. hinzufügen die folgenden Importanweisungen am Anfang der Datei:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Die `fasterxml.jackson`-APIs werden verwendet, um die Daten zu serialisieren, die wir als JSON bereitstellen möchten. Die `ComponentUtils` AEM Kernkomponenten werden verwendet, um zu prüfen, ob die Datenschicht aktiviert ist.

1. hinzufügen Sie die nicht implementierte Methode `getData()` auf `BylineImple.java`:

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

   In der obigen Methode wird ein neuer `HashMap` verwendet, um die Eigenschaften zu erfassen, die wir als JSON verfügbar machen möchten. Beachten Sie, dass vorhandene Methoden wie `getName()` und `getOccupations()` verwendet werden. `@type` stellt den eindeutigen Ressourcentyp der Komponente dar. Auf diese Weise kann ein Client Ereignis und/oder Trigger anhand des Komponententyps leicht identifizieren.

   `ObjectMapper` wird verwendet, um die Eigenschaften zu serialisieren und eine JSON-Zeichenfolge zurückzugeben. Diese JSON-Zeichenfolge kann dann in die Datenschicht eingefügt werden.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `core`-Modul mit Ihren Maven-Fähigkeiten:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Aktualisieren der Autorenzeile HTL {#htl}

Aktualisieren Sie anschließend `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTML (HTML Template Language) ist die Vorlage, die zum Rendern der HTML-Datei der Komponente verwendet wird.

Für jede AEM Komponente wird ein spezielles Datenattribut `data-cmp-data-layer` verwendet, um ihre Datenschicht anzuzeigen.  JavaScript, das von AEM Core Components bereitgestellt wird, sucht nach diesem Datenattribut, dessen Wert mit der JSON-Zeichenfolge gefüllt wird, die von der `getData()`-Methode des Byline-Sling-Modells zurückgegeben wird, und injiziert die Werte in die Adobe Client-Datenschicht.

1. Öffnen Sie in der IDE das Projekt `aem-guides-wknd`. Navigieren Sie zum Modul `ui.apps`.
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byte-HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Aktualisieren Sie `byline.html`, um das `data-cmp-data-layer`-Attribut einzuschließen:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   Der Wert von `data-cmp-data-layer` wurde auf `"${byline.data}"` gesetzt, wobei `byline` das Sling-Modell ist, das zuvor aktualisiert wurde. `.data` ist die Standardnotation für den Aufruf einer Java Getter-Methode in HTML, die in der vorherigen Übung  `getData()` implementiert wurde.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `ui.apps`-Modul mit Ihren Maven-Fähigkeiten:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit einer Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Öffnen Sie die Entwicklerwerkzeuge und überprüfen Sie die HTML-Quelle der Seite für die Komponente &quot;Byline&quot;:

   ![Autorendatenschicht](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Sie sollten sehen, dass `data-cmp-data-layer` mit der JSON-Zeichenfolge aus dem Sling-Modell gefüllt wurde.

1. Öffnen Sie die Entwicklerwerkzeuge des Browsers und geben Sie den folgenden Befehl in **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort unter `component`, um die Instanz der Komponente `byline` zu der Datenschicht zu suchen:

   ![Autorenbereich der Datenschicht](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Es sollte ein Eintrag wie der folgende angezeigt werden:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Beachten Sie, dass es sich bei den offen gelegten Eigenschaften um dieselben handelt, die im Sling-Modell unter `HashMap` hinzugefügt werden.

## hinzufügen eines Click-Ereignisses {#click-event}

Die Adobe Client-Datenschicht wird vom Ereignis gesteuert und eines der häufigsten Ereignis für den Trigger einer Aktion ist das `cmp:click`-Ereignis. Die AEM Core-Komponenten erleichtern die Registrierung der Komponente mithilfe des Datenelements: `data-cmp-clickable`.

Klickbare Elemente sind in der Regel eine CTA-Schaltfläche oder ein Navigationslink. Leider hat die Byline-Komponente keines davon, aber wir werden sie auf jeden Fall registrieren, da dies für andere benutzerdefinierte Komponenten üblich sein könnte.

1. Öffnen Sie das `ui.apps`-Modul in Ihrer IDE
1. Öffnen Sie die Datei `byline.html` unter `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Aktualisieren Sie `byline.html`, um das `data-cmp-clickable`-Attribut im **name**-Element der Autorenzeile einzuschließen:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Öffnen Sie ein neues Terminal. Erstellen und implementieren Sie nur das `ui.apps`-Modul mit Ihren Maven-Fähigkeiten:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Um unser Ereignis zu testen, fügen wir manuell JavaScript mit der Entwicklerkonsole hinzu. Eine Anleitung dazu finden Sie unter [Verwenden der Adobe Client-Datenschicht mit AEM Kernkomponenten](data-layer-overview.md).

1. Öffnen Sie die Entwicklerwerkzeuge des Browsers und geben Sie die folgende Methode in **Console** ein:

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

1. Geben Sie in **Console** die folgende Methode ein:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   Die obige Methode schiebt einen Ereignis-Listener auf die Datenschicht, um auf das `cmp:click`-Ereignis zu warten, und ruft `bylineClickHandler` auf.

   >[!CAUTION]
   >
   > Es ist wichtig, dass **nicht** den Browser während dieser Übung aktualisiert wird, da sonst das Konsolen-JavaScript verloren geht.

1. Klicken Sie im Browser, während **Console** geöffnet ist, auf den Namen des Autors in der Komponente &quot;Byline&quot;:

   ![Auf Autorenkomponente geklickt](assets/adobe-client-data-layer/byline-component-clicked.png)

   Sie sollten die Konsolenmeldung `Byline Clicked!` und den Autorenzeilennamen sehen.

   Das `cmp:click`-Ereignis ist am einfachsten einzubinden. Für komplexere Komponenten und zur Verfolgung anderer Verhaltensweisen ist es möglich, benutzerspezifische JavaScript hinzuzufügen, um neue Ereignis hinzuzufügen und zu registrieren. Ein gutes Beispiel ist die Karussell-Komponente, die ein `cmp:show`-Ereignis Trigger, wenn eine Folie umgeschaltet wird. Weitere Informationen finden Sie unter [Quellcode](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Verwenden Sie das DataLayerBuilder-Dienstprogramm {#data-layer-builder}

Als das Sling-Modell zu einem früheren Zeitpunkt im Kapitel [aktualisiert wurde, haben wir uns dafür entschieden, die JSON-Zeichenfolge mit einem `HashMap` zu erstellen und die einzelnen Eigenschaften manuell festzulegen. ](#sling-model) Diese Methode eignet sich hervorragend für kleine Einmal-Komponenten. Bei Komponenten, die die AEM Kernkomponenten erweitern, kann dies jedoch zu einer Menge zusätzlichen Codes führen.

Eine Dienstprogrammklasse, `DataLayerBuilder`, existiert, um die meisten Schwerstarbeit zu leisten. Dadurch können Implementierungen nur die Eigenschaften erweitern, die sie benötigen. Aktualisieren wir das Sling-Modell, um das `DataLayerBuilder` zu verwenden.

1. Kehren Sie zur IDE zurück und navigieren Sie zum Modul `core`.
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

   `ComponentData` ist ein Objekt, das von AEM Kernkomponenten bereitgestellt wird. Es führt zu einer JSON-Zeichenfolge, wie im vorherigen Beispiel, aber auch zu einer Menge zusätzlicher Arbeit.

1. Öffnen Sie die Datei `BylineImpl.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. hinzufügen die folgenden Importanweisungen:

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

   Die Autorenkomponente verwendet Teile der Image-Core-Komponente erneut, um ein Bild anzuzeigen, das den Autor darstellt. Im obigen Codeausschnitt wird der [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) verwendet, um die Datenschicht der `Image`-Komponente zu erweitern. Dadurch wird das JSON-Objekt mit allen Daten zum verwendeten Bild vorausgefüllt. Es führt außerdem einige Routinefunktionen aus, wie z. B. das Festlegen von `@type` und die eindeutige Kennung der Komponente. Beachten Sie, dass die Methode wirklich klein ist!

   Die einzige Eigenschaft erweiterte die Eigenschaft `withTitle`, die durch den Wert `getName()` ersetzt wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `core`-Modul mit Ihren Maven-Fähigkeiten:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Kehren Sie zur IDE zurück und öffnen Sie die Datei `byline.html` unter `ui.apps`
1. Aktualisieren Sie das HTML, um `byline.data.json` zum Füllen des Attributs `data-cmp-data-layer` zu verwenden:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Denken Sie daran, dass wir jetzt ein Objekt vom Typ `ComponentData` zurückgeben. Dieses Objekt enthält die Getter-Methode `getJson()`, mit der das `data-cmp-data-layer`-Attribut gefüllt wird.

1. Öffnen Sie ein Terminal-Fenster. Erstellen und implementieren Sie nur das `ui.apps`-Modul mit Ihren Maven-Fähigkeiten:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Kehren Sie zum Browser zurück und öffnen Sie die Seite mit der hinzugefügten Byline-Komponente erneut: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Öffnen Sie die Entwicklerwerkzeuge des Browsers und geben Sie den folgenden Befehl in **Console** ein:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigieren Sie unter der Antwort unter `component`, um die Instanz der Komponente `byline` zu suchen:

   ![Autorendatenschicht aktualisiert](assets/adobe-client-data-layer/byline-data-layer-builder.png)

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

   Beachten Sie, dass es nun ein `image`-Objekt innerhalb des `byline`-Komponenteneintrags gibt. Das enthält viele weitere Informationen zum Asset im DAM. Beachten Sie auch, dass die Werte `@type` und die eindeutige ID (in diesem Fall `byline-136073cfcb`) automatisch ausgefüllt wurden, sowie die Zeichen `repo:modifyDate`, die angeben, wann die Komponente geändert wurde.

## Weitere Beispiele {#additional-examples}

1. Ein weiteres Beispiel zum Erweitern der Datenschicht kann durch Prüfen der Komponente `ImageList` in der WKND-Codebasis angezeigt werden:
   * `ImageList.java` - Java-Schnittstelle im  `core` Modul.
   * `ImageListImpl.java` - Sling-Modell im  `core` Modul.
   * `image-list.html` - HTML-Vorlage im  `ui.apps` Modul.

   >[!NOTE]
   >
   > Es ist etwas schwieriger, benutzerdefinierte Eigenschaften wie `occupation` einzuschließen, wenn Sie [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) verwenden. Wenn Sie jedoch eine Core-Komponente erweitern, die ein Bild oder eine Seite enthält, spart das Dienstprogramm viel Zeit.

   >[!NOTE]
   >
   > Wenn Sie eine erweiterte Datenschicht für Objekte erstellen, die während der gesamten Implementierung wiederverwendet werden, empfiehlt es sich, die Datenschichtelemente in eigene Datenschichtspezifische Java-Objekte zu extrahieren. Die Commerce-Kernkomponenten haben beispielsweise Schnittstellen für `ProductData` und `CategoryData` hinzugefügt, da diese in einer Commerce-Implementierung für viele Komponenten verwendet werden können. Weitere Informationen finden Sie unter [Code im aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer).

## Herzlichen Glückwunsch! {#congratulations}

Sie haben soeben einige Möglichkeiten erforscht, um die Adobe Client Data Layer mit AEM Komponenten zu erweitern und anzupassen!

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zur Adobe Client-Datenschicht](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Datenschichtintegration mit den Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Verwenden der Adobe Client-Datenschicht und der Dokumentation der Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/data-layer/overview.html)
