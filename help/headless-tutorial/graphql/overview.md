---
title: Erste Schritte mit AEM Headless – GraphQL
description: Erfahren Sie mehr über die GraphQL-APIs in Experience Manager und ihre Funktionen.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: ht
source-wordcount: '268'
ht-degree: 100%

---

# Erste Schritte mit AEM Headless – GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

AEM GraphQL-APIs für Inhaltsfragmente 
unterstützen Headless-CMS-Szenarien, in denen externe Client-Anwendungen Erlebnisse mithilfe von in AEM verwalteten Inhalten rendern.

Eine moderne API zur Inhaltsbereitstellung ist für die Effizienz und Leistung von Javascript-basierten Frontend-Anwendungen von entscheidender Bedeutung. Die Verwendung einer REST-API bringt Herausforderungen mit sich:

* Große Anzahl von Anfragen beim Abrufen eines Objekts nach dem anderen
* Oft werden zu viel Inhalte bereitgestellt, das heißt, die Anwendung erhält mehr Inhalte als benötigt

Um diese Herausforderungen zu bewältigen, stellt GraphQL eine abfragebasierte API bereit, mit der Kunden AEM nur nach den benötigten Inhalten abfragen und mit einem einzigen API-Aufruf empfangen können.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

In diesem Video erhalten Sie einen Überblick über die in AEM implementierte GraphQL-API. Die GraphQL-API in AEM ist in erster Linie dazu bestimmt, im Rahmen einer Headless-Bereitstellung Inhaltsfragmente von AEM an nachgelagerte Anwendungen zu senden.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Erste Schritte mit AEM Headless – GraphQL"
>abstract="Erfahren Sie, wie Sie Inhaltsfragmente mit GraphQL bereitstellen."
>additional-url="https://video.tv.adobe.com/v/328618?captions=ger" text="Überblick zu GraphQL in AEM"

## Videoserie zu AEM Headless GraphQL

Erfahren Sie mehr über die GraphQL-Funktionen in AEM in der ausführlichen Anleitung zu Inhaltsfragmenten, AEM-GraphQL-APIs und Entwicklungstools.

* [Videoserie zu AEM Headless GraphQL](./video-series/modeling-basics.md)

## Praktisches Tutorial zu AEM Headless GraphQL

Erfahren Sie mehr über GraphQL-Funktionen in AEM, indem Sie eine React-App erstellen, die Inhaltsfragmente über AEM GraphQL-APIs nutzt.

* [Praktisches Tutorial zu AEM Headless GraphQL](./multi-step/overview.md)

## AEM GraphQL und AEM Content Services im Vergleich

|                                | AEM-GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM-Komponenten |
| Inhalt | Inhaltsfragmente | AEM-Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM-Seite |
| Bereitstellungsformat | GraphQL-JSON | AEM ComponentExporter-JSON |
