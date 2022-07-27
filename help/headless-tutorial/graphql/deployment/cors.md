---
title: CORS-Konfiguration für AEM GraphQL
description: Erfahren Sie, wie Sie die Cross-Origin Resource Sharing (CORS) für die Verwendung mit AEM GraphQL konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 4%

---


# Cross-Origin Resource Sharing (CORS)

Die Cross-Origin Resource Sharing (CORS) von Adobe Experience Manager as a Cloud Service erleichtert nicht AEM Webeigenschaften das Ausführen browserbasierter clientseitiger Aufrufe an AEM GraphQL-APIs.

>[!TIP]
>
> Die folgenden Konfigurationen sind Beispiele. Stellen Sie sicher, dass Sie sie an die Anforderungen Ihres Projekts anpassen.



## CORS-Anforderung

CORS ist für browserbasierte Verbindungen zu AEM GraphQL-APIs erforderlich, wenn der Client, der eine Verbindung zu AEM herstellt, NICHT von derselben Herkunft (auch als Host oder Domäne bezeichnet) wie AEM bereitgestellt wird.

| Client-Typ | Einzelseitenanwendung (SPA) | Webkomponente/JS | Mobilgerät | Server an Server |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Erfordert CORS-Konfiguration | ms | ms | ✘ | ✘ |

## OSGi-Konfiguration

Die AEM CORS OSGi-Konfigurationsfactory definiert die Zulassungskriterien für die Annahme von CORS-HTTP-Anfragen.

| Client stellt Verbindungen her | AEM Author | AEM Publish | AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Erfordert CORS OSGi-Konfiguration | ms | ms | ms |


Im folgenden Beispiel wird eine OSGi-Konfiguration für AEM Publish (`../config.publish/..`), kann aber hinzugefügt werden zu [Beliebiger unterstützter Runmode-Ordner](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Die wichtigsten Konfigurationseigenschaften sind:

+ `alloworigin` und/oder `alloworiginregexp` gibt die Herkunft(en) des Clients an, der eine Verbindung zu AEM Web-Ausführung herstellt.
+ `allowedpaths` gibt die URL-Pfadmuster an, die aus den angegebenen Quellen zulässig sind. Verwenden Sie das folgende Muster, um AEM von GraphQL beibehaltene Abfragen zu unterstützen: `"/graphql/execute.json.*"`
+ `supportedmethods` gibt die zulässigen HTTP-Methoden für die CORS-Anforderungen an. Hinzufügen `GET`, um AEM von GraphQL gespeicherten Abfragen zu unterstützen.

[Erfahren Sie mehr über die CORS OSGi-Konfiguration.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=de)


Diese Beispielkonfiguration unterstützt die Verwendung AEM von GraphQL-gespeicherten Abfragen. Um clientdefinierte GraphQL-Abfragen zu verwenden, fügen Sie eine GraphQL-Endpunkt-URL hinzu in `allowedpaths` und `POST` nach `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## Dispatcherkonfiguration

Der Dispatcher des AEM-Veröffentlichungs- (und Vorschau-)Diensts muss zur Unterstützung von CORS konfiguriert werden.

| Client stellt Verbindungen her | AEM-Autor | AEM-Veröffentlichung | AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Erfordert Dispatcher-CORS-Konfiguration | ✘ | ms | ms |

### Zulassen von CORS-Headern für HTTP-Anforderungen

Aktualisieren Sie die `clientheaders.any` Datei, um die HTTP-Anforderungsheader zuzulassen `Origin`,  `Access-Control-Request-Method` und `Access-Control-Request-Headers` an AEM weitergeleitet werden, sodass die HTTP-Anforderung von der [AEM CORS-Konfiguration](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### Bereitstellen von HTTP-Antwort-Headern für CORS

Konfigurieren der Dispatcher-Farm zum Zwischenspeichern **CORS HTTP-Antwortheader** , um sicherzustellen, dass sie enthalten sind, wenn AEM von GraphQL persistente Abfragen aus dem Dispatcher-Cache bereitgestellt werden, indem die `Access-Control-...` Header zur Cache-Header-Liste.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```
