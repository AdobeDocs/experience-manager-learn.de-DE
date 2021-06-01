---
title: Erste Schritte mit AEM Headless - GraphQL
description: Eine Übersicht über AEM GraphQL-APIs und -Funktionen.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---


# Erste Schritte mit AEM Headless - GraphQL

AEM GraphQL-APIs für Inhaltsfragmente
unterstützt Headless-CMS-Szenarien, in denen externe Client-Anwendungen Erlebnisse mithilfe von in AEM verwalteten Inhalten rendern.

Eine moderne API zur Inhaltsbereitstellung ist für Effizienz und Leistung von Javascript-basierten Frontend-Anwendungen von entscheidender Bedeutung. Die Verwendung einer REST-API bringt Herausforderungen mit sich:

* Große Anzahl von Anforderungen zum Abrufen eines Objekts nach dem anderen
* Oft &quot;übermäßige Bereitstellung&quot;von Inhalten, d. h., die Anwendung erhält mehr als benötigt

Um diese Herausforderungen zu bewältigen, stellt GraphQL eine abfragebasierte API bereit, mit der Kunden AEM nur nach den benötigten Inhalten abfragen und mit einem einzigen API-Aufruf empfangen können.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

In diesem Video erhalten Sie einen Überblick über die in AEM implementierte GraphQL-API. Die GraphQL-API in AEM ist in erster Linie dazu bestimmt, im Rahmen einer Headless-Implementierung AEM Inhaltsfragmente an nachgelagerte Anwendungen zu senden.

## AEM Headless-GraphQL-Videoserie

Erfahren Sie mehr über AEM GraphQL-Funktionen durch die ausführliche Anleitung von Inhaltsfragmenten und AEM GraphQL-APIs und Entwicklungstools.

* [AEM Headless-GraphQL-Videoserie](./video-series/modeling-basics.md)

## AEM Headless GraphQL Hands-On-Tutorial

Erkunden Sie AEM GraphQL-Funktionen, indem Sie eine React-App erstellen, die Inhaltsfragmente über AEM GraphQL-APIs nutzt.

* [AEM Headless GraphQL Hands-On-Tutorial](./multi-step/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | AEM GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM von Komponenten |
| Inhalt | Inhaltsfragmente | AEM von Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM Seite |
| Versandformat | GraphQL JSON | AEM ComponentExporter JSON |