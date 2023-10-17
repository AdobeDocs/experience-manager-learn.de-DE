---
title: Grundlegendes zum Code für das AEM-Stilsystem
description: In diesem Video werden wir uns die Anatomie von CSS (oder LESS) und JavaScript ansehen, die verwendet werden, um die Kerntitelkomponente von Adobe Experience Manager mithilfe des Stilsystems zu gestalten, sowie die Anwendung dieser Stile auf HTML und DOM.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '1090'
ht-degree: 100%

---

# Grundlegendes zum Code für das Stilsystem{#understanding-how-to-code-for-the-aem-style-system}

In diesem Video werden wir uns die Anatomie von CSS (oder [!DNL LESS]) und JavaScript ansehen, die verwendet werden, um die Kerntitelkomponente von Experience Manager mithilfe des Stilsystems zu gestalten, sowie die Anwendung dieser Stile auf HTML und DOM.


## Grundlegendes zum Code für das Stilsystem {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

Das bereitgestellte AEM-Paket (**technical-review.sites.style-system-1.0.0.zip**) installiert den Beispieltitelstil, die Beispielrichtlinien für den We.Retail-Layout-Container und die Titelkomponenten sowie eine Beispielseite.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Im Folgenden finden Sie die [!DNL LESS]-Definition für den Beispielstil unter:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Wer CSS bevorzugt, findet unter diesem Code-Fragment das CSS, in dem [!DNL LESS] kompiliert ist.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

Das obige [!DNL LESS] wird nativ durch Experience Manager ins folgende CSS kompiliert.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### JavaScript {#example-javascript}

Das folgende JavaScript erfasst das Datum und die Uhrzeit der letzten Änderung der Seite und fügt sie unter dem Titeltext ein, wenn der Beispielstil auf die Titelkomponente angewendet wird.

Die Verwendung von jQuery sowie der angewendeten Benennungskonventionen sind optional.

