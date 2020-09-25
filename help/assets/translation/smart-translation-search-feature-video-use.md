---
title: Intelligente Übersetzungssuche mit AEM Assets
seo-title: Intelligente Übersetzungssuche mit AEM Assets
description: Die intelligente Suche nach Übersetzungen ermöglicht die automatische Suche und Erkennung von Inhalten in mehreren Sprachen, sowohl über Assets als auch über Seiten hinweg. Dadurch werden mehr als 50 Sprachen unterstützt und die manuelle Übersetzung von Inhalten wird reduziert.
seo-description: Die intelligente Suche nach Übersetzungen ermöglicht die automatische Suche und Erkennung von Inhalten in mehreren Sprachen, sowohl über Assets als auch über Seiten hinweg. Dadurch werden mehr als 50 Sprachen unterstützt und die manuelle Übersetzung von Inhalten wird reduziert.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Intelligente Übersetzungssuche mit AEM Assets{#using-smart-translation-search-with-aem-assets}

Die intelligente Suche nach Übersetzungen ermöglicht die automatische Suche und Erkennung von Inhalten in mehreren Sprachen, sowohl über Assets als auch über Seiten hinweg. Dadurch werden mehr als 50 Sprachen unterstützt und die manuelle Übersetzung von Inhalten wird reduziert.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Suche nach intelligenter Übersetzung ermöglicht es Benutzern, mithilfe von nicht englischen Begriffen nach Inhalten in AEM zu suchen, um mit den Assets in AEM zu übereinstimmen, die entsprechende englische Begriffe enthalten.

Die intelligente Übersetzungssuche ist ein perfektes Kompliment für AEM Smart Tags, die auf Assets in Englisch angewendet werden.

In diesem Video wird davon ausgegangen, dass [AEM intelligente Übersetzungssuche](smart-translation-search-technical-video-setup.md) eingerichtet wurde.

## Funktionsweise der Suche nach intelligenten Übersetzungen {#how-smart-translation-search-works}

![Diagramm des intelligenten Übersetzungssuchablaufs](assets/smart-translation-search-flow.png)

1. AEM Benutzer führt eine Volltextsuche durch und gibt einen lokalisierten Suchbegriff (z. der spanische Begriff &quot;Mann&quot;, &quot;hombre&quot;).
2. Die intelligente Übersetzungssuche, die vom Apache Oak Machine Translation OSGi-Bundle bereitgestellt wird, wird aktiviert und bewertet, ob die angegebenen Suchbegriffe mit den registrierten Sprachpaketen übersetzt werden können.
3. Alle übersetzten Begriffe aus Schritt 2 werden erfasst und die Abfrage wird intern erweitert, um sie als Suchbegriffe einzuschließen. Dieser erweiterte Satz von Suchbegriffen, wenn diese normal mit AEM Suchindizes ausgewertet werden, die relevante Übereinstimmungen finden.
4. Die Suchergebnisse, die mit dem ursprünglichen Begriff (&#39;hombre&#39;) oder dem übersetzten Begriff (&#39;man&#39;) übereinstimmen, werden erfasst und dem Benutzer als Suchergebnisse zurückgegeben.

## Zusätzliche Ressourcen{#additional-resources}

* [Intelligente Übersetzungssuche mit AEM Assets einrichten](smart-translation-search-technical-video-setup.md)
* [Apache Joshua-Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)