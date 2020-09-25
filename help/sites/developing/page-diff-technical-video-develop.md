---
title: Entwickeln für Seitenunterschiede in AEM Sites
description: In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitendifferenz-Funktion von AEM-Sites bereitstellen.
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---


# Entwicklung für Seitendifferenz {#developing-for-page-difference}

In diesem Video wird gezeigt, wie Sie benutzerdefinierte Stile für die Seitendifferenz-Funktion von AEM-Sites bereitstellen.

## Anpassen von Seitenunterschieden {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>In diesem Video wird der Client-Bibliothek we.Retail benutzerdefinierte CSS hinzugefügt, wobei diese Änderungen am AEM Sites-Projekt des Kunden vorgenommen werden sollten. im folgenden Beispielcode: `my-project`.

AEM Seitendifferenz ruft die OOTB CSS über eine direkte Ladung von `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Aufgrund dieser direkten Belastung mit CSS anstatt mit einer Client-Bibliotheks-Kategorie müssen wir einen weiteren Einfügepunkt für die benutzerdefinierten Stile finden. Dieser benutzerdefinierte Einfügepunkt ist der Authoring-Client des Projekts.

Dies hat den Vorteil, dass diese benutzerdefinierten Stilüberschreibungen für Pächter spezifisch sein können.

### Vorbereiten der Authoring-Clientlib {#prepare-the-authoring-clientlib}

Stellen Sie sicher, dass für Ihr Projekt eine `authoring` clientlib vorhanden ist unter `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Benutzerdefinierte CSS bereitstellen {#provide-the-custom-css}

hinzufügen auf die `authoring` clientlib des Projekts, `css.txt` die auf die less-Datei verweist, die die überschreibenden Stile bereitstellt. [Weniger](https://lesscss.org/) wird aufgrund der zahlreichen praktischen Funktionen bevorzugt, einschließlich der in diesem Beispiel verwendeten Klassenumbrüche.

```shell
base=./css

htmldiff.less
```

Erstellen Sie die `less` Datei, die die Stilüberschreibungen am enthält, `/apps/my-project/clientlibs/authoring/css/htmldiff.less`und stellen Sie die gewünschten Überlaufstile bereit.

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

Schließen Sie die Authoring-clientlibs-Kategorie direkt vor dem `/apps/my-project/components/structure/page/customheaderlibs.html` `</head>` -Tag in die Basisseite des Projekts ein, um sicherzustellen, dass die Stile geladen werden.

Diese Stile sollten auf den WCM-Modus [!UICONTROL Bearbeiten] und [!UICONTROL Vorschau] beschränkt sein.

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
