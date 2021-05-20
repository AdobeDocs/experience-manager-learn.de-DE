---
title: Best Practices für Stilsysteme mit AEM Sites
description: Ein ausführlicher Artikel, der die Best Practices für die Implementierung des Stilsystems mit Adobe Experience Manager Sites erläutert.
feature: Stilsystem
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1541'
ht-degree: 3%

---


# Best Practices für Stilsysteme{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Lesen Sie den Inhalt unter [Grundlegendes zum Code für das Stilsystem](style-system-technical-video-understand.md), um ein Verständnis der BEM-ähnlichen Konventionen sicherzustellen, die vom AEM Stilsystem verwendet werden.

Es gibt zwei Hauptformate oder -stile, die für das AEM Stilsystem implementiert sind:

* **Layoutstile**
* **Anzeigestile**

**Layout-** Stile wirken sich auf viele Elemente einer Komponente aus, um eine gut definierte und identifizierbare Ausgabedarstellung (Design und Layout) der Komponente zu erstellen und oft an einem bestimmten wiederverwendbaren Markenkonzept auszurichten. Beispielsweise kann eine Teaser-Komponente im herkömmlichen kartenbasierten Layout, einem horizontalen Promotionstil oder als Hero-Layout-Überlagerungstext für ein Bild dargestellt werden.

**Anzeigestile** wirken sich nur auf geringfügige Änderungen an Layout-Stilen aus. Sie ändern jedoch nicht die grundlegende Natur oder den Zweck des Layoutstils. Beispielsweise kann ein Hero-Layoutstil Anzeigestile aufweisen, die das Farbschema vom primären Markenfarbschema in das sekundäre Markenfarbschema ändern.

## Best Practices für die Stilorganisation {#style-organization-best-practices}

Beim Definieren der Stilnamen, die AEM Autoren zur Verfügung stehen, sollten Sie Folgendes tun:

* Benennen von Stilen mithilfe eines von den Autoren verständlichen Vokabulars
* Minimieren der Anzahl der Stiloptionen
* Zeigt nur Stiloptionen und Kombinationen an, die nach Markenstandards zulässig sind
* Zeigt nur Stilkombinationen an, die einen Effekt haben
   * Werden ineffektive Kombinationen ausgesetzt, stellen Sie sicher, dass diese zumindest keine schädlichen Auswirkungen haben.

Je mehr Stilkombinationen AEM Autoren zur Verfügung stehen, desto mehr Permutationen müssen durch Qualitätssicherung und Validierung anhand von Markenstandards vorgenommen werden. Zu viele Optionen können Autoren auch verwirren, da unklar werden kann, welche Option oder Kombination erforderlich ist, um den gewünschten Effekt zu erzielen.

### Stilnamen vs. CSS-Klassen {#style-names-vs-css-classes}

Stilnamen oder die Optionen, die AEM Autoren angezeigt werden, und die implementierenden CSS-Klassennamen werden in AEM entkoppelt.

Dadurch können Stiloptionen in einem Vokabular deutlich beschriftet und von den AEM-Autoren verstanden werden, aber CSS-Entwickler können die CSS-Klassen auf zukunftssichere, semantische Weise benennen. Beispiel:

Eine Komponente muss über die Optionen verfügen, die mit den Farben **primary** und **secondary** der Marke farbig dargestellt werden. Die AEM Autoren kennen die Farben jedoch als **green** und **gelb** und nicht als Designsprache der primären und sekundären Komponente.

Das AEM Stilsystem kann diese farbigen Anzeigestile mithilfe der Authoring-freundlichen Beschriftungen **Green** und **Gelb** verfügbar machen, während es den CSS-Entwicklern ermöglicht, die semantische Benennung von `.cmp-component--primary-color` und `.cmp-component--secondary-color` zu verwenden, um die tatsächliche Stilimplementierung in CSS zu definieren.

Der Stilname von **Green** wird `.cmp-component--primary-color` und **Gelb** `.cmp-component--secondary-color` zugeordnet.

Wenn sich die Markenfarbe des Unternehmens in Zukunft ändert, müssen nur die einzelnen Implementierungen von `.cmp-component--primary-color` und `.cmp-component--secondary-color` sowie die Stilnamen geändert werden.

## Die Teaser-Komponente als Beispielanwendungsfall {#the-teaser-component-as-an-example-use-case}

Im Folgenden finden Sie ein Anwendungsbeispiel für das Formatieren einer Teaser-Komponente mit mehreren verschiedenen Layout- und Anzeigestilen.

Dadurch wird untersucht, wie Stilnamen (für Autoren verfügbar gemacht) und wie die unterstützenden CSS-Klassen organisiert sind.

### Konfiguration der Teaser-Komponentenstile {#component-styles-configuration}

