---
title: Seitenvorlagen
description: Erfahren Sie, wie Sie Seitenvorlagen erstellen und ändern. Machen Sie sich mit der Beziehung zwischen einer Seitenvorlage und einer Seite vertraut. Erfahren Sie, wie Sie Richtlinien einer Seitenvorlage konfigurieren, um granulare Governance und Markenkonsistenz für Inhalte bereitzustellen.  Eine gut strukturierte Zeitschriftenartikelvorlage wird basierend auf einem Mockup aus Adobe XD erstellt.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---

# Seitenvorlagen {#page-templates}

In diesem Kapitel werden wir die Beziehung zwischen einer Seitenvorlage und einer Seite untersuchen. Wir werden eine nicht formatierte Zeitschriftenartikelvorlage erstellen, die auf einigen Mockups von [AdobeXD](https://www.adobe.com/products/xd.html). Beim Erstellen der Vorlage werden Kernkomponenten und erweiterte Richtlinienkonfigurationen behandelt.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Verfassen von Inhalten und Veröffentlichen von Änderungen](./author-content-publish.md) -Kapitel abgeschlossen.

## Ziel

1. Machen Sie sich mit den Details von Seitenvorlagen und der Verwendung von Richtlinien zur Erzwingung der granularen Kontrolle von Seiteninhalten vertraut.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden.
1. Erstellen Sie eine neue Vorlage und erstellen Sie eine Seite.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Vorlage für Zeitschriftenartikel, mit der neue Zeitschriftenartikel erstellt und an einer gemeinsamen Struktur ausgerichtet werden können. Die Vorlage basiert auf Designs und einem in Adobe XD erstellten UI Kit. Dieses Kapitel konzentriert sich ausschließlich auf die Erstellung der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten sind funktionsfähig.

## Erstellen der Vorlage für die Zeitschriftenartikelseite

Beim Erstellen einer Seite müssen Sie eine Vorlage auswählen, die als Grundlage für die Erstellung der neuen Seite verwendet wird. Die Vorlage definiert die Struktur der resultierenden Seite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche von [Seitenvorlagen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=de):

1. **Struktur** - definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltsautoren nicht bearbeitet werden.
1. **Anfänglicher Inhalt** - definiert Komponenten, mit denen die Vorlage beginnt. Diese können von Inhaltsautoren bearbeitet und/oder gelöscht werden.
1. **Richtlinien** - definiert Konfigurationen dazu, wie sich Komponenten verhalten und welche Optionen Autoren zur Verfügung stehen.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Sicherungen entspricht. Dies geschieht in einer lokalen Instanz von AEM. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

Sie können die folgende Miniaturansicht verwenden, um Ihre Vorlage zu identifizieren (oder Ihre eigene hochladen!)

![Miniaturansicht der Artikelseitenvorlage](./assets/page-templates/article-page-template-thumbnail.png)


### Lösungspaket

A abgeschlossen [Lösung der Magazine-Vorlage](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) kann über Package Manager heruntergeladen und installiert werden.

## Kopf- und Fußzeile mit Experience Fragments aktualisieren {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, besteht darin, eine [Experience Fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Experience Fragments ermöglicht es Benutzern, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Experience Fragments bieten die Vorteile der Unterstützung von Multi-Site-Management und [Lokalisierung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

Die Site-Vorlage erzeugte eine Kopf- und Fußzeile. Aktualisieren Sie anschließend die Experience Fragments, um sie an die Modelle anzupassen. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Allgemeine Schritte für das folgende Video:

1. Beispielinhaltspaket herunterladen **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Laden Sie das Inhaltspaket mit Package Manager hoch und installieren Sie es.
1. Aktualisieren Sie die Erlebnisfragmente für Kopf- und Fußzeilen, um das WKND-Logo zu verwenden.

## Erstellen einer Magazine-Artikelseite

Erstellen Sie anschließend eine neue Seite mithilfe der Vorlage &quot;Zeitschriftenartikelseite&quot;. Verfassen Sie den Inhalt der Seite so, dass er mit den Site-Stichproben übereinstimmt. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

Verwenden Sie die [bereitgestellter Text](./assets/page-templates/la-skateparks-copy.txt) , um den Artikeltext zu füllen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben soeben eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An dieser Stelle entsprechen die Zeitschriftenartikelseite und die Website nicht den Markenstilen für WKND. Befolgen Sie die [Themen](theming.md) Tutorial zu den Best Practices zum Aktualisieren von CSS- und JavaScript-Frontend-Code, der zum Anwenden globaler Stile auf die Site verwendet wird.

### Lösungspaket

Ein Lösungspaket für dieses Kapitel kann heruntergeladen werden: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