Im Folgenden finden Sie die [!DNL LESS]-Definition für den Beispielstil unter:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Best Practices für die Entwicklung {#development-best-practices}

### Best Practices für HTML {#html-best-practices}

* HTML (über HTL generiert) sollte möglichst strukturell semantisch sein; eine unnötige Gruppierung/Verschachtelung von Elementen sollte vermieden werden.
* HTML-Elemente sollten über CSS-Klassen im BEM-Stil adressierbar sein.

**Gut** – Alle Elemente in der Komponente können mit der BEM-Notation adressiert werden:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Schlecht** – Die Liste und die Listenelemente können nur mit dem Elementnamen adressiert werden:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es ist besser, mehr Daten verfügbar zu machen und sie zu verbergen, als zu wenig Daten verfügbar zu machen, die eine zukünftige Back-End-Entwicklung erfordern, um sie verfügbar zu machen.

   * Das Implementieren von erstellbaren Inhalts-Umschaltern kann dazu beitragen, dieses HTML schlank zu halten, wodurch Autorinnen und Autoren auswählen können, welche Inhaltselemente auf das HTML geschrieben werden. Dies kann besonders beim Schreiben von Bildern auf das HTML wichtig sein, das möglicherweise nicht für alle Stile verwendet wird.
   * Die Ausnahme von dieser Regel besteht darin, dass teure Ressourcen, z. B. Bilder, standardmäßig bereitgestellt werden, da von CSS ausgeblendete Ereignisbilder in diesem Fall unnötigerweise abgerufen werden.

      * Moderne Bildkomponenten verwenden häufig JavaScript, um das für den Anwendungsfall (Breitenbereich) am besten geeignete Bild auszuwählen und zu laden.

### Best Practices für CSS {#css-best-practices}

>[!NOTE]
>
>Das Stilsystem unterscheidet sich technisch geringfügig von [BEM](https://en.bem.info/), indem `BLOCK` und `BLOCK--MODIFIER` nicht auf dasselbe Element angewendet werden wie durch [BEM](https://en.bem.info/) angegeben.
>
>Stattdessen wird aufgrund von Produktbeschränkungen der `BLOCK--MODIFIER` auf das übergeordnete Element des `BLOCK`-Elements angewendet.
>
>An alle anderen Mandanten von [BEM](https://en.bem.info/) sollte eine Angleichung erfolgen.

* Verwenden Sie Präprozessoren wie [LESS](https://lesscss.org/) (nativ von AEM unterstützt) oder [SCSS](https://sass-lang.com/) (erfordert ein benutzerdefiniertes Build-System), um eine klare CSS-Definition und Wiederverwendbarkeit zu ermöglichen.

* Halten Sie die Selektorgewichtung/Spezifität einheitlich. Dies hilft, schwer zu identifizierende CSS-Kaskadierkonflikte zu vermeiden bzw. beheben.
* Organisieren Sie jeden Stil in einer separaten Datei.
   * Diese Dateien können mit `@imports` bei LESS/SCSS kombiniert werden oder, wenn rohe CSS erforderlich ist, über die HTML-Client-Bibliotheksdatei-Einbindung oder benutzerdefinierte Front-End-Asset-Build-Systeme.
* Vermeiden Sie das Mischen vieler komplexer Stile.
   * Je mehr Stile gleichzeitig auf eine Komponente angewendet werden können, desto größer ist die Vielfalt an Permutationen. Dadurch kann es schwierig werden, die Markenausrichtung zu verwalten/zu überprüfen/sicherzustellen.
* Verwenden Sie immer CSS-Klassen (nach BEM-Notation), um CSS-Regeln zu definieren.
   * Wenn die Auswahl von Elementen ohne CSS-Klassen (d. h. bloßen Elementen) unbedingt erforderlich ist, verschieben Sie sie in der CSS-Definition höher, um deutlich zu machen, dass sie eine geringere Spezifität aufweisen als Konflikte mit Elementen dieses Typs, die über auswählbare CSS-Klassen verfügen.
* Vermeiden Sie die direkte Formatierung des `BLOCK--MODIFIER`, da dieser an das responsive Raster angehängt ist. Eine Änderung der Anzeige dieses Elements kann sich auf das Rendering und die Funktionalität des responsiven Rasters auswirken. Ändern Sie den Stil auf dieser Ebene daher nur, wenn das Verhalten des responsiven Rasters geändert werden soll.
* Anwenden des Stilbereichs mithilfe des `BLOCK--MODIFIER`. Die `BLOCK__ELEMENT--MODIFIERS` können in der Komponente verwendet werden, da jedoch der `BLOCK` die Komponente darstellt, und auf die Komponente der Stil angewendet wird, ist der Stil „definiert“ und sein Bereich wird über `BLOCK--MODIFIER` festgelegt.

Ein Beispiel für eine CSS-Selektorstruktur sollte wie folgt aussehen:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Auswahl der ersten Ebene</p> <p>BLOCK--MODIFIER</p> </td> 
   <td valign="bottom"><p>Auswahl auf zweiter Ebene</p> <p>BLOCK</p> </td> 
   <td valign="bottom"><p>Auswahl auf dritter Ebene</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Effektive CSS-Auswahl</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list--dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list--dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list_item { </span></strong></p> <p><strong> color: blue;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image--hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image--hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: red;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Bei verschachtelten Komponenten überschreitet die CSS-Auswahltiefe für diese verschachtelten Komponentenelemente die Auswahltiefe der dritten Ebene. Wiederholen Sie dasselbe Muster für die verschachtelte Komponente, jedoch im Bereich der übergeordneten Komponente `BLOCK`. Mit anderen Worten starten Sie `BLOCK` der verschachtelten Komponente auf der dritten Ebene und `ELEMENT` der verschachtelten Komponente befindet sich auf der vierten Auswahlebene.

### Best Practices für JavaScript {#javascript-best-practices}

Die in diesem Abschnitt definierten Best Practices beziehen sich auf „style-JavaScript“ bzw. JavaScript, das speziell dazu bestimmt ist, die Komponente für stilistische statt für funktionale Zwecke zu bearbeiten.

* style-JavaScript sollte mit Bedacht verwendet werden und ist ein Anwendungsfall von geringer Bedeutung.
* style-JavaScript sollte hauptsächlich zur Bearbeitung des DOM der Komponente verwendet werden, um die Formatierung durch CSS zu unterstützen.
* Ändern Sie ggf. die Verwendung von JavaScript, wenn Komponenten mehrmals auf einer Seite angezeigt werden, und stellen Sie sicher, dass Sie die Kosten für die Berechnung/Neuzeichnung überblicken.
* Ändern Sie ggf. die Verwendung von JavaScript, wenn es neue Daten/Inhalte asynchron (über AJAX) abruft, wenn die Komponente evtl. häufig auf einer Seite angezeigt wird.
* Verarbeiten Sie sowohl Publishing- als auch Authoring-Erlebnisse.
* Verwenden Sie nach Möglichkeit style-JavaScript erneut.
   * Wenn beispielsweise wegen mehreren Stilen einer Komponente das Bild in ein Hintergrundbild verschoben werden muss, kann style-JavaScript einmal implementiert und mit mehreren `BLOCK--MODIFIERs` verbunden werden.
* Trennen Sie style-JavaScript nach Möglichkeit von funktionalem JavaScript.
* Werten Sie die Kosten von JavaScript im Vergleich zu Manifestationen dieser DOM-Änderungen direkt über HTL in HTML aus.
   * Wenn eine Komponente, die style-JavaScript verwendet, eine Server-seitige Änderung erfordert, sollten Sie prüfen, ob die JavaScript-Bearbeitung zu diesem Zeitpunkt eingebunden werden kann und welche Auswirkungen sich auf die Leistung und Unterstützbarkeit der Komponente ergeben.

#### Leistungsaspekte {#performance-considerations}

* style-JavaScript sollte leicht und schlank gehalten werden.
* Um Flackern und unnötige Wiederholungen zu vermeiden, blenden Sie die Komponente zunächst über `BLOCK--MODIFIER BLOCK` aus und zeigen Sie sie an, wenn alle DOM-Bearbeitungen in JavaScript abgeschlossen sind.
* Die Leistung der style-JavaScript-Bearbeitungen ähnelt den grundlegenden jQuery-Plug-ins, die Elemente in DOMReady anhängen und ändern.
* Stellen Sie sicher, dass die Anforderungen komprimiert sind und CSS und JavaScript minimiert wurden.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zum Stilsystem](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Erstellen von AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Dokumentations-Website zu BEM (Block Element Modifier)](https://getbem.com/)
* [Dokumentations-Website zu LESS](https://lesscss.org/)
* [jQuery-Website](https://jquery.com/)
