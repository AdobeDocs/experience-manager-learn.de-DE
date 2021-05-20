---
title: Seitenvorlagen
seo-title: Erste Schritte mit AEM Sites - Seitenvorlagen
description: Erfahren Sie, wie Sie Seitenvorlagen erstellen und ändern. Machen Sie sich mit der Beziehung zwischen einer Seitenvorlage und einer Seite vertraut. Erfahren Sie, wie Sie Richtlinien einer Seitenvorlage konfigurieren, um granulare Governance und Markenkonsistenz für Inhalte bereitzustellen.  Eine gut strukturierte Zeitschriftenartikelvorlage wird auf der Grundlage eines Mockups aus Adobe XD erstellt.
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Kernkomponenten, bearbeitbare Vorlagen, Seiten-Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 4%

---


# Seitenvorlagen {#page-templates}

>[!CAUTION]
>
> Die hier vorgestellten Funktionen zur schnellen Site-Erstellung werden im zweiten Halbjahr 2021 veröffentlicht. Die zugehörige Dokumentation steht für Vorschauzwecke zur Verfügung.

In diesem Kapitel werden wir die Beziehung zwischen einer Seitenvorlage und einer Seite untersuchen. Wir werden eine nicht formatierte Zeitschriftenartikelvorlage erstellen, die auf einigen Mustern von [AdobeXD](https://www.adobe.com/products/xd.html) basiert. Beim Erstellen der Vorlage werden Kernkomponenten und erweiterte Richtlinienkonfigurationen behandelt.

## Voraussetzungen {#prerequisites}

Es handelt sich hierbei um ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im Kapitel [Autoren- und Veröffentlichungsänderungen](./author-content-publish.md) beschriebenen Schritte abgeschlossen sind.

## Vorgabe

1. Inspect ist ein in Adobe XD erstellter Seitenentwurf und ordnet ihn Kernkomponenten zu.
1. Machen Sie sich mit den Details von Seitenvorlagen und der Verwendung von Richtlinien zur Erzwingung der granularen Kontrolle von Seiteninhalten vertraut.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Vorlage für Zeitschriftenartikel, mit der neue Zeitschriftenartikel erstellt und an einer gemeinsamen Struktur ausgerichtet werden können. Die Vorlage basiert auf Designs und einem in Adobe XD erstellten UI Kit. Dieses Kapitel konzentriert sich ausschließlich auf die Erstellung der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten funktionieren.

## Benutzeroberflächenplanung mit Adobe XD {#adobexd}

In den meisten Fällen beginnt die Planung für eine neue Website mit Stichproben und statischen Designs. [Adobe ](https://www.adobe.com/products/xd.html) XDist ein Design-Tool zum Erstellen von Benutzererlebnissen. Als Nächstes werden wir ein UI-Kit und Modelle untersuchen, um die Struktur der Artikelseitenvorlage zu planen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Laden Sie die  [WKND-Artikelentwurfsdatei](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** herunter.

>[!NOTE]
>
> Ein generisches [AEM Kernkomponenten-UI-Kit ist auch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) als Ausgangspunkt für benutzerdefinierte Projekte verfügbar.

## Erstellen der Vorlage für die Zeitschriftenartikelseite

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der neuen Seite verwendet. Die Vorlage definiert die Struktur der resultierenden Seite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt drei Hauptbereiche von [Seitenvorlagen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Struktur**  - definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltsautoren nicht bearbeitet werden.
1. **Anfänglicher Inhalt**  - definiert Komponenten, mit denen die Vorlage beginnen soll. Diese können von Inhaltsautoren bearbeitet und/oder gelöscht werden
1. **Richtlinien**  - Definition von Konfigurationen zum Verhalten von Komponenten und zu den verfügbaren Optionen für Autoren.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Sicherungen entspricht. Dies geschieht in einer lokalen Instanz von AEM. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Lösungspaket

Eine fertige [Lösung der Magazine-Vorlage](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) kann über Package Manager heruntergeladen und installiert werden.

## Kopf- und Fußzeile mit Experience Fragments aktualisieren {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte wie Kopf- oder Fußzeilen besteht darin, ein [Experience Fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html) zu verwenden. Experience Fragments ermöglicht es Benutzern, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Experience Fragments bieten den Vorteil, dass die Verwaltung mehrerer Websites und [Lokalisierung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure) unterstützt werden.

Die Site-Vorlage erzeugte eine Kopf- und Fußzeile. Aktualisieren Sie anschließend die Experience Fragments, um sie an die Modelle anzupassen. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Allgemeine Schritte für das folgende Video:

1. Laden Sie das Beispielinhaltspaket **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)** herunter.
1. Laden Sie das Inhaltspaket mit Package Manager hoch und installieren Sie es.
1. Aktualisieren Sie die Erlebnisfragmente für Kopf- und Fußzeilen, um das WKND-Logo zu verwenden.

## Erstellen einer Magazine-Artikelseite

Erstellen Sie anschließend eine neue Seite mithilfe der Vorlage &quot;Zeitschriftenartikelseite&quot;. Verfassen Sie den Inhalt der Seite so, dass er mit den Site-Stichproben übereinstimmt. Führen Sie die folgenden Schritte aus:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben soeben eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An dieser Stelle entsprechen die Zeitschriftenartikelseite und die Website nicht den Markenstilen für WKND. Im Tutorial [Designing](theming.md) erfahren Sie mehr über die Best Practices zum Aktualisieren von CSS- und JavaScript-Frontend-Code, der zum Anwenden globaler Stile auf die Site verwendet wird.

### Lösungspaket

Ein Lösungspaket für dieses Kapitel kann heruntergeladen werden: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
