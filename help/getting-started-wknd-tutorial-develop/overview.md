---
title: Erste Schritte mit AEM Sites - WKND-Tutorial
description: Erste Schritte mit AEM Sites - WKND-Tutorial. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu bei Adobe Experience Manager sind. Das Tutorial führt durch die Implementierung einer AEM Site für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Kernkomponenten, Seiten-Editor, bearbeitbare Vorlagen, AEM Projektarchetyp
topic: Content Management, Entwicklung
role: Developer
level: Beginner
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 6%

---


# Erste Schritte mit AEM Sites - WKND-Tutorial {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial führt Sie durch die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Kernkomponenten, bearbeitbare Vorlagen, Client-seitige Bibliotheken und Komponentenentwicklung mit Adobe Experience Manager Sites.

## Überblick {#wknd-tutorial-overview}

Ziel dieses mehrteiligen Tutorials ist es, Entwicklern beizubringen, wie eine Website mit den neuesten Standards und Technologien in Adobe Experience Manager (AEM) implementiert wird. Nach Abschluss dieses Tutorials sollte ein Entwickler die grundlegende Grundlage der Plattform und Kenntnisse über allgemeine Designmuster in AEM verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Optionen zum Starten eines Sites-Projekts

Es gibt zwei grundlegende Ansätze zum Starten eines AEM Sites-Projekts.

**AEM Projektarchetyp**  - Traditioneller Ansatz zur AEM Entwicklung durch Generierung eines AEM mit einer Maven-Vorlage. Dies ist der empfohlene Ansatz für AEM 6.5/6.4-Projekte und AEM als Cloud Service-Projekte, die eine umfangreiche Anpassung erwarten. Das Tutorial bietet einen tieferen Einblick in AEM Entwicklung.

[Starten Sie das Tutorial mit dem AEM Projektarchetyp](./project-archetype/overview.md)

**AEM Site-Vorlagen**  - Ein Ansatz mit niedriger Code zum Generieren einer AEM Site mithilfe einer vordefinierten Site-Vorlage. Nutzen Sie vordefinierte Komponenten und Vorlagen, um eine Site schnell einzurichten und zu nutzen. Verwenden Sie einen Designarbeitsablauf, um markenspezifische Stile und Anpassungen nur mit CSS und JavaScript anzuwenden. Wird für neue Projekte und Entwickler empfohlen. Derzeit nur für AEM als Cloud Service verfügbar.

[Tutorial mit einer Site-Vorlage starten](./site-template/create-site.md)

## Adobe XD UI Kit

Um dieses Tutorial näher an ein reales Szenario heranzuführen, haben die talentierten UX-Designer der Adobe die Modelle für die Site mit [Adobe XD](https://www.adobe.com/products/xd.html) erstellt. Im Laufe des Tutorials werden verschiedene Teile der Designs in eine vollwertige AEM-Site implementiert. Besonderer Dank an **Lorenzo Buosi** und **Kilian Amendment**, die das schöne Design für die WKND-Site erstellt haben.

Laden Sie die XD UI-Kits herunter:

* [AEM UI-Kit der Kernkomponente](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Referenzsite {#reference-site}

Eine fertige Version der WKND-Site ist ebenfalls als Referenz verfügbar: [https://wknd.site/](https://wknd.site/)

Das Tutorial behandelt die wichtigsten Entwicklungsfähigkeiten, die für einen AEM-Entwickler erforderlich sind, wird jedoch *nicht* die gesamte Site von Ende zu Ende erstellen. Die fertige Referenz-Website ist eine weitere großartige Ressource, um mehr AEM vorkonfigurierten Funktionen zu erkunden und zu sehen.

Um den neuesten Code zu testen, bevor Sie in das Tutorial springen, laden Sie die **[neueste Version von GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)** herunter und installieren Sie sie.

### Powered by Adobe Stock

Viele der Bilder auf der WKND Reference-Website stammen aus [Adobe Stock](https://stock.adobe.com/) und sind Material von Drittanbietern, wie in den zusätzlichen Begriffen für Demo-Asset unter [https://www.adobe.com/legal/terms.html](https://www.adobe.com/de/legal/terms.html) definiert. Wenn Sie ein Adobe Stock-Bild für andere Zwecke verwenden möchten, die über die Anzeige dieser Demowebsite hinausgehen, z. B. für die Präsentation auf einer Website oder für Marketingmaterialien, können Sie eine Lizenz für Adobe Stock erwerben.

Mit Adobe Stock haben Sie Zugriff auf mehr als 140 Millionen hochqualitative, gebührenfreie Bilder, darunter Fotos, Grafiken, Videos und Vorlagen, um Ihre Kreativprojekte zu starten.

## Nächste Schritte {#next-steps}

Worauf wartest du?! Erfahren Sie, wie Sie [ein neues Adobe Experience Manager-Projekt mit dem AEM Projektarchetyp](./project-archetype/overview.md) oder [erstellen, indem Sie eine Site mit einer Site-Vorlage](./site-template/create-site.md) erstellen.
