---
title: Erweiterungen von AEM-Inhaltsfragmenten
description: Erfahren Sie, wie Sie Erweiterungen von Inhaltsfragmenten in AEM as a Cloud Service erstellen und bereitstellen
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 100%

---

# Erweiterbarkeit von AEM-Inhaltsfragmenten

Die Benutzeroberfläche für Inhaltsfragmente von AEM ist eine leistungsstarke, erweiterbare Benutzeroberfläche zum Erstellen, Verwalten und Bearbeiten von Inhaltsfragmenten.  Es stehen mehrere Erweiterungspunkte zur Verfügung, mit denen Sie die Benutzeroberfläche an Ihre Anforderungen anpassen können. Je nachdem, welche Benutzeroberfläche Sie erweitern, stehen verschiedene Erweiterungspunkte zur Verfügung.

## Erweiterungspunkte der Inhaltsfragmentkonsole

Die Inhaltsfragmentkonsole in AEM (Adobe Experience Manager) ist eine Benutzeroberfläche, die einen zentralen Ort für die Verwaltung und Organisation von Inhaltsfragmenten bietet. Sie bietet einen umfassenden Satz von Tools und Funktionen zum Erstellen, Bearbeiten, Veröffentlichen und Tracking von Inhaltsfragmenten, sodass Benutzende strukturierte Inhalte effizient über verschiedene Kanäle und Touchpoints hinweg verwalten können.

![Inhaltsfragmentkonsole](./assets/overview/cfc.png)

Die [AEM-Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=de) ist die erweiterbare Benutzeroberfläche zum Auflisten und Verwalten von Inhaltsfragmenten. [Erweiterungen der AEM-Inhaltsfragmentkonsole werden](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation?lang=de) mit der App-Entwicklungs-Vorlage `@adobe/aem-cf-admin-ui-ext-tpl` erstellt.

Die folgenden Erweiterungspunkte der Inhaltsfragmentkonsole sind verfügbar:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Aktionsleiste" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Aktionsleiste">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Aktionsleiste" target="_blank" rel="referrer">Aktionsleiste</a></p>
              <p class="is-size-6">Anpassung von Aktionen, wenn ein oder mehrere Inhaltsfragmente ausgewählt sind.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Rasterspalten" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Rasterspalten">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Rasterspalten" target="_blank" rel="referrer">Rasterspalten</a></p>
          <p class="is-size-6">Anpassung der Daten, die in der Inhaltsfragmentliste angezeigt werden.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Kopfzeilenmenü" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Kopfzeilenmenü">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Kopfzeilenmenü" target="_blank" rel="referrer">Kopfzeilenmenü</a></p>
          <p class="is-size-6">Anpassung der Aktionen für den Fall, dass keine Inhaltsfragmente ausgewählt sind.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Erweiterungspunkte für den Inhaltsfragmenteditor

Der Inhaltsfragmenteditor in AEM (Adobe Experience Manager) ist eine Komponente der Benutzeroberfläche, mit der Benutzende Inhaltsfragmente erstellen, bearbeiten und verwalten können. Er bietet eine visuell intuitive und benutzerfreundliche Umgebung für die Arbeit mit strukturierten Inhalten, die es Benutzenden ermöglicht, Inhaltselemente zu definieren und zu organisieren, Vorlagen anzuwenden, Varianten zu verwalten und eine Vorschau der Inhalte über verschiedene Kanäle hinweg anzuzeigen. Der Inhaltsfragmenteditor optimiert den Prozess der Erstellung wiederverwendbarer und modularer Inhalte, die einfach über mehrere digitale Erlebnisse hinweg verteilt und veröffentlicht werden können.

![Inhaltsfragmenteditor](./assets/overview/cfe.png)

Der AEM-Inhaltsfragmenteditor ist die erweiterbare Benutzeroberfläche zum Bearbeiten von Inhaltsfragmenten. [AEM-Inhaltsfragmenteditor-Erweiterungen werden](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) mit der App-Entwicklungs-Vorlage `@adobe/aem-cf-editor-ui-ext-tpl` erstellt.

