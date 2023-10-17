---
title: Best Practices für das Stilsystem mit AEM Sites
description: Ein ausführlicher Artikel, der die Best Practices für die Implementierung des Stilsystems mit Adobe Experience Manager Sites erläutert.
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '1536'
ht-degree: 100%

---

# Best Practices für das Stilsystem{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Lesen Sie den Inhalt unter [Grundlegendes zum Code für das Stilsystem](style-system-technical-video-understand.md) nach, um ein Verständnis der BEM-ähnlichen Konventionen zu gewährleisten, die das AEM-Stilsystem verwendet.

Es gibt zwei Hauptvarianten oder -stile, die für das AEM-Stilsystem implementiert sind:

* **Layout-Stile**
* **Anzeigestile**

**Layout-Stile** beeinflussen viele Elemente einer Komponente, um eine gut definierte und identifizierbare Ausgabedarstellung (Design und Layout) der Komponente zu erstellen, und richten sich häufig an einem bestimmten, wiederverwendbaren Markenkonzept aus. Beispielsweise kann eine Teaser-Komponente im herkömmlichen kartenbasierten Layout, einem horizontalen Werbestil oder als Hero-Layout-Überlagerungstext auf einem Bild dargestellt werden.

**Anzeigestile** werden verwendet, um geringfügige Änderungen an Layout-Stilen vorzunehmen. Sie ändern jedoch nicht die grundlegende Natur oder den Zweck des Layout-Stils. Beispielsweise kann ein Hero-Layout-Stil Anzeigestile aufweisen, für die sich das Farbschema vom primären Markenfarbschema in das sekundäre Markenfarbschema ändert.

## Best Practices für die Stilorganisation {#style-organization-best-practices}

Beim Definieren der Stilnamen, die AEM-Autorinnen und -Autoren zur Verfügung stehen, sollten Sie Folgendes tun:

* Benennen Sie Stile mithilfe eines für Autorinnen und Autoren verständlichen Vokabulars
* Minimieren Sie die Anzahl der Stiloptionen
* Zeigen Sie nur Stiloptionen und Kombinationen an, die nach Markenstandards zulässig sind
* Zeigen Sie nur Stilkombinationen mit einem Effekt an
   * Falls ineffektive Kombinationen angezeigt werden, stellen Sie sicher, dass diese zumindest keine schädlichen Auswirkungen haben.

Je mehr Stilkombinationen den AEM-Autorinnen und -Autoren zur Verfügung stehen, desto mehr Permutationen sind möglich, die durch Qualitätssicherung und Validierung auf Markenstandards geprüft werden müssen. Zu viele Optionen können Autorinnen und Autoren auch verwirren, da unklar sein kann, welche Option oder Kombination erforderlich ist, um den gewünschten Effekt zu erzielen.

### Stilnamen vs. CSS-Klassen {#style-names-vs-css-classes}

Stilnamen oder die Optionen, die AEM-Autorinnen und -Autoren angezeigt werden, und die implementierenden CSS-Klassennamen sind in AEM entkoppelt.

Dadurch können Stiloptionen in einem klaren Vokabular beschriftet werden und sind von den AEM-Autorinnen und -Autoren leicht zu verstehen, aber gleichzeitig können CSS-Entwicklungspersonen die CSS-Klassen zukunftssicher semantisch benennen. Zum Beispiel:

Eine Komponente soll über die Optionen verfügen, mit den **primären** und **sekundären** Markenfarben gefärbt zu werden, die AEM-Autorinnen und -Autoren kennen die Farben jedoch als **grün** und **gelb** anstatt als Entwurfssprache von primär und sekundär.

Das AEM-Stilsystem kann diese farbigen Anzeigestile mithilfe von den autorenfreundlichen Beschriftungen **Grün** und **Gelb** anzeigen, während es den CSS-Entwicklungspersonen ermöglicht, die semantische Benennung von `.cmp-component--primary-color` und `.cmp-component--secondary-color` zu nutzen, um die tatsächliche Stilimplementierung in CSS zu definieren.

Der Stilname **Grün** wird zu `.cmp-component--primary-color`, und **Gelb** zu `.cmp-component--secondary-color` zugeordnet.

Wenn sich die Markenfarbe des Unternehmens in Zukunft ändert, müssen nur die einzelnen Implementierungen von `.cmp-component--primary-color` und `.cmp-component--secondary-color` und die Stilnamen geändert werden.

## Die Teaser-Komponente als Anwendungsbeispiel {#the-teaser-component-as-an-example-use-case}

Im Folgenden finden Sie ein Anwendungsbeispiel für das Formatieren einer Teaser-Komponente mit mehreren verschiedenen Layout- und Anzeigestilen.

Dadurch wird untersucht, wie Stilnamen (angezeigt für Autorinnen und Autoren) und wie die unterstützenden CSS-Klassen organisiert sind.

