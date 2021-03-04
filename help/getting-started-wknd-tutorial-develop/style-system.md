---
title: Entwickeln mit dem Stilsystem
seo-title: Entwickeln mit dem Stilsystem
description: Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. Dieses Lernprogramm behandelt die Entwicklung für das Stilsystem, um Core-Komponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors zu erweitern.
sub-product: Sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
feature: '"Kernkomponenten, Stilsystem"'
topic: '"Content-Management, Entwicklung"'
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '2005'
ht-degree: 3%

---


# Entwickeln mit dem Stilsystem {#developing-with-the-style-system}

Erfahren Sie, wie Sie individuelle Stile implementieren und Kernkomponenten mit dem Stil-System des Experience Managers wiederverwenden. Dieses Lernprogramm behandelt die Entwicklung für das Stilsystem, um Core-Komponenten mit markenspezifischem CSS und erweiterten Richtlinienkonfigurationen des Vorlagen-Editors zu erweitern.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

Es wird außerdem empfohlen, das Lernprogramm [Clientseitige Bibliotheken und Front-End-Arbeitsablauf](client-side-libraries.md) zu überprüfen, um die Grundlagen clientseitiger Bibliotheken und der verschiedenen Front-End-Tools, die in das AEM Projekt integriert sind, zu verstehen.

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt erneut verwenden und die Schritte zum Auschecken des Startprojekts überspringen.

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Sehen Sie sich die Verzweigung `tutorial/style-system-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) oder lokal prüfen, indem Sie zur Verzweigung `tutorial/style-system-solution` wechseln.

## Vorgabe

1. Erfahren Sie, wie Sie mit dem Stilsystem markenspezifische CSS auf AEM Kernkomponenten anwenden.
1. Erfahren Sie mehr über die BEM-Notation und wie sie zum sorgfältigen Scope von Stilen verwendet werden kann.
1. Wenden Sie erweiterte Richtlinienkonfigurationen mit bearbeitbaren Vorlagen an.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Kapitel verwenden wir die Funktion [Stilsystem](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html), um Varianten der Komponenten **title** und **Text** zu erstellen, die auf der Artikelseite verwendet werden.

![Für Titel verfügbare Stile](assets/style-system/styles-added-title.png)

*Für die Titel-Komponente verfügbarer Unterstrichstil*

## Hintergrund {#background}

Das [Stilsystem](https://docs.adobe.com/content/help/de-DE/experience-manager-65/developing/components/style-system.html) ermöglicht es Entwicklern und Vorlageneditoren, mehrere visuelle Varianten einer Komponente zu erstellen. Autoren können dann wiederum entscheiden, welcher Stil beim Erstellen einer Seite verwendet werden soll. Wir werden das Stilsystem während des gesamten Tutorials nutzen, um mehrere einzigartige Stile zu erzielen und gleichzeitig die Kernkomponenten in einem Low-Code-Ansatz zu nutzen.

Die allgemeine Idee beim Stilsystem ist, dass Autoren verschiedene Stile für das Aussehen einer Komponente wählen können. Die &quot;Stile&quot;werden durch zusätzliche CSS-Klassen unterstützt, die in das äußere div einer Komponente injiziert werden. In den Client-Bibliotheken werden CSS-Regeln auf Grundlage dieser Stilklassen hinzugefügt, sodass sich das Erscheinungsbild der Komponente ändert.

Die detaillierte Dokumentation für das Stilsystem finden Sie hier. [](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html) Es gibt auch ein großartiges [technisches Video zum Verständnis des Stilsystems](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Unterstreichungsstil - Titel {#underline-style}

Die [Title-Komponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) wurde im Rahmen des Moduls **ui.apps** in das Projekt unter `/apps/wknd/components/title` kopiert. Die Standardstile von Überschriftenelementen (`H1`, `H2`, `H3`...) wurden bereits im Modul **ui.frontend** implementiert.

Die [WKND-Artikelentwürfe](assets/pages-templates/wknd-article-design.xd) enthalten einen eindeutigen Stil für die Titelkomponente mit einer Unterstreichung. Statt zwei Komponenten zu erstellen oder das Komponentendialogfeld zu ändern, kann das Stilsystem verwendet werden, um Autoren die Möglichkeit zu geben, einen Unterstrichstil hinzuzufügen.

![Unterstreichungsstil - Titelkomponente](assets/style-system/title-underline-style.png)

### Inspect-Titelmarkierung

Als Front-End-Entwickler besteht der erste Schritt zum Formatieren einer Core-Komponente darin, das von der Komponente generierte Markup zu verstehen.

1. Öffnen Sie einen neuen Browser und Ansicht der Titelleiste auf der Website AEM Core Component Library: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. Nachstehend finden Sie das Markup für die Komponente &quot;Titel&quot;:

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   BEM-Notation der Title-Komponente:

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. Das Style-System fügt dem äußeren div, das die Komponente umgibt, eine CSS-Klasse hinzu. Daher ähnelt das Markup, das wir als Targeting einsetzen werden, in etwa dem Folgenden:

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### Implementierung des Underline-Stils - ui.frontend

Anschließend implementieren Sie den Stil &quot;Unterstreichen&quot;mit dem Modul **ui.frontend** unseres Projekts. Wir verwenden den Webpack-Entwicklungsserver, der zum Lieferumfang des Moduls **ui.frontend** gehört, um die Stile *vor der Bereitstellung auf einer lokalen Instanz von AEM zu Vorschau.*

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   Dadurch sollte ein Browser unter [http://localhost:8080](http://localhost:8080) geöffnet werden.

   >[!NOTE]
   >
   > Wenn Bilder beschädigt angezeigt werden, stellen Sie sicher, dass das Startprojekt auf einer lokalen Instanz von AEM bereitgestellt wurde (auf Port 4502 ausgeführt) und der verwendete Browser sich auch bei der lokalen AEM angemeldet hat.

   ![Webpack-Entwicklungsserver](assets/style-system/static-webpack-server.png)

1. Öffnen Sie in der IDE die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`. Dies ist das statische Markup, das vom Webpack-Entwicklungsserver verwendet wird.
1. Suchen Sie in `index.html` eine Instanz der Titellomponente, der der Unterstrichstil hinzugefügt werden soll, indem Sie im Dokument nach *cmp-title* suchen. Wählen Sie die Komponente &quot;Title&quot;mit dem Text *&quot;Vans off the Wall Skatepark&quot;* (Zeile 218). hinzufügen Sie die Klasse `cmp-title--underline` in das umliegende div:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. Kehren Sie zum Browser zurück und vergewissern Sie sich, dass die zusätzliche Klasse im Markup angezeigt wird.
1. Kehren Sie zum Modul **ui.frontend** zurück und aktualisieren Sie die Datei `title.scss` unter: `ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >Es wird als Best Practice erachtet, die Zielgruppe immer eng zu unterteilen. Dadurch wird sichergestellt, dass zusätzliche Stile keine Auswirkungen auf andere Bereiche der Seite haben.
   >
   >Alle Kernkomponenten halten die **[BEM-Notation](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)** ein. Es empfiehlt sich, beim Erstellen eines Standardstils für eine Komponente die äußere CSS-Klasse Zielgruppe. Eine weitere Best Practice ist die Verwendung von Klassennamen, die von der BEM-Notation der Kernkomponente anstelle von HTML-Elementen angegeben werden.

1. Kehren Sie erneut zum Browser zurück und Sie sollten den Stil &quot;Unterstreichen&quot;sehen:

   ![Unterstreichen Sie den Stil, der auf dem Webpack-Dev-Server sichtbar ist](assets/style-system/underline-implemented-webpack.png)

1. Beenden Sie den Webpack-Entwicklungsserver.

### hinzufügen einer Titelrichtlinie

Als Nächstes müssen wir eine neue Richtlinie für Titel-Komponenten hinzufügen, damit Inhaltsersteller den Stil &quot;Unterstreichen&quot;wählen können, der auf bestimmte Komponenten angewendet werden soll. Dies erfolgt mithilfe des Vorlageneditors in AEM.

1. Stellen Sie die Code-Basis mithilfe Ihrer Maven-Fähigkeiten auf einer lokalen AEM-Instanz bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zur Vorlage **Article Page** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Wählen Sie im Modus **Struktur** im Container **Layout** das Symbol **Richtlinie** neben der unter *Zulässige Komponenten* aufgelisteten Komponente **Titel** aus:

   ![Richtlinie für Titel konfigurieren](assets/style-system/article-template-title-policy-icon.png)

1. Erstellen Sie eine neue Richtlinie für die Komponente &quot;Titel&quot;mit den folgenden Werten:

   *Richtlinientitel **:  **WKND-Titel**

   *Eigenschaften* > Registerkarte &quot; *Stile&quot;* >  *Hinzufügen neuen Stils*

   **Unterstreichen** :  `cmp-title--underline`

   ![Stilrichtlinien-Konfiguration für Titel](assets/style-system/title-style-policy.png)

   Klicken Sie auf **Fertig**, um die Änderungen an der Titelrichtlinie zu speichern.

   >[!NOTE]
   >
   > Der Wert `cmp-title--underline` stimmt mit der CSS-Klasse überein, die wir früher bei der Entwicklung im Modul **ui.frontend** ausgewählt haben.

### Unterstreichen-Stil anwenden

Als Autor können wir schließlich den Stil der Unterstreichung auf bestimmte Komponenten des Titels anwenden.

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Wählen Sie im Modus **Bearbeiten** eine Titelkomponente. Klicken Sie auf das Symbol **Paintbrush** und wählen Sie den Stil **Unterstreichen** aus:

   ![Unterstreichen-Stil anwenden](assets/style-system/apply-underline-style-title.png)

   Als Autor sollten Sie den Stil ein-/ausschalten können.

1. Klicken Sie auf das Symbol **Seiteninformationen** > **Ansicht als Veröffentlicht**, um die Seite außerhalb AEM Editors zu überprüfen.

   ![Als veröffentlicht anzeigen](assets/style-system/view-as-published.png)

   Verwenden Sie Ihre Browser-Entwicklerwerkzeuge, um zu überprüfen, ob die CSS-Klasse `cmp-title--underline` auf das äußere div angewendet wurde.

## Stil des Anführungszeichenblocks - Text {#text-component}

Wiederholen Sie anschließend ähnliche Schritte, um einen eindeutigen Stil auf die [Textkomponente](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) anzuwenden. Die Textkomponente wurde im Rahmen des Moduls `/apps/wknd/components/text` in das Projekt unter **ui.apps** kopiert. Die Standardstile von Absatzelementen wurden bereits in **ui.frontend** implementiert.

Die [WKND-Artikelentwürfe](assets/pages-templates/wknd-article-design.xd) enthalten einen eindeutigen Stil für die Textkomponente mit einem Anführungsblock:

![Stil des Anführungsblocks - Textkomponente](assets/style-system/quote-block-style.png)

### Inspect Text Component Markup

Wieder einmal werden wir das Markup der Textkomponente überprüfen.

1. Überprüfen Sie das Markup für die Textkomponente unter: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. Nachstehend finden Sie das Markup für die Komponente Text:

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   Die BEM-Notation der Textkomponente:

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. Das Style-System fügt dem äußeren div, das die Komponente umgibt, eine CSS-Klasse hinzu. Daher ähnelt das Markup, das wir als Targeting einsetzen werden, in etwa dem Folgenden:

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### Implementieren des Anführungsblock-Stils - ui.frontend

Als Nächstes implementieren wir den Stil des Angebotsblocks mit dem Modul **ui.frontend** unseres Projekts.

1. Beginn Sie den Webpack-Dev-Server, indem Sie den folgenden Befehl aus dem Modul **ui.frontend** ausführen:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. Öffnen Sie in der IDE die Datei `index.html` unter: `ui.frontend/src/main/webpack/static/index.html`.
1. Suchen Sie in `index.html` nach einer Instanz der Textkomponente, indem Sie nach dem Text *&quot;Jacob Wester&quot;* (Zeile 210) suchen. hinzufügen Sie die Klasse `cmp-text--quote` in das umliegende div:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. Aktualisieren Sie die Datei `text.scss` unter: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > In diesem Fall werden rohe HTML-Elemente von den Stilen als Ziel ausgewählt. Dies liegt daran, dass die Komponente Text einen Rich-Text-Editor für Inhaltsersteller bereitstellt. Die Erstellung von Stilen direkt gegen RTE-Inhalte sollte mit Sorgfalt erfolgen, und es ist umso wichtiger, die Stile genau zu definieren.

1. Kehren Sie erneut zum Browser zurück und Sie sollten den Stil des Angebotsblocks sehen:

   ![Stil des Anführungsblocks im Webpack-Dev-Server sichtbar](assets/style-system/quoteblock-implemented-webpack.png)

1. Beenden Sie den Webpack-Entwicklungsserver.

### hinzufügen einer Textrichtlinie

Fügen Sie dann eine neue Richtlinie für die Textkomponenten hinzu.

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navigieren Sie zum Ordner **Artikelseitenvorlage** unter: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)).

1. Wählen Sie im Modus **Struktur** im Container **Layout** das Symbol **Richtlinie** neben der unter *Zulässige Komponenten* aufgelisteten Komponente **aus:**

   ![Textrichtlinie konfigurieren](assets/style-system/article-template-text-policy-icon.png)

1. Aktualisieren Sie die Text-Komponentenrichtlinie mit den folgenden Werten:

   *Richtlinientitel **:  **Inhaltstext**

   *Plugins* >  *Absatzformate* > Absatzformate  *aktivieren*

   *Stile, Registerkarte* >  *Hinzufügen neuen Stil*

   **Anführungsblock** :  `cmp-text--quote`

   ![Richtlinie für Textkomponenten](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Textkomponentenrichtlinie 2](assets/style-system/text-policy-enable-quotestyle.png)

   Klicken Sie auf **Fertig**, um die Änderungen an der Textrichtlinie zu speichern.

### Anwenden des Anführungsblock-Stils

1. Navigieren Sie zum Artikel **La Skateparks** im AEM Sites-Editor unter: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Wählen Sie im Modus **Bearbeiten** eine Textkomponente. Bearbeiten Sie die Komponente, um ein Anführungselement einzuschließen:

   ![Konfiguration der Textkomponente](assets/style-system/configure-text-component.png)

1. Wählen Sie die Textkomponente aus und klicken Sie auf das Symbol **Paintbrush** und wählen Sie den Stil **Anführungsblock** aus:

   ![Anwenden des Anführungsblock-Stils](assets/style-system/quote-block-style-applied.png)

   Als Autor sollten Sie den Stil ein-/ausschalten können.

## Feste Breite - Container (Bonus) {#layout-container}

Container-Komponenten wurden verwendet, um die Grundstruktur der Artikelseitenvorlage zu erstellen und die Ablagebereiche für Inhaltsersteller bereitzustellen, um Inhalte auf einer Seite hinzuzufügen. Container können auch das Stilsystem nutzen und Autoren noch mehr Optionen zum Entwerfen von Layouts bereitstellen.

Der **Main-Container** der Artikelseitenvorlage enthält die beiden Authoring-fähigen Container und hat eine feste Breite.

![Wichtigster Container](assets/style-system/main-container-article-page-template.png)

*Wichtigster Container in der Artikelseitenvorlage*.

Die Richtlinie des Containers **Main** legt das Standardelement auf `main` fest:

![Hauptpolitik des Containers](assets/style-system/main-container-policy.png)

Die CSS, die den **Main-Container** fest macht, wird im **ui.frontend**-Modul unter `ui.frontend/src/main/webpack/site/styles/container_main.scss` eingestellt:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Anstatt das HTML-Element `main` als Ziel festzulegen, könnte das Stilsystem verwendet werden, um einen Stil **Feste Container** zu erstellen. Das Stilsystem bietet Benutzern die Möglichkeit, zwischen den Containern **Feste Breite** und **Fließbreite** zu wechseln.

1. **Bonusherausforderung** : Nutzen Sie die aus den vorherigen Übungen gewonnenen Erfahrungen und verwenden Sie das Stilsystem, um eine  **feste** Breite und  **Fließende** Width-Stile für die Container-Komponente zu implementieren.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, die Artikelseite ist fast vollständig formatiert und Sie haben praktische Erfahrungen mit dem AEM Style System gesammelt.

### Nächste Schritte {#next-steps}

Lernen Sie die Schritte von Ende zu Ende, um eine [benutzerdefinierte AEM-Komponente](custom-component.md) zu erstellen, die in einem Dialog verfasste Inhalte anzeigt, und erforschen Sie die Entwicklung eines Sling-Modells, um eine Geschäftslogik einzukapseln, die die HTML-Datei der Komponente ausfüllt.

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `tutorial/style-system-solution`.

1. Klonen Sie das [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)-Repository.
1. Sehen Sie sich die Verzweigung `tutorial/style-system-solution` an.
