---
title: Erste Schritte mit AEM Sites – WKND-Tutorial
description: Erfahren Sie, wie Sie eine AEM-Site für eine fiktive Lifestyle-Marke namens WKND implementieren. Hier bekommen Sie eine Anleitung zu Experience Manager-Themen wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbaren Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: ht
source-wordcount: '599'
ht-degree: 100%

---

# Erste Schritte mit AEM Sites – WKND-Tutorial {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwicklerinnen und Entwickler konzipiert ist, für die Adobe Experience Manager (AEM) neu ist. Dieses Tutorial führt durch die Implementierung einer AEM-Site für die fiktive Lifestyle-Marke WKND. Das Tutorial geht auf grundlegende Themen wie Projekteinrichtung, Kernkomponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung mit Adobe Experience Manager Sites ein.

## Übersicht {#wknd-tutorial-overview}

Ziel dieses mehrteiligen Tutorials ist es, Entwicklerinnen und Entwicklern beizubringen, wie eine Website mit den neuesten Standards und Technologien in Adobe Experience Manager (AEM) implementiert wird. Nach Abschluss dieses Tutorials sollten Entwicklerinnen und Entwickler die grundlegende Basis der Plattform und die gängigen Designmuster in AEM verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Optionen zum Starten eines Sites-Projekts

Es gibt zwei grundlegende Ansätze zum Starten eines AEM Sites-Projekts.

**AEM-Projektarchetyp** – Traditioneller Ansatz für die AEM-Entwicklung durch Generierung eines AEM-Projekts mit einer Maven-Vorlage. Dies ist der empfohlene Ansatz für AEM 6.5/6.4-Projekte und AEM as a Cloud Service-Projekte, für die eine umfangreiche Anpassung erwartet wird. Das Tutorial bietet einen tieferen Einblick in die AEM-Entwicklung.

[Starten des Tutorials mit dem AEM-Projektarchetyp](./project-archetype/overview.md)

**AEM-Site-Vorlagen** – Wird auch als „Schnelle Site-Erstellung“ bezeichnet. Dies ist ein Ansatz mit wenig Code zum Generieren einer AEM-Site mithilfe einer vordefinierten Site-Vorlage. Nutzen Sie vorkonfigurierte Komponenten und Vorlagen, um eine Site schnell einzurichten und zu veröffentlichen. Verwenden Sie einen Design-Workflow, um markenspezifische Stile und Anpassungen mit lediglich CSS und JavaScript anzuwenden. Empfohlen für neue Projekte und Entwickelnde. Nur für AEM as a Cloud Service verfügbar.

[Tutorial mit einer Site-Vorlage starten](./site-template/create-site.md)

## Adobe XD-Benutzeroberflächen-Kit

Um dieses Tutorial näher an ein reales Szenario heranzuführen, haben die kompetenten UX-Designer von Adobe die Modelle für die Site mit [Adobe XD](https://www.adobe.com/products/xd.html?lang=de) erstellt. Im Laufe des Tutorials werden verschiedene Teile der Designs zu einer vollständig bearbeitbaren AEM-Site zusammengefügt. Ein besonderer Dank geht an **Lorenzo Buosi** und **Kilian Amenola**, die das schöne Design für die WKND-Site erstellt haben.

Laden Sie die XD-Benutzeroberflächen-Kits herunter:

* [AEM-Kernkomponente Benutzeroberflächen-Kit](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND Benutzeroberflächen-Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Referenz-Site {#reference-site}

Eine fertige Version der WKND-Site ist ebenfalls als Referenz verfügbar: [https://wknd.site/](https://wknd.site/)

Das Tutorial behandelt die wichtigsten Entwicklungsfähigkeiten, die für AEM-Entwicklerinnen und -Entwickler erforderlich sind, umfasst jedoch *nicht* das vollständige Erstellen der gesamten Site von Anfang bis Ende. Die fertige Referenz-Website ist eine weitere großartige Ressource, um mehr vorkonfigurierten AEM-Funktionen zu erkunden.

Um den neuesten Code zu testen, bevor Sie mit dem Tutorial beginnen, laden Sie die **[neueste Version von GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)** herunter.

### Basierend auf Adobe Stock

Viele der Bilder auf der WKND Reference-Website stammen aus [Adobe Stock](https://stock.adobe.com/de) und sind Material von Drittanbieterfirmen, wie in den zusätzlichen Bedingungen zu Demo-Asset unter [https://www.adobe.com/legal/terms.html](https://www.adobe.com/de/legal/terms.html) definiert. Wenn Sie ein Adobe Stock-Bild für andere Zwecke verwenden möchten, die über die Anzeige dieser Demowebsite hinausgehen, z. B. für die Präsentation auf einer Website oder für Marketingmaterialien, können Sie eine Lizenz für Adobe Stock erwerben.

Mit Adobe Stock haben Sie Zugriff auf mehr als 140 Millionen hochqualitative, gebührenfreie Bilder, darunter Fotos, Grafiken, Videos und Vorlagen, um Ihre Kreativprojekte voranzutreiben.

## Nächste Schritte {#next-steps}

Worauf warten Sie noch? Erlernen Sie das [Erstellen eines neuen Adobe Experience Manager-Projekts mithilfe des AEM-Projektarchetyps](./project-archetype/overview.md) oder das [Erstellen einer Site mit einer Site-Vorlage](./site-template/create-site.md).
