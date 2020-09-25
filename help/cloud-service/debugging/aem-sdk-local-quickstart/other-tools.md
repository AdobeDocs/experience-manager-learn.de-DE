---
title: Weitere Tools zum Debugging AEM SDK
description: Eine Reihe anderer Tools können beim Debugging des lokalen Schnellstarts des AEM SDK helfen.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 10%

---


# Weitere Tools zum Debugging AEM SDK

Eine Reihe anderer Tools können beim Debugging Ihrer Anwendung auf dem lokalen Schnellstart des AEM SDK helfen.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite ist eine webbasierte Schnittstelle zur Interaktion mit dem JCR, AEM Data Repository. CRXDE Lite bietet vollständige Sichtbarkeit in die JCR-Datei, einschließlich Knoten, Eigenschaften, Eigenschaftenwerte und Berechtigungen.

CRXDE Lite befindet sich unter:

+ Tools > Allgemein > CRXDE Lite
+ oder direkt unter [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Abfrage erläutern

![Abfrage erläutern](./assets/other-tools/explain-query.png)

Erläutern Sie das webbasierte Tool für die Abfrage in AEM SDK-Kurzanleitung, das wichtige Einblicke in die Interpretation und Ausführung von Abfragen bietet und ein unschätzbares Tool darstellt, um sicherzustellen, dass Abfragen von AEM ausgeführt werden.

Die Abfrage erklären finden Sie unter:

+ Tools > Diagnose > Abfrage Performance > Registerkarte Abfrage erklären
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Registerkarte &quot;Abfrage&quot;

## QueryBuilder-Debugger

![QueryBuilder-Debugger](./assets/other-tools/query-debugger.png)

Der QueryBuilder-Debugger ist ein webbasiertes Tool, mit dem Sie Abfragen mithilfe AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) -Syntax debuggen und verstehen können.

Der QueryBuilder-Debugger befindet sich unter:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer und AEM Chrome Plug-In

![Sling Log Tracer und AEM Chrome Plug-In](./assets/other-tools/log-tracer.png)

[Der Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), der mit dem lokalen Schnellstart AEM SDK geliefert wird, ermöglicht eine detaillierte Verfolgung von HTTP-Anforderungen und bietet detaillierte Debugging-Informationen pro Anforderung. Zur Aktivierung dieser Funktion muss die OSGi-Konfiguration des [Protokolltraders konfiguriert](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1) werden.

Das Open Source [AEM Chrome-Plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) für den [Google Chrome-Webbrowser](https://www.google.com/chrome/)ist mit Log Tracer integriert und stellt die Debugging-Informationen direkt in den Dev Tools von Chrome zur Verfügung.

_Das AEM Chrome-Plug-in ist ein Open-Source-Tool und wird von Adobe nicht unterstützt._