Die folgende Abbildung zeigt die [!UICONTROL Styles]-Konfiguration für die Teaser-Komponente für die im Anwendungsfall behandelten Varianten.

Die Namen, das Layout und die Anzeige der [!UICONTROL Stilgruppe] entsprechen glücklicherweise den allgemeinen Konzepten von Anzeigestilen und Layoutstilen, die zur konzeptionellen Kategorisierung von Stiltypen in diesem Artikel verwendet werden.

Die Namen [!UICONTROL Stilgruppe] und die Anzahl der [!UICONTROL Stilgruppen] sollten auf den Anwendungsfall der Komponente und die projektspezifischen Komponentenstil-Konventionen zugeschnitten sein.

Beispielsweise könnte der Stilgruppenname **Display** **Colors** heißen.

![Stilgruppe anzeigen](assets/style-config.png)

### Stilauswahl-Menü {#style-selection-menu}

Das folgende Bild zeigt die [!UICONTROL Style]-Menüautoren, mit denen interagiert wird, um die entsprechenden Stile für die Komponente auszuwählen. Beachten Sie, dass die Namen [!UICONTROL Style Grpi] sowie die Stilnamen dem Autor angezeigt werden.

![Dropdown-Menü &quot;Stil&quot;](assets/style-menu.png)

### Standardstil {#default-style}

Der Standardstil ist häufig der am häufigsten verwendete Stil der Komponente und die standardmäßige, nicht formatierte Ansicht des Teasers beim Hinzufügen zu einer Seite.

Je nach der Gemeinsamkeit des Standardstils kann das CSS direkt auf das `.cmp-teaser` (ohne Modifikatoren) oder auf ein `.cmp-teaser--default` angewendet werden.

