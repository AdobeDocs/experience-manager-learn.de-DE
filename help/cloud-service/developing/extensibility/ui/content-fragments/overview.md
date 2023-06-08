---
title: AEM Content Fragments-Erweiterungen
description: Erfahren Sie, wie Sie AEM as a Cloud Service Inhaltsfragmenterweiterungen erstellen und bereitstellen
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 14%

---

# Erweiterbarkeit AEM Inhaltsfragmente

AEM Benutzeroberfläche für Inhaltsfragmente ist eine leistungsstarke, erweiterbare Benutzeroberfläche für die Verwaltung der Erstellung, Verwaltung und Bearbeitung von Inhaltsfragmenten. Es stehen mehrere Erweiterungspunkte zur Verfügung, mit denen Sie die Benutzeroberfläche an Ihre Anforderungen anpassen können. Je nachdem, welche Benutzeroberfläche Sie erweitern, stehen verschiedene Erweiterungspunkte zur Verfügung.

## Erweiterungspunkte der Inhaltsfragmente-Konsole

Die Inhaltsfragmentkonsole in AEM (Adobe Experience Manager) ist eine Benutzeroberfläche, die einen zentralen Speicherort für die Verwaltung und Organisation von Inhaltsfragmenten bietet. Es bietet einen umfassenden Satz von Tools und Funktionen zum Erstellen, Bearbeiten, Veröffentlichen und Tracking von Inhaltsfragmenten, sodass Benutzer strukturierte Inhalte effizient über verschiedene Kanäle und Touchpoints verwalten können.

![Inhaltsfragmentkonsole](./assets/overview/cfc.png)

[AEM Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=de) ist die erweiterbare Benutzeroberfläche zum Auflisten und Verwalten von Inhaltsfragmenten. [AEM Erweiterungen der Inhaltsfragmentkonsole erstellt werden](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation?lang=de) mithilfe der `@adobe/aem-cf-admin-ui-ext-tpl` App Builder-Vorlage.

Die folgenden Erweiterungspunkte der Inhaltsfragmente-Konsole sind verfügbar:

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
              <p class="is-size-6">Aktionen anpassen, wenn mindestens ein Inhaltsfragment ausgewählt ist.</p>
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
          <p class="is-size-6">Passen Sie die Daten an, die in der Liste "Inhaltsfragmente"angezeigt werden.</p>
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
          <p class="is-size-6">Aktionen anpassen für den Fall, dass keine Inhaltsfragmente ausgewählt sind</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Erweiterungspunkte für den Inhaltsfragmente-Editor

Der Inhaltsfragment-Editor in AEM (Adobe Experience Manager) ist eine Komponente der Benutzeroberfläche, mit der Benutzer Inhaltsfragmente erstellen, bearbeiten und verwalten können. Es bietet eine visuell intuitive und benutzerfreundliche Umgebung für die Arbeit mit strukturierten Inhalten, die es Benutzern ermöglicht, Inhaltselemente zu definieren und zu organisieren, Vorlagen anzuwenden, Varianten zu verwalten und eine Vorschau der Inhalte über verschiedene Kanäle hinweg anzuzeigen. Der Inhaltsfragment-Editor optimiert den Prozess der Erstellung wiederverwendbarer und modularer Inhalte, die einfach über mehrere digitale Erlebnisse verteilt und veröffentlicht werden können.

![Inhaltsfragmente-Editor](./assets/overview/cfe.png)

AEM Inhaltsfragmente-Editor ist die erweiterbare Benutzeroberfläche zum Bearbeiten von Inhaltsfragmenten. [AEM Inhaltsfragment-Editor-Erweiterungen erstellt werden](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) mithilfe der `@adobe/aem-cf-editor-ui-ext-tpl` App Builder-Vorlage.

Die folgenden Erweiterungspunkte des Inhaltsfragmente-Editors sind verfügbar:

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
            <p class="is-size-6">Passen Sie Aktionen im Kopfzeilenmenü des Inhaltsfragment-Editors an.</p>
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
          <p class="is-size-6">Fügen Sie eine benutzerdefinierte Schaltfläche zum Rich-Text-Editor (RTE) des Inhaltsfragment-Editors hinzu.</p>
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
          <p class="is-size-6">Passen Sie Aktionen im RTE an, die an Tastenanschläge gebunden sind.</p>
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
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Rich-Text-Editor-Abzeichen" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Rich-Text-Editor-Abzeichen">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Rich-Text-Editor-Abzeichen" target="_blank" rel="referrer">Rich-Text-Editor-Abzeichen</a></p>
          <p class="is-size-6">Passen Sie nicht bearbeitbare Gestaltungsbausteine innerhalb des RTE an.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dokumente anzeigen</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Beispiele für Erweiterungen

Willkommen bei einer Sammlung von Codebeispielen für AEM Benutzeroberflächenerweiterbarkeit! Diese Ressource soll praktische Demonstrationen und Einblicke in die Erweiterung der Adobe Experience Manager-Benutzeroberfläche (AEM) bieten. Unabhängig davon, ob Sie Entwickler sind, die die Funktionalität von AEM verbessern möchten, dienen diese Codebeispiele als wertvolle Referenz.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Massenaktualisierung der Eigenschaften" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Massenaktualisierung der Eigenschaften">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Massenaktualisierung der Eigenschaften">Aktualisierung der Masseninhaltsfragment-Eigenschaft</a></p>
          <p class="is-size-6">Eine Aktionsleistenerweiterung der Inhaltsfragmentkonsole mit modaler und Adobe I/O Runtime-Aktion.</p>
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
          <a href="./examples/editor-export-to-xml.md" title="Exportieren in XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Exportieren in XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Exportieren in XML">Exportieren in XML</a></p>
          <p class="is-size-6">Exportieren Sie ein Inhaltsfragment als XML aus dem Inhaltsfragment-Editor.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
</div>
