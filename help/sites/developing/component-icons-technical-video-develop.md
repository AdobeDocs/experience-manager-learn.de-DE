---
title: Anpassen von Komponentensymbolen in Adobe Experience Manager Sites
description: Mithilfe von Komponentensymbolen können Autorinnen und Autoren eine Komponente anhand von Symbolen oder aussagekräftigen Abkürzungen schnell erkennen. Autorinnen und Autoren können jetzt die zum Erstellen ihrer Web-Erlebnisse erforderlichen Komponenten schneller als je zuvor finden.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '374'
ht-degree: 100%

---

# Anpassen von Komponentensymbolen {#developing-component-icons-in-aem-sites}

Mithilfe von Komponentensymbolen können Autorinnen und Autoren eine Komponente anhand von Symbolen oder aussagekräftigen Abkürzungen schnell erkennen. Autorinnen und Autoren können jetzt die zum Erstellen ihrer Web-Erlebnisse erforderlichen Komponenten schneller als je zuvor finden.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

Der Komponenten-Browser wird jetzt in einem konsistenten grauen Design mit folgenden Elementen angezeigt:

* **[!UICONTROL Komponentengruppe]**
* **[!UICONTROL Komponententitel]**
* **[!UICONTROL Komponentenbeschreibung]**
* **[!UICONTROL Komponentensymbol]**
   * die ersten beiden Buchstaben des Komponententitels *(Standard)*
   * benutzerdefiniertes PNG-Bild *(von einer Entwicklerin oder einem Entwickler konfiguriert)*
   * benutzerdefiniertes SVG-Bild *(von einer Entwicklerin oder einem Entwickler konfiguriert)*
   * CoralUI-Symbol *(von einer Entwicklerin oder einem Entwickler konfiguriert)*

## Konfigurationsoptionen für Komponentensymbole {#component-icon-configuration-options}

### Abkürzungen {#abbreviations}

Standardmäßig werden die ersten beiden Zeichen des Komponententitels (**[cq:Component]@jcr:title**) als Abkürzung verwendet. Beispielsweise würde **[cq:Component]@jcr:title=Artikelliste** zu der Abkürzung „**Ar**“ führen.

Die Abkürzung kann über die Eigenschaft **[cq:Component]@abbreviation** angepasst werden. Dieser Wert kann zwar mehr als 2 Zeichen enthalten. Es wird jedoch empfohlen, die Abkürzung auf 2 Zeichen zu beschränken, um optische Beeinträchtigungen zu vermeiden.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-Symbole {#coralui-icons}

Von AEM bereitgestellte CoralUI-Symbole können als Komponentensymbole verwendet werden. Um ein CoralUI-Symbol zu konfigurieren, setzen Sie die Eigenschaft **[cq:Component]@cq:icon** auf den HTML-Symbol-Attributwert des gewünschten CoralUI-Symbols (aufgelistet in der [CoralUI-Dokumentation](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-Bilder {#png-images}

PNG-Bilder können als Komponentensymbole verwendet werden. Um ein PNG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte Bild als **nt:file** namens **cq:icon.png** unter **[cq:Component]** hinzu.

Das PNG-Bild sollte einen transparenten Hintergrund aufweisen oder **#707070** als Hintergrundfarbe festgelegt haben.

Die PNG-Bilder werden auf **20 x 20 px** skaliert. Für Retina-Displays kann jedoch ein Wert von **40 x 40 px** **** vorzuziehen sein.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG-Bilder {#svg-images}

(Vektorbasierte) SVG-Bilder können als Komponentensymbole verwendet werden. Um ein SVG-Bild als Komponentensymbol zu konfigurieren, fügen Sie die gewünschte SVG als **nt:file** namens **cq:icon.svg** unter **[cq:Component]** hinzu.

SVG-Bilder sollten **#707070** als Hintergrundfarbe haben und **20 x 20 px** groß sein.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Zusätzliche Ressourcen {#additional-resources}

* [Verfügbare CoralUI-Symbole](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
