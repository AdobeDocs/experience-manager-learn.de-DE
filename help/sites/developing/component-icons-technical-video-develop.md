---
title: Anpassen der Komponentensymbole in Adobe Experience Manager Sites
description: Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---


# Anpassen von Komponentensymbolen {#developing-component-icons-in-aem-sites}

Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

Der Komponenten-Browser wird nun in einem konsistenten grauen Design angezeigt und zeigt Folgendes an:

* **[!UICONTROL Komponentengruppe]**
* **[!UICONTROL Komponentenname]**
* **[!UICONTROL Komponentenbeschreibung]**
* **[!UICONTROL Komponentensymbol]**
   * Die ersten beiden Buchstaben des Komponententitels *(Standard)*
   * Benutzerdefiniertes PNG-Bild *(von einem Entwickler konfiguriert)*
   * Benutzerdefiniertes SVG-Bild *(von einem Entwickler konfiguriert)*
   * CoralUI-Symbol *(von einem Entwickler konfiguriert)*

## Konfigurationsoptionen für Komponentensymbole {#component-icon-configuration-options}

### Abkürzungen {#abbreviations}

Standardmäßig werden die ersten beiden Zeichen des Komponententitels (**[cq:Component]@jcr:title**) als Abkürzung verwendet. Wenn beispielsweise **[cq:Component]@jcr:title=Article-Liste** , wird die Abkürzung als &quot;**Ar**&quot;angezeigt.

Die Abkürzung kann über die **[Eigenschaft cq:Component]@abkürzung** angepasst werden. Dieser Wert kann zwar mehr als 2 Zeichen enthalten, es wird jedoch empfohlen, die Abkürzung auf 2 Zeichen zu beschränken, um visuelle Störungen zu vermeiden.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-Symbole {#coralui-icons}

Die von AEM bereitgestellten CoralUI-Symbole können für Komponentensymbole verwendet werden. Um ein CoralUI-Symbol zu konfigurieren, legen Sie eine **[cq:Component]@cq:icon** -Eigenschaft auf den HTML-Attributwert des gewünschten CoralUI-Symbols fest (in der Dokumentation [](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)zur CoralUI aufgeführt).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-Bilder {#png-images}

PNG-Bilder können für Komponentensymbole verwendet werden. Um ein PNG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte Bild als **nt:file** mit dem Namen **cq:icon.png** unter &quot; **[cq:Component]**&quot;hinzu.

Die PNG-Datei sollte einen transparenten Hintergrund oder eine Hintergrundfarbe **#707070** haben.

Die PNG-Bilder werden auf **20 px** skaliert. Um Retina-Displays mit **40px** x **40px** zu unterbringen, ist es jedoch empfehlenswert.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-Bilder {#svg-images}

SVG-Bilder (vektorbasiert) können für Komponentensymbole verwendet werden. Um ein SVG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte SVG als **nt:file** mit dem Namen **cq:icon.svg** unter der **[cq:Component]** hinzu.

SVG-Bilder sollten eine Hintergrundfarbe auf **#707070** und eine Größe von **20 px mal 20 px haben.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Zusätzliche Ressourcen {#additional-resources}

* [Verfügbare CoralUI-Symbole](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
