---
title: Seitenvorlagen
seo-title: Erste Schritte mit AEM Sites - Seitenvorlagen
description: Erfahren Sie, wie Sie Seitenvorlagen erstellen und ändern. Verstehen Sie die Beziehung zwischen einer Seitenvorlage und einer Seite. Erfahren Sie, wie Sie die Richtlinien einer Seitenvorlage konfigurieren, um granulare Governance und Markenkonsistenz für Inhalte bereitzustellen.  Eine gut strukturierte Artikelvorlage für Zeitschriften wird auf der Grundlage eines Mockups von Adobe XD erstellt.
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Hauptkomponenten, bearbeitbare Vorlagen, Seiten-Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 4%

---


# Seitenvorlagen {#page-templates}

>[!CAUTION]
>
> Die hier präsentierten Funktionen zur schnellen Site-Erstellung werden in der zweiten Jahreshälfte 2021 veröffentlicht. Die zugehörige Dokumentation steht zu Vorschauen zur Verfügung.

In diesem Kapitel werden wir die Beziehung zwischen einer Seitenvorlage und einer Seite untersuchen. Wir werden eine nicht formatierte Zeitschriftenartikelvorlage erstellen, die auf einigen Modellen von [AdobeXD](https://www.adobe.com/products/xd.html) basiert. Beim Erstellen der Vorlage werden die Kernkomponenten und die erweiterten Richtlinienkonfigurationen behandelt.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Lernprogramm und es wird davon ausgegangen, dass die im Kapitel [Autor- und Veröffentlichungsänderungen](./author-content-publish.md) beschriebenen Schritte abgeschlossen wurden.

## Vorgabe

1. Inspect ein in Adobe XD erstelltes Seitendesign und ordnen Sie es Kernkomponenten zu.
1. Machen Sie sich mit den Details von Seitenvorlagen und der Art und Weise vertraut, wie Richtlinien verwendet werden können, um eine granulare Steuerung des Seiteninhalts zu erzwingen.
1. Erfahren Sie, wie Vorlagen und Seiten verknüpft werden.

## Was Sie erstellen werden {#what-you-will-build}

In diesem Teil des Tutorials erstellen Sie eine neue Vorlage für die Zeitschriftenartikelseite, mit der neue Zeitschriftenartikel erstellt und an eine gemeinsame Struktur angepasst werden können. Die Vorlage basiert auf Designs und einem UI-Kit, das in AdobeXD produziert wird. Dieses Kapitel konzentriert sich nur auf das Erstellen der Struktur oder des Skeletts der Vorlage. Es werden keine Stile implementiert, aber die Vorlage und die Seiten funktionieren.

## Benutzeroberfläche mit Adobe XD {#adobexd}

In den meisten Fällen ist die Planung für einen neuen Website-Beginn mit Mockups und statischen Designs. [Adobe ](https://www.adobe.com/products/xd.html) XDist ein Design-Tool zum Erstellen von Benutzererlebnissen. Anschließend werden wir ein UI-Kit und -Modelle prüfen, um die Struktur der Artikelseitenvorlage zu planen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Laden Sie die  [WKND-Artikelentwurfsdatei](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** herunter.

>[!NOTE]
>
> Ein generisches [AEM Core-Komponenten-UI-Kit ist ebenfalls als Ausgangspunkt für benutzerdefinierte Projekte verfügbar.](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)

## Erstellen der Zeitschriftenartikelseitenvorlage

Wenn Sie eine Seite erstellen, müssen Sie eine Vorlage auswählen. Diese wird als Grundlage für die Erstellung der neuen Seite verwendet. Die Vorlage definiert die Struktur der Zielseite, den anfänglichen Inhalt und die zulässigen Komponenten.

Es gibt 3 Hauptbereiche von [Seitenvorlagen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Struktur** : Definiert Komponenten, die Teil der Vorlage sind. Diese können von Inhaltserstellern nicht bearbeitet werden.
1. **Anfänglicher Inhalt** : Definiert Komponenten, mit denen die Vorlage Beginn wird. Diese können von Inhaltserstellern bearbeitet und/oder gelöscht werden
1. **Richtlinien**  - legt Konfigurationen fest, wie sich Komponenten verhalten und welche Optionen Autoren zur Verfügung stehen.

Erstellen Sie anschließend eine neue Vorlage in AEM, die der Struktur der Sicherungen entspricht. Dies geschieht in einer lokalen Instanz von AEM. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Lösungspaket

Eine fertige [Lösung der Zeitschriftenvorlage](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) kann über Package Manager heruntergeladen und installiert werden.

## Kopf- und Fußzeile mit Erlebnisfragmenten aktualisieren {#experience-fragments}

Eine gängige Praxis bei der Erstellung globaler Inhalte, wie Kopf- oder Fußzeilen, ist die Verwendung eines [Erlebnisfragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Erlebnisfragmente ermöglichen es Benutzern, mehrere Komponenten zu kombinieren, um eine einzelne, referenzierbare Komponente zu erstellen. Erlebnisfragmente haben den Vorteil, dass die Verwaltung mehrerer Sites und [lokale Anpassung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure) unterstützt werden.

Die Site-Vorlage hat eine Kopf- und Fußzeile generiert. Aktualisieren Sie anschließend die Erlebnisfragmente, um sie mit den Sicherungen abzustimmen. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Schritte auf hoher Ebene für das folgende Video:

1. Laden Sie das Beispiel-Inhaltspaket **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)** herunter.
1. Laden Sie das Inhaltspaket mit Package Manager hoch und installieren Sie es.
1. Aktualisieren Sie die Erlebnisfragmente für Kopf- und Fußzeile, um das WKND-Logo zu verwenden.

## Erstellen einer Zeitschriftenartikelseite

Erstellen Sie anschließend eine neue Seite mit der Vorlage &quot;Zeitschriftenartikelseite&quot;. Verfassen Sie den Inhalt der Seite so, dass er mit den Site-Modellen übereinstimmt. Gehen Sie wie folgt vor:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade eine neue Vorlage und Seite mit Adobe Experience Manager Sites erstellt.

### Nächste Schritte {#next-steps}

An diesem Punkt die Magazin Artikel-Seite und die Website nicht mit den Markenstilen für WKND. Folgen Sie dem Tutorial [Designing](theming.md), um die Best Practices für die Aktualisierung von CSS- und JavaScript-Frontend-Code zu erfahren, mit dem globale Stile auf die Site angewendet werden.

### Lösungspaket

Ein Lösungspaket für dieses Kapitel steht zum Download bereit: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
