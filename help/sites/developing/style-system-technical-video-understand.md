---
title: Code für das AEM-Stilsystem
description: In diesem Video betrachten wir die Anatomie von CSS (oder LESS) und JavaScript, die zum Stilen der Core Title-Komponente von Adobe Experience Manager mithilfe des Style-Systems verwendet werden, sowie die Anwendung dieser Stile auf HTML und DOM.
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 4%

---


# Code für das Stilsystem{#understanding-how-to-code-for-the-aem-style-system}

In diesem Video betrachten wir die Anatomie der CSS (oder [!DNL LESS]) und JavaScript, die zum Stilen der Core Title-Komponente von Experience Manager mithilfe des Style-Systems verwendet werden, sowie die Anwendung dieser Stile auf HTML und DOM.

>[!NOTE]
>
>Das AEM Style System wurde mit [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-1/release-notes/sp3-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)eingeführt.
>
>Das Video geht davon aus, dass die Komponente &quot;We.Retail Title&quot;aktualisiert wurde, um von [Core Components v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)zu übernehmen.

## Code für das Stilsystem {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

Das bereitgestellte AEM (**technical-review.sites.style-system-1.0.0.zip**) installiert den Beispieltitelstil, die Beispielrichtlinien für die Container &quot;We.Retail&quot;-Layout und &quot;Title&quot;-Komponenten sowie eine Beispielseite.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

Die folgende [!DNL LESS] Definition für den Beispielstil lautet:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Für diejenigen, die CSS bevorzugen, ist unter diesem Codefragment das CSS, in das es [!DNL LESS] kompiliert wird.

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

Die obigen Daten [!DNL LESS] werden nativ nach Experience Manager in die folgende CSS kompiliert.

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

Das folgende JavaScript erfasst und fügt das Datum und die Uhrzeit der letzten Änderung der Seite unter dem Titeltext ein, wenn der Beispielstil auf die Komponente &quot;Titel&quot;angewendet wird.

Die Verwendung von jQuery sowie die verwendeten Benennungskonventionen sind optional.

Die folgende [!DNL LESS] Definition für den Beispielstil lautet:

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

## Development best practices {#development-best-practices}

### Best Practices für HTML {#html-best-practices}

* HTML (über HTML generiert) sollte so strukturell wie möglich semantisch sein; Vermeidung unnötiger Gruppierung/Verschachtelung von Elementen.
* HTML-Elemente sollten über CSS-Klassen im BEM-Stil adressierbar sein.

**Gut** - Alle Elemente in der Komponente sind mit BEM-Notation adressierbar:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Schlecht** : Die Elemente für Liste und Liste können nur mit dem Elementnamen adressiert werden:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Es ist besser, mehr Daten verfügbar zu machen und sie zu verstecken, als zu wenige Daten offen zu legen, die eine zukünftige Back-End-Entwicklung erfordern, um sie verfügbar zu machen.

   * Die Implementierung von Authoring-fähigen Content-Umleitungen kann dazu beitragen, dass dieser HTML-Leitfaden erhalten bleibt, wodurch Autoren auswählen können, welche Inhaltselemente in den HTML-Code geschrieben werden. Das kann besonders wichtig sein, wenn Bilder in HTML geschrieben werden, die nicht für alle Stile verwendet werden.
   * Eine Ausnahme bilden kostenintensive Ressourcen, z. B. Bilder, die standardmäßig verfügbar sind, da Ereignis-Bilder, die durch CSS ausgeblendet werden, in diesem Fall unnötigerweise abgerufen werden.

      * Moderne Bildkomponenten verwenden häufig JavaScript, um das für den Anwendungsfall am besten geeignete Bild auszuwählen und zu laden (Viewport).

### Best Practices für CSS {#css-best-practices}

