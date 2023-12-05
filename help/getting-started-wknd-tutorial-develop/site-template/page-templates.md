---
title: Seitenvorlagen
description: Erfahren Sie, wie Sie Seitenvorlagen erstellen und ändern. Lernen Sie den Zusammenhang zwishen Seitenvorlage und Seite kennen. Erfahren Sie, wie Sie die Richtlinien einer Seitenvorlage konfigurieren, um für Inhalte granulare Governance und Markenkonsistenz zu gewährleisten.  Basierend auf einem Mockup wird in Adobe XD eine gut strukturierte Zeitschriftenartikelvorlage erstellt.
version: Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1601
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 100%

---

# Seitenvorlagen {#page-templates}

In diesem Kapitel werden wir die Beziehung zwischen einer Seitenvorlage und einer Seite erkunden. Wir werden eine unformatierte Zeitschriftenartikel-Vorlage erstellen, die auf einigen Mockups von [AdobeXD](https://www.adobe.com/products/xd.html?lang=de) basiert. Im Laufe des Erstellens der Vorlage werden Kernkomponenten und erweiterte Richtlinienkonfigurationen behandelt.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird vorausgesetzt, dass die Schritte, die im Kapitel [Inhalt verfassen und Änderungen veröffentlichen](./author-content-publish.md) beschrieben sind, abgeschlossen wurden.

## Ziel

1. Machen Sie sich mit den Details von Seitenvorlagen und der Verwendung von Richtlinien zur Erzwingung der granularen Steuerung von Seiteninhalten vertraut.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden.
1. Erstellen Sie eine neue Vorlage und verfassen Sie eine Seite.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Vorlage für Zeitschriftenartikel, mit der neue Zeitschriftenartikel erstellt und an einer gemeinsamen Struktur ausgerichtet werden können. Die Vorlage basiert auf Designs und einem in AdobeXD erstellten UI-Kit. Dieses Kapitel konzentriert sich ausschließlich auf die Erstellung der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten sind funktionsfähig.

## Erstellen der Vorlage für die Zeitschriftenartikelseite

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der neuen Seite verwendet. Die Vorlage definiert die Struktur der resultierenden Seite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche bei [Seitenvorlagen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=de):

1. **Struktur**: definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltsautorinnen und -autoren nicht bearbeitet werden.
1. **Anfänglicher Inhalt**: definiert Komponenten, mit denen die Vorlage beginnt. Diese können von Inhaltsautorinnen und -autoren bearbeitet und/oder gelöscht werden.
1. **Richtlinien**: definiert Konfigurationen dazu, wie sich Komponenten verhalten und welche Optionen Autorinnen und Autoren zur Verfügung stehen.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Mockups entspricht. Dies geschieht in einer lokalen Instanz von AEM. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Sie können die folgende Miniaturansicht verwenden, um Ihre Vorlage zu identifizieren (oder Ihre eigene hochladen!)

![Miniaturansicht der Artikelseitenvorlage](./assets/page-templates/article-page-template-thumbnail.png)


### Lösungspaket

Eine fertige [Lösung der Magazinvorlage](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) kann über den Package Manager heruntergeladen und installiert werden.

## Aktualisieren von Kopf- und Fußzeile mit Experience Fragments {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, ist die Verwendung eines [Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de). Experience Fragments ermöglichen es Benutzenden, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Experience Fragments bieten die Vorteile der Unterstützung von Multi-Site-Management und [Lokalisierung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=de#localized-site-structure).

Die Site-Vorlage hat schon eine Kopf- und Fußzeile generiert. Aktualisieren Sie anschließend die Experience Fragments, um sie an die Mockups anzupassen. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Allgemeine Schritte für das folgende Video:

1. Laden Sie das Beispielinhaltspaket **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)** herunter.
1. Laden Sie das Inhaltspaket mit dem Package Manager hoch und installieren Sie es.
1. Aktualisieren Sie die Experience Fragments für Kopf- und Fußzeilen, um das WKND-Logo zu verwenden.

## Erstellen einer Zeitschriftenartikelseite

Erstellen Sie anschließend eine neue Seite mithilfe der Vorlage „Zeitschriftenartikelseite“. Verfassen Sie den Inhalt der Seite so, dass er mit den Mockups der Site übereinstimmt. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Verwenden Sie den [vorgegebenen Text](./assets/page-templates/la-skateparks-copy.txt), um den Artikelinhalt zu füllen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben soeben eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An dieser Stelle entsprechen die Artikelseite des Magazins und die Website nicht den Markenstilen für WKND. Befolgen Sie das Tutorial [Themen](theming.md) zu den Best Practices beim Aktualisieren von CSS- und JavaScript-Frontend-Code, der zum Anwenden globaler Stile auf die Site verwendet wird.

### Lösungspaket

Ein Lösungspaket für dieses Kapitel kann hier heruntergeladen werden: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
