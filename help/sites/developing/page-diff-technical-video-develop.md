---
title: Entwickeln für Seitenunterschiede in AEM Sites
description: In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitendifferenz-Funktion von AEM-Sites bereitstellen.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 7%

---


# Entwickeln für Seitendifferenz {#developing-for-page-difference}

In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitendifferenz-Funktion von AEM-Sites bereitstellen.

## Anpassen von Seitenunterschieden-Stilen {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>In diesem Video wird der Client-Bibliothek we.Retail benutzerdefinierte CSS hinzugefügt, wobei diese Änderungen am AEM Sites-Projekt des Kunden vorgenommen werden sollten. im folgenden Beispielcode: `my-project`.

AEM Seitendifferenz ruft die OOTB-CSS über eine direkte Last von `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css` ab.

Aufgrund dieser direkten Belastung mit CSS anstatt mit einer Client-Bibliotheks-Kategorie müssen wir einen weiteren Einfügepunkt für die benutzerdefinierten Stile finden. Dieser benutzerdefinierte Einfügepunkt ist der Authoring-Client des Projekts.

Dies hat den Vorteil, dass diese benutzerdefinierten Stilüberschreibungen für Pächter spezifisch sein können.

### Vorbereiten der Authoring-Client-Bibliothek {#prepare-the-authoring-clientlib}

Stellen Sie sicher, dass `authoring` clientlib für Ihr Projekt unter `/apps/my-project/clientlib/authoring.` vorhanden ist.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Geben Sie die benutzerdefinierte CSS {#provide-the-custom-css} an

hinzufügen auf die clientlib-Datei `authoring` des Projekts `css.txt`, die auf die less-Datei verweist, die die überschreibenden Stile bereitstellt. [](https://lesscss.org/) Lessis wird aufgrund seiner zahlreichen praktischen Funktionen bevorzugt, einschließlich der in diesem Beispiel verwendeten Klassenumhüllung.

```shell
base=./css

htmldiff.less
```

Erstellen Sie die Datei `less`, die die Stilüberschreibungen unter `/apps/my-project/clientlibs/authoring/css/htmldiff.less` enthält, und stellen Sie die gewünschten Überlaufstile bereit.

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

### Include the authoring clientlib CSS via the page component {#include-the-authoring-clientlib-css-via-the-page-component}

Schließen Sie die Authoring-clientlibs-Kategorie direkt vor dem `</head>`-Tag auf der Basisseite des Projekts ein, um sicherzustellen, dass die Stile geladen werden.`/apps/my-project/components/structure/page/customheaderlibs.html`

Diese Stile sollten auf die Modi [!UICONTROL Bearbeiten] und [!UICONTROL Vorschau] WCM beschränkt sein.

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

* [Beispielseite &quot;we.Retail&quot;herunterladen](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Verwenden AEM Client-Bibliotheken](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Weniger CSS-Dokumentation](https://lesscss.org/)
