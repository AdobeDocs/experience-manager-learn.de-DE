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
workflow-type: ht
source-wordcount: '211'
ht-degree: 100%

---


# Dispatcher-Filter

Adobe Experience Manager as a Cloud Service verwendet AEM Publish Dispatcher-Filter, um sicherzustellen, dass AEM nur von Anfragen erreicht wird, die es auch erreichen sollen. Standardmäßig werden alle Anfragen abgelehnt, und Muster für erlaubte URLs müssen explizit hinzugefügt werden.

| Client-Typ | [Single Page App (SPA)](../spa.md) | [Web-Komponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Erfordert die Konfiguration von Dispatcher-Filtern | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Im Folgenden finden Sie Beispiele für Konfigurationen. Passen Sie diese an die Anforderungen Ihres Projekts an.

## Konfiguration von Dispatcher-Filtern

Die Konfiguration von AEM Publish Dispatcher-Filtern definiert die URL-Muster, die AEM erreichen dürfen. Es muss das URL-Präfix für den AEM-Endpunkt für persistierte Abfragen enthalten sein.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Erfordert die Konfiguration von Dispatcher-Filtern | ✘ | ✔ | ✔ |

Fügen Sie eine `allow`-Regel mit dem URL-Muster `/graphql/execute.json/*` hinzu und stellen Sie sicher, dass die Datei-ID (z. B. `/0600` in der Beispiels-Farmdatei) eindeutig ist.
Dies ermöglicht eine HTTP-GET-Anfrage an den Endpunkt der persistierten Abfrage, z. B. `HTTP GET /graphql/execute.json/wknd-shared/adventures-all`, bis zu AEM Publish.

Wenn Sie in Ihrem AEM Headless-Erlebnis Experience Fragments verwenden, tun Sie dasselbe für diese Pfade.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Beispiel für die Konfiguration von Filtern

+ [Ein Beispiel für den Dispatcher-Filter finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
