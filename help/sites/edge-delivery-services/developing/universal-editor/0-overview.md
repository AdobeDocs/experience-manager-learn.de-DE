---
title: Entwickler-Tutorial für Edge Delivery Services und den universellen Editor
description: Lernen Sie die Grundlagen der Entwicklung einer neuen Website kennen, die im universellen Editor von AEM verfasst und mit Edge Delivery Services bereitgestellt wird.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Catalog
jira: KT-15832
duration: 88
exl-id: aeac08a2-75a0-4adb-b32e-0e7f85e7eb1d
source-git-commit: 52b7e6afbfe448fd350e84c3e8987973c87c4718
workflow-type: ht
source-wordcount: '586'
ht-degree: 100%

---

# Entwickler-Tutorial für Edge Delivery Services und den universellen Editor

![Entwickler-Tutorial für Edge Delivery Services und den universellen Editor](./assets/0-overview/hero.png)

In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer AEM-Website kennen, die leistungsstarkes Authoring mit dem universellen Editor und eine blitzschnelle Bereitstellung mithilfe von Edge Delivery Services kombinieren. Am Ende werden Sie über grundlegende Kenntnisse zum Erstellen eines neuen Projekts, zum Einrichten einer lokalen Entwicklungsumgebung und zum Erstellen eines neuen Blocks verfügen.

## Projekt-Setup

Erfahren Sie, wie Sie in AEM as a Cloud Service ein Code-Projekt erstellen und eine neue Site konfigurieren. Dieses Setup ermöglicht eine nahtlose Entwicklung mit dem universellen Editor für die Inhaltserstellung und die schnelle Inhaltsbereitstellung über Edge Delivery Services.

<!-- CARDS 

* ./1-new-code-project.md
* ./2-new-aem-site.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a code project">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./1-new-code-project.md" title="Erstellen eines Code-Projekts" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/1-new-project/new-project.png" alt="Erstellen eines Code-Projekts"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./1-new-code-project.md" target="_blank" rel="referrer" title="Erstellen eines Code-Projekts">Erstellen eines Code-Projekts</a>
                    </p>
                    <p class="is-size-6">Erstellen Sie ein Code-Projekt für Edge Delivery Services, das mit dem universellen Editor bearbeitet werden kann.</p>
                </div>
                <a href="./1-new-code-project.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create an AEM site">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./2-new-aem-site.md" title="Erstellen einer AEM-Site" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/2-new-aem-site/new-site.png" alt="Erstellen einer AEM-Site"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./2-new-aem-site.md" target="_blank" rel="referrer" title="Erstellen einer AEM-Site">Erstellen einer AEM-Site</a>
                    </p>
                    <p class="is-size-6">Erstellen Sie eine Site in AEM Sites für Edge Delivery Services, die mit dem universellen Editor bearbeitet werden kann.</p>
                </div>
                <a href="./2-new-aem-site.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Entwicklungs-Setup

Erfahren Sie, wie Sie Ihre lokale Entwicklungsumgebung für eine schnelle Website-Entwicklung konfigurieren. Dieses Setup ermöglicht eine nahtlose Site-Erstellung mit dem universellen Editor und eine effiziente Inhaltsbereitstellung über Edge Delivery Services, wodurch ein reibungsloser und optimierter Entwicklungs-Workflow gewährleistet wird.
<!-- CARDS 

* ./3-local-development-environment.md
* ./4-website-branding.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up a local development environment">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./3-local-development-environment.md" title="Einrichten einer lokalen Entwicklungsumgebung" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3443978/?format=jpeg&nocache=1741027443737" alt="Einrichten einer lokalen Entwicklungsumgebung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./3-local-development-environment.md" target="_blank" rel="referrer" title="Einrichten einer lokalen Entwicklungsumgebung">Einrichten einer lokalen Entwicklungsumgebung</a>
                    </p>
                    <p class="is-size-6">Richten Sie eine lokale Entwicklungsumgebung für Sites ein, die mit Edge Delivery Services bereitgestellt werden und mit dem universellen Editor bearbeitet werden können.</p>
                </div>
                <a href="./3-local-development-environment.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Add website branding">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./4-website-branding.md" title="Hinzufügen von Website-Branding" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/4-website-branding/github-issues.png" alt="Hinzufügen von Website-Branding"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./4-website-branding.md" target="_blank" rel="referrer" title="Hinzufügen von Website-Branding">Hinzufügen von Website-Branding</a>
                    </p>
                    <p class="is-size-6">Definieren Sie globales CSS, CSS-Variablen und Web-Schriften für eine Edge Delivery Services-Site.</p>
                </div>
                <a href="./4-website-branding.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Blockentwicklung

