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


# Anpassen der Komponentensymbole {#developing-component-icons-in-aem-sites}

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

Standardmäßig werden die ersten beiden Zeichen des Komponententitels (**[cq:Component]@jcr:title**) als Abkürzung verwendet. Beispiel: Wenn **[cq:Component]@jcr:title=Article Liste** die Abkürzung als &quot;**Ar**&quot;angezeigt wird.

Die Abkürzung kann über die Eigenschaft **[cq:Component]@abkürzung** angepasst werden. Dieser Wert kann zwar mehr als 2 Zeichen enthalten, es wird jedoch empfohlen, die Abkürzung auf 2 Zeichen zu beschränken, um visuelle Störungen zu vermeiden.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI Icons {#coralui-icons}

Die von AEM bereitgestellten CoralUI-Symbole können für Komponentensymbole verwendet werden. Um ein CoralUI-Symbol zu konfigurieren, legen Sie eine **[cq:Component]@cq:icon**-Eigenschaft auf den HTML-Attributwert des gewünschten CoralUI-Symbols fest (aufgeführt in der [CoralUI-Dokumentation](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-Bilder {#png-images}

PNG-Bilder können für Komponentensymbole verwendet werden. Um ein PNG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte Bild als **nt:file** mit dem Namen **cq:icon.png** unter **[cq:Component]** hinzu.

Die PNG-Datei sollte einen transparenten Hintergrund oder eine Hintergrundfarbe auf **#707070** setzen.

Die PNG-Bilder werden auf **20px um 20px** skaliert. Zur Aufnahme von Retina-Anzeigen ist es jedoch empfehlenswert, **40px** von **40px** anzuzeigen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-Bilder {#svg-images}

SVG-Bilder (vektorbasiert) können für Komponentensymbole verwendet werden. Um ein SVG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte SVG als **nt:file** mit dem Namen **cq:icon.svg** unter **[cq:Component]** hinzu.

SVG-Bilder sollten eine Hintergrundfarbe auf **#707070** und eine Größe von **20px x x.** haben.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Zusätzliche Ressourcen {#additional-resources}

* [Verfügbare CoralUI-Symbole](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
