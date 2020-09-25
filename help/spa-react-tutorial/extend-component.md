---
title: Komponente erweitern | Erste Schritte mit dem AEM SPA Editor und React
description: Erfahren Sie, wie Sie eine bestehende Core-Komponente erweitern, um sie mit dem AEM SPA-Editor zu verwenden. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern. Erfahren Sie, wie Sie mit dem Delegierungsparameter Sling-Modelle und Funktionen von Sling Resource Merger erweitern können.
sub-product: Sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '1969'
ht-degree: 2%

---


# Erweitern einer Kernkomponente {#extend-component}

Erfahren Sie, wie Sie eine bestehende Core-Komponente erweitern, um sie mit dem AEM SPA-Editor zu verwenden. Das Verstehen, wie eine vorhandene Komponente erweitert wird, ist eine leistungsstarke Methode zum Anpassen und Erweitern der Funktionen einer AEM SPA Editor-Implementierung.

## Vorgabe

1. Erweitern Sie eine vorhandene Core-Komponente mit zusätzlichen Eigenschaften und Inhalten.
2. Verstehen Sie die Grundlagen der Komponentenvererbung mit der Verwendung von `sling:resourceSuperType`.
3. Erfahren Sie, wie Sie das [Delegationsmuster](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) für Sling-Modelle nutzen können, um vorhandene Logik und Funktionalität wiederzuverwenden.

## Was Sie erstellen

In diesem Kapitel wird eine neue `Card` Komponente erstellt. Die `Card` Komponente erweitert die [Image-Core-Komponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html) und fügt zusätzliche Inhaltsfelder wie Titel und Aktionsaufruf hinzu, um die Rolle eines Teasers für andere Inhalte innerhalb der SPA auszuführen.