>[!NOTE]
>
>Das Stilsystem unterscheidet sich geringfügig von [BEM](https://en.bem.info/)insofern, als das `BLOCK` und `BLOCK--MODIFIER` nicht auf dasselbe Element angewendet werden, wie von [BEM](https://en.bem.info/)angegeben.
>
>Stattdessen `BLOCK--MODIFIER` wird das Element aufgrund von Produktbeschränkungen auf das übergeordnete Element des `BLOCK` Elements angewendet.
>
>Alle anderen Mieter von [BEM](https://en.bem.info/) sollten mit angeglichen werden.

* Verwenden Sie Präprozessoren wie [LESS](https://lesscss.org/) (unterstützt von AEM nativ) oder [SCSS](https://sass-lang.com/) (erfordert ein benutzerdefiniertes Buildsystem), um eine klare CSS-Definition und Wiederverwendbarkeit zu ermöglichen.

* einheitliche Gewichtung/Spezifität des Selektors; Auf diese Weise lassen sich schwierige CSS-Kaskadenkonflikte vermeiden und lösen.
* Organisieren Sie jeden Stil in einer separaten Datei.
   * Diese Dateien können mithilfe von LESS/SCSS kombiniert werden `@imports` oder, falls unverarbeitetes CSS erforderlich ist, über die Aufnahme der HTML-Client-Bibliotheksdatei oder benutzerdefinierte Front-End-Asset-Buildsysteme.
* Vermeiden Sie das Mischen vieler komplexer Stile.
   * Je mehr Stile gleichzeitig auf eine Komponente angewendet werden können, desto vielfältiger sind die Permutationen. Dies kann schwierig werden, die Markenausrichtung beizubehalten/zu überprüfen/sicherzustellen.
* Verwenden Sie immer CSS-Klassen (nach BEM-Notation), um CSS-Regeln zu definieren.
   * Wenn die Auswahl von Elementen ohne CSS-Klassen (d. h. bloße Elemente) unbedingt erforderlich ist, verschieben Sie sie in der CSS-Definition höher, um deutlich zu machen, dass sie eine geringere Spezifität aufweisen als Kollisionen mit Elementen dieses Typs, die über auswählbare CSS-Klassen verfügen.
* Vermeiden Sie die Formatierung `BLOCK--MODIFIER` direkt beim Responsive Grid. Eine Änderung der Anzeige dieses Elements kann sich auf das Rendering und die Funktionalität des Responsive Grid auswirken. Daher ist der Stil auf dieser Ebene nur dann sinnvoll, wenn das Verhalten des Responsive Grid geändert werden soll.
* Wenden Sie den Stilbereich mit `BLOCK--MODIFIER`. Der Stil `BLOCK__ELEMENT--MODIFIERS` kann in der Komponente verwendet werden. Da die Komponente jedoch die Komponente `BLOCK` darstellt und die Komponente den Stil hat, wird der Stil &quot;definiert&quot;und per Scoped `BLOCK--MODIFIER`.

Beispiel für eine CSS-Auswahlstruktur:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Auswahl der ersten Ebene</p> <p>BLOCK - MODIFIZIERUNG</p> </td> 
   <td valign="bottom"><p>Auswahl der 2. Ebene</p> <p>BLOCK</p> </td> 
   <td valign="bottom"><p>Auswahl auf 3. Ebene</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Effektiver CSS-Selektor</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-Liste - dark</span></td> 
   <td valign="middle"><span class="code">.cmp-Liste</span></td> 
   <td valign="middle"><span class="code">.cmp-Liste__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-Liste - dark</span></p> <p><span class="code"> .cmp-Liste</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-Liste__item { </span></strong></p> <p><strong> color: blau;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> color: rot;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

Bei verschachtelten Komponenten überschreitet die CSS-Selektortiefe für diese verschachtelten Komponenten den Selektor der dritten Ebene. Wiederholen Sie das gleiche Muster für die verschachtelte Komponente, jedoch durch das Scoping der übergeordneten Komponente `BLOCK`. Oder anders ausgedrückt: Beginn der verschachtelten Komponente auf der 3. Ebene `BLOCK` , und die verschachtelte Komponente `ELEMENT` befindet sich auf der 4. Ebene der Auswahl.

### Best Practices für JavaScript {#javascript-best-practices}

Die in diesem Abschnitt definierten Best Practices beziehen sich auf &quot;style-JavaScript&quot;oder JavaScript, das speziell zum Manipulieren der Komponente für stilistische anstatt für funktionelle Zwecke gedacht ist.

* Style-JavaScript sollte vorsichtig verwendet werden und ist ein Minderheitsfall.
* Style-JavaScript sollte hauptsächlich zur Manipulation des DOM der Komponente verwendet werden, um die Formatierung durch CSS zu unterstützen.
* Beurteilen Sie die Verwendung von JavaScript neu, wenn Komponenten mehrmals auf einer Seite angezeigt werden, und verstehen Sie die Kosten für die Berechnung/Neuerstellung.
* Beurteilen Sie die Verwendung von JavaScript erneut, wenn es neue Daten/Inhalte asynchron (über AJAX) einzieht, wenn die Komponente möglicherweise mehrmals auf einer Seite angezeigt wird.
* Verarbeiten Sie sowohl Veröffentlichungs- als auch Authoring-Erlebnisse.
* Verwenden Sie nach Möglichkeit erneut style-Javascript.
   * Wenn beispielsweise mehrere Stile einer Komponente erfordern, dass das Bild in ein Hintergrundbild verschoben wird, kann das style-JavaScript einmalig implementiert und an mehrere Stile angehängt werden `BLOCK--MODIFIERs`.
* Trennen Sie style-JavaScript nach Möglichkeit von funktionellem JavaScript.
* Bewerten Sie die Kosten für JavaScript im Vergleich zum Manifesting dieser DOM-Änderungen im HTML direkt über HTML.
   * Wenn eine Komponente, die mit Stil-JavaScript arbeitet, eine serverseitige Änderung erfordert, prüfen Sie, ob die JavaScript-Manipulation zu diesem Zeitpunkt eingebunden werden kann und welche Auswirkungen/Auswirkungen auf die Leistung und Supportabilität der Komponente haben.

#### Leistungsaspekte {#performance-considerations}

* Style-JavaScript sollte leicht und schlank gehalten werden.
* Um Flimmern und unnötige Neuladungen zu vermeiden, blenden Sie die Komponente zunächst über aus `BLOCK--MODIFIER BLOCK`und zeigen Sie sie an, wenn alle DOM-Manipulationen im JavaScript abgeschlossen sind.
* Die Leistung der Stil-JavaScript-Manipulationen ist vergleichbar mit einfachen jQuery-Plugins, die an Elemente in DOMReady angehängt und geändert werden.
* Stellen Sie sicher, dass Anforderungen komprimiert und CSS und JavaScript minimiert werden.

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zum Stilsystem](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Erstellen AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Website zur BEM-Dokumentation (Block Element Modifier)](https://getbem.com/)
* [Website der LESS-Dokumentation](https://lesscss.org/)
* [jQuery-Website](https://jquery.com/)
