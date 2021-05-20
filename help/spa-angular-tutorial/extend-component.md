---
title: Komponenten erweitern | Erste Schritte mit dem AEM SPA Editor und Angular
description: Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM SPA Editor zu verwenden. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung zu erweitern. Erfahren Sie, wie Sie mit dem Delegationsparameter Sling-Modelle und Funktionen von Sling Resource Merger erweitern können.
sub-product: Sites
feature: SPA Editor, Kernkomponenten
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 4%

---


# Erweitern einer Kernkomponente {#extend-component}

Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM SPA Editor zu verwenden. Das Verständnis, wie eine vorhandene Komponente erweitert wird, ist eine leistungsstarke Methode, um die Funktionen einer AEM SPA Editor-Implementierung anzupassen und zu erweitern.

## Vorgabe

1. Erweitern einer vorhandenen Kernkomponente mit zusätzlichen Eigenschaften und Inhalten.
2. Verstehen Sie die Grundlagen der Komponentenvererbung mit der Verwendung von `sling:resourceSuperType`.
3. Erfahren Sie, wie Sie das [Delegationsmuster](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) für Sling-Modelle nutzen können, um vorhandene Logik und Funktionalität wiederzuverwenden.

## Was Sie erstellen werden

In diesem Kapitel wird eine neue `Card` -Komponente erstellt. Die Komponente `Card` erweitert die [Bild-Kernkomponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/image.html) und fügt zusätzliche Inhaltsfelder wie einen Titel und eine Schaltfläche &quot;Aktionsaufruf&quot;hinzu, um die Rolle eines Teasers für andere Inhalte in der SPA auszuführen.

