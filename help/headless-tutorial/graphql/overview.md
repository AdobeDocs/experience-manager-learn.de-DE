---
title: Erste Schritte mit AEM Headless - GraphQL
description: Erfahren Sie mehr über die Experience Manager GraphQL-APIs und ihre Funktionen.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 8%

---

# Erste Schritte mit AEM Headless – GraphQL {#getting-started-with-aem-headless}

AEM GraphQL-APIs für Inhaltsfragmente unterstützen Headless-CMS-Szenarien, in denen externe Client-Anwendungen Erlebnisse mit in AEM verwalteten Inhalten rendern.

Eine moderne API zur Inhaltsbereitstellung ist für Effizienz und Leistung von Javascript-basierten Frontend-Anwendungen von entscheidender Bedeutung. Die Verwendung einer REST-API bringt Herausforderungen mit sich:

* Große Anzahl von Anforderungen zum Abrufen eines Objekts nach dem anderen
* Oft &quot;übermäßige Bereitstellung&quot;von Inhalten, d. h., die Anwendung erhält mehr als benötigt

Um diese Herausforderungen zu bewältigen, stellt GraphQL eine abfragebasierte API bereit, mit der Kunden AEM nur nach den benötigten Inhalten abfragen und mit einem einzigen API-Aufruf empfangen können.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

In diesem Video erhalten Sie einen Überblick über die in AEM implementierte GraphQL-API. Die GraphQL-API in AEM ist in erster Linie dazu bestimmt, im Rahmen einer Headless-Implementierung AEM Inhaltsfragmente an nachgelagerte Anwendungen zu senden.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Erste Schritte mit AEM Headless – GraphQL"
>abstract="Erfahren Sie, wie Sie Inhaltsfragmente mit GraphQL bereitstellen."
>additional-url="https://video.tv.adobe.com/v/328618?captions=ger" text="Überblick über GraphQL in AEM"

## GraphQL-Videoserie mit Headless-AEM

Erfahren Sie mehr über AEM GraphQL-Funktionen durch die ausführliche Anleitung von Inhaltsfragmenten und AEM GraphQL-APIs und Entwicklungstools.

* [GraphQL-Videoserie mit Headless-AEM](./video-series/modeling-basics.md)

## Tutorial AEM Headless GraphQL Hands-On

Erfahren Sie mehr über AEM GraphQL-Funktionen, indem Sie eine React-App erstellen, die Inhaltsfragmente über AEM GraphQL-APIs nutzt.

* [Tutorial AEM Headless GraphQL Hands-On](./multi-step/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | AEM GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM-Komponenten |
| Inhalt | Inhaltsfragmente | AEM-Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM Seite |
| Versandformat | GraphQL JSON | AEM ComponentExporter JSON |
