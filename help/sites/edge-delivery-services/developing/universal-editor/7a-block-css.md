---
title: Entwickeln eines Bausteins mit CSS
description: Entwickeln Sie einen Baustein mit CSS für Edge Delivery Services, der mit dem universellen Editor bearbeitet werden kann.
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
source-wordcount: '437'
ht-degree: 0%

---


# Entwickeln eines Bausteins mit CSS

Blöcke in Edge Delivery Services werden mit CSS formatiert. Die CSS-Datei für einen Block wird im Verzeichnis des Blocks gespeichert und hat denselben Namen wie der Block. Beispielsweise befindet sich die CSS-Datei für einen Block mit dem Namen `teaser` unter `blocks/teaser/teaser.css`.

Idealerweise sollte ein Block nur CSS für die Formatierung benötigen, ohne sich darauf zu verlassen, dass JavaScript das DOM ändert oder CSS-Klassen hinzufügt. Die Notwendigkeit von JavaScript hängt von der Inhaltsmodellierung [ Bausteins ](./5-new-block.md#block-model) seiner Komplexität ab. Bei Bedarf kann [JavaScript blockieren](./7b-block-js-css.md) hinzugefügt werden.

Bei einem reinen CSS-Ansatz werden die (meist) nackten semantischen HTML-Elemente des Bausteins zielgerichtet und formatiert.

## HTML blockieren

Um zu verstehen, wie Sie einen Baustein formatieren, überprüfen Sie zunächst das von Edge Delivery Services angezeigte DOM, da es für die Formatierung verfügbar ist. Das DOM finden Sie, indem Sie den Baustein überprüfen, der von der lokalen Entwicklungsumgebung der AEM-CLI bereitgestellt wird. Verwenden Sie nicht das DOM des universellen Editors, da es leicht abweicht.

>[!BEGINTABS]

>[!TAB DOM zu Stil]

Im Folgenden finden Sie das DOM des Teaser-Blocks, das das Ziel für die Formatierung ist.

Beachten Sie das `<p class="button-container">...`, das [automatisch erweitert](./4-website-branding.md#inferred-elements) als abgeleitetes Element von Edge Delivery Services JavaScript angezeigt wird.

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

Um das zu formatierende DOM zu finden, öffnen Sie die Seite mit dem nicht formatierten Block in Ihrer lokalen Entwicklungsumgebung, wählen Sie den Block aus und überprüfen Sie das DOM.

![Inspect-Block-DOM](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]

## CSS blockieren

Erstellen Sie eine neue CSS-Datei im -Ordner des Blocks, wobei der Name des Blocks als Dateiname verwendet wird. Beispiel: Für den **Teaser**-Block befindet sich die Datei unter `/blocks/teaser/teaser.css`.

Diese CSS-Datei wird automatisch geladen, wenn JavaScript eines Edge Delivery Services ein DOM-Element auf der Seite erkennt, das einen Teaser-Block darstellt.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in .6s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
}

/* The image is rendered to the first div in the block */
.block.teaser picture {
    position: absolute;
    z-index: -1;
    inset: 0;
    box-sizing: border-box;
}

.block.teaser picture img {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: 100%;
}

/** 
The teaser's text is rendered to the second (also the last) div in the block.

These styles are scoped to the second (also the last) div in the block (.block.teaser > div:last-child).

This div order can be used to target different styles to the same semantic elements in the block. 
For example, if the block has two images, we could target the first image with `.block.teaser > div:first-child img`, 
and the second image with `.block.teaser > div:nth-child(2) img`.
**/
.block.teaser > div:last-child {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    background: var(--background-color);
    padding: 1.5rem 1.5rem 1rem;
    width: 80vw;
    max-width: 1200px;
}

/** 
The following elements reside within `.block.teaser > div:last-child` and could be scoped as such, for example:

 .block.teaser > div:last-child p { .. }

However since these element can only appear in the second/last div per our block's model, it's unnecessary to add this additional scope.
**/

/* Regardless of the authored heading level, we only want one style the heading */
.block.teaser h1,
.block.teaser h2,
.block.teaser h3,
.block.teaser h4,
.block.teaser h5,
.block.teaser h6 {
    font-size: var(--heading-font-size-xl);
    margin: 0;
}

.block.teaser h1::after,
.block.teaser h2::after,
.block.teaser h3::after,
.block.teaser h4::after,
.block.teaser h5::after,
.block.teaser h6::after {
    border-bottom: 0;
}

.block.teaser p {
    font-size: var(--body-font-size-s);
    margin-bottom: 1rem;
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

## Vorschau der Entwicklung

Da das CSS in das Code-Projekt geschrieben wurde, werden die Änderungen durch die Hot-Neuladung der AEM-CLI übernommen, sodass schnell und einfach nachvollziehbar ist, wie sich das CSS auf den Block auswirkt.

![Nur CSS-Vorschau](./assets/7a-block-css/local-development-preview.png)

## Code fusseln

Achten Sie darauf[ dass Ihr Code häufig ](./3-local-development-environment.md#linting) wird, um sicherzustellen, dass er sauber und konsistent ist. Linting hilft, Probleme frühzeitig zu erkennen, und reduziert die gesamte Entwicklungszeit. Denken Sie daran, dass Sie Ihre Entwicklungsarbeit erst dann mit `main` zusammenführen können, wenn alle Ihre Verknüpfungsprobleme gelöst sind!

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:css
```

## Vorschau im universellen Editor

Um Änderungen im universellen Editor von AEM anzuzeigen, fügen Sie sie hinzu, übertragen Sie sie und übertragen Sie sie in die Git-Repository-Verzweigung, die vom universellen Editor verwendet wird. Mit diesem Schritt wird sichergestellt, dass die Blockimplementierung das Authoring-Erlebnis nicht beeinträchtigt.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add CSS-only implementation for teaser block"
$ git push origin teaser
```

Jetzt können Sie die Änderungen im universellen Editor in der Vorschau anzeigen, wenn Sie den `?ref=teaser` Abfrageparameter hinzufügen.

![Teaser im universellen Editor](./assets/7a-block-css/universal-editor-preview.png)

