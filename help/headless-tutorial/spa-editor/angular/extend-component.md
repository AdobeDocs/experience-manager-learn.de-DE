---
title: Erweitern einer Komponente | Erste Schritte mit dem AEM-SPA-Editor und Angular
description: Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM-SPA-Editor zu verwenden. Das Verständnis, wie Eigenschaften und Inhalte zu einer vorhandenen Komponente hinzugefügt werden, liefert eine wirkungsvolle Methode, um die Funktionen einer AEM-SPA-Editor-Implementierung zu erweitern. Erfahren Sie, wie Sie mit dem Delegationsmuster Sling-Modelle und Sling Resource Merger-Funktionen erweitern können.
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
duration: 563
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 100%

---

# Erweitern einer Kernkomponente {#extend-component}

Erfahren Sie, wie Sie eine vorhandene Kernkomponente erweitern, um sie mit dem AEM-SPA-Editor zu verwenden. Das Verständnis, wie eine vorhandene Komponente erweitert wird, liefert eine wirkungsvolle Methode, um die Funktionen einer AEM-SPA-Editor-Implementierung anzupassen und zu erweitern.

## Ziel

1. Erweitern einer vorhandenen Kernkomponente mit zusätzlichen Eigenschaften und Inhalten.
2. Verstehen der Grundlagen zur Komponentenvererbung bei Verwendung von `sling:resourceSuperType`.
3. Erfahren Sie, wie mit dem [Delegationsmuster](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) für Sling-Modelle vorhandene Logik und Funktionalität wiederverwendet werden kann.

## Was Sie erstellen werden

In diesem Kapitel wird eine neue `Card`-Komponente erstellt. Die `Card`-Komponente erweitert die [Bild-Kernkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=de) und fügt zusätzliche Inhaltsfelder wie einen Titel und eine Aktionsaufruf-Schaltfläche hinzu, um die Rolle eines Teasers für andere Inhalte in der SPA auszuführen.

