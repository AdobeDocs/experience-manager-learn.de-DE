---
title: Grundlegendes zum Code für das AEM Stilsystem
description: In diesem Video werden wir uns die Anatomie von CSS (oder LESS) und JavaScript ansehen, die verwendet werden, um die Kerntitelkomponente von Adobe Experience Manager mithilfe des Stilsystems zu gestalten, sowie die Anwendung dieser Stile auf HTML und DOM.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 2%

---

# Grundlegendes zum Code für das Stilsystem{#understanding-how-to-code-for-the-aem-style-system}

In diesem Video werden wir uns die Anatomie des CSS (oder [!DNL LESS]) und JavaScript verwendet, um die Kerntitelkomponente von Experience Manager mithilfe des Stilsystems zu formatieren, sowie wie diese Stile auf das HTML und DOM angewendet werden.


## Grundlegendes zum Code für das Stilsystem {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=12&learn=on)

Das bereitgestellte AEM (**technical-review.sites.style-system-1.0.0.zip**) installiert den Beispieltitelstil, Beispielrichtlinien für den We.Retail-Layout-Container und die Komponenten Titel sowie eine Beispielseite.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### Die CSS {#the-css}

Im Folgenden wird die [!DNL LESS] -Definition für den Beispielstil gefunden unter:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Für diejenigen, die CSS bevorzugen, ist unter diesem Codefragment das CSS dies [!DNL LESS] kompiliert in.

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

Die oben genannten [!DNL LESS] wird nativ durch Experience Manager in die folgende CSS kompiliert.

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

### Das JavaScript {#example-javascript}

Das folgende JavaScript erfasst und fügt das Datum und die Uhrzeit der letzten Änderung der Seite unter dem Titeltext ein, wenn der Beispielstil auf die Titelkomponente angewendet wird.

Die Verwendung von jQuery sowie die verwendeten Benennungskonventionen sind optional.

Im Folgenden wird die [!DNL LESS] -Definition für den Beispielstil gefunden unter:

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

* HTML (über HTL generiert) sollte möglichst strukturell semantisch sein; Vermeidung unnötiger Gruppierung/Verschachtelung von Elementen.
* HTML-Elemente sollten über CSS-Klassen im BEM-Stil adressierbar sein.

**Gut** - Alle Elemente in der Komponente können mit der BEM-Notation adressiert werden:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Bad** - Die Listen- und Listenelemente können nur nach Elementnamen adressiert werden:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es ist besser, mehr Daten verfügbar zu machen und sie zu verbergen, als zu wenig Daten verfügbar zu machen, die eine zukünftige Back-End-Entwicklung erfordern, um sie verfügbar zu machen.

   * Das Implementieren von autorfähigen Inhaltswechseln kann dazu beitragen, diesen HTML-Lean beizubehalten, wodurch Autoren auswählen können, welche Inhaltselemente auf die HTML geschrieben werden. Die kann besonders beim Schreiben von Bildern auf die HTML wichtig sein, die möglicherweise nicht für alle Stile verwendet werden.
   * Die Ausnahme von dieser Regel besteht darin, dass teure Ressourcen, z. B. Bilder, standardmäßig bereitgestellt werden, da von CSS ausgeblendete Ereignisbilder in diesem Fall unnötigerweise abgerufen werden.

      * Moderne Bildkomponenten verwenden häufig JavaScript, um das für den Anwendungsfall am besten geeignete Bild (Viewport) auszuwählen und zu laden.

### Best Practices für CSS {#css-best-practices}

