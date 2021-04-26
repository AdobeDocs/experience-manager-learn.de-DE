---
title: Erste Schritte mit AEM Sites - WKND-Tutorial
description: Erste Schritte mit AEM Sites - WKND-Tutorial. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager sind. Das Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke, die WKND. Das Lernprogramm behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Core-Komponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
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
feature: Hauptkomponenten, Seiten-Editor, bearbeitbare Vorlagen, AEM Projektarchiv
topic: Content-Management, Entwicklung
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 6%

---


# Erste Schritte mit AEM Sites - WKND-Tutorial {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler entwickelt wurde, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke der WKND. Das Lernprogramm behandelt grundlegende Themen wie Projekteinrichtung, Kernkomponenten, bearbeitbare Vorlagen, clientseitige Bibliotheken und Komponentenentwicklung mit Adobe Experience Manager Sites.

## Überblick {#wknd-tutorial-overview}

Ziel dieses mehrteiligen Lernprogramms ist es, Entwicklern beizubringen, wie eine Website mit den neuesten Standards und Technologien in Adobe Experience Manager (AEM) implementiert wird. Nach Abschluss dieses Lernprogramms sollte ein Entwickler die Grundlagen der Plattform und die Kenntnisse über gängige Designmuster in AEM verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Optionen zum Starten eines Sites-Projekts

Es gibt zwei grundsätzliche Ansätze, um ein AEM Sites-Projekt zu starten.

**AEM Project Archetype**  - Traditioneller Ansatz zur AEM Entwicklung durch Generierung eines AEM mit einer Maven-Vorlage. Dies ist der empfohlene Ansatz für AEM 6.5/6.4-Projekte und AEM als Cloud Service-Projekte, die eine starke Anpassung vorwegnehmen. Das Tutorial Angebot einen tieferen Einstieg in AEM Entwicklung.

[Beginn des Lernprogramms mit dem AEM](./project-archetype/overview.md)

**AEM Site-Vorlagen**  - Ein Ansatz mit niedrigem Code zum Generieren einer AEM Site mithilfe einer vordefinierten Site-Vorlage. Verwenden Sie vordefinierte Komponenten und Vorlagen, um eine Site schnell in Betrieb zu nehmen. Verwenden Sie einen Arbeitsablauf zum Erstellen von Designs, um markenspezifische Stile und Anpassungen nur mit CSS und JavaScript anzuwenden. Empfohlen für neue Projekte und Entwickler. Zurzeit nur für AEM als Cloud Service verfügbar.

[Beginn des Tutorials mit einer Site-Vorlage](./site-template/create-site.md)

## Adobe XD UI Kit

Um dieses Tutorial einem realen Szenario näher zu bringen, haben talentierte UX-Designer der Adobe die Mocker für die Site mit [Adobe XD](https://www.adobe.com/products/xd.html) erstellt. Im Laufe des Tutorials werden verschiedene Teile der Entwürfe in eine vollständig autorfähige AEM Site implementiert. Besonderer Dank an **Lorenzo Buosi** und **Kilian Amenola**, die das schöne Design für die WKND-Site erstellt haben.

Laden Sie die XD UI-Kits herunter:

* [AEM-UI-Kit](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND-UI-Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Referenzsite {#reference-site}

Eine fertige Version der WKND-Site ist auch als Referenz verfügbar: [https://wknd.site/](https://wknd.site/)

Das Tutorial behandelt die wichtigsten Entwicklungsfähigkeiten, die für einen AEM-Entwickler erforderlich sind, erstellt aber *nicht* die gesamte Site von Ende zu Ende. Die fertige Referenz-Website ist eine weitere großartige Ressource, um mehr von AEM vordefinierten Funktionen zu erforschen und zu sehen.

Um den neuesten Code zu testen, bevor Sie in das Tutorial springen, laden Sie die **[neueste Version von GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)** herunter und installieren Sie sie.

### Powered by Adobe Stock

Viele der Bilder auf der WKND-Referenz-Website stammen von [Adobe Stock](https://stock.adobe.com/) und sind Material von Drittanbietern, wie in den zusätzlichen Begriffen für das Demoelement unter [https://www.adobe.com/legal/terms.html](https://www.adobe.com/de/legal/terms.html) definiert. Wenn Sie ein Adobe Stock-Bild für andere Zwecke als die Anzeige dieser Demo-Website verwenden möchten, z. B. für die Darstellung auf einer Website oder in Marketingmaterialien, können Sie eine Lizenz auf Adobe Stock erwerben.

Mit Adobe Stock haben Sie Zugang zu über 140 Millionen hochwertigen, gebührenfreien Bildern, darunter Fotos, Grafiken, Videos und Vorlagen, um Ihre Kreativprojekte in Gang zu setzen.

## Nächste Schritte {#next-steps}

Worauf wartest du? Erfahren Sie, wie Sie mit dem AEM Projekttyp [oder [eine Site mit einer Site-Vorlage](./site-template/create-site.md) erstellen.](./project-archetype/overview.md)
