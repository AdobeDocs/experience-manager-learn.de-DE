---
title: Entwickeln für Seitenunterschiede in AEM Sites
description: In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitenunterschiede-Funktion von AEM Sites bereitstellen.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---

# Entwickeln für Seitenunterschiede {#developing-for-page-difference}

In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitenunterschiede-Funktion von AEM Sites bereitstellen.

## Anpassen von Seitendifferenz-Stilen {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>In diesem Video wird der Client-Bibliothek &quot;we.Retail&quot;benutzerdefiniertes CSS hinzugefügt, wobei diese Änderungen am AEM Sites-Projekt des Anpassers vorgenommen werden sollten. im folgenden Beispielcode: `my-project`.

AEM Seitendifferenz ruft die OOTB-CSS über eine direkte Last von `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Aufgrund dieser direkten Belastung von CSS anstatt einer Client-Bibliothekskategorie müssen wir einen weiteren Einfügepunkt für die benutzerdefinierten Stile finden. Dieser benutzerdefinierte Einfügepunkt ist die Authoring-Client-Bibliothek des Projekts.

Dies hat den Vorteil, dass diese benutzerdefinierten Stilüberschreibungen mandantenspezifisch sein können.

### Vorbereiten der Authoring-clientlib {#prepare-the-authoring-clientlib}

das Vorhandensein eines `authoring` clientlib für Ihr Projekt unter `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Bereitstellen des benutzerdefinierten CSS {#provide-the-custom-css}

Hinzufügen zum Projekt `authoring` clientlib a `css.txt` , der auf die Datei less verweist, die die überschreibenden Stile bereitstellt. [Weniger](https://lesscss.org/) wird aufgrund der vielen praktischen Funktionen bevorzugt, darunter auch das Klassenwrapping, das in diesem Beispiel verwendet wird.

```shell
base=./css

htmldiff.less
```

Erstellen Sie die `less` -Datei, die den Stil enthält, überschreibt unter `/apps/my-project/clientlibs/authoring/css/htmldiff.less`und geben Sie bei Bedarf die Überschreibungsstile an.

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

### Einfügen der clientlib-CSS für die Bearbeitung über die Seitenkomponente {#include-the-authoring-clientlib-css-via-the-page-component}

Fügen Sie die Kategorie der Autoren-Client-Bibliotheken in die Basisseite des Projekts ein. `/apps/my-project/components/structure/page/customheaderlibs.html` direkt vor dem `</head>` -Tag, um sicherzustellen, dass die Stile geladen werden.

Diese Stile sollten auf [!UICONTROL Bearbeiten] und [!UICONTROL Vorschau] WCM-Modi.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Das Endergebnis einer diff&#39;d-Seite mit den oben genannten Stilen würde wie folgt aussehen (HTML hinzugefügt und Komponente geändert).

![Seitenunterschied](assets/page-diff.png)

## Zusätzliche Ressourcen {#additional-resources}

* [Beispielsite &quot;we.Retail&quot;herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Verwenden AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Weniger CSS-Dokumentation](https://lesscss.org/)