Wenn die standardmäßigen Stilregeln häufiger als nicht für alle Varianten gelten, empfiehlt es sich, `.cmp-teaser` als CSS-Klassen des Standardstils zu verwenden, da alle Varianten diese implizit übernehmen sollten, vorausgesetzt, BEM-ähnliche Konventionen werden eingehalten. Andernfalls sollten sie über den Standardmodifikator angewendet werden, z. B. `.cmp-teaser--default`, der wiederum zum Feld [CSS-Standardklassen](#component-styles-configuration) der Komponentenstil-Konfiguration hinzugefügt werden muss. Andernfalls müssen diese Stilregeln in jeder Variante überschrieben werden.

Es ist sogar möglich, einen &quot;benannten&quot;Stil als Standardstil zuzuweisen, z. B. den unten definierten Hero-Stil `(.cmp-teaser--hero)`. Es ist jedoch klarer, den Standardstil für die CSS-Klassenimplementierungen `.cmp-teaser` oder `.cmp-teaser--default` zu implementieren.

>[!NOTE]
>
>Beachten Sie, dass der Standard-Layoutstil KEINEN Anzeigestil-Namen hat. Der Autor kann jedoch im AEM Stilsystem-Auswahlwerkzeug eine Anzeigeoption auswählen.
>
>Dies verstößt gegen die Best Practice:
>
>**Zeigt nur Stilkombinationen an, die einen Effekt haben**
>
>Wenn ein Autor den Anzeigestil von **Green** auswählt, wird nichts passieren.
>
>In diesem Anwendungsfall wird diese Verletzung zugegeben, da alle anderen Layoutstile mithilfe der Markenfarben farbig sein müssen.
>
>Im Abschnitt **Angebot (rechts ausgerichtet)** unten sehen wir, wie wir unerwünschte Stilkombinationen verhindern können.

![Standardstil](assets/default.png)

* **Layout-Stil**
   * Default
* **Anzeigeformat**
   * Kein
* **Effektive CSS-Klassen**:  `.cmp-teaser--promo` oder  `.cmp-teaser--default`

### Angebotsstil {#promo-style}

Der Layoutstil **Promo** wird verwendet, um hochwertige Inhalte auf der Site zu bewerben. Er ist horizontal so ausgelegt, dass er einen Bereich Platz auf der Webseite aufnimmt, und muss mit Markenfarben formatiert sein, wobei der standardmäßige Layoutstil Promo mit schwarzem Text verwendet wird.

Um dies zu erreichen, werden ein **Layoutstil** von **Promo** und die **Anzeigestile** von **Green** und **Gelb** im AEM Stilsystem für die Teaser-Komponente konfiguriert.

#### Promo-Standard

![promo default](assets/promo-default.png)

* **Layout-Stil**
   * Stilname: **Promo**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigeformat**
   * Kein
* **Effektive CSS-Klassen**:  `.cmp-teaser--promo`

#### Promo Primär

![promo primary](assets/promo-primary.png)

* **Layout-Stil**
   * Stilname: **Promo**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigeformat**
   * Stilname: **Green**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### Promo Sekundär

![Promo Sekundär](assets/promo-secondary.png)

* **Layout-Stil**
   * Stilname: **Promo**
   * CSS-Klasse: `cmp-teaser--promo`
* **Anzeigeformat**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### Angebot Rechter Stil {#promo-r-align}

Der Layout-Stil **Angebot rechts ausgerichtet** ist eine Variante des Angebotsstils, der die Position des Bildes und Textes (Bild rechts, Text links) im Stil spiegelt.

Die richtige Ausrichtung im Kern ist ein Anzeigestil, der in das AEM Stilsystem als Anzeigestil eingegeben werden kann, der in Verbindung mit dem Layout-Stil von Promo ausgewählt wird. Dies verstößt gegen die Best Practice von:

**Zeigt nur Stilkombinationen an, die einen Effekt haben**

.die bereits im [Standardstil](#default-style) verletzt wurde.

Da sich die richtige Ausrichtung nur auf den Layoutstil von Promo und nicht auf die beiden anderen Layoutstile auswirkt: Standard und Hero können wir einen neuen Layoutstil &quot;Promo&quot;(rechts ausgerichtet) erstellen, der die CSS-Klasse enthält, die den Inhalt der Promo-Layoutstile rechtsbündig ausrichtet: `cmp -teaser--alternate`.

Diese Kombination mehrerer Stile zu einem einzelnen Stileintrag kann auch dazu beitragen, die Anzahl der verfügbaren Stile und Stilpermutationen zu reduzieren, was am besten zu minimieren ist.

Beachten Sie, dass der Name der CSS-Klasse `cmp-teaser--alternate` nicht mit der Authoring-freundlichen Nomenklatur von &quot;rechtsbündig&quot;übereinstimmen muss.

#### Angebot - Rechtsbündiger Standard

![Angebot rechts ausgerichtet](assets/promo-alternate-default.png)

* **Layout-Stil**
   * Stilname: **Angebot (rechts ausgerichtet)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigeformat**
   * Kein
* **Effektive CSS-Klassen**:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### Angebot rechts ausgerichtet Primär

![Angebot rechts ausgerichtet Primär](assets/promo-alternate-primary.png)

* **Layout-Stil**
   * Stilname: **Angebot (rechts ausgerichtet)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigeformat**
   * Stilname: **Green**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Angebot rechts ausgerichtet Sekundär

![Angebot rechts ausgerichtet Sekundär](assets/promo-alternate-secondary.png)

* **Layout-Stil**
   * Stilname: **Angebot (rechts ausgerichtet)**
   * CSS-Klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Anzeigeformat**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Hero-Stil {#hero-style}

Der Hero-Layoutstil zeigt das Bild der Komponenten als Hintergrund mit überlagerter Überschrift und überlagerter Verknüpfung an. Der Hero-Layoutstil, wie auch der Promo-Layoutstil, muss mit Markenfarben farbig sein.

Um den Hero-Layoutstil mit Markenfarben zu färben, können dieselben Anzeigestile verwendet werden, die auch für das Layout &quot;Promo&quot;verwendet werden.

Pro Komponente wird der Stilname dem einzelnen Satz von CSS-Klassen zugeordnet. Das bedeutet, dass die CSS-Klassennamen, die den Hintergrund des Layoutstils &quot;Promo&quot;farblich markieren, den Text und die Verknüpfung des Hero-Layoutstils farblich markieren müssen.

Dies kann durch das Scoping der CSS-Regeln zunächst erreicht werden. Dies erfordert jedoch, dass die CSS-Entwickler verstehen, wie diese Permutationen auf AEM angewendet werden.

CSS zum Färben des Hintergrunds des Layoutstils **Hervorheben** mit der primären (grünen) Farbe:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS zum Färben des Textes des Layoutstils **Hero** mit der primären (grünen) Farbe:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Default

![Hero Style](assets/hero.png)

* **Layout-Stil**
   * Stilname: **Hero**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigeformat**
   * Kein
* **Effektive CSS-Klassen**:  `.cmp-teaser--hero`

#### Hero Primär

![Hero Primär](assets/hero-primary.png)

* **Layout-Stil**
   * Stilname: **Promo**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigeformat**
   * Stilname: **Green**
   * CSS-Klasse: `cmp-teaser--primary-color`
* **Effektive CSS-Klassen**:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Sekundär

![Hero Sekundär](assets/hero-secondary.png)

* **Layout-Stil**
   * Stilname: **Promo**
   * CSS-Klasse: `cmp-teaser--hero`
* **Anzeigeformat**
   * Stilname: **Gelb**
   * CSS-Klasse: `cmp-teaser--secondary-color`
* **Effektive CSS-Klassen**:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zum Stilsystem](https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Erstellen AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Website zur BEM-Dokumentation (Block Element Modifier)](https://getbem.com/)
* [Website der LESS-Dokumentation](https://lesscss.org/)
* [jQuery-Website](https://jquery.com/)