### Konfiguration von Teaser-Komponentenstilen {#component-styles-configuration}

Die folgende Abbildung zeigt die Konfiguration der [!UICONTROL Stile] für die Teaser-Komponente für die im Anwendungsbeispiel behandelten Varianten.

Die Namen, das Layout und die Anzeige der [!UICONTROL Stilgruppe] stimmen zufällig mit den allgemeinen Konzepten der Anzeige- und Layout-Stile überein, die zur konzeptionellen Kategorisierung von Stiltypen in diesem Artikel verwendet werden.

Die Namen der [!UICONTROL Stilgruppe] und die Anzahl der [!UICONTROL Stilgruppen] sollte auf den Anwendungsfall der Komponente und die projektspezifischen Komponentenstil-Konventionen zugeschnitten sein.

Zum Beispiel hätte der Name der **Anzeige**-Stilgruppe auch **Farben** genannt werden können.

![Anzeige der Stilgruppe](assets/style-config.png)

### Stilauswahlmenü {#style-selection-menu}

Das folgende Bild zeigt das Manü [!UICONTROL Stil], mit dem Autorinnen und Autoren interagieren, um die passenden Stile für die Komponente auszuwählen. Beachten Sie, dass die [!UICONTROL Stil-GRPI-Namen] sowie die Stilnamen den Autorinnen und Autoren angezeigt werden.

![Stil-Dropdown-Menü](assets/style-menu.png)

### Standardstil {#default-style}

Der Standardstil ist oft der am häufigsten verwendete Stil der Komponente und die standardmäßige, nicht formatierte Ansicht des Teasers beim Hinzufügen zu einer Seite.

Je nachdem, wie verbreitet der Standardstil ist, kann CSS direkt auf den `.cmp-teaser` (ohne Modifikatoren) oder auf einen `.cmp-teaser--default` angewendet werden.

