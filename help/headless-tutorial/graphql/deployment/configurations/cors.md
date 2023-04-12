---
title: CORS-Konfiguration für AEM GraphQL
description: Erfahren Sie, wie Sie Cross-Origin Resource Sharing (CORS) für die Verwendung mit AEM GraphQL konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: 6f1000db880c3126a01fa0b74abdb39ffc38a227
workflow-type: ht
source-wordcount: '572'
ht-degree: 100%

---


# Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) von Adobe Experience Manager as a Cloud Service ermöglicht AEM-fremden Web-Eigenschaften, Browser-basierte Client-seitige Aufrufe an AEM GraphQL-APIs durchzuführen.

>[!TIP]
>
> Im Folgenden finden Sie Beispiele für Konfigurationen. Passen Sie diese an die Anforderungen Ihres Projekts an.

## CORS-Anforderung

CORS ist für Browser-basierte Verbindungen zu AEM GraphQL-APIs erforderlich, wenn der Client, der eine Verbindung zu AEM herstellt, NICHT vom selben Ursprung (auch als Host oder Domain bezeichnet) wie AEM beliefert wird.

| Client-Typ | [Single Page App (SPA)](../spa.md) | [Web-Komponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS-Konfiguration erforderlich | ✔ | ✔ | ✘ | ✘ |

## OSGi-Konfiguration

Die AEM-CORS-OSGi-Konfigurations-Factory definiert die Zulassungskriterien für die Annahme von CORS-HTTP-Anfragen.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS-OSGi-Konfiguration erforderlich | ✔ | ✔ | ✔ |


Im folgenden Beispiel wird eine OSGi-Konfiguration für AEM Publish (`../config.publish/..`) definiert, kann aber zu [jedem beliebigen unterstützten Ausführungsmodus-Ordner](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#runmode-resolution) hinzugefügt werden.

Die wichtigsten Konfigurationseigenschaften lauten wie folgt:

+ `alloworigin` und/oder `alloworiginregexp` spezifizieren die Ursprünge, über die der Client, der eine AEM-Web-Verbindung herstellt, ausgeführt wird.
+ `allowedpaths` spezifiziert die URL-Pfadmuster, die in den spezifizierten Ursprüngen zulässig sind.
   + Um AEM GraphQL-persistierte Abfragen zu unterstützen, fügen Sie das folgende Muster hinzu: `/graphql/execute.json.*`
   + Um Experience Fragments zu unterstützen, fügen Sie das folgende Muster hinzu: `/content/experience-fragments/.*`
+ `supportedmethods` spezifiziert die zulässigen HTTP-Methoden für die CORS-Anfragen. Fügen Sie `GET` hinzu, um AEM GraphQL-persistierte Abfragen (und Experience Fragments) zu unterstützen.

[Hier finden Sie weitere Informationen zur CORS-OSGi-Konfiguration.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=de)

Diese Beispielkonfiguration unterstützt die Verwendung AEM GraphQL-persistierter Abfragen. Um Client-definierte GraphQL-Abfragen zu verwenden, fügen Sie eine GraphQL-Endpunkt-URL in `allowedpaths` und `POST` zu `supportedmethods` hinzu.

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
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
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
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Autorisierte AEM GraphQL-API-Anfragen

Stellen Sie beim Zugriff auf AEM GraphQL-APIs, die eine Autorisierung erfordern (normalerweise bei AEM Author oder bei geschützten Inhalten auf AEM Publish), sicher, dass die CORS-OSGi-Konfiguration über die folgenden zusätzlichen Werte verfügt:

+ `supportedheaders` führt auch `"Authorization"` auf.
+ `supportscredentials` ist auf `true` eingestellt.

Autorisierte Anfragen an AEM GraphQL-APIs, für die eine CORS-Konfiguration erforderlich ist, sind ungewöhnlich, da sie normalerweise im Kontext von [Server-zu-Server-Apps](../server-to-server.md) auftreten und daher keine CORS-Konfiguration voraussetzen. Browser-basierte Apps, für die CORS-Konfigurationen erforderlich sind, z. B. [Single Page Apps](../spa.md) oder [Web-Komponenten](../web-component.md), nutzen in der Regel eine Autorisierung, da es schwierig ist, die Anmeldeinformationen zu sichern.

Diese beiden Einstellungen werden in einer `CORSPolicyImpl`-OSGi-Factory-Konfiguration beispielsweise wie folgt festgelegt:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Beispiel einer OSGi-Konfiguration

+ [Ein Beispiel für die OSGi-Konfiguration finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher-Konfiguration

Der Dispatcher des AEM-Veröffentlichungs-Service (und der Vorschau) muss zur Unterstützung von CORS konfiguriert werden.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher-CORS-Konfiguration erforderlich | ✘ | ✔ | ✔ |

### Zulassen von CORS-Headern bei HTTP-Anfragen

Aktualisieren Sie die Datei `clientheaders.any`, damit die HTTP-Anfrage-Header `Origin`, `Access-Control-Request-Method` und `Access-Control-Request-Headers` an AEM übergeben werden können, sodass die HTTP-Anfrage von der [AEM-CORS-Konfiguration](#osgi-configuration) verarbeitet werden kann.

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Beispiel einer Dispatcher-Konfiguration

+ [Ein Beispiel für die Dispatcher-_Client-Header_-Konfiguration finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Bereitstellen von CORS-HTTP-Antwort-Headern

Konfigurieren Sie die Dispatcher-Farm so, dass **CORS-HTTP-Antwort-Header** zwischengespeichert werden, indem Sie die `Access-Control-...`-Header zur Cache-Header-Liste hinzufügen, um sicherzustellen, dass diese eingeschlossen sind, wenn AEM GraphQL-persistierte Abfragen aus dem Dispatcher-Cache bereitgestellt werden.

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

#### Beispiel einer Dispatcher-Konfiguration

+ [Ein Beispiel für die Konfiguration der Dispatcher-_CORS-HTTP-Antwort-Header_ finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