Die folgenden Erweiterungspunkte für den Inhaltsfragmenteditor sind verfügbar:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="Kopfzeilenmenü" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Kopfzeilenmenü">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Kopfzeilenmenü" target="_blank" rel="referrer">Kopfzeilenmenü</a></p>
            <p class="is-size-6">Anpassung der Aktionen im Kopfzeilenmenü des Inhaltsfragmenteditors.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Rich-Text-Editor-Symbolleiste" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Rich-Text-Editor-Symbolleiste">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Rich-Text-Editor-Symbolleiste"  target="_blank" rel="referrer">Rich-Text-Editor-Symbolleiste</a></p>
          <p class="is-size-6">Hinzufügung einer benutzerdefinierten Schaltfläche zum Rich-Text-Editor (RTE) des Inhaltsfragmenteditors.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Rich-Text-Editor-Widgets" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Rich-Text-Editor-Widgets">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Rich-Text-Editor-Widgets" target="_blank" rel="referrer">Rich-Text-Editor-Widgets</a></p>
          <p class="is-size-6">Anpassung von Aktionen in RTE, die an Tastatureingaben gebunden sind.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Rich-Text-Editor-Badges" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Rich-Text-Editor-Badges">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Rich-Text-Editor-Badges" target="_blank" rel="referrer">Rich-Text-Editor-Badges</a></p>
          <p class="is-size-6">Anpassung nicht editierbarer, gestalteter Blöcke im RTE.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>
</div>

## Beispiele für Erweiterungen

Willkommen bei einer Sammlung von Erweiterungs-Code-Beispielen für die AEM-Benutzeroberfläche! Diese Ressource soll praktische Demonstrationen und Insights in die Erweiterung der Adobe Experience Manager-Benutzeroberfläche (AEM) bieten. Unabhängig davon, ob Sie zum Entwicklungs-Team gehören, das die Funktionalität von AEM verbessern möchte, dienen diese Code-Beispiele als wertvolle Referenz.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Stapelweise Aktualisierung von Eigenschaften" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Stapelweise Aktualisierung von Eigenschaften">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Stapelweise Aktualisierung der Eigenschaften">Stapelweise Aktualisierung einer Inhaltsfragment-Eigenschaft</a></p>
          <p class="is-size-6">Eine Aktionsleistenerweiterung der Inhaltsfragmentkonsole mit Modal und Adobe I/O Runtime-Aktion.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM">OpenAPI-Bildgenerierung</a></p>
                    <p class="is-size-6">Erkunden Sie das Beispiel einer Aktionsleistenerweiterung, die ein Bild mit OpenAI generiert, es in AEM hochlädt und die Bildeigenschaft für das ausgewählte Inhaltsfragment aktualisiert.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="Benutzerdefinierte Spalten" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="Benutzerdefinierte Spalten">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="Benutzerdefinierte Spalten">Benutzerdefinierte Spalten</a></p>
          <p class="is-size-6">Fügen Sie der Inhaltsfragmentkonsole eine benutzerdefinierte Spalte hinzu.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="In XML exportieren" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="In XML exportieren">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="In XML exportieren">In XML exportieren</a></p>
          <p class="is-size-6">Exportieren Sie ein Inhaltsfragment als XML aus dem Inhaltsfragmenteditor.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Schaltfläche in der Rich-Text-Editor-Symbolleiste" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Schaltfläche in der Rich-Text-Editor-Symbolleiste">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Schaltfläche in der Rich-Text-Editor-Symbolleiste">Schaltfläche in der Rich-Text-Editor-Symbolleiste</a></p>
          <p class="is-size-6">Fügen Sie im Inhaltsfragmenteditor benutzerdefinierte Symbolleistenschaltflächen zu RTE-Feldern hinzu.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="Rich-Text-Editor-Widget" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Rich-Text-Editor-Widget">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Rich-Text-Editor-Widget">Rich-Text-Editor-Widget</a></p>
          <p class="is-size-6">Fügen Sie dem Rich-Text-Editor im Inhaltsfragmenteditor Widgets hinzu.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Rich-Text-Editor-Badge" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Rich-Text-Editor-Badge">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Rich-Text-Editor-Badge">Rich-Text-Editor-Badge</a></p>
          <p class="is-size-6">Fügen Sie im Inhaltsfragmenteditor dem Rich-Text-Editor Badges hinzu.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
</a>
        </div>
      </div>
    </div>
  </div> 
</div>
