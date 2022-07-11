---
title: Erste Schritte mit AEM Sites - WKND-Tutorial
description: Erfahren Sie, wie Sie eine AEM Site für eine fiktive Lifestyle-Marke namens WKND implementieren. Machen Sie sich mit grundlegenden Themen zu Experience Managern wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbaren Vorlagen, Client-Bibliotheken und Komponentenentwicklung vertraut.
sub-product: sites
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 72abe1cddcf6a012403887203d38509bde8f2d23
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 3%

---

# Erste Schritte mit AEM Sites - WKND-Tutorial {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial führt Sie durch die Implementierung einer AEM Site für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Kernkomponenten, bearbeitbare Vorlagen, Client-seitige Bibliotheken und Komponentenentwicklung mit Adobe Experience Manager Sites.

## Übersicht {#wknd-tutorial-overview}

Ziel dieses mehrteiligen Tutorials ist es, Entwicklern beizubringen, wie eine Website mit den neuesten Standards und Technologien in Adobe Experience Manager (AEM) implementiert wird. Nach Abschluss dieses Tutorials sollte ein Entwickler die grundlegende Grundlage der Plattform und die gängigen Designmuster in AEM verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Optionen zum Starten eines Sites-Projekts

Es gibt zwei grundlegende Ansätze zum Starten eines AEM Sites-Projekts.

**AEM Projektarchetyp** - Traditioneller Ansatz für AEM Entwicklung durch Generierung eines AEM mit einer Maven-Vorlage. Dies ist der empfohlene Ansatz für AEM 6.5/6.4-Projekte und AEM as a Cloud Service Projekte, die eine umfangreiche Anpassung erwarten. Das Tutorial bietet einen tieferen Einblick in AEM Entwicklung.

[Starten Sie das Tutorial mit dem AEM Projektarchetyp](./project-archetype/overview.md)

**AEM Site-Vorlagen** - Wird auch als schnelle Site-Erstellung bezeichnet. Dies ist ein Ansatz mit niedriger Code-Methode zum Generieren einer AEM Site mithilfe einer vordefinierten Site-Vorlage. Nutzen Sie vordefinierte Komponenten und Vorlagen, um eine Site schnell einzurichten und zu nutzen. Verwenden Sie einen Designarbeitsablauf, um markenspezifische Stile und Anpassungen nur mit CSS und JavaScript anzuwenden. Wird für neue Projekte und Entwickler empfohlen. Nur für AEM as a Cloud Service verfügbar.

[Tutorial mit einer Site-Vorlage starten](./site-template/create-site.md)

## Adobe XD UI Kit

Um dieses Tutorial näher an ein reales Szenario heranzuführen, haben die talentierten UX-Designer der Adobe die Modelle für die Site mit [Adobe XD](https://www.adobe.com/products/xd.html). Im Laufe des Tutorials werden verschiedene Teile der Designs in eine vollwertige AEM-Site implementiert. Besonderer Dank an **Lorenzo Buosi** und **Kilian Amenola** die das wunderschöne Design für die WKND-Site erstellt haben.

Laden Sie die XD UI-Kits herunter:

* [AEM UI-Kit der Kernkomponente](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Referenzsite {#reference-site}

Eine fertige Version der WKND-Site ist ebenfalls als Referenz verfügbar: [https://wknd.site/](https://wknd.site/)

Das Tutorial behandelt die wichtigsten Entwicklungsfähigkeiten, die für einen AEM-Entwickler erforderlich sind, behandelt jedoch *not* Erstellen Sie die gesamte Site von Ende zu Ende. Die fertige Referenz-Website ist eine weitere großartige Ressource, um mehr AEM vorkonfigurierten Funktionen zu erkunden und zu sehen.

Um den neuesten Code zu testen, bevor Sie mit dem Tutorial beginnen, laden Sie die **[neueste Version von GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

Viele der Bilder auf der WKND Reference-Website stammen aus [Adobe Stock](https://stock.adobe.com/de) und sind Material von Drittanbietern, wie in den zusätzlichen Begriffen zu Demo-Asset unter definiert [https://www.adobe.com/legal/terms.html](https://www.adobe.com/de/legal/terms.html). Wenn Sie ein Adobe Stock-Bild für andere Zwecke verwenden möchten, die über die Anzeige dieser Demowebsite hinausgehen, z. B. für die Präsentation auf einer Website oder für Marketingmaterialien, können Sie eine Lizenz für Adobe Stock erwerben.

Mit Adobe Stock haben Sie Zugriff auf mehr als 140 Millionen hochqualitative, gebührenfreie Bilder, darunter Fotos, Grafiken, Videos und Vorlagen, um Ihre Kreativprojekte zu starten.

## Nächste Schritte {#next-steps}

Worauf wartest du?! Erfahren Sie, wie Sie [ein neues Adobe Experience Manager-Projekt mithilfe des AEM Projektarchetyps erstellen](./project-archetype/overview.md) oder [Erstellen einer Site mit einer Site-Vorlage](./site-template/create-site.md).
