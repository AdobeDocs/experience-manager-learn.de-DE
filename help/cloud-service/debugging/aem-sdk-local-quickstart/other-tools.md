---
title: Weitere Tools zum Debugging AEM SDK
description: Eine Vielzahl anderer Tools kann beim Debugging des lokalen Schnellstarts des AEM SDK helfen.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 12%

---

# Weitere Tools zum Debugging AEM SDK

Eine Vielzahl anderer Tools kann beim Debugging Ihrer Anwendung auf dem lokalen Schnellstart des AEM SDK helfen.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite ist eine webbasierte Schnittstelle für die Interaktion mit dem JCR, AEM Datenrepository. CRXDE Lite bietet vollständige Sichtbarkeit in das JCR, einschließlich Knoten, Eigenschaften, Eigenschaftswerten und Berechtigungen.

Die CRXDE Lite befindet sich unter:

+ Tools > Allgemein > CRXDE Lite
+ oder direkt unter [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Abfrage erläutern

![Abfrage erläutern](./assets/other-tools/explain-query.png)

Erläuterung des webbasierten Abfragetools im lokalen Schnellstart AEM SDK, das wichtige Einblicke in die Interpretation und Ausführung von Abfragen durch AEM liefert und ein unschätzbares Tool darstellt, um sicherzustellen, dass Abfragen von AEM auf leistungsstarke Weise ausgeführt werden.

&quot;Abfrage erläutern&quot;befindet sich unter:

+ Tools > Diagnose > Abfrageleistung > Registerkarte &quot;Abfrage erläutern&quot;
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Registerkarte &quot;Abfrage erläutern&quot;

## QueryBuilder-Debugger

![QueryBuilder-Debugger](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger ist ein webbasiertes Tool, mit dem Sie Suchanfragen mithilfe der AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)-Syntax debuggen und verstehen können.

QueryBuilder Debugger befindet sich unter:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
