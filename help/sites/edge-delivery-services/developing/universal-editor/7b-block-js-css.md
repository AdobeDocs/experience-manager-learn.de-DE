---
title: Entwickeln eines Bausteins mit CSS und JS
description: Entwickeln eines Bausteins mit CSS und JavaScript für Edge Delivery Services, der mit dem universellen Editor bearbeitet werden kann.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---


# Entwickeln eines Bausteins mit CSS und JavaScript

Im [ Kapitel ](./7b-block-js-css.md) wurde die Formatierung eines Blocks ausschließlich mit CSS behandelt. Jetzt verlagert sich der Fokus auf die Entwicklung eines Bausteins mit JavaScript und CSS.

In diesem Beispiel werden drei Möglichkeiten zur Erweiterung eines Blocks veranschaulicht:

1. Hinzufügen benutzerdefinierter CSS-Klassen.
1. Verwenden von Ereignis-Listenern zum Hinzufügen von Bewegungen.
1. Geschäftsbedingungen, die optional in den Text des Teasers aufgenommen werden können.

## Häufige Anwendungsfälle

Dieser Ansatz ist besonders in folgenden Szenarien nützlich:

- **Externe CSS-Verwaltung:** Wenn das CSS des Blocks außerhalb von Edge Delivery Services verwaltet wird und nicht mit seiner HTML-Struktur übereinstimmt.
- **Zusätzliche Attribute:** Wenn zusätzliche Attribute wie [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) für Barrierefreiheit oder [Microdata](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata) erforderlich sind.
- **JavaScript-Verbesserungen:** Wenn interaktive Funktionen wie Ereignis-Listener erforderlich sind.

Diese Methode beruht auf der browsernativen Manipulation von JavaScript-DOMs, erfordert jedoch Vorsicht bei der Änderung des DOM, insbesondere beim Verschieben von Elementen. Solche Änderungen können das Authoring-Erlebnis des universellen Editors beeinträchtigen. Idealerweise sollte das -Inhaltsmodell [ Blocks sorgfältig ](./5-new-block.md#block-model) werden, um die Notwendigkeit umfangreicher DOM-Änderungen zu minimieren.

## HTML blockieren

Um sich der Blockentwicklung anzunähern, überprüfen Sie zunächst das von Edge Delivery Services angezeigte DOM. Die Struktur wird mit JavaScript verbessert und mit CSS formatiert.

>[!BEGINTABS]

>[!TAB DOM zum Dekorieren]

Im Folgenden finden Sie das DOM des Teaser-Blocks, das mithilfe von JavaScript und CSS dekoriert werden soll.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB So finden Sie das DOM]

Um das zu dekorierende DOM zu finden, öffnen Sie die Seite mit dem nicht dekorierten Block in Ihrer lokalen Entwicklungsumgebung, wählen Sie den Block aus und überprüfen Sie das DOM.

