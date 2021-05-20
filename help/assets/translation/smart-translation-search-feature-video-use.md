---
title: Verwenden der intelligenten Übersetzungssuche mit AEM Assets
description: Die intelligente Suche nach Übersetzungen ermöglicht die automatische Suche und Erkennung von Fremdsprachen über AEM Inhalte hinweg, sowohl Assets als auch Seiten, wodurch mehr als 50 Sprachen unterstützt und die manuelle Übersetzung von Inhalten verringert wird.
version: 6.3, 6.4, 6.5
feature: Suchen
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# Verwenden der intelligenten Übersetzungssuche mit AEM Assets{#using-smart-translation-search-with-aem-assets}

Die intelligente Suche nach Übersetzungen ermöglicht die automatische Suche und Erkennung von Fremdsprachen über AEM Inhalte hinweg, sowohl Assets als auch Seiten, wodurch mehr als 50 Sprachen unterstützt und die manuelle Übersetzung von Inhalten verringert wird.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM intelligente Übersetzungssuche ermöglicht es Benutzern, mithilfe von nicht englischen Begriffen nach Inhalten in AEM zu suchen, um sie mit den Assets in AEM abzugleichen, die entsprechende englische Begriffe enthalten.

Die intelligente Übersetzungssuche ist ein perfektes Kompliment für AEM Smart-Tags, die auf Assets in englischer Sprache angewendet werden.

In diesem Video wird davon ausgegangen, dass [AEM Smart Translation Search](smart-translation-search-technical-video-setup.md) eingerichtet wurde.

## Funktionsweise der intelligenten Übersetzungssuche {#how-smart-translation-search-works}

![Flussdiagramm für intelligente Übersetzungssuche](assets/smart-translation-search-flow.png)

1. AEM Benutzer führt eine Volltextsuche durch und stellt dabei einen lokalisierten Suchbegriff bereit (z. B. der spanische Begriff &quot;man&quot;, &quot;hombre&quot;).
2. Die intelligente Übersetzungssuche, die vom OSGi-Bundle für maschinelle Übersetzung von Apache Oak bereitgestellt wird, ist aktiv und wertet aus, ob die bereitgestellten Suchbegriffe mithilfe der registrierten Sprachpakete übersetzt werden können.
3. Alle übersetzten Begriffe aus Schritt 2 werden erfasst und die Abfrage wird intern erweitert, um sie als Suchbegriffe einzuschließen. Dieser erweiterte Satz von Suchbegriffen, wenn er normal mit AEM Suchindizes ausgewertet wird, die relevante Übereinstimmungen finden.
4. Die Suchergebnisse, die mit dem ursprünglichen Begriff (&#39;hombre&#39;) oder dem übersetzten Begriff (&#39;man&#39;) übereinstimmen, werden erfasst und dem Benutzer als Suchergebnisse zurückgegeben.

## Zusätzliche Ressourcen{#additional-resources}

* [Einrichten der intelligenten Übersetzungssuche mit AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)