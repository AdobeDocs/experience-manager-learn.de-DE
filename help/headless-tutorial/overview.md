---
title: AEM Übungen ohne Kopf
description: Eine Sammlung von Übungen zur Verwendung von Adobe Experience Manager als Headless-CMS.
feature: Inhaltsfragmente, APIs
topic: Kopflos, Content-Management
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# AEM Übungen ohne Kopf

Adobe Experience Manager verfügt über mehrere Optionen zum Definieren von Headless-Endpunkten und zum Bereitstellen des Inhalts als JSON. Verwenden Sie praktische Übungen, um zu untersuchen, wie Sie die verschiedenen Optionen nutzen und wählen, was für Sie am besten ist.

## Lernprogramm AEM GraphQL APIs

AEM GraphQL APIs für Inhaltsfragmente
unterstützt kopflose CMS-Szenarien, in denen externe Clientanwendungen Erlebnisse mit in AEM verwalteten Inhalten wiedergeben.

Eine moderne Content Versand API ist der Schlüssel zur Effizienz und Performance von JavaScript-basierten Frontend-Anwendungen. Die Verwendung der REST-API birgt Herausforderungen:

* Große Anzahl von Anforderungen zum Abrufen eines Objekts
* Oft werden Inhalte, die übermäßig bereitgestellt werden, d. h., die Anwendung erhält mehr, als sie benötigt

Um diese Herausforderungen zu meistern, stellt GraphQL eine Abfrage-basierte API bereit, mit der Kunden AEM nur für die benötigten Inhalte erstellen und mit einem einzigen API-Aufruf empfangen können.

* Erfahren Sie, wie Sie AEM GraphQL APIs im Lernprogramm [Erste Schritte mit AEM GraphQL APIs verwenden.](./graphql/overview.md)

## Token-basiertes Authentifizierungstraining

AEM stellt eine Vielzahl von HTTP-Endpunkten bereit, mit denen auf kostenlose Weise interagiert werden kann, von GraphQL über Content Services bis hin AEM Asset HTTP API. Häufig müssen sich diese kostenlosen Verbraucher bei AEM authentifizieren, um auf geschützte Inhalte oder Aktionen zugreifen zu können. Um dies zu erleichtern, unterstützt AEM die Token-basierte Authentifizierung von HTTP-Anforderungen von externen Anwendungen, Diensten oder Systemen.

* Erfahren Sie, wie Sie sich mithilfe der Zugriffstoken im [Authentifizierung bei AEM als Cloud Service über HTTP authentifizieren, indem Sie sich über ein externes Anwendungstutorial](./authentication/overview.md) authentifizieren.

## Lernprogramm zu AEM Content Services

AEM Content Services nutzt herkömmliche AEM-Seiten, um kopflose REST-API-Endpunkte zu erstellen, und AEM Komponenten definieren oder verweisen den Inhalt, der an diesen Endpunkten verfügbar gemacht werden soll.

AEM Content Services ermöglicht die Erstellung von Webseiten in AEM Sites mit denselben Inhaltsabstraktionen, um den Inhalt und die Schemas dieser HTTP-APIs zu definieren. Die Verwendung von AEM Seiten und AEM Komponenten ermöglicht es Marketingexperten, flexible JSON-APIs, mit denen jede Anwendung ausgeführt werden kann, schnell zu erstellen und zu aktualisieren.

* Erfahren Sie, wie Sie AEM Content Services im Lernprogramm [Erste Schritte mit AEM Content Services verwenden können.](./content-services/overview.md)

## AEM GraphQL im Vergleich zu AEM Content Services

|  | AEM GraphQL APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-Definition | Fragmentmodelle für strukturierte Inhalte | AEM |
| Inhalt | Inhaltsfragmente | AEM |
| Inhaltssuche | Von GraphQL-Abfrage | Nach AEM |
| Versand-Format | GraphQL JSON | AEM ComponentExporter JSON |

## Weitere hilfreiche Übungen

Weitere AEM zu kostenlosen Konzepten:

* [Erste Schritte mit dem AEM SPA-Editor und Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Erste Schritte mit dem AEM SPA-Editor und React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)