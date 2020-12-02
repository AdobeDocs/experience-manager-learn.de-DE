---
title: AEM Übungen ohne Kopf
description: Eine Sammlung von Übungen zur Verwendung von Adobe Experience Manager als Headless-CMS.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# AEM Übungen ohne Kopf

Adobe Experience Manager verfügt über mehrere Optionen zum Definieren von Headless-Endpunkten und zum Bereitstellen des Inhalts als JSON. Verwenden Sie praktische Übungen, um zu untersuchen, wie Sie die verschiedenen Optionen nutzen und wählen, was für Sie am besten ist.

## Lernprogramm AEM GraphQL APIs

>[!CAUTION]
>
> Die AEM GraphQL API für Content Fragment Versand wird Anfang 2021 veröffentlicht.
> Die zugehörige Dokumentation steht zu Vorschauen zur Verfügung.

AEM GraphQL APIs für Inhaltsfragmente
unterstützt kopflose CMS-Szenarien, in denen externe Clientanwendungen Erlebnisse mit in AEM verwalteten Inhalten wiedergeben.

Eine moderne Content Versand API ist der Schlüssel zur Effizienz und Performance von JavaScript-basierten Frontend-Anwendungen. Die Verwendung der REST-API birgt Herausforderungen:

* Große Anzahl von Anforderungen zum Abrufen eines Objekts
* Oft werden Inhalte, die übermäßig bereitgestellt werden, d. h., die Anwendung erhält mehr, als sie benötigt

Um diese Herausforderungen zu meistern, stellt GraphQL eine Abfrage-basierte API bereit, mit der Kunden AEM nur für die benötigten Inhalte erstellen und mit einem einzigen API-Aufruf empfangen können.

* Erfahren Sie, wie Sie AEM GraphQL-APIs verwenden, im Tutorial [Erste Schritte mit AEM GraphQL-APIs](./graphql/overview.md)

## Lernprogramm zu AEM Content Services

AEM Content Services nutzt herkömmliche AEM-Seiten, um kopflose REST-API-Endpunkte zu erstellen, und AEM Komponenten definieren oder verweisen den Inhalt, der an diesen Endpunkten verfügbar gemacht werden soll.

AEM Content Services ermöglicht die Erstellung von Webseiten in AEM Sites mit denselben Inhaltsabstraktionen, um den Inhalt und die Schemas dieser HTTP-APIs zu definieren. Die Verwendung von AEM Seiten und AEM Komponenten ermöglicht es Marketingexperten, flexible JSON-APIs, mit denen jede Anwendung ausgeführt werden kann, schnell zu erstellen und zu aktualisieren.

* Erfahren Sie, wie Sie AEM Content Services verwenden, indem Sie das Lehrgang [Erste Schritte mit AEM Content Services (Lehrgang ](./content-services/overview.md))

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