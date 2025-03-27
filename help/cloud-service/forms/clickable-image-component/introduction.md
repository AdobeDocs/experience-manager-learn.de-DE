---
title: Erstellen einer klickbaren Bildkomponente
description: Erstellen Sie klickbare Bildkomponenten in AEM Forms as a Cloud Service.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '138'
ht-degree: 100%

---

# Einführung in klickbare Bilder

Die Verwendung klickbarer Bilder in Forms kann zu einem Anwendererlebnis führen, das fesselnder, intuitiver und visuell ansprechender ist. Für diesen Artikel wird SVG als Format für die klickbaren Bilder verwendet, denn es bietet verschiedene Vorteile, insbesondere im Hinblick auf die Design-Flexibilität, Leistung und das Anwendererlebnis.
SVG kann mit Adobe Illustrator oder einem beliebigen kostenlosen Onlinetool erstellt werden. Der Anwendungsfall wird anhand der [US-Karte](https://simplemaps.com/resources/svg-us) von simplemaps demonstriert.

## Anwendungsfall für die Verwendung einer klickbaren Karte der USA

Die klickbare Karte der USA ermöglicht es Benutzenden, sich bundesstaatenspezifische Formularübermittlungen anzusehen. Wenn eine Benutzerin oder ein Benutzer auf einen Bundesstaat klickt, werden die Übermittlungen von diesem Bundesstaat mit der Option zum Öffnen einer bestimmten Übermittlung aufgelistet.