![Abschließendes Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Bei einer echten Implementierung kann es je nach Projektanforderungen sinnvoller sein, einfach die [Teaser-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html?lang=de) zu verwenden und nicht die [Bild-Kernkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=de) zu erweitern, um eine `Card`-Komponente zu erstellen. Es wird immer empfohlen, [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) direkt zu verwenden, soweit möglich.

## Voraussetzungen

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

### Abrufen des Codes

1. Laden Sie den Ausgangspunkt für dieses Tutorial über Git herunter:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Stellen Sie die Code-Basis mithilfe von Maven in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Installieren Sie das fertige Paket für die herkömmliche [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). Die von der [WKND-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases/latest) bereitgestellten Bilder werden in der WKND-SPA wiederverwendet. Das Paket kann mit [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) installiert werden.

   ![Package Manager-Installation von wknd.all](./assets/map-components/package-manager-wknd-all.png)

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `Angular/extend-component-solution` wechseln.

## Prüfen der ersten Kartenimplementierung

Eine erste Kartenkomponente wurde durch den Code zu Beginn des Kapitels bereitgestellt. Überprüfen Sie den Ausgangspunkt für die Kartenimplementierung.

1. Öffnen Sie das `ui.apps`-Modul in der IDE Ihrer Wahl.
2. Navigieren Sie zu `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` und zeigen Sie die Datei `.content.xml` an.

   ![Kartenkomponente – AEM-Definition – Start](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Die Eigenschaft `sling:resourceSuperType` verweist auf `wknd-spa-angular/components/image` und gibt an, dass die `Card`-Komponente die Funktionalität von der WKND-SPA-Bildkomponente erbt.

3. Überprüfen Sie die Datei `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Beachten Sie, dass `sling:resourceSuperType` auf `core/wcm/components/image/v2/image` verweist. Dies bedeutet, dass die WKND SPA-Bildkomponente die Funktionalität vom Kernkomponentenbild übernimmt.

   Die Vererbung von Sling-Ressourcen, auch bekannt als [Proxy-Muster](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=de#proxy-component-pattern), ist ein leistungsfähiges Design-Muster, das es untergeordneten Komponenten ermöglicht, Funktionalitäten zu erben und das Verhalten bei Bedarf zu erweitern oder zu überschreiben. Die Sling-Vererbung unterstützt mehrere Vererbungsebenen, sodass die neue `Card`-Komponente letztendlich die Funktionalität des Kernkomponentenbilds erbt.

   Viele Entwicklungs-Teams streben danach, D.R.Y. zu sein, sich also nicht selbst zu wiederholen. Die Sling-Vererbung ermöglicht dies mit AEM.

4. Öffnen Sie die Datei `_cq_dialog/.content.xml` unter dem Ordner `card`.

   Diese Datei ist die Definition des Komponentendialogfelds für die `Card`-Komponente. Bei Verwendung der Sling-Vererbung ist es möglich, Funktionen des [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=de) zu verwenden, um Teile des Dialogfelds zu überschreiben oder zu erweitern. In diesem Beispiel wurde dem Dialogfeld eine neue Registerkarte hinzugefügt, um zusätzliche Daten von einem Autor oder einer Autorin zu erfassen und die Kartenkomponente zu befüllen.

   Mithilfe von Eigenschaften wie `sling:orderBefore` kann ausgewählt werden, wo neue Registerkarten oder Formularfelder eingefügt werden sollen. In diesem Fall wird die Registerkarte `Text` vor der Registerkarte `asset` eingefügt. Um den Sling Resource Merger vollständig zu nutzen, ist es wichtig, die ursprüngliche Struktur der Dialogfeldknoten für das [Dialogfeld „Bildkomponente“](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml) zu kennen.

5. Öffnen Sie die Datei `_cq_editConfig.xml` unter dem Ordner `card`. Diese Datei bestimmt das Drag-and-Drop-Verhalten in der Authoring-Benutzeroberfläche von AEM. Bei der Erweiterung der Bildkomponente ist es wichtig, dass der Ressourcentyp der Komponente selbst entspricht. Überprüfen Sie den Knoten `<parameters>`:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Die meisten Komponenten erfordern keine `cq:editConfig`, das Bild und untergeordnete Elemente der Bildkomponente sind Ausnahmen.

6. Wechseln Sie in der IDE zum Modul `ui.frontend` und navigieren Sie zu `ui.frontend/src/app/components/card`:

   ![Start der Angular-Komponente](assets/extend-component/angular-card-component-start.png)

7. Überprüfen Sie die Datei `card.component.ts`.

   Die Komponente wurde bereits ausgelagert, um sie der AEM-Komponente `Card` unter Verwendung der standardmäßigen `MapTo`-Funktion zuzuordnen.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Überprüfen Sie die drei `@Input`-Parameter in der Klasse für `src`, `alt`und `title`. Dies sind erwartete JSON-Werte aus der AEM Komponente, die der Angular-Komponente zugeordnet sind.

8. Öffnen Sie die Datei `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   In diesem Beispiel haben wir die vorhandene Angular-Bildkomponente `app-image` durch einfache Übergabe der `@Input`-Parameter aus `card.component.ts` wiederverwendet. Später im Tutorial werden zusätzliche Eigenschaften hinzugefügt und angezeigt.

## Aktualisieren der Vorlagenrichtlinie

Mit dieser ersten Implementierung von `Card` wird die Funktionalität im AEM-SPA-Editor überprüft. Um die ursprüngliche `Card`-Komponente zu sehen, ist eine Aktualisierung der Vorlagenrichtlinie erforderlich.

1. Stellen Sie den Starter-Code auf einer lokalen Instanz von AEM bereit, falls Sie dies noch nicht getan haben:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigieren Sie zur SPA-Seitenvorlage unter [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Aktualisieren Sie die Richtlinie des Layout-Containers, um die neue `Card`-Komponente als zulässige Komponente hinzuzufügen:

   ![Aktualisieren der Layout-Container-Richtlinie](assets/extend-component/card-component-allowed.png)

   Speichern Sie die Änderungen an der Richtlinie und beobachten Sie die `Card`-Komponente als zulässige Komponente:

   ![Kartenkomponente als zulässige Komponente](assets/extend-component/card-component-allowed-layout-container.png)

## Erstellen der anfänglichen Kartenkomponente

Als Nächstes erstellen Sie die `Card`-Komponente mit dem AEM-SPA-Editor.

1. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Im Modus `Edit` fügen Sie die `Card`-Komponente zum `Layout Container` hinzu:

   ![Neue Komponente einfügen](assets/extend-component/insert-custom-component.png)

3. Ziehen Sie ein Bild aus dem Asset Finder und legen Sie es in der `Card`-Komponente ab:

   ![Bild hinzufügen](assets/extend-component/card-add-image.png)

4. Öffnen Sie den `Card`-Komponentendialog, wo Sie die neu hinzugefügte Registerkarte **Text** sehen.
5. Geben Sie die folgenden Werte in der Registerkarte **Text** ein:

   ![Registerkarte „Textkomponente“](assets/extend-component/card-component-text.png)

   **Kartenpfad** – Wählen Sie eine Seite unter der SPA Homepage aus.

   **CTA-Text** – „Mehr erfahren“

   **Kartentitel** – Leer lassen.

   **Titel von verknüpfter Seite abrufen** – Aktivieren Sie das Kontrollkästchen, um dies auf „true“ zu setzen.

6. Aktualisieren Sie die Registerkarte **Asset-Metadaten**, um Werte für **Alternativer Text** und **Beschriftung** hinzuzufügen.

   Derzeit werden nach der Aktualisierung des Dialogfelds keine weiteren Änderungen angezeigt. Um die neuen Felder für die Angular-Komponente verfügbar zu machen, müssen wir das Sling-Modell für die `Card`-Komponente aktualisieren.

7. Öffnen Sie eine neue Registerkarte und navigieren Sie zu [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Suchen Sie in den Inhaltsknoten unterhalb von `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` nach dem Inhalt der Komponente `Card`.

   ![Komponenteneigenschaften von CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Beachten Sie, dass die Eigenschaften `cardPath`, `ctaText`, `titleFromPage` vom Dialogfeld beibehalten werden.

## Aktualisieren des Karten-Sling-Modells

Um die Werte aus dem Komponentendialogfeld letztendlich für die Angular-Komponente verfügbar zu machen, müssen wir das Sling-Modell aktualisieren, das die JSON für die `Card`-Komponente befüllt. Wir haben auch die Möglichkeit, zwei Elemente einer Business-Logik zu implementieren:

* Wenn `titleFromPage` auf **wahr** gesetzt ist, gib den Titel der durch `cardPath` spezifizierten Seite zurück, andernfalls gib den Wert des Textfeldes `cardTitle` zurück.
* Gib das Datum der letzten Änderung der durch `cardPath` spezifizierten Seite zurück.

Kehren Sie zur IDE Ihrer Wahl zurück und öffnen Sie das Modul `core`.

1. Öffnen Sie die Datei `Card.java` unter `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Beachten Sie, dass die Schnittstelle `Card` derzeit `com.adobe.cq.wcm.core.components.models.Image` erweitert und daher die Methoden der Schnittstelle `Image` erbt. Die Schnittstelle `Image` erweitert bereits die Schnittstelle `ComponentExporter`, über die das Sling-Modell als JSON exportiert und vom SPA-Editor zugeordnet werden kann. Daher müssen wir die Schnittstelle `ComponentExporter` nicht explizit erweitern, wie wir es im [Kapitel „Benutzerdefinierte Komponente“](custom-component.md) getan haben.

2. Fügen Sie der Schnittstelle die folgenden Methoden hinzu:

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

   Diese Methoden werden über die JSON-Modell-API bereitgestellt und an die Angular-Komponente übergeben.

3. Öffnen Sie `CardImpl.java`. Dies ist die Implementierung der Schnittstelle `Card.java`. Diese Implementierung wurde teilweise gekürzt, um das Tutorial zu beschleunigen.  Beachten Sie die Verwendung der Anmerkungen `@Model` und `@Exporter`, um sicherzustellen, dass das Sling-Modell über den Sling Model Exporter als JSON serialisiert werden kann.

   `CardImpl.java` verwendet auch das [Delegationsmuster für Sling-Modelle](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models), um zu vermeiden, dass die Logik aus der Bild-Kernkomponente umgeschrieben wird.

4. Sehen Sie sich die folgenden Zeilen an:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Die obige Annotation instanziiert ein Bildobjekt mit dem Namen `image`, basierend auf der Vererbung von `sling:resourceSuperType` der `Card`-Komponente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Dann ist es möglich, einfach das Objekt `image` für die Implementierung von Methoden zu verwenden, die von der Schnittstelle `Image` definiert werden, ohne die Logik selbst schreiben zu müssen. Diese Technik wird für `getSrc()`, `getAlt()` und `getTitle()` verwendet.

5. Implementieren Sie anschließend basierend auf dem Wert von `cardPath` die Methode `initModel()` zum Initiieren der privaten Variablen `cardPage`.

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   `@PostConstruct initModel()` wird aufgerufen, wenn das Sling-Modell initialisiert wird. Daher ist es eine gute Gelegenheit, Objekte zu initialisieren, die von anderen Methoden im Modell verwendet werden können. Der `pageManager` ist eines von mehreren [Java™-unterstützten globalen Objekten](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=de), die für Sling-Modelle über die Annotation `@ScriptVariable` bereitgestellt werden. Die Methode [getPage](https://developer.adobe.com/de/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) akzeptiert einen Pfad und gibt ein AEM-[Seiten](https://developer.adobe.com/de/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html)-Objekt zurück – oder null, wenn der Pfad nicht auf eine gültige Seite verweist.

   Dadurch wird die Variable `cardPage` initialisiert, die von den anderen neuen Methoden verwendet wird, um Daten zur zugrunde liegenden verknüpften Seite zurückzugeben.

6. Überprüfen Sie die globalen Variablen, die den JCR-Eigenschaften bereits zugeordnet und im Autorendialogfeld gespeichert sind. Die Annotation `@ValueMapValue` wird verwendet, um die Zuordnung automatisch durchzuführen.

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

   Diese Variablen werden verwendet, um die zusätzlichen Methoden für die Schnittstelle `Card.java` zu implementieren.

7. Implementieren Sie die zusätzlichen Methoden, die in der Schnittstelle `Card.java` definiert sind:

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
   > Sie können die [fertiggestellte CardImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java) ansehen.

8. Öffnen Sie ein Terminal-Fenster und stellen Sie nur die Aktualisierungen des Moduls `core` unter Verwendung des Maven-Profils `autoInstallBundle` aus dem Verzeichnis `core` bereit.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Wenn Sie [AEM 6.x](overview.md#compatibility) verwenden, fügen Sie das Profil `classic` hinzu.

9. Öffnen Sie die JSON-Modellantwort unter [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) und suchen Sie nach `wknd-spa-angular/components/card`:

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

   Beachten Sie, dass das JSON-Modell zusätzliche Schlüssel/Wert-Paare erhält, nachdem die Methoden im Sling-Modell `CardImpl` aktualisiert wurden.

## Aktualisieren der Angular-Komponente

Jetzt, da das JSON-Modell mit neuen Eigenschaften für `ctaLinkURL`, `ctaText`, `cardTitle` und `cardLastModified` befüllt ist, können wir die Angular-Komponente aktualisieren, um diese neuen Eigenschaften anzuzeigen.

1. Kehren Sie zur IDE zurück und öffnen Sie das Modul `ui.frontend`. Optional können Sie den webpack-Development-Server über ein neues Terminal-Fenster starten, um die Änderungen in Echtzeit anzuzeigen:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Öffnen Sie `card.component.ts` unter `ui.frontend/src/app/components/card/card.component.ts`. Fügen Sie die zusätzlichen `@Input`-Anmerkungen hinzu, um das neue Modell zu erfassen:

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

3. Fügen Sie Methoden hinzu, um zu überprüfen, ob der Aktionsaufruf bereit ist, und um eine Datums-/Uhrzeitzeichenfolge basierend auf der Eingabe für `cardLastModified` zurückzugeben:

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

   Sass-Regeln wurden bereits unter `card.component.scss` hinzugefügt, um den Titel, den Aktionsaufruf und das Datum der letzten Änderung zu formatieren.

   >[!NOTE]
   >
   > Sie können den fertigen [Code der Angular-Kartenkomponente hier](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card) ansehen.

5. Stellen Sie die vollständigen Änderungen an AEM aus dem Stammverzeichnis des Projekts mithilfe von Maven bereit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Navigieren Sie zu [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html), um die aktualisierte Komponente anzuzeigen:

   ![Aktualisierte Kartenkomponente in AEM](assets/extend-component/updated-card-in-aem.png)

7. Sie sollten den vorhandenen Inhalt überarbeiten können, um eine Seite wie die folgende zu erstellen:

   ![Abschließendes Authoring der Kartenkomponente](assets/extend-component/final-authoring-card.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gelernt, wie Sie eine AEM-Komponente erweitern und wie Sling-Modelle und -Dialogfelder mit dem JSON-Modell verwendet werden.

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `Angular/extend-component-solution` wechseln.