Wenn für alle Varianten überwiegend die standardmäßigen Stilregeln gelten, empfiehlt es sich, `.cmp-teaser` als CSS-Klassen des Standardstils zu verwenden, da alle Varianten diese implizit übernehmen sollten, vorausgesetzt, es werden BEM-ähnliche Konventionen befolgt. Ist dies nicht der Fall, sollten sie über den Standard-Modifikator angewendet werden, z. B. `.cmp-teaser--default`, der wiederum zum Feld [CSS-Standardklassen der Stilkonfiguration der Komponente](#component-styles-configuration) hinzugefügt werden muss, da diese Stilregeln andernfalls in jeder Variante überschrieben werden müssen.

Es ist sogar möglich, einen „benannten“ Stil als Standardstil zuzuweisen, z. B. den Hero-Stil `(.cmp-teaser--hero)` (unten definiert). Es ist jedoch klarer, wenn der Standardstil entsprechend der Implementierungen der CSS-Klassen `.cmp-teaser` oder `.cmp-teaser--default` implementiert wird.

>[!NOTE]
>
>Beachten Sie, dass der Standard-Layout-Stil KEINEN Anzeigestil-Namen aufweist. Die Autorin bzw. der Autor kann jedoch im AEM-Stilsystem-Auswahlwerkzeug eine Anzeigeoption auswählen.
>
>Dies verstößt gegen die Best Practice:
>
>**Zeigen Sie nur Stilkombinationen mit einem Effekt an**
>
>Wenn eine Autorin oder ein Autor den Anzeigestil **Grün** wählt, wird nichts passieren.
>
>In diesem Anwendungsfall wird diese Verletzung in Kauf genommen, da alle anderen Layout-Stile mithilfe der Markenfarben farbig sein müssen.
>
>Im Abschnitt **Werbung (rechts ausgerichtet)** unten erfahren Sie, wie Sie unerwünschte Stilkombinationen vermeiden können.

![Standardstil](assets/default.png)

* **Layout-Stil**
   * Standard
* **Anzeigestil**
   * Ohne
* **Effektive CSS-Klassen**: `.cmp-teaser--promo` oder `.cmp-teaser--default`

### Werbungsstil {#promo-style}

Der **Werbungs-Layout-Stil** wird verwendet, um hochwertige Inhalte auf der Site zu bewerben. Diese Inhalte werden in einem horizontalen Kasten auf der Web-Seite angeordnet und müssen mit Markenfarben formatierbar sein, wobei der standardmäßige Werbungs-Layout-Stil mit schwarzem Text verwendet wird.

Dazu werden im AEM-Stilsystem für die Teaser-Komponente ein **Werbungs**-**Layout-Stil** und die **Anzeigestile** **Grün** und **Gelb** konfiguriert.

#### Werbungs-Standard

![Werbungs-Standard](assets/promo-default.png)

* **Layout-Stil**
   * Stilname: **Werbung**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigestil**
   * Ohne
* **Effektive CSS-Klassen**: `.cmp-teaser--promo`

#### Werbungs-Primär

![Werbungs-Primär](assets/promo-primary.png)

* **Layout-Stil**
   * Stilname: **Werbung**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigestil**
   * Stilname: **Grün**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Werbungs-Sekundär

![Werbungs-Sekundär](assets/promo-secondary.png)

* **Layout-Stil**
   * Stilname: **Werbung**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigestil**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Rechtsbündiger Werbungsstil {#promo-r-align}

Der Layout-Stil **Werbung (rechtsbündig)** ist eine Variation des Werbungsstils, der die Position des Bildes und des Textes (Bild rechts, Text links) im Stil spiegelt.

Die rechtsbündige Ausrichtung ist im Grunde ein Anzeigestil, der in das AEM-Stilsystem als Anzeigestil eingegeben werden kann und in Verbindung mit dem Werbungs-Layout-Stil ausgewählt wird. Dies verstößt gegen folgende Best Practice:

**Zeigen Sie nur Stilkombinationen mit einem Effekt an**

…gegen die bereits bei [Standardstil](#default-style) verstoßen wurde.

Da sich die Rechtsbündigkeit nur auf den Werbungs-Layout-Stil und nicht auf die anderen beiden Layout-Stile „Standard“ und „Hero“ auswirkt, können wir den neuen Layout-Stil „Werbung (rechtsbündig)“ erstellen, der die CSS-Klasse enthält, die den Inhalt der Werbungs-Layout-Stile rechtsbündig ausrichtet: `cmp -teaser--alternate`.

Diese Kombination mehrerer Stile zu einem einzelnen Stileintrag kann auch dabei helfen, die Anzahl der verfügbaren Stile und Stilpermutationen zu reduzieren, die am besten minimiert werden sollte.

Beachten Sie, dass der Name der CSS-Klasse, `cmp-teaser--alternate`, nicht mit der autorenfreundlichen Nomenklatur von „rechtsbündig“ übereinstimmen muss.

#### Werbung rechtsbündig Standard

![Werbung rechtsbündig](assets/promo-alternate-default.png)

* **Layout-Stil**
   * Stilname: **Werbung (rechtsbündig)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigestil**
   * Ohne
* **Effektive CSS-Klassen**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Werbung rechtsbündig primär

![Werbung rechtsbündig primär](assets/promo-alternate-primary.png)

* **Layout-Stil**
   * Stilname: **Werbung (rechtsbündig)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigestil**
   * Stilname: **Grün**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Werbung rechtsbündig sekundär

![Werbung rechtsbündig sekundär](assets/promo-alternate-secondary.png)

* **Layout-Stil**
   * Stilname: **Werbung (rechtsbündig)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigestil**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Hero-Stil {#hero-style}

Der Hero-Layout-Stil zeigt das Bild der Komponenten als Hintergrund mit überlagerter Überschrift und Verknüpfung an. Der Hero-Layout-Stil, wie auch der Werbe-Layout-Stil, muss mit den Markenfarben gefärbt werden können.

Um den Hero-Layout-Stil mit Markenfarben zu färben, können dieselben Anzeigestile verwendet werden, die auch für den Werbe-Layout-Stil verwendet werden.

Pro Komponente wird der Stilname dem einzelnen Satz von CSS-Klassen zugeordnet. Das bedeutet, dass die CSS-Klassennamen, die den Hintergrund des Werbe-Layout-Stils farblich markieren, den Text und die Verknüpfung des Hero-Layout-Stils farblich markieren müssen.

Dies kann durch das Anwenden der CSS-Regeln leicht erreicht werden, es erfordert jedoch, dass die CSS-Entwicklungspersonen verstehen, wie diese Permutationen auf AEM angewendet werden.

CSS zum Färben des Hintergrunds des **Werbe**-Layout-Stils mit der primären (grünen) Farbe:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS zum Färben des Texts des **Hero**-Layout-Stils mit der primären (grünen) Farbe:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Standard

![Hero-Stil](assets/hero.png)

* **Layout-Stil**
   * Stilname: **Hero**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigestil**
   * Ohne
* **Effektive CSS-Klassen**: `.cmp-teaser--hero`

#### Hero primär

![Hero primär](assets/hero-primary.png)

* **Layout-Stil**
   * Stilname: **Werbung**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigestil**
   * Stilname: **Grün**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero sekundär

![Hero sekundär](assets/hero-secondary.png)

* **Layout-Stil**
   * Stilname: **Werbung**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigestil**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zum Stilsystem](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Erstellen von AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Dokumentations-Website zu BEM (Block Element Modifier)](https://getbem.com/)
* [Dokumentations-Website zu LESS](https://lesscss.org/)
* [jQuery-Website](https://jquery.com/)