Erfahren Sie, wie Sie einen neuen Block erstellen, indem Sie sein Inhaltsmodell definieren und Beispielinhalte für Tests und Entwicklung einrichten. Erkunden Sie zwei Methoden zum Rendern des Blocks und erfahren Sie, wie Sie ihn strukturieren, um eine optimale Leistung und Flexibilität in AEM und Edge Delivery Services zu erzielen.

<!-- CARDS 

* ./5-new-block.md {image = ./assets/5-new-block/card.png}
* ./6-author-block.md {image = ./assets/6-author-block/card.png}
* ./7a-block-css.md {image = ./assets/7a-block-css/card.png}
* ./7b-block-js-css.md {image = ./assets/7b-block-js-css/card.png}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./5-new-block.md" title="Erstellen eines Blocks" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/5-new-block/card.png" alt="Erstellen eines Blocks"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./5-new-block.md" target="_blank" rel="referrer" title="Erstellen eines Blocks">Erstellen eines Blocks</a>
                    </p>
                    <p class="is-size-6">Erstellen Sie einen Block für eine Edge Delivery Services-Website, die mit dem universellen Editor bearbeitet werden kann.</p>
                </div>
                <a href="./5-new-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Author a block">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./6-author-block.md" title="Erstellen eines Blocks" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/6-author-block/card.png" alt="Erstellen eines Blocks"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./6-author-block.md" target="_blank" rel="referrer" title="Erstellen eines Blocks">Erstellen eines Blocks</a>
                    </p>
                    <p class="is-size-6">Erstellen Sie einen Edge Delivery Services-Block mit dem universellen Editor.</p>
                </div>
                <a href="./6-author-block.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7a-block-css.md" title="Entwickeln eines Blocks mit CSS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7a-block-css/card.png" alt="Entwickeln eines Blocks mit CSS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7a-block-css.md" target="_blank" rel="referrer" title="Entwickeln eines Blocks mit CSS">Entwickeln eines Blocks mit CSS</a>
                    </p>
                    <p class="is-size-6">Entwickeln Sie einen Block mit CSS für Edge Delivery Services, der mit dem universellen Editor bearbeitet werden kann.</p>
                </div>
                <a href="./7a-block-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Develop a block with CSS and JS">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./7b-block-js-css.md" title="Entwickeln eines Blocks mit CSS und JS" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/7b-block-js-css/card.png" alt="Entwickeln eines Blocks mit CSS und JS"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./7b-block-js-css.md" target="_blank" rel="referrer" title="Entwickeln eines Blocks mit CSS und JS">Entwickeln eines Blocks mit CSS und JS</a>
                    </p>
                    <p class="is-size-6">Entwickeln Sie einen Block mit CSS und JavaScript für Edge Delivery Services, der mit dem universellen Editor bearbeitet werden kann.</p>
                </div>
                <a href="./7b-block-js-css.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Nächste Schritte

Nachdem Sie dieses Tutorial nun abgeschlossen haben, bauen Sie auf dem auf, was Sie mit diesen zielgerichteten Anleitungen gelernt haben. In diesen Handbüchern wird näher auf den hier behandelten Code und die hier behandelten Konzepte eingegangen. Es werden rollenspezifische Anwendungsfälle, erweiterte Techniken und zusätzliche Tipps zur Verbesserung der Entwicklungsfähigkeiten von Edge Delivery Services und dem universellen Editor untersucht.

<!-- CARDS 

* ./how-to/block-options.md
* ./how-to/header-and-footer.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Block options">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/block-options.md" title="Blockoptionen" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/block-options/main.png" alt="Blockoptionen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/block-options.md" target="_blank" rel="referrer" title="Blockoptionen">Blockoptionen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie einen Block mit mehreren Anzeigeoptionen erstellen.</p>
                </div>
                <a href="./how-to/block-options.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header and Footer">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/header-and-footer.md" title="Kopf- und Fußzeile" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="how-to/assets/header-and-footer/hero.png" alt="Kopf- und Fußzeile"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" title="Kopf- und Fußzeile">Kopf- und Fußzeile</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Kopf- und Fußzeilen in Edge Delivery Services und im universellen Editor verwendet werden.</p>
                </div>
                <a href="./how-to/header-and-footer.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
