---
title: Website-Branding hinzufügen
description: Definieren von globalem CSS, CSS-Variablen und Web Fonts für eine Edge Delivery Services-Site.
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
source-wordcount: '1308'
ht-degree: 0%

---


# Website-Branding hinzufügen

Richten Sie zunächst das gesamte Branding ein, indem Sie globale Stile aktualisieren, CSS-Variablen definieren und Web-Schriftarten hinzufügen. Diese grundlegenden Elemente stellen sicher, dass die Website konsistent und wartbar bleibt und auf der gesamten Website konsistent angewendet werden sollte.

## GitHub-Problem erstellen

Um alles zu organisieren, verwenden Sie GitHub, um die Arbeit zu verfolgen. Erstellen Sie zunächst ein GitHub-Problem für diesen Textkörper:

1. Navigieren Sie zum GitHub-Repository (weitere Informationen finden Sie [ Kapitel ](./1-new-code-project.md)Erstellen eines Code-Projekts„).
2. Klicken Sie auf die Registerkarte **Probleme** und dann auf **Neues Problem**.
3. Schreiben Sie **Titel** und **Beschreibung** für die zu erledigende Arbeit.
4. Klicken Sie **Neue Anfrage senden**.

Das GitHub-Problem wird später beim [Erstellen einer Pull-Anfrage“ ](#merge-code-changes).

![GitHub - neue Anfrage erstellen](./assets/4-website-branding/github-issues.png)

## Erstellen einer Arbeitsverzweigung

Um die Organisation beizubehalten und die Code-Qualität sicherzustellen, erstellen Sie für jeden Arbeitskörper eine neue Verzweigung. Dadurch wird verhindert, dass neuer Code die Leistung beeinträchtigt, und sichergestellt, dass Änderungen nicht live sind, bevor sie abgeschlossen sind.

Erstellen Sie für dieses Kapitel, das sich auf die grundlegenden Stile der Website konzentriert, eine Verzweigung mit dem Namen `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## Globales CSS

Edge Delivery Services verwendet eine globale CSS-Datei unter `styles/styles.css`, um die allgemeinen Stile für die gesamte Website einzurichten. Der `styles.css` steuert Aspekte wie Farben, Schriftarten und Abstände und stellt sicher, dass alles auf der Site konsistent aussieht.

Globale CSS sollte unabhängig von Konstrukten auf niedrigerer Ebene, z. B. Blöcken, sein Augenmerk auf das allgemeine Erscheinungsbild der Site sowie gemeinsame visuelle Behandlungen richten.

Beachten Sie, dass globale CSS-Stile bei Bedarf überschrieben werden können.

### CSS-Variablen

[CSS-Variablen](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) eignen sich hervorragend, um Design-Einstellungen wie Farben, Schriftarten und Größen zu speichern. Durch die Verwendung von Variablen können Sie diese Elemente an einem Ort ändern und auf der gesamten Site aktualisieren.

Gehen Sie wie folgt vor, um mit der Anpassung der CSS-Variablen zu beginnen:

1. Öffnen Sie die `styles/styles.css` im Code-Editor.
2. Suchen Sie die `:root` Deklaration, in der globale CSS-Variablen gespeichert werden.
3. Ändern Sie die Farb- und Schriftvariablen, um sie an die WKND-Marke anzupassen.

Hier ein Beispiel:


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Erkunden Sie die anderen Variablen im Abschnitt `:root` und überprüfen Sie die Standardeinstellungen.

Wenn Sie eine Website entwickeln und dieselben CSS-Werte wiederholen, sollten Sie ggf. neue Variablen erstellen, um die Stile einfacher zu verwalten. Beispiele für andere CSS-Eigenschaften, die von CSS-Variablen profitieren können, sind: `border-radius`, `padding`, `margin` und `box-shadow`.

### Nackte Elemente

Bare Elemente werden direkt über ihren Elementnamen formatiert, anstatt eine CSS-Klasse zu verwenden. Anstatt beispielsweise eine `.page-heading` CSS-Klasse zu gestalten, werden Stile mithilfe von `h1 { ... }` auf das `h1` angewendet.

In der `styles/styles.css`-Datei wird eine Reihe von Basisstilen auf bloße HTML-Elemente angewendet. Edge Delivery Services-Websites haben bei der Verwendung bloßer Elemente Priorität, da sie mit der nativen semantischen HTML von Edge Delivery Service übereinstimmen.

Zur Anpassung an das WKND-Branding gestalten wir einige einfache Elemente in `styles.css`:

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Diese Stile stellen sicher, dass `h2` Elemente, sofern sie nicht überschrieben werden, mit dem WKND-Branding konsistent formatiert sind, wodurch eine klare visuelle Hierarchie erstellt werden kann. Die teilweise gelbe Unterstreichung unter jedem `h2` verleiht den Überschriften eine unverwechselbare Note.

### Abgeleitete Elemente

In Edge Delivery Services verbessern die `scripts.js` und der `aem.js` des Projekts automatisch bestimmte bloße HTML-Elemente basierend auf ihrem Kontext innerhalb der HTML.

Anker(`<a>`)-Elemente beispielsweise, die in ihrer eigenen Zeile erstellt wurden - und nicht inline mit dem umgebenden Text - werden als Schaltflächen auf Grundlage dieses Kontexts abgeleitet. Diese Anker werden automatisch mit einem Container umschlossen, der mit der CSS-Klasse `button-container` `div` ist, und dem Ankerelement wird eine `button` CSS-Klasse hinzugefügt.

Wenn beispielsweise ein Link in einer eigenen Zeile erstellt wird, aktualisiert Edge Delivery Services JavaScript sein DOM wie folgt:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Diese Schaltflächen können an die WKND-Marke angepasst werden. Dadurch werden Schaltflächen als gelbe Rechtecke mit schwarzem Text angezeigt.

Im Folgenden finden Sie ein Beispiel für die Gestaltung der „abgeleiteten Schaltflächen“ in `styles.css`:

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Dieses CSS definiert die grundlegenden Schaltflächenstile und enthält WKND-spezifische Behandlungen wie Großbuchstaben, gelben Hintergrund und schwarzen Text. Die `background-color`- und `color` verwenden CSS-Variablen, damit der Schaltflächenstil an den Farben der Marke ausgerichtet bleibt. Dadurch wird sichergestellt, dass Schaltflächen auf der gesamten Site konsistent formatiert werden und gleichzeitig flexibel bleiben.

## Web Fonts

Edge Delivery Services-Projekte optimieren die Verwendung von Web-Schriftarten, um eine hohe Leistung zu erzielen und die Auswirkungen auf Lighthouse-Bewertungen zu minimieren. Diese Methode stellt ein schnelles Rendering sicher, ohne die visuelle Identität der Site zu beeinträchtigen. So implementieren Sie Webfonts effizient für eine optimale Leistung.

### Schriftflächen

Fügen Sie benutzerdefinierte Web-Schriftarten mithilfe von CSS-`@font-face` in der `styles/fonts.css` hinzu. Durch Hinzufügen des `@font-faces` zu `fonts.css` wird sichergestellt, dass Web-Schriftarten zum optimalen Zeitpunkt geladen werden, wodurch Lighthouse-Werte beibehalten werden können.

1. Öffnen Sie `styles/fonts.css`.
2. Fügen Sie die folgenden `@font-face` hinzu, um die Markenschriftarten WKND einzuschließen: `Asar` und `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

Die in diesem Tutorial verwendeten Schriftarten werden von Google Fonts bezogen, Webschriftarten können jedoch von jedem Schriftartenanbieter bezogen werden, einschließlich [Adobe Fonts](https://fonts.adobe.com/).

+++Verwenden lokaler Web-Schriftartdateien

Alternativ können Web Fonts-Dateien in das Projekt im `/fonts` kopiert und in den `@font-face`-Deklarationen referenziert werden.

In diesem Tutorial werden die Remote-Webfonts verwendet, die gehostet werden, damit sie leichter befolgt werden können.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Aktualisieren Sie abschließend die `styles/styles.css` CSS-Variablen, um die neuen Schriftarten zu verwenden:

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` und `roboto-condensed-fallback` sind Fallback-Schriftarten, die im Abschnitt [Fallback-](#fallback-fonts)&quot; aktualisiert werden, um sie zur Unterstützung der benutzerdefinierten `Asar` und `Source Sans Pro` Web-Schriftarten auszurichten.

### Ersatzschriftarten

Web-Schriftarten beeinträchtigen oft die Leistung aufgrund ihrer Größe, erhöhen möglicherweise die Werte für Cumulative Layout Shift (CLS) und verringern die Gesamtwerte für Lighthouse. Um die sofortige Textanzeige beim Laden von Web-Schriftarten sicherzustellen, verwenden Edge Delivery Services-Projekte browsernative Fallback-Schriftarten. Dieser Ansatz hilft, ein reibungsloses Benutzererlebnis aufrechtzuerhalten, während die gewünschte Schriftart angewendet wird.

Um die beste Fallback-Schriftart auszuwählen, verwenden Sie Adobe [Helix Font Fallback Chrome Extension](https://www.aem.live/developer/font-fallback), die eine eng übereinstimmende Schriftart für Browser bestimmt, bevor die benutzerdefinierte Schriftart geladen wird. Die resultierenden Fallback-Schriftartdeklarationen sollten der `styles/styles.css`-Datei hinzugefügt werden, um die Leistung zu verbessern und ein nahtloses Erlebnis für Benutzende sicherzustellen.

Um die [Helix Font Fallback Chrome-Erweiterung](https://www.aem.live/developer/font-fallback) zu verwenden, stellen Sie sicher, dass auf die Web-Seite Web Fonts in den gleichen Varianten angewendet werden wie auf der Edge Delivery Services-Website. Dieses Tutorial zeigt die Erweiterung auf [wknd.site](http://wknd.site/us/en.html). Wenden Sie beim Entwickeln einer Website die Erweiterung auf die Site an, an der gearbeitet wird, und nicht auf [wknd.site](http://wknd.site/us/en.html).

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Fügen Sie die Ersatzschriftfamilie-Namen den CSS-Variablen für Schriftarten in `styles/styles.css` nach den „echten“ Schriftfamiliennamen hinzu.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Vorschau der Entwicklung

Beim Hinzufügen von CSS lädt die lokale Entwicklungsumgebung der AEM-CLI die Änderungen automatisch neu, sodass schnell und einfach zu sehen ist, wie sich die CSS auf den Block auswirkt.

![Entwicklungsvorschau der WKND-Marken-CSS](./assets/4-website-branding/preview.png)


## Herunterladen der endgültigen CSS-Dateien

Sie können die aktualisierten CSS-Dateien über die folgenden Links herunterladen:

* [`styles.css`](https://adobe.com#TODO)
* [`fonts.css`](https://adobe.com#TODO)

## Verknüpfen der CSS-Dateien

Achten Sie darauf[ dass Ihr Code häufig ](./3-local-development-environment.md#linting) wird, um sicherzustellen, dass er sauber und konsistent ist. Regelmäßiges Linting hilft, Probleme frühzeitig zu erkennen und reduziert die Entwicklungszeit. Denken Sie daran, dass Sie Ihre Arbeit erst dann mit dem Hauptzweig zusammenführen können, wenn alle Verknüpfungsprobleme behoben sind!

```bash
$ npm run lint:css
```

## Code-Änderungen zusammenführen

Führen Sie die Änderungen in der `main` auf GitHub zusammen, um zukünftige Arbeiten an diesen Aktualisierungen zu erstellen.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Sobald die Änderungen an die `wknd-styles` Verzweigung gepusht wurden, erstellen Sie eine Pull-Anfrage auf GitHub, um sie in der `main` Verzweigung zusammenzuführen.

1. Navigieren Sie im Kapitel [Neues Projekt erstellen](./1-new-code-project.md) zum GitHub-Repository.
2. Klicken Sie auf die **Pull-**&quot; und wählen Sie **Neue Pull-Anfrage** aus.
3. Legen Sie `wknd-styles` als Quellverzweigung und `main` als Zielverzweigung fest.
4. Überprüfen Sie die Änderungen und klicken Sie auf **Pull-Anfrage erstellen**.
5. Fügen Sie in den Details zur Pull **Anfrage Folgendes hinzu**:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * Die `Fix #1` verweist auf das zuvor erstellte GitHub-Problem.
   * Die Test-URLs teilen AEM Code Sync mit, welche Verzweigungen für die Validierung und den Vergleich verwendet werden sollen. Die Nachher-URL verwendet die Arbeitszweig-`wknd-styles`, um zu überprüfen, wie sich der Code-Wechsel auf die Website-Leistung auswirkt.

6. Klicken Sie **Pull-Anfrage erstellen**.
7. Warten Sie, bis die GitHub-App für die [AEM](./1-new-code-project.md)Code-Synchronisierung **Qualitätsprüfungen abgeschlossen**. Wenn sie fehlschlagen, beheben Sie die Fehler und führen Sie die Prüfungen erneut aus.
8. Sobald die Prüfungen bestanden sind, **fusionieren Sie die Pull-** mit `main`.

Nachdem die Änderungen in `main` zusammengeführt wurden, werden sie jetzt als in der Produktion bereitgestellt betrachtet, und die neue Entwicklung kann auf Grundlage dieser Aktualisierungen fortgesetzt werden.
