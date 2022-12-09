---
title: Dispatcher-Filter für AEM GraphQL
description: Erfahren Sie, wie Sie AEM Publish Dispatcher-Filter für die Verwendung mit AEM GraphQL konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: 442020d854d8f42c5d8a1340afd907548875866e
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---


# Dispatcher-Filter

Adobe Experience Manager as a Cloud Service verwendet AEM Publish Dispatcher-Filter, um sicherzustellen, dass nur Anforderungen, die AEM erreichen sollten, AEM erreichen. Standardmäßig werden alle Anfragen verweigert und Muster für zulässige URLs müssen explizit hinzugefügt werden.

| Client-Typ | [Einzelseitenanwendung (SPA)](../spa.md) | [Webkomponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Erfordert die Konfiguration von Dispatcher-Filtern | ms | ms | ms | ms |

>[!TIP]
>
> Die folgenden Konfigurationen sind Beispiele. Stellen Sie sicher, dass Sie sie an die Anforderungen Ihres Projekts anpassen.

## Dispatcher-Filterkonfiguration

Die AEM Publish Dispatcher-Filterkonfiguration definiert die URL-Muster, die erreicht AEM werden dürfen, und muss das URL-Präfix für den AEM persistenten Abfrageendpunkt enthalten.

| Client stellt Verbindungen her | AEM Author | AEM Publish | AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Erfordert die Konfiguration von Dispatcher-Filtern | ✘ | ms | ms |

Hinzufügen einer `allow` Regel mit dem URL-Muster `/graphql/execute.json/*`und stellen Sie die Datei-ID (z. B. `/0600`, ist in der Beispiel-Farm-Datei eindeutig).
Dadurch kann die HTTP-GET an den persistenten Abfrageendpunkt gesendet werden, z. B. `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` bis zur AEM-Veröffentlichung.

Wenn Sie Experience Fragments in Ihrem AEM Headless-Erlebnis verwenden, führen Sie für diese Pfade dasselbe aus.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Konfiguration von Beispielfiltern

+ [Ein Beispiel für den Dispatcher-Filter finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