>[!NOTE]
>
>Das Stilsystem unterscheidet sich geringfügig von [BEM](https://en.bem.info/), wobei `BLOCK` und `BLOCK--MODIFIER` nicht auf dasselbe Element angewendet werden, wie durch [BEM](https://en.bem.info/).
>
>Stattdessen wird aufgrund von Produktbeschränkungen die `BLOCK--MODIFIER` wird auf das übergeordnete Element des `BLOCK` -Element.
>
>Alle anderen Mieter von [BEM](https://en.bem.info/) sollte mit angeglichen werden.

* Verwenden Sie Präprozessoren wie [WENIGER](https://lesscss.org/) (von nativ AEM unterstützt) oder [SCSS](https://sass-lang.com/) (benutzerdefiniertes Build-System erforderlich), um eine klare CSS-Definition und Wiederverwendbarkeit zu ermöglichen.

* Selektorgewicht/Spezifität einheitlich halten; Dies hilft, schwierige CSS-Kaskadierkonflikte zu vermeiden und zu beheben.
* Organisieren Sie jeden Stil in einer separaten Datei.
   * Diese Dateien können mit LESS/SCSS kombiniert werden `@imports` oder wenn rohe CSS erforderlich ist, über die HTML Client-Bibliotheksdatei-Einbindung oder benutzerdefinierte Front-End-Asset-Build-Systeme.
* Vermeiden Sie das Mischen vieler komplexer Stile.
   * Je mehr Stile gleichzeitig auf eine Komponente angewendet werden können, desto größer ist die Vielfalt an Permutationen. Dies kann schwierig werden, die Markenausrichtung zu verwalten/zu überprüfen/sicherzustellen.
* Verwenden Sie immer CSS-Klassen (nach BEM-Notation), um CSS-Regeln zu definieren.
   * Wenn die Auswahl von Elementen ohne CSS-Klassen (d. h. bloße Elemente) unbedingt erforderlich ist, verschieben Sie sie in der CSS-Definition höher, um deutlich zu machen, dass sie eine geringere Spezifität aufweisen als Kollisionen mit Elementen dieses Typs, die über auswählbare CSS-Klassen verfügen.
* Vermeiden Sie die Formatierung der `BLOCK--MODIFIER` direkt, da diese an das responsive Raster angehängt ist. Eine Änderung der Anzeige dieses Elements kann sich auf das Rendering und die Funktionalität des responsiven Rasters auswirken. Daher wird der Stil nur auf dieser Ebene festgelegt, wenn das Verhalten des responsiven Rasters geändert werden soll.
* Anwenden des Stilumfangs mithilfe von `BLOCK--MODIFIER`. Die `BLOCK__ELEMENT--MODIFIERS` kann in der Komponente verwendet werden, da jedoch die `BLOCK` stellt die Komponente dar, und die Komponente ist der Stil, der Stil ist &quot;definiert&quot;und ist über `BLOCK--MODIFIER`.

Beispiel für eine CSS-Selektorstruktur:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Auswahl der ersten Ebene</p> <p>BLOCK—MODIFIER</p> </td> 
   <td valign="bottom"><p>Auswahl der zweiten Ebene</p> <p>BLOCK</p> </td> 
   <td valign="bottom"><p>Auswahl auf dritter Ebene</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Effektiver CSS-Selektor</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list_item { </span></strong></p> <p><strong> color: blau;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image_caption {</span></p> <p><span class="code"> color: rot;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Bei verschachtelten Komponenten überschreitet die CSS-Selektortiefe für diese verschachtelten Komponentenelemente die Selektortiefe der dritten Ebene. Wiederholen Sie dasselbe Muster für die verschachtelte Komponente, jedoch in den Bereich der übergeordneten Komponente. `BLOCK`. Oder mit anderen Worten, starten Sie die `BLOCK` auf der dritten Ebene und der verschachtelten Komponente `ELEMENT` befindet sich auf der vierten Selektorebene.

### Best Practices für JavaScript {#javascript-best-practices}

Die in diesem Abschnitt definierten Best Practices beziehen sich auf &quot;Stil-JavaScript&quot;oder JavaScript, das speziell dazu bestimmt ist, die Komponente für stilistische anstatt für funktionale Zwecke zu bearbeiten.

* Style-JavaScript sollte umsichtig verwendet werden und ist ein Anwendungsfall von geringer Bedeutung.
* Style-JavaScript sollte hauptsächlich zur Bearbeitung des DOM der Komponente verwendet werden, um die Formatierung durch CSS zu unterstützen.
* Evaluieren Sie die Verwendung von JavaScript neu, wenn Komponenten mehrmals auf einer Seite angezeigt werden, und verstehen Sie die Kosten für die Berechnung/Neuzeichnung.
* Beurteilen Sie die Verwendung von JavaScript neu, wenn es neue Daten/Inhalte asynchron (über AJAX) abruft, wenn die Komponente häufig auf einer Seite angezeigt werden kann.
* Verarbeiten Sie sowohl Veröffentlichungs- als auch Authoring-Erlebnisse.
* Verwenden Sie nach Möglichkeit Stil-JavaScript erneut.
   * Wenn beispielsweise mehrere Stile einer Komponente erfordern, dass das Bild in ein Hintergrundbild verschoben wird, kann das Stil-JavaScript einmal implementiert und an mehrere `BLOCK--MODIFIERs`.
* Trennen Sie style-JavaScript nach Möglichkeit von funktionalem JavaScript.
* Bewerten Sie die Kosten von JavaScript im Vergleich zu Manifestationen dieser DOM-Änderungen direkt über HTL im HTML.
   * Wenn eine Komponente, die Stil-JavaScript verwendet, eine serverseitige Änderung erfordert, sollten Sie prüfen, ob die JavaScript-Manipulation zu diesem Zeitpunkt eingebunden werden kann und welche Auswirkungen/Auswirkungen auf die Leistung und Unterstützbarkeit der Komponente haben.

#### Leistungsaspekte {#performance-considerations}

* Style-JavaScript sollte leicht und schlank gehalten werden.
* Um Flackern und unnötige Wiederholungen zu vermeiden, blenden Sie die Komponente zunächst über aus. `BLOCK--MODIFIER BLOCK`und zeigen Sie sie an, wenn alle DOM-Manipulationen in JavaScript abgeschlossen sind.
* Die Leistung der Stil-JavaScript-Manipulationen ähnelt den grundlegenden jQuery-Plug-ins, die Elemente in DOMReady anhängen und ändern.
* Stellen Sie sicher, dass die Anforderungen komprimiert sind und CSS und JavaScript minimiert wurden.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zum Stilsystem](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Erstellen AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Website zur BEM-Dokumentation (Block Element Modifier)](https://getbem.com/)
* [Website der LESS-Dokumentation](https://lesscss.org/)
* [jQuery-Website](https://jquery.com/)
