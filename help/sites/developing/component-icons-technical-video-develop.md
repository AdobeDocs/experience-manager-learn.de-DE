---
title: Anpassen von Komponentensymbolen in Adobe Experience Manager Sites
description: Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.
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
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 7%

---

# Anpassen von Komponentensymbolen {#developing-component-icons-in-aem-sites}

Mithilfe von Komponentensymbolen können Autoren eine Komponente schnell mit Symbolen oder aussagekräftigen Abkürzungen identifizieren. Autoren können jetzt die Komponenten finden, die erforderlich sind, um ihre Web-Erlebnisse schneller als je zuvor zu erstellen.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

Der Komponenten-Browser wird jetzt in einem konsistenten grauen Design angezeigt, in dem Folgendes angezeigt wird:

* **[!UICONTROL Komponentengruppe]**
* **[!UICONTROL Komponententitel]**
* **[!UICONTROL Komponentenbeschreibung]**
* **[!UICONTROL Komponentensymbol]**
   * Die ersten beiden Buchstaben des Komponententitels *(Standard)*
   * Benutzerdefiniertes PNG-Bild *(von einem Entwickler konfiguriert)*
   * Benutzerdefiniertes SVG-Bild *(von einem Entwickler konfiguriert)*
   * CoralUI-Symbol *(von einem Entwickler konfiguriert)*

## Konfigurationsoptionen für Komponenten-Symbole {#component-icon-configuration-options}

### Abkürzungen {#abbreviations}

Standardmäßig sind die ersten 2 Zeichen des Komponententitels (**[cq:Component]@jcr:title**) werden als Abkürzung verwendet. Wenn beispielsweise **[cq:Component]@jcr:title=Artikelliste** Die Abkürzung würde als &quot;**Ar**&quot;.

Die Abkürzung kann über die **[cq:Component]@abkürzung** -Eigenschaft. Dieser Wert kann zwar mehr als 2 Zeichen enthalten, es wird jedoch empfohlen, die Abkürzung auf 2 Zeichen zu beschränken, um visuelle Störungen zu vermeiden.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI-Symbole {#coralui-icons}

CoralUI-Symbole, die von AEM bereitgestellt werden, können für Komponenten-Symbole verwendet werden. Um ein CoralUI-Symbol zu konfigurieren, legen Sie eine **[cq:Component]@cq:icon** -Eigenschaft auf den Attributwert des gewünschten CoralUI-Symbols HTML (aufgezählt in der [CoralUI-Dokumentation](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG-Bilder {#png-images}

PNG-Bilder können für Komponentensymbole verwendet werden. Um ein PNG-Bild als Komponentensymbol zu konfigurieren, fügen Sie das gewünschte Bild als **nt:file** benannt **cq:icon.png** unter **[cq:Component]**.

Das PNG sollte einen transparenten Hintergrund oder eine Hintergrundfarbe haben, die auf **#707070**.

Die PNG-Bilder werden auf **20 px mal 20 px**. Retina-Displays können jedoch angepasst werden **40 px** von **40 px** möglicherweise vorzuziehen.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG von Bildern {#svg-images}

SVG-Bilder (vektorbasiert) können für Komponentensymbole verwendet werden. Um ein SVG-Bild als Komponentensymbol zu konfigurieren, fügen Sie die gewünschte SVG als **nt:file** benannt **cq:icon.svg** unter **[cq:Component]**.

SVG-Bilder sollten eine Hintergrundfarbe haben, die auf **#707070** und eine Größe von **20px mal 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Zusätzliche Ressourcen {#additional-resources}

* [Verfügbare CoralUI-Symbole](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
