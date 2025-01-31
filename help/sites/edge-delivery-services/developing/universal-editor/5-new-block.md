---
title: Erstellen eines Bausteins
description: Erstellen Sie einen Baustein für eine Edge Delivery Services-Website, die mit dem universellen Editor bearbeitet werden kann.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 775821f37df87905ea176b11ecf0ed4a42d00940
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Neuen Block erstellen

In diesem Kapitel wird der Prozess zum Erstellen eines neuen, bearbeitbaren Teaser-Blocks für eine Edge Delivery Services-Website mit dem universellen Editor beschrieben.

![Neuer Teaser-Block](./assets//5-new-block/teaser-block.png)

Der Block mit dem Namen `teaser` zeigt die folgenden Elemente:

- **Bild**: Ein visuell ansprechendes Bild.
- **Textinhalt**:
   - **Titel**: Eine überzeugende Überschrift, um den Fokus zu lenken.
   - **Haupttext**: Beschreibender Inhalt mit Kontext oder Details, einschließlich optionaler Bedingungen.
   - **Aktionsaufruf (CTA)-Schaltfläche**: Ein Link, der die Benutzerinteraktion anregt und sie zu weiterer Interaktion anleitet.

Der Inhalt des `teaser`-Blocks kann im universellen Editor bearbeitet werden, wodurch die Benutzerfreundlichkeit und Wiederverwendbarkeit auf der gesamten Website gewährleistet ist.

Beachten Sie, dass der `teaser` Block dem `hero` Block des Textbausteins ähnelt; daher soll `teaser` Block nur als einfaches Beispiel zur Veranschaulichung von Entwicklungskonzepten dienen.

## Erstellen einer neuen Git-Verzweigung

Um einen sauberen und gut organisierten Workflow aufrechtzuerhalten, erstellen Sie für jede spezifische Entwicklungsaufgabe eine neue Verzweigung. Dadurch werden Probleme bei der Bereitstellung von unvollständigem oder ungetestetem Code in der Produktion vermieden.

1. **Vom Hauptzweig starten**: Die Arbeit mit dem aktuellsten Produktions-Code gewährleistet eine solide Grundlage.
2. **Remote-Änderungen abrufen**: Durch das Abrufen der neuesten Aktualisierungen von GitHub wird sichergestellt, dass der aktuelle Code verfügbar ist, bevor mit der Entwicklung begonnen wird.
   - Beispiel: Rufen Sie nach dem Zusammenführen von Änderungen aus der `wknd-styles` Verzweigung in `main` die neuesten Aktualisierungen ab.
3. **Erstellen Sie eine neue Verzweigung**:

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

Sobald die `teaser` Verzweigung erstellt ist, können Sie mit der Entwicklung des Teaser-Blocks beginnen.

## Ordner blockieren

Erstellen Sie einen neuen Ordner mit dem Namen `teaser` im `blocks`. Dieser Ordner enthält die JSON-, CSS- und JavaScript-Dateien des Blocks, wobei die Dateien des Blocks an einem Speicherort organisiert werden:

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

Der Name des Blockordners fungiert als ID des Blocks und wird verwendet, um auf den Block während seiner Entwicklung zu verweisen.

## JSON blockieren

Die Block-JSON definiert drei wichtige Aspekte des Blocks:

- **Definition**: Registriert den Block als bearbeitbare Komponente im universellen Editor und verknüpft ihn mit einem Blockmodell und optional einem Filter.
- **Model**: Gibt die Authoring-Felder des Blocks an und legt fest, wie diese Felder als semantische Edge Delivery Services-HTML gerendert werden.
- **Filter**: Konfiguriert Filterregeln, um festzulegen, welchen Containern der Block über den universellen Editor hinzugefügt werden kann. Die meisten Blöcke sind keine Container, sondern ihre IDs werden den Filtern anderer Container-Blöcke hinzugefügt.

Erstellen Sie eine neue Datei in `/blocks/teaser/_teaser.json` mit der folgenden anfänglichen Struktur in der exakten Reihenfolge. Wenn die Schlüssel nicht in der richtigen Reihenfolge vorliegen, werden sie möglicherweise nicht ordnungsgemäß erstellt.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### Blockmodell

Das Blockmodell ist ein wichtiger Teil der Konfiguration des Blocks, da es Folgendes definiert:

1. Das Authoring-Erlebnis durch Definieren der Felder, die zur Bearbeitung verfügbar sind.

   ![Felder des universellen Editors](./assets/5-new-block/fields-in-universal-editor.png)

2. Wie die Feldwerte in Edge Delivery Services HTML gerendert werden.

Modellen wird ein `id` zugewiesen, der der Definition des [Blocks“ entspricht](#block-definition) und ein `fields`-Array zur Angabe der bearbeitbaren Felder enthält.

Jedes Feld im `fields`-Array verfügt über ein JSON-Objekt, das die folgenden erforderlichen Eigenschaften enthält:

| JSON-Eigenschaft | Beschreibung |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | Der [Feldtyp](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types) wie `text`, `reference` oder `aem-content`. |
| `name` | Der Name des Felds, das der JCR-Eigenschaft zugeordnet ist, in der der Wert in AEM gespeichert ist. |
| `label` | Die Beschriftung, die Autoren im universellen Editor angezeigt wird. |

Eine umfassende Liste der Eigenschaften, einschließlich optionaler, finden Sie in der [Dokumentation zu universellen Editor-Feldern](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields).

#### Blockentwurf

![Teaser-Block](./assets/5-new-block/block-design.png)

Der Teaser-Block enthält die folgenden bearbeitbaren Elemente:

1. **Image**: Stellt den visuellen Inhalt des Teasers dar.
2. **Textinhalt**: Enthält den Titel, den Textkörper und die Aktionsaufruf-Schaltfläche und befindet sich in einem weißen Rechteck.
   - **title** und **body text** können mit demselben Rich-Text-Editor erstellt werden.
   - Die **CTA** kann über ein `text` für das **label** und `aem-content` Feld für den **link** erstellt werden.

Das Design des Teaser-Blocks ist in diese beiden logischen Komponenten (Bild- und Textinhalt) unterteilt, sodass Benutzende ein strukturiertes und intuitives Authoring-Erlebnis erhalten.

### Felder blockieren

Definieren Sie die für den Block erforderlichen Felder: Bild, Bild, Alternativtext, Text, CTA-Kennzeichnung und CTA-Link.

>[!BEGINTABS]

>[!TAB Der richtige Weg]

**Diese Registerkarte veranschaulicht die richtige Methode zum Modellieren des Teaser-Blocks.**

Der Teaser besteht aus zwei logischen Bereichen: Bild und Text. Um den Code zu vereinfachen, der zum Anzeigen der Edge Delivery Services-HTML als gewünschtes Web-Erlebnis erforderlich ist, sollte das Blockmodell diese Struktur widerspiegeln.

- Gruppieren Sie **Bild** und **Bild-Alternativtext** mithilfe von [Feld reduzieren](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).
- Gruppieren Sie die Textinhaltsfelder mithilfe von [Elementgruppierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) und [Feld reduzieren für die CTA](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).

Wenn Sie nicht mit [Feldreduzierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse), [Elementgruppierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) oder [Typrückschluss“ vertraut sind, ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) Sie die verknüpfte Dokumentation, bevor Sie fortfahren, da diese für die Erstellung eines gut strukturierten Blockmodells unerlässlich sind.

Im folgenden Beispiel:

- [Typrückschluss](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) wird verwendet, um automatisch ein `<img>` HTML-Element aus dem `image` zu erstellen. Das Reduzieren von Feldern wird zusammen mit den `image`- und `imageAlt`-Feldern verwendet, um ein `<img>` HTML-Element zu erstellen. Das `src`-Attribut wird auf den Wert des `image`-Felds festgelegt, während das `alt`-Attribut auf den Wert des `imageAlt`-Felds festgelegt wird.
- `textContent` ist ein Gruppenname, mit dem Felder kategorisiert werden. Er sollte semantisch sein, kann aber alles sein, was für diesen Block einzigartig ist. Dadurch wird der universelle Editor angewiesen, alle Felder mit diesem Präfix im selben `<div>` in der endgültigen HTML-Ausgabe zu rendern.
- Das Reduzieren von Feldern wird auch innerhalb der `textContent` für den Aktionsaufruf (CTA) angewendet. Die CTA wird als `<a>` über [Typrückschluss](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) erstellt. Das Feld `cta` wird verwendet, um das `href` des `<a>` festzulegen, und das Feld `ctaText` liefert den Textinhalt für den Link innerhalb der `<a ...>` Tags.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Dieses Modell definiert die Autoreneingaben im universellen Editor für den Block.

Der resultierende Edge Delivery Services HTML für diesen Baustein legt das Bild in den ersten Div und die Elementgruppe `textContent` Felder in den zweiten Div.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

Wie [ nächsten Kapitel gezeigt, ](./7a-block-css.md) diese HTML-Struktur die Gestaltung des Bausteins als zusammenhängende Einheit.

Informationen zu den Folgen, die sich ergeben, wenn Sie das Reduzieren von Feldern und die Elementgruppierung nicht verwenden **finden Sie oben auf** Registerkarte „Falsche Vorgehensweise“.

>[!TAB Der falsche Weg]

**Diese Registerkarte veranschaulicht eine suboptimale Methode zum Modellieren des Teaser-Blocks und ist nur eine Nebeneinanderstellung auf die richtige Weise.**

Die Definition jedes Felds als eigenständiges Feld im Blockmodell ohne Verwendung von [Feld reduzieren](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) und [Elementgruppierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) mag verlockend erscheinen. Diese Aufsicht erschwert jedoch die Gestaltung des Blocks als zusammenhängende Einheit.

Beispielsweise könnte das Teaser-Modell wie folgt **ohne** Felderreduzierung oder Elementgruppierung definiert werden:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Die Edge Delivery Services-HTML für den -Block rendert den Wert jedes Felds in einem separaten `div`, wodurch das Inhaltsverständnis, die Stilanwendung und die Anpassungen der HTML-Struktur kompliziert werden, um das gewünschte Design zu erzielen.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

Jedes Feld ist in seiner eigenen `div` isoliert, sodass es schwierig ist, Bild- und Textinhalte als zusammenhängende Einheiten zu formatieren. Das gewünschte Design mit Mühe und Kreativität zu erreichen, ist möglich, aber die Verwendung von [Elementgruppierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) zum Gruppieren von Textinhaltsfeldern und [Felderreduzierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) zum Hinzufügen von erstellten Werten als Elementattribute ist einfacher, einfacher und semantisch korrekt.

Informationen **besseren Modellierung des Teaser-Blocks finden Sie** der obigen Registerkarte „Schreibmethode“.

>[!ENDTABS]


### Blockdefinition

Die Blockdefinition registriert den Block im universellen Editor. Im Folgenden finden Sie eine Aufschlüsselung der JSON-Eigenschaften, die in der Blockdefinition verwendet werden:

| JSON-Eigenschaft | Beschreibung |
|---------------|-------------|
| `definition.title` | Der Titel des Blocks, wie er in den „Hinzufügen“-Blöcken **universellen Editors** wird. |
| `definition.id` | Eine eindeutige ID für den Block, mit der dessen Verwendung in `filters` gesteuert wird. |
| `definition.plugins.xwalk.page.resourceType` | Definiert den Sling-Ressourcentyp für das Rendern der Komponente im universellen Editor. Verwenden Sie immer einen `core/franklin/components/block/v#/block` Ressourcentyp. |
| `definition.plugins.xwalk.page.template.name` | Der Name des Blocks. Sie sollte in Kleinbuchstaben geschrieben und mit Bindestrichen versehen werden, damit sie zum Ordnernamen des Blocks passt. Dieser Wert wird auch zur Kennzeichnung der Instanz des Blocks im universellen Editor verwendet. |
| `definition.plugins.xwalk.page.template.model` | Verknüpft diese Definition mit ihrer `model`, die die im universellen Editor für den Block angezeigten Bearbeitungsfelder steuert. Der Wert hier muss mit einem `model.id` Wert übereinstimmen. |
| `definition.plugins.xwalk.page.template.classes` | Optionale -Eigenschaft, deren Wert dem `class` des Block-HTML-Elements hinzugefügt wird. Dies ermöglicht Varianten desselben Blocks. Der `classes` kann durch Hinzufügen [ Klassenfelds ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options) das -Modell des [ bearbeitbar ](#block-model). |


Im Folgenden finden Sie ein Beispiel-JSON für die Blockdefinition:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

In diesem Beispiel:

- Der Block trägt den Namen „Teaser“ und verwendet das `teaser`, das bestimmt, welche Felder im universellen Editor zur Bearbeitung verfügbar sind.
- Der Block enthält Standardinhalt für das Feld `textContent_text` , das ein Rich-Text-Bereich für den Titel und den Textkörper ist, sowie `textContent_cta` und `textContent_ctaText` für den CTA-Link (Aktionsaufruf) und die Kennzeichnung. Die Feldnamen der Vorlage, die den anfänglichen Inhalt enthalten, stimmen mit den Feldnamen überein, die im Feld-Array [Inhaltsmodells“ definiert ](#block-model).

Durch diese Struktur wird sichergestellt, dass der Block im universellen Editor mit den richtigen Feldern, dem Inhaltsmodell und dem Ressourcentyp für die Wiedergabe eingerichtet wird.

### Filter blockieren

Das `filters`-Array des Blocks definiert für [Container-Blöcke](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container) welche anderen Blöcke zum Container hinzugefügt werden können. Filter definieren eine Liste von Block-IDs (`model.id`), die dem Container hinzugefügt werden können.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

Die Teaser-Komponente ist kein [Container-Block](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), d. h. Sie können ihr keine anderen Blöcke hinzufügen. Daher bleibt das `filters`-Array leer. Fügen Sie stattdessen die Teaser-ID zur Filterliste des Abschnittsblocks hinzu, sodass der Teaser zu einem Abschnitt hinzugefügt werden kann.

![Filter blockieren](./assets/5-new-block/filters.png)

Von Adobe bereitgestellte Blöcke, wie der Abschnittsblock, speichern Filter im `models` des Projekts. Suchen Sie zum Anpassen die JSON-Datei für den von Adobe bereitgestellten Block (z. B. `/models/_section.json`) und fügen Sie die Teaser-ID (`teaser`) zur Filterliste hinzu. Die Konfiguration signalisiert dem universellen Editor, dass die Teaser-Komponente zum Abschnitt-Container-Block hinzugefügt werden kann.

[!BADGE /models/_section.json]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

Die Teaser-Blockdefinitions-ID `teaser` wird dem `components`-Array hinzugefügt.

## Verknüpfen von JSON-Dateien

Achten Sie darauf[ Ihre Änderungen häufig ](./3-local-development-environment.md#linting), um sicherzustellen, dass sie sauber und konsistent sind. Häufig hilft Linting, Probleme frühzeitig zu erkennen, und reduziert die gesamte Entwicklungszeit. Der `npm run lint:js` Befehl verknüpft auch JSON-Dateien und fängt Syntaxfehler ab.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## Projekt-JSON erstellen

Nach der Konfiguration der JSON-Blockdateien (`blocks/teaser/_teaser.json`, `models/_section.json`) müssen sie in die `component-models.json`-, `component-definitions.json`- und `component-filters.json` des Projekts kompiliert werden. Die Kompilierung erfolgt durch Ausführen der npm-Skripte [build JSON](./3-local-development-environment.md#build-json-fragments) des Projekts.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run build:json
```

## Bereitstellen der Blockdefinition

Um den Block im universellen Editor verfügbar zu machen, muss das Projekt übertragen und an die Verzweigung des GitHub-Repositorys gepusht werden, in diesem Fall an die `teaser`.

Der genaue Name der Verzweigung, den der universelle Editor verwendet, kann für jede Benutzerin und jeden Benutzer über die URL des universellen Editors angepasst werden.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
$ git push origin teaser
```

Wenn der universelle Editor mit dem Abfrageparameter `?ref=teaser` geöffnet wird, wird der neue `teaser`-Block in der Blockpalette angezeigt. Beachten Sie, dass der Baustein keine Formatierung aufweist; er rendert die Felder des Bausteins als semantisches HTML, das nur über das [globale CSS](./4-website-branding.md#global-css) formatiert wird.