![Abschließendes Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> In einer realen Implementierung ist es möglicherweise sinnvoller, einfach die [Teaser-Komponente](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/components/teaser.html) zu verwenden und dann die [Bild-Kernkomponente](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) zu erweitern, um eine `Card`-Komponente entsprechend den Projektanforderungen zu erstellen. Es wird immer empfohlen, [Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html) nach Möglichkeit direkt zu verwenden.

## Voraussetzungen

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Code abrufen

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
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

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/extend-component-solution` wechseln.

## Inspect-ErstKartenimplementierung

Eine anfängliche Kartenkomponente wurde vom Code des Kapitelstarters bereitgestellt. Inspect als Ausgangspunkt für die Kartenimplementierung.

1. Öffnen Sie in der IDE Ihrer Wahl das Modul `ui.apps` .
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` und zeigen Sie die Datei `.content.xml` an.

   ![AEM der Kartenkomponente](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Die Eigenschaft `sling:resourceSuperType` verweist auf `wknd-spa-angular/components/image` und gibt an, dass die `Card`-Komponente alle Funktionen von der WKND-SPA-Bildkomponente übernimmt.

3. Prüfen Sie die Datei `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Beachten Sie, dass `sling:resourceSuperType` auf `core/wcm/components/image/v2/image` verweist. Dies bedeutet, dass die WKND SPA Image-Komponente alle Funktionen vom Kernkomponentenbild übernimmt.

   Die Vererbung der Sling-Ressource wird auch als [Proxy-Muster](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) Sling-Ressource bezeichnet und ist ein leistungsstarkes Designmuster, mit dem untergeordnete Komponenten Funktionen übernehmen und bei Bedarf das Verhalten erweitern/überschreiben können. Die Sling-Vererbung unterstützt mehrere Vererbungsstufen, sodass letztendlich die neue Komponente `Card` die Funktionalität der Kernkomponente &quot;Bild&quot;übernimmt.

   Viele Entwicklungsteams streben danach, D.R.Y. zu sein (wiederholen Sie sich nicht selbst). Die Sling-Vererbung ermöglicht dies mit AEM.

4. Öffnen Sie unter dem Ordner `card` die Datei `_cq_dialog/.content.xml`.

   Diese Datei ist die Definition des Komponentendialogfelds für die Komponente `Card` . Bei Verwendung der Sling-Vererbung ist es möglich, Funktionen des [Sling Resource Merger](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) zu verwenden, um Teile des Dialogfelds zu überschreiben oder zu erweitern. In diesem Beispiel wurde dem Dialogfeld eine neue Registerkarte hinzugefügt, um zusätzliche Daten von einem Autor zu erfassen und die Kartenkomponente zu füllen.

   Mit Eigenschaften wie `sling:orderBefore` können Entwickler festlegen, wo neue Registerkarten oder Formularfelder eingefügt werden sollen. In diesem Fall wird die Registerkarte `Text` vor der Registerkarte `asset` eingefügt. Um den Sling Resource Merger vollständig zu nutzen, ist es wichtig, die ursprüngliche Dialogfeldknotenstruktur für das [Bild-Komponentendialogfeld](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml) zu kennen.

5. Öffnen Sie unter dem Ordner `card` die Datei `_cq_editConfig.xml`. Diese Datei bestimmt das Drag-and-Drop-Verhalten in der AEM Authoring-Benutzeroberfläche. Beim Erweitern der Bildkomponente ist es wichtig, dass der Ressourcentyp mit der Komponente selbst übereinstimmt. Überprüfen Sie den Knoten `<parameters>`:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Die meisten Komponenten erfordern keinen `cq:editConfig`. Die Bild- und untergeordneten untergeordneten Elemente der Bildkomponente sind Ausnahmen.

6. Wechseln Sie in der IDE zum Modul `ui.frontend` und navigieren Sie zu `ui.frontend/src/app/components/card`:

   ![Start der angular-Komponente](assets/extend-component/angular-card-component-start.png)

7. Prüfen Sie die Datei `card.component.ts`.

   Die Komponente wurde bereits mit der Standardfunktion `MapTo` herausgestochen, um sie der AEM `Card`-Komponente zuzuordnen.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Überprüfen Sie die drei Parameter `@Input` in der Klasse für `src`, `alt` und `title`. Dies sind erwartete JSON-Werte aus der AEM Komponente, die der Angular-Komponente zugeordnet wird.

8. Öffnen Sie die Datei `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In diesem Beispiel haben wir beschlossen, die vorhandene Angular-Bildkomponente `app-image` wiederzuverwenden, indem einfach die `@Input` -Parameter von `card.component.ts` übergeben werden. Später im Tutorial werden weitere Eigenschaften hinzugefügt und angezeigt.

## Vorlagenrichtlinie aktualisieren

Mit dieser ersten `Card`-Implementierung überprüfen Sie die Funktionalität im AEM SPA Editor. Um die ursprüngliche Komponente `Card` anzuzeigen, ist ein Update der Vorlagenrichtlinie erforderlich.

1. Stellen Sie den Starter-Code auf einer lokalen Instanz von AEM bereit, falls Sie dies noch nicht getan haben:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie zur SPA Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue Komponente `Card` als zulässige Komponente hinzuzufügen:

   ![Aktualisieren der Layout-Container-Richtlinie](assets/extend-component/card-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beachten Sie die Komponente `Card` als zulässige Komponente:

   ![Kartenkomponente als zulässige Komponente](assets/extend-component/card-component-allowed-layout-container.png)

## Autorenkartenkomponente

Als Nächstes erstellen Sie die Komponente `Card` mit dem AEM SPA Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Fügen Sie im Modus `Edit` die Komponente `Card` zur Komponente `Layout Container` hinzu:

   ![Neue Komponente einfügen](assets/extend-component/insert-custom-component.png)

3. Ziehen Sie ein Bild aus der Asset-Suche auf die Komponente `Card`:

   ![Bild hinzufügen](assets/extend-component/card-add-image.png)

4. Öffnen Sie das Komponentendialogfeld `Card` und beachten Sie das Hinzufügen einer Registerkarte **Text** .
5. Geben Sie die folgenden Werte auf der Registerkarte **Text** ein:

   ![Registerkarte &quot;Textkomponente&quot;](assets/extend-component/card-component-text.png)

   **Kartenpfad** : Wählen Sie eine Seite unter der SPA Homepage aus.

   **CTA-Text**  - &quot;mehr dazu&quot;

   **Kartentitel**  - leer lassen

   **Titel von verknüpfter Seite abrufen**  - Aktivieren Sie das Kontrollkästchen, um &quot;true&quot;anzugeben.

6. Aktualisieren Sie die Registerkarte **Asset-Metadaten**, um Werte für **Alternativtext** und **Beschriftung** hinzuzufügen.

   Derzeit werden nach der Aktualisierung des Dialogfelds keine weiteren Änderungen angezeigt. Um die neuen Felder der Angular-Komponente verfügbar zu machen, müssen wir das Sling-Modell für die Komponente `Card` aktualisieren.

7. Öffnen Sie eine neue Registerkarte und navigieren Sie zu [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect Sie die Inhaltsknoten unter `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` , um den `Card`-Komponenteninhalt zu finden.

   ![Komponenteneigenschaften von CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Beachten Sie, dass die Eigenschaften `cardPath`, `ctaText`, `titleFromPage` vom Dialogfeld beibehalten werden.

## Aktualisieren des Karten-Sling-Modells

Um die Werte aus dem Komponentendialogfeld letztendlich für die Angular-Komponente verfügbar zu machen, müssen wir das Sling-Modell aktualisieren, das die JSON für die Komponente `Card` füllt. Wir haben auch die Möglichkeit, zwei Elemente der Geschäftslogik zu implementieren:

* Wenn `titleFromPage` auf **true** gesetzt ist, geben Sie den Seitentitel zurück, der von `cardPath` angegeben wird. Andernfalls wird der Wert von `cardTitle` im Textfeld zurückgegeben.
* Gibt das Datum der letzten Änderung der Seite zurück, die von `cardPath` angegeben wurde.

Kehren Sie zur IDE Ihrer Wahl zurück und öffnen Sie das Modul `core` .

1. Öffnen Sie die Datei `Card.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Beachten Sie, dass die `Card`-Schnittstelle derzeit `com.adobe.cq.wcm.core.components.models.Image` erweitert und daher alle Methoden der `Image`-Schnittstelle übernimmt. Die `Image`-Schnittstelle erweitert bereits die `ComponentExporter`-Schnittstelle, über die das Sling-Modell als JSON exportiert und vom SPA Editor zugeordnet werden kann. Daher müssen wir die `ComponentExporter`-Schnittstelle nicht explizit erweitern, wie dies im [Kapitel der benutzerdefinierten Komponente](custom-component.md) der Fall war.

2. Fügen Sie der -Oberfläche die folgenden Methoden hinzu:

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

   Diese Methoden werden über die JSON-Modell-API verfügbar gemacht und an die Angular-Komponente übergeben.

3. Öffnen Sie `CardImpl.java`. Dies ist die Implementierung der `Card.java`-Schnittstelle. Diese Implementierung wurde bereits teilweise gestoppt, um das Tutorial zu beschleunigen.  Beachten Sie die Verwendung der Anmerkungen `@Model` und `@Exporter` , um sicherzustellen, dass das Sling-Modell über den Sling Model Exporter als JSON serialisiert werden kann.

   `CardImpl.java` verwendet außerdem das  [Delegationsmuster für Sling-](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Modelle, um zu vermeiden, dass die gesamte Logik aus der Image-Kernkomponente umgeschrieben wird.

4. Beachten Sie die folgenden Zeilen:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Die obige Anmerkung instanziiert ein Bildobjekt mit dem Namen `image` basierend auf der `sling:resourceSuperType`-Vererbung der `Card`-Komponente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Dann ist es möglich, einfach das `image`-Objekt zu verwenden, um Methoden zu implementieren, die von der `Image`-Schnittstelle definiert werden, ohne die Logik selbst schreiben zu müssen. Diese Technik wird für `getSrc()`, `getAlt()` und `getTitle()` verwendet.

5. Implementieren Sie anschließend die `initModel()`-Methode, um eine private Variable `cardPage` basierend auf dem Wert von `cardPath` zu initiieren.

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Das `@PostConstruct initModel()` wird immer aufgerufen, wenn das Sling-Modell initialisiert wird. Daher ist es eine gute Gelegenheit, Objekte zu initialisieren, die von anderen Methoden im Modell verwendet werden können. Das `pageManager` ist eines von mehreren [Java-unterstützten globalen Objekten](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects), die Sling-Modellen über die Anmerkung `@ScriptVariable` zur Verfügung gestellt werden. Die [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-)-Methode nimmt einen Pfad an und gibt ein AEM [Page](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html)-Objekt oder null zurück, wenn der Pfad nicht auf eine gültige Seite verweist.

   Dadurch wird die Variable `cardPage` initialisiert, die von den anderen neuen Methoden verwendet wird, um Daten über die zugrunde liegende verknüpfte Seite zurückzugeben.

6. Überprüfen Sie die globalen Variablen, die den JCR-Eigenschaften bereits zugeordnet sind, und speichern Sie das Autorendialogfeld. Die Anmerkung `@ValueMapValue` wird verwendet, um automatisch die Zuordnung durchzuführen.

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

   Diese Variablen werden verwendet, um die zusätzlichen Methoden für die `Card.java`-Schnittstelle zu implementieren.

7. Implementieren Sie die in der `Card.java`-Schnittstelle definierten zusätzlichen Methoden:

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
   > Sie können die [fertige CardImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java) anzeigen.

8. Öffnen Sie ein Terminal-Fenster und stellen Sie mithilfe des Maven-Profils `autoInstallBundle` aus dem Verzeichnis `core` nur die Updates für das Modul `core` bereit.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu.

9. Zeigen Sie die JSON-Modellantwort unter: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) und suchen Sie nach `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   Beachten Sie, dass das JSON-Modell mit zusätzlichen Schlüssel/Wert-Paaren aktualisiert wird, nachdem die Methoden im Sling-Modell `CardImpl` aktualisiert wurden.

## Angular-Komponente aktualisieren

Nachdem das JSON-Modell mit neuen Eigenschaften für `ctaLinkURL`, `ctaText`, `cardTitle` und `cardLastModified` gefüllt ist, können wir die Angular-Komponente aktualisieren, um diese anzuzeigen.

1. Kehren Sie zur IDE zurück und öffnen Sie das Modul `ui.frontend` . Optional können Sie den Webpack Development Server über ein neues Terminal-Fenster starten, um die Änderungen in Echtzeit anzuzeigen:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Öffnen Sie `card.component.ts` unter `ui.frontend/src/app/components/card/card.component.ts`. Fügen Sie die zusätzlichen `@Input` -Anmerkungen hinzu, um das neue Modell zu erfassen:

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. Fügen Sie Methoden hinzu, um zu überprüfen, ob der Aktionsaufruf bereit ist, und um anhand der `cardLastModified` -Eingabe eine Datums-/Uhrzeit-Zeichenfolge zurückzugeben:

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. Öffnen Sie `card.component.html` und fügen Sie das folgende Markup hinzu, um den Titel, den Aktionsaufruf und das Datum der letzten Änderung anzuzeigen:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   Unter `card.component.scss` wurden bereits Segmentregeln hinzugefügt, um den Titel, den Aktionsaufruf und das Datum der letzten Änderung zu gestalten.

   >[!NOTE]
   >
   > Sie können den fertigen [Angular-Kartenkomponentencode hier](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card) anzeigen.

5. Stellen Sie mithilfe von Maven die vollständigen Änderungen an AEM aus dem Stammverzeichnis des Projekts bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) , um die aktualisierte Komponente anzuzeigen:

   ![Aktualisierte Kartenkomponente in AEM](assets/extend-component/updated-card-in-aem.png)

7. Sie sollten den vorhandenen Inhalt erneut verfassen können, um eine Seite ähnlich der folgenden zu erstellen:

   ![Abschließendes Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie eine AEM-Komponente mithilfe der erweitert wird und wie Sling-Modelle und -Dialogfelder mit dem JSON-Modell funktionieren.

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `Angular/extend-component-solution` wechseln.