![Inspect-Block-DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## JavaScript blockieren

Um einem Baustein JavaScript-Funktionen hinzuzufügen, erstellen Sie eine JavaScript-Datei im Verzeichnis des Bausteins mit demselben Namen wie der Baustein, z. B. `/blocks/teaser/teaser.js`.

Die JavaScript-Datei sollte eine Standardfunktion exportieren:

```javascript
export default function decorate(block) { ... }
```

Die Standardfunktion akzeptiert das DOM-Element bzw. die Baumstruktur, die den Block auf der Edge Delivery Services-HTML darstellt, und enthält die benutzerdefinierte JavaScript, die ausgeführt wird, wenn der Block gerendert wird.

In diesem Beispiel führt JavaScript drei Hauptaktionen durch:

1. Fügt der CTA-Schaltfläche einen Ereignis-Listener hinzu, wodurch das Bild beim Bewegen des Mauszeigers vergrößert wird.
1. Fügt semantische CSS-Klassen zu den -Elementen des Blocks hinzu, die bei der Integration vorhandener CSS-Design-Systeme nützlich sind.
1. Fügt eine spezielle CSS-Klasse zu Absätzen hinzu, die mit `Terms and conditions:` beginnen.

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## CSS blockieren

Wenn Sie ein `teaser.css` im [ Kapitel erstellt haben, ](./7a-block-css.md) Sie es oder benennen Sie es in `teaser.css.bak` um, da dieses Kapitel anderes CSS für den Teaser-Block implementiert.

Erstellen Sie eine `teaser.css` Datei im Ordner des Blocks. Diese Datei enthält den CSS-Code, der den Block formatiert. Dieser CSS-Code zielt auf die -Elemente des Blocks und die spezifischen CSS-Semantikklassen ab, die vom JavaScript in `teaser.js` hinzugefügt wurden.

Bare Elemente können weiterhin direkt oder mit den benutzerdefinierten CSS-Klassen formatiert werden. Bei komplexeren Blöcken kann die Anwendung semantischer CSS-Klassen dazu beitragen, das CSS verständlicher und verwaltbarer zu machen, insbesondere wenn Sie mit größeren Teams über längere Zeiträume hinweg arbeiten.

[Wie zuvor ](./7a-block-css.md#develop-a-block-with-css) Sie CSS auf `.block.teaser` ein, um Konflikte mit anderen Blöcken zu vermeiden.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
}

/* The image is rendered to the first div in the block */
.block.teaser .image-wrapper {
    position: absolute;
    z-index: -1;
    inset: 0;
    box-sizing: border-box;
    overflow: hidden; 
}

.block.teaser .image {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
    transform: scale(1); 
    transition: transform 0.6s ease-in-out;
}

.block.teaser .content {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    background: var(--background-color);
    padding: 1.5rem 1.5rem 1rem;
    width: 80vw;
    max-width: 1200px;
}

.block.teaser .title {
    font-size: var(--heading-font-size-xl);
    margin: 0;
}

.block.teaser .title::after {
    border-bottom: 0;
}

.block.teaser p {
    font-size: var(--body-font-size-s);
    margin-bottom: 1rem;
    animation: teaser-fade-in .6s;
}

.block.teaser p.terms-and-conditions {
    font-size: var(--body-font-size-xs);
    color: var(--secondary-color);
    padding: .5rem 1rem;
    font-style: italic;
    border: solid var(--light-color);
    border-width: 0 0 0 10px;
}

/* Add underlines to links in the text */
.block.teaser a:hover {
    text-decoration: underline;
}

/* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
.block.teaser .button-container {
    margin: 0;
    padding: 0;
}

.block.teaser .button {   
    background-color: var(--primary-color);
    border-radius: 0;
    color: var(--dark-color);
    font-size: var(--body-font-size-xs);
    font-weight: bold;
    padding: 1em 2.5em;
    margin: 0;
    text-transform: uppercase;
}

.block.teaser .zoom {
    transform: scale(1.1);
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## Geschäftsbedingungen hinzufügen

Die obige Implementierung unterstützt jetzt speziell formatierte Absätze, die mit dem `Terms and conditions:` beginnen. Um diese Funktion im universellen Editor zu überprüfen, aktualisieren Sie den Textinhalt des Teaser-Blocks, sodass er Geschäftsbedingungen enthält.

Führen Sie die Schritte im Abschnitt [Erstellen eines Blocks](./6-author-block.md) aus und bearbeiten Sie den Text, um am Ende einen **Geschäftsbedingungen**-Absatz einzufügen:

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

Stellen Sie sicher, dass der Absatz in der lokalen Entwicklungsumgebung mit dem Stil Geschäftsbedingungen gerendert wird. Denken Sie daran, dass diese Code-Änderungen im universellen Editor erst dann widergespiegelt werden, wenn sie [in eine Verzweigung auf GitHub verschoben) ](#preview-in-universal-editor), für die der universelle Editor konfiguriert wurde.

## Vorschau der Entwicklung

Beim Hinzufügen von CSS und JavaScript lädt die lokale Entwicklungsumgebung der AEM-CLI die Änderungen neu, sodass schnell und einfach visualisiert werden kann, wie sich Code auf den Baustein auswirkt. Bewegen Sie den Mauszeiger über die CTA und überprüfen Sie, ob das Bild des Teasers ein- und ausgeblendet wird.

![Lokale Entwicklungsvorschau des Teasers mithilfe von CSS und JS](./assets/7b-block-js-css/local-development-preview.png)

## Code fusseln

Achten Sie darauf[ Ihren Code regelmäßig ](./3-local-development-environment.md#linting) ändern, um ihn sauber und konsistent zu halten. Regelmäßige Links helfen, Probleme frühzeitig zu erkennen und so die Entwicklungszeit zu verkürzen. Denken Sie daran, dass Sie Ihre Entwicklungsarbeit erst dann mit der `main` zusammenführen können, wenn alle Verknüpfungsprobleme behoben sind!

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## Vorschau im universellen Editor

Um Änderungen im universellen Editor von AEM anzuzeigen, fügen Sie sie hinzu, übertragen Sie sie und übertragen Sie sie in die Git-Repository-Verzweigung, die vom universellen Editor verwendet wird. Dadurch wird sichergestellt, dass die Blockimplementierung das Authoring-Erlebnis nicht beeinträchtigt.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

Jetzt können Sie die Änderungen im universellen Editor in der Vorschau anzeigen, wenn Sie den `?ref=teaser` Abfrageparameter hinzufügen.

![Teaser im universellen Editor](./assets/7b-block-js-css/universal-editor-preview.png)

