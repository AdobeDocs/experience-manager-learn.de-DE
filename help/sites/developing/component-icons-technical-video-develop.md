---
title: Anpassen von Komponentensymbolen in Adobe Experience Manager Sites
description: Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Kernkomponenten
topic: Entwicklung
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 8%

---


# Anpassen von Komponentensymbolen {#developing-component-icons-in-aem-sites}

Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

Der Komponenten-Browser wird jetzt in einem konsistenten grauen Design angezeigt, in dem Folgendes angezeigt wird:

* **[!UICONTROL Komponentengruppe]**
* **[!UICONTROL Komponentenname]**
* **[!UICONTROL Komponentenbeschreibung]**
* **[!UICONTROL Komponentensymbol]**
   * Die ersten beiden Buchstaben des Komponententitels *(Standard)*
   * Benutzerdefiniertes PNG-Bild *(von einem Entwickler konfiguriert)*
   * Benutzerdefiniertes SVG-Bild *(von einem Entwickler konfiguriert)*
   * CoralUI-Symbol *(von einem Entwickler konfiguriert)*

## Konfigurationsoptionen für Komponenten-Symbole {#component-icon-configuration-options}

### Abkürzungen {#abbreviations}

Standardmäßig werden die ersten beiden Zeichen des Komponententitels (**[cq:Component]@jcr:title**) als Abkürzung verwendet. Beispiel: Wenn **[cq:Component]@jcr:title=Article List**, würde die Abkürzung als &quot;**Ar**&quot;angezeigt werden.

Die Abkürzung kann über die Eigenschaft **[cq:Component]@abbreviation** angepasst werden. Dieser Wert kann zwar mehr als 2 Zeichen enthalten, es wird jedoch empfohlen, die Abkürzung auf 2 Zeichen zu beschränken, um visuelle Störungen zu vermeiden.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-Symbole {#coralui-icons}

CoralUI-Symbole, die von AEM bereitgestellt werden, können für Komponenten-Symbole verwendet werden. Um ein CoralUI-Symbol zu konfigurieren, legen Sie eine **[cq:Component]@cq:icon** -Eigenschaft auf den HTML-Symbolattributwert des gewünschten CoralUI-Symbols fest (aufgezählt in der [CoralUI-Dokumentation](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-Bilder {#png-images}

PNG-Bilder können für Komponentensymbole verwendet werden. Um ein PNG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte Bild als **nt:file** mit dem Namen **cq:icon.png** unter **[cq:Component]** hinzu.

Das PNG sollte einen transparenten Hintergrund haben oder eine Hintergrundfarbe, die auf **#7070** festgelegt ist.

Die PNG-Bilder werden auf **20px um 20px** skaliert. Um Retina-Anzeigen zu ermöglichen, ist es möglicherweise empfehlenswert, **40px** von **40px** anzuzeigen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-Bilder {#svg-images}

SVG-Bilder (vektorbasiert) können für Komponentensymbole verwendet werden. Um ein SVG-Bild als Komponentensymbol zu konfigurieren, fügen Sie die gewünschte SVG als **nt:file** mit dem Namen **cq:icon.svg** unter **[cq:Component]** hinzu.

SVG-Bilder sollten eine Hintergrundfarbe haben, die auf **#7070** und eine Größe von **20px x x 20px eingestellt ist.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Zusätzliche Ressourcen {#additional-resources}

* [Verfügbare CoralUI-Symbole](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