![Endgültiges Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In einer realen Implementierung ist es möglicherweise angemessener, einfach die [Teaser-Komponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) zu verwenden und dann die [Image-Core-Komponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html) zu erweitern, um eine `Card` Komponente je nach Projektanforderungen zu erstellen. Es wird immer empfohlen, [Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html) möglichst direkt zu verwenden.

## Voraussetzungen

Überprüfen Sie die erforderlichen Werkzeuge und Anleitungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Lernprogramm über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/extend-component-start
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

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `React/extend-component-solution`.

## Inspect-Erstimplementierung der Karte

Eine anfängliche Kartenkomponente wurde vom Kapitelstartercode bereitgestellt. Inspect als Ausgangspunkt für die Kartenimplementierung.

1. Öffnen Sie in der IDE Ihrer Wahl das `ui.apps` Modul.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card` der `.content.xml` Datei und führen Sie sie aus.

   ![AEM Beginn zur Definition der Kartenkomponente](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Die Eigenschaft `sling:resourceSuperType` zeigt `wknd-spa-react/components/image` an, dass die `Card` Komponente alle Funktionen der WKND-SPA-Bildkomponente übernimmt.

3. Inspect der Datei `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Beachten Sie, dass die `sling:resourceSuperType` Punkte auf `core/wcm/components/image/v2/image`. Dies bedeutet, dass die WKND SPA Image-Komponente alle Funktionen des Core Component Image übernimmt.

   Die Vererbung von Sling-Ressourcen wird auch als [Proxy-Muster](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) bezeichnet und ist ein leistungsstarkes Designmuster, mit dem untergeordnete Komponenten Funktionen übernehmen und nach Bedarf erweitern/überschreiben können. Die Sling-Vererbung unterstützt mehrere Vererbungsstufen, sodass letztendlich die neue `Card` Komponente die Funktionalität des Core Component Image übernimmt.

   Viele Entwicklungsteams streben danach, D.R.Y. (Wiederholen Sie sich nicht!) Die Sling-Vererbung ermöglicht dies mit AEM.

4. Öffnen Sie die Datei unter dem `card` Ordner `_cq_dialog/.content.xml`.

   Diese Datei ist die Definition des Komponentendialogs für die `Card` Komponente. Bei Verwendung der Sling-Vererbung ist es möglich, Funktionen des [Sling Resource Merger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) zu verwenden, um Teile des Dialogs zu überschreiben oder zu erweitern. In diesem Beispiel wurde eine neue Registerkarte zum Dialog hinzugefügt, um zusätzliche Daten von einem Autor zu erfassen, um die Kartenkomponente zu füllen.

   Mit Eigenschaften wie `sling:orderBefore` können Entwickler festlegen, wo neue Registerkarten oder Formularfelder eingefügt werden sollen. In diesem Fall wird die `Text` Registerkarte vor der `asset` Registerkarte eingefügt. Um den Sling Resource Merger vollständig zu nutzen, ist es wichtig, die ursprüngliche Knotenstruktur des Dialogfelds &quot; [Image&quot; zu kennen](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Öffnen Sie die Datei unter dem `card` Ordner `_cq_editConfig.xml`. Diese Datei gibt das Drag &amp; Drop-Verhalten in der Benutzeroberfläche für das AEM Authoring vor. Beim Erweitern der Image-Komponente ist es wichtig, dass der Ressourcentyp mit der Komponente selbst übereinstimmt. Überprüfen Sie den `<parameters>` Knoten:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Bei den meisten Komponenten ist kein `cq:editConfig`Image- und untergeordnetes Image-Element der Image-Komponente erforderlich.

6. Navigieren Sie im IDE-Switch zum `ui.frontend` Modul zu `ui.frontend/src/components/Card`:

   ![Beginn der Antwortkomponente](assets/extend-component/react-card-component-start.png)

7. Inspect die Datei `Card.js`.

   Die Komponente wurde bereits auseinandergesteckt, um der AEM `Card` -Komponente mithilfe der `MapTo` Standardfunktion zuzuordnen.

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect der Methode `get imageContent()`:

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   In diesem Beispiel haben wir uns entschieden, die vorhandene Komponente &quot;React Image&quot;wiederzuverwenden, `Image` indem wir einfach die `this.props` Komponente von der `Card` Komponente übergeben. Später wird die `get bodyContent()` Methode implementiert, um einen Titel, ein Datum und eine Aktionsschaltfläche anzuzeigen.

## Vorlagenrichtlinie aktualisieren

Mit dieser ersten `Card` Implementierung überprüfen Sie die Funktionalität im AEM SPA Editor. Zum Anzeigen der ursprünglichen `Card` Komponente ist ein Update der Vorlagenrichtlinie erforderlich.

1. Stellen Sie den Starter-Code auf einer lokalen Instanz von AEM bereit, falls Sie nicht bereits:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie zur Vorlage der SPA-Seite unter [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue `Card` Komponente als zulässige Komponente hinzuzufügen:

   ![Richtlinie &quot;Layout Container aktualisieren&quot;](assets/extend-component/card-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie die `Card` Komponente als zulässige Komponente:

   ![Kartenkomponente als zulässige Komponente](assets/extend-component/card-component-allowed-layout-container.png)

## Autorenkartenkomponente

Als Nächstes erstellen Sie die `Card` Komponente mit dem AEM SPA-Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. Fügen Sie im `Edit` Modus die `Card` Komponente dem `Layout Container`hinzu:

   ![Neue Komponente einfügen](assets/extend-component/insert-card-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![hinzufügen](assets/extend-component/card-add-image.png)

4. Öffnen Sie das Dialogfeld &quot; `Card` Komponente&quot;und beachten Sie die Hinzufügung einer **Registerkarte &quot;Text** &quot;.
5. Geben Sie auf der Registerkarte &quot; **Text** &quot;die folgenden Werte ein:

   ![Registerkarte &quot;Textkomponente&quot;](assets/extend-component/card-component-text.png)

   **Kartenpfad** : Wählen Sie eine Seite unter der SPA-Homepage aus.

   **CTA-Text** - &quot;Weitere Informationen&quot;

   **Kartentitel** - leer lassen

   **Titel von verknüpfter Seite** abrufen - Kontrollkästchen aktivieren, um true anzugeben.

6. Aktualisieren Sie die Registerkarte &quot; **Asset-Metadaten** &quot;, um Werte für **Alternativtext** und **Beschriftung** hinzuzufügen.

   Derzeit werden nach der Aktualisierung des Dialogfelds keine weiteren Änderungen angezeigt. Um die neuen Felder der React-Komponente zugänglich zu machen, müssen wir das Sling-Modell für die `Card` Komponente aktualisieren.

7. Öffnen Sie eine neue Registerkarte und navigieren Sie zu [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect Sie die unten stehenden Inhaltsknoten, `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid` um den `Card` Komponenteninhalt zu suchen.

   ![Eigenschaften von CRXDE-Lite-Komponenten](assets/extend-component/crxde-lite-properties.png)

   Beachten Sie, dass Eigenschaften `cardPath``ctaText`vom Dialog beibehalten `titleFromPage` werden.

## Kartenauslagerungsmodell aktualisieren

Um die Werte des Komponentendialogs letztendlich der React-Komponente zur Verfügung zu stellen, müssen wir das Sling-Modell aktualisieren, das die JSON für die `Card` Komponente füllt. Wir haben auch die Möglichkeit, zwei Teile der Geschäftslogik umzusetzen:

* Wenn `titleFromPage` &quot; **true**&quot;festgelegt ist, geben Sie den Seitentitel zurück, der `cardPath` andernfalls den Wert des `cardTitle` Textfelds zurückgibt.
* Gibt das Datum der letzten Änderung der Seite zurück, die von `cardPath`angegeben wurde.

Kehren Sie zur IDE Ihrer Wahl zurück und öffnen Sie das `core` Modul.

1. Open the file `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`.

   Beachten Sie, dass die `Card` Schnittstelle derzeit erweitert wird `com.adobe.cq.wcm.core.components.models.Image` und daher alle Methoden der `Image` Schnittstelle erbt. Die `Image` Oberfläche erweitert bereits die `ComponentExporter` Oberfläche, die den Export des Sling-Modells als JSON und die Zuordnung durch den SPA-Editor ermöglicht. Daher müssen wir die `ComponentExporter` Schnittstelle nicht explizit erweitern, wie wir es im Kapitel [Benutzerdefinierte Komponente](custom-component.md)getan haben.

2. hinzufügen Sie die folgenden Methoden an der Schnittstelle:

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Diese Methoden werden über die JSON-Modell-API bereitgestellt und an die React-Komponente übergeben.

3. Öffnen Sie `CardImpl.java`. Dies ist die Implementierung der `Card.java` Schnittstelle. Diese Implementierung wurde bereits teilweise gestört, um das Tutorial zu beschleunigen.  Beachten Sie die Verwendung der Anmerkungen `@Model` und `@Exporter` Anmerkungen, um sicherzustellen, dass das Sling-Modell über den Sling Model Exporter als JSON serialisiert werden kann.

   `CardImpl.java` verwendet auch das [Delegationsmuster für Sling-Modelle](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) , um zu vermeiden, dass die gesamte Logik aus der Image-Core-Komponente umgeschrieben wird.

4. Beachten Sie die folgenden Zeilen:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Die obige Anmerkung instanziiert ein Bildobjekt, das `image` auf der `sling:resourceSuperType` Vererbung der `Card` Komponente basiert.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Es ist dann möglich, das `image` Objekt einfach zu verwenden, um Methoden zu implementieren, die durch die `Image` Schnittstelle definiert werden, ohne die Logik selbst zu schreiben. Diese Technik wird für `getSrc()`, `getAlt()` und `getTitle()`.

5. Implementieren Sie anschließend die `initModel()` Methode, um eine private Variable `cardPage` basierend auf dem Wert von `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Das `@PostConstruct initModel()` wird immer aufgerufen, wenn das Sling-Modell initialisiert wird. Daher ist es eine gute Gelegenheit, Objekte zu initialisieren, die von anderen Methoden im Modell verwendet werden können. Das `pageManager` ist eines von mehreren durch [Java unterstützten globalen Objekten](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) , die Sling Modellen über die `@ScriptVariable` Anmerkung zur Verfügung gestellt werden. Die [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) -Methode nimmt einen Pfad und gibt ein AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) -Objekt oder null zurück, wenn der Pfad nicht auf eine gültige Seite verweist.

   Dadurch wird die `cardPage` Variable initialisiert, die von den anderen neuen Methoden verwendet wird, um Daten über die zugrunde liegende verknüpfte Seite zurückzugeben.

6. Überprüfen Sie die globalen Variablen, die den JCR-Eigenschaften bereits zugeordnet sind und im Autorendialogfeld gespeichert wurden. Die `@ValueMapValue` Anmerkung wird verwendet, um die Zuordnung automatisch durchzuführen.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Diese Variablen werden zur Implementierung der zusätzlichen Methoden für die `Card.java` Schnittstelle verwendet.

7. Implementieren Sie die in der `Card.java` Oberfläche definierten zusätzlichen Methoden:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > Sie können die [fertige CardImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java)Ansicht.

8. Öffnen Sie ein Terminalfenster und stellen Sie mithilfe des Maven- `core` Profils nur die Updates für das `autoInstallBundle` Modul `core` bereit.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Bei Verwendung von [AEM 6.x](overview.md#compatibility) fügen Sie das `classic` Profil hinzu.

9. Ansicht der JSON-Modellantwort unter: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) und suchen Sie nach dem `wknd-spa-react/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-react/us/en/home/page-1.html",
       ":type": "wknd-spa-react/components/card"
   }
   ```

   Beachten Sie, dass das JSON-Modell nach der Aktualisierung der Methoden im `CardImpl` Sling-Modell mit zusätzlichen Schlüssel/Wert-Paaren aktualisiert wird.

## Komponente &quot;React aktualisieren&quot;

Nun, da das JSON-Modell mit neuen Eigenschaften für `ctaLinkURL`, `ctaText`, `cardTitle` und `cardLastModified` wir können die React-Komponente aktualisieren, um diese anzuzeigen.

1. Kehren Sie zur IDE zurück und öffnen Sie das `ui.frontend` Modul. Optional können Sie den Webpack-Dev-Server aus einem neuen Terminalfenster heraus Beginn haben, um die Änderungen in Echtzeit anzuzeigen:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Öffnen Sie `Card.js` bei `ui.frontend/src/components/Card/Card.js`.
3. hinzufügen die Methode `get ctaButton()` zum Rendern des Aktionsaufrufs:

   ```js
   import {Link} from "react-router-dom";
   ...
   
   export default class Card extends Component {
   
       get ctaButton() {
           if(this.props && this.props.ctaLinkURL && this.props.ctaText) {
               return (
                   <div className="Card__action-container">
                       <Link to={this.props.ctaLinkURL} title={this.props.title}
                           className="Card__action-link">
                           {this.props.ctaText}
                       </Link>
                   </div>
               );
           }
   
           return null;
       }
       ...
   }
   ```

4. hinzufügen eine Methode zur Konvertierung `get lastModifiedDisplayDate()` `this.props.cardLastModified` in eine lokalisierte Zeichenfolge, die das Datum darstellt.

   ```js
   export default class Card extends Component {
       ...
       get lastModifiedDisplayDate() {
           const lastModifiedDate = this.props.cardLastModified ? new Date(this.props.cardLastModified) : null;
   
           if (lastModifiedDate) {
               return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

5. Aktualisieren Sie die `get bodyContent()` Anzeige `this.props.cardTitle` und verwenden Sie die in den vorherigen Schritten erstellten Methoden:

   ```js
   export default class Card extends Component {
       ...
       get bodyContent() {
          return (<div class="Card__content">
                       <h2 class="Card__title"> {this.props.cardTitle}
                           <span class="Card__lastmod">
                               {this.lastModifiedDisplayDate}
                           </span>
                       </h2>
                       {this.ctaButton}
               </div>);
       }
       ...
   }
   ```

6. Es wurden bereits Segmentregeln hinzugefügt, `Card.scss` um den Titel, den Aktionsaufruf und das Datum der letzten Änderung festzulegen. Fügen Sie diese Stile hinzu, indem Sie die folgende Zeile oben in der Datei `Card.js` einfügen:

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > Sie können den fertigen Code der [React-Card-Komponente hier](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js)Ansicht haben.

7. Stellen Sie mithilfe von Maven die vollständigen Änderungen an AEM aus dem Stammordner des Projekts bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) , um die aktualisierte Komponente anzuzeigen:

   ![Aktualisierte Kartenkomponente in AEM](assets/extend-component/updated-card-in-aem.png)

9. Sie sollten den vorhandenen Inhalt erneut erstellen können, um eine Seite wie die folgende zu erstellen:

   ![Endgültiges Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gelernt, wie Sie eine AEM Komponente mit dem erweitern und wie Sling-Modelle und Dialoge mit dem JSON-Modell funktionieren.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) Ansicht oder den Code lokal auschecken, indem Sie zur Verzweigung wechseln `React/extend-component-solution`.
