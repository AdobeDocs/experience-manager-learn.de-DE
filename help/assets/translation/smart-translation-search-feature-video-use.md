---
title: Verwenden der intelligenten Übersetzungssuche mit AEM Assets
description: Die intelligente Übersetzungssuche ermöglicht die automatische Suche und Erkennung von Fremdsprachen über AEM-Inhalte hinweg, sowohl Assets als auch Seiten, wobei mehr als 50 Sprachen unterstützt und damit der Bedarf an manueller Übersetzung von Inhalten reduziert wird.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 100%

---

# Verwenden der intelligenten Übersetzungssuche mit AEM Assets{#using-smart-translation-search-with-aem-assets}

Die intelligente Übersetzungssuche ermöglicht die automatische Suche und Erkennung von Fremdsprachen über AEM-Inhalte hinweg, sowohl Assets als auch Seiten, wobei mehr als 50 Sprachen unterstützt und damit der Bedarf an manueller Übersetzung von Inhalten reduziert wird.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

Die intelligente Übersetzungssuche von AEM ermöglicht es Benutzenden, mithilfe von nicht englischen Begriffen nach Inhalten in AEM zu suchen, um sie mit den Assets in AEM abzugleichen, diedie entsprechenden englischen Begriffe enthalten.

Die intelligente Übersetzungssuche ist eine perfekte Ergänzung zu AEM Smart Tags, die auf Assets in englischer Sprache angewendet werden.

Dieses Video setzt voraus, dass die [intelligente Übersetzungssuche von AEM](smart-translation-search-technical-video-setup.md) eingerichtet wurde.

## Funktionsweise der intelligenten Übersetzungssuche {#how-smart-translation-search-works}

![Flussdiagramm der intelligenten Übersetzungssuche](assets/smart-translation-search-flow.png)

1. AEM-Benutzende führen eine Volltextsuche durch und stellen dabei einen lokalisierten Suchbegriff bereit (z. B. den spanischen Begriff für „Mann“, „hombre“).
2. Die intelligente Übersetzungssuche, die vom OSGi-Bundle für maschinelle Übersetzung von Apache Oak bereitgestellt wird, ist aktiv und wertet aus, ob die bereitgestellten Suchbegriffe mithilfe der registrierten Sprachpakete übersetzt werden können.
3. Alle übersetzten Begriffe aus Schritt 2 werden erfasst und die Abfrage wird intern erweitert, um sie als Suchbegriffe einzuschließen. Dieser erweiterte Satz von Suchbegriffen wird normalerweise mit den Suchindizes von AEM verglichen, um relevante Treffer zu finden.
4. Die Suchergebnisse, die mit dem ursprünglichen Begriff („hombre“) oder dem auf Englisch übersetzten Begriff („man“) übereinstimmen, werden erfasst und den Benutzenden als Suchergebnisse zurückgegeben.

## Zusätzliche Ressourcen{#additional-resources}

* [Einrichten der intelligenten Übersetzungssuche mit AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua-Sprachpakete](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
