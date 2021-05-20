---
title: AEM Headless-Tutorials
description: Eine Sammlung von Tutorials zur Verwendung von Adobe Experience Manager as a Headless CMS.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# AEM Headless-Tutorials

Adobe Experience Manager bietet mehrere Optionen zum Definieren von Headless-Endpunkten und zum Bereitstellen seines Inhalts als JSON. Verwenden Sie praktische Tutorials, um zu erfahren, wie Sie die verschiedenen Optionen verwenden und auswählen können, was für Sie am besten ist.

## Tutorial AEM GraphQL-APIs

AEM GraphQL-APIs für Inhaltsfragmente
unterstützt Headless-CMS-Szenarien, in denen externe Client-Anwendungen Erlebnisse mithilfe von in AEM verwalteten Inhalten rendern.

Eine moderne API zur Inhaltsbereitstellung ist für Effizienz und Leistung von Javascript-basierten Frontend-Anwendungen von entscheidender Bedeutung. Die Verwendung einer REST-API bringt Herausforderungen mit sich:

* Große Anzahl von Anforderungen zum Abrufen eines Objekts nach dem anderen
* Oft &quot;übermäßige Bereitstellung&quot;von Inhalten, d. h., die Anwendung erhält mehr als benötigt

Um diese Herausforderungen zu bewältigen, stellt GraphQL eine abfragebasierte API bereit, mit der Kunden AEM nur nach den benötigten Inhalten abfragen und mit einem einzigen API-Aufruf empfangen können.

* Erfahren Sie, wie Sie AEM GraphQL-APIs im Tutorial [Erste Schritte mit AEM GraphQL-APIs](./graphql/overview.md) verwenden.

## Token-basiertes Authentifizierungs-Tutorial

AEM stellt eine Vielzahl von HTTP-Endpunkten bereit, mit denen per Headless interagiert werden kann, von GraphQL über Content Services bis hin zur Assets-HTTP-API. Häufig müssen sich diese Headless-Konsumenten bei AEM authentifizieren, um auf geschützte Inhalte oder Aktionen zugreifen zu können. Um dies zu erleichtern, unterstützt AEM die Token-basierte Authentifizierung von HTTP-Anforderungen von externen Anwendungen, Diensten oder Systemen.

* Erfahren Sie, wie Sie sich mithilfe von Zugriffstoken im [Authentifizieren bei AEM als Cloud Service über ein externes Anwendungs-Tutorial](./authentication/overview.md) bei HTTP authentifizieren können.

## AEM Content Services-Tutorial

AEM Content Services nutzt herkömmliche AEM Seiten, um Headless-REST-API-Endpunkte zu erstellen, und AEM Komponenten definieren oder referenzieren den Inhalt, der für diese Endpunkte verfügbar gemacht werden soll.

AEM Content Services ermöglicht die Verwendung der gleichen Inhaltsabstraktionen, die für die Erstellung von Web-Seiten in AEM Sites verwendet werden, um den Inhalt und die Schemas dieser HTTP-APIs zu definieren. Durch die Verwendung von AEM Seiten und AEM-Komponenten können Marketingexperten flexible JSON-APIs, die für jede Anwendung geeignet sind, schnell erstellen und aktualisieren.

* Erfahren Sie, wie Sie AEM Content Services im Tutorial [Erste Schritte mit AEM Content Services verwenden.](./content-services/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | AEM GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM von Komponenten |
| Inhalt | Inhaltsfragmente | AEM von Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM Seite |
| Versandformat | GraphQL JSON | AEM ComponentExporter JSON |

## Weitere hilfreiche Tutorials

Weitere AEM-Tutorials zu Headless-Konzepten umfassen:

* [Erste Schritte mit dem AEM SPA-Editor und Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Erste Schritte mit dem AEM SPA-Editor und React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)