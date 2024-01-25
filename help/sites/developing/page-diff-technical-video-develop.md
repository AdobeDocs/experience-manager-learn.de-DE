---
title: Entwickeln für Seitenunterschiede in AEM Sites
description: In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitenunterschiede-Funktion von AEM Sites bereitstellen.
feature: Authoring
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 299
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 100%

---

# Entwickeln für Seitenunterschiede {#developing-for-page-difference}

In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitenunterschiede-Funktion von AEM Sites bereitstellen.

## Anpassen von Stilen für Seitenunterschiede {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>In diesem Video wird der Client-Bibliothek „we.Retail“ benutzerdefiniertes CSS hinzugefügt, wobei diese Änderungen am AEM Sites-Projekt des Customizers vorgenommen werden sollten. Im folgenden Beispiel-Code ist dies `my-project`.

Für Seitenunterschiede von AEM wird die vorkonfigurierte CSS über eine direkte Last von `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css` abgerufen.

Aufgrund dieser direkten Last von CSS anstatt der Verwendung einer Client-Bibliothekskategorie müssen wir einen weiteren Einfügepunkt für die benutzerdefinierten Stile finden. Dieser benutzerdefinierte Einfügepunkt ist die Authoring-Clientlib des Projekts.

Dies hat den Vorteil, dass diese benutzerdefinierten Stilüberschreibungen mandantenspezifisch sein können.

### Vorbereiten der Authoring-Clientlib {#prepare-the-authoring-clientlib}

Stellen Sie sicher, dass eine `authoring`-Clientlib für Ihr Projekt unter `/apps/my-project/clientlib/authoring.` vorhanden ist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Bereitstellen des benutzerdefinierten CSS {#provide-the-custom-css}

Fügen Sie zur `authoring`-Clientlib des Projekts eine `css.txt` hinzu, die auf die less-Datei verweist, welche die überschreibenden Stile bereitstellt. [Less](https://lesscss.org/) wird aufgrund der vielen praktischen Funktionen bevorzugt, darunter auch das Klassen-Wrapping, das in diesem Beispiel verwendet wird.

```shell
base=./css

htmldiff.less
```

Erstellen Sie die `less`-Datei, die die Überschreibungsstile unter `/apps/my-project/clientlibs/authoring/css/htmldiff.less` enthält, und geben Sie ggf. die Überschreibungsstile an.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### Einfügen der Authoring-Clientlib-CSS über die Seitenkomponente {#include-the-authoring-clientlib-css-via-the-page-component}

Fügen Sie die Kategorie der Authoring-Clientlibs in der `/apps/my-project/components/structure/page/customheaderlibs.html` der Basisseite des Projekts ein, direkt vor dem Tag `</head>`, um sicherzustellen, dass die Stile geladen werden.

Diese Stile sollten auf die WCM-Modi [!UICONTROL Bearbeiten] und [!UICONTROL Vorschau] beschränkt sein.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Das Endergebnis einer abgewandelten Seite, auf die die oben genannten Stile angewendet wurden, würde wie folgt aussehen (HTML hinzugefügt und Komponente geändert).

![Seitenunterschied](assets/page-diff.png)

## Zusätzliche Ressourcen {#additional-resources}

* [Beispiel-Site „we.Retail“ herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEM Client-Bibliotheken verwenden](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Less-CSS-Dokumentation](https://lesscss.org/)
