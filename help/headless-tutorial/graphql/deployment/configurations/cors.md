---
title: CORS-Konfiguration für AEM GraphQL
description: Erfahren Sie, wie Sie Cross-Origin Resource Sharing (CORS) für die Verwendung mit AEM GraphQL konfigurieren.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
duration: 240
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 99%

---

# Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) von Adobe Experience Manager as a Cloud Service ermöglicht AEM-fremden Web-Eigenschaften, Browser-basierte Client-seitige Aufrufe an AEM GraphQL-APIs und andere AEM Headless-Ressourcen durchzuführen.

>[!TIP]
>
> Im Folgenden finden Sie Beispiele für Konfigurationen. Passen Sie diese an die Anforderungen Ihres Projekts an.

## CORS-Anforderung

CORS ist für Browser-basierte Verbindungen zu AEM GraphQL-APIs erforderlich, wenn der Client, der eine Verbindung zu AEM herstellt, NICHT vom selben Ursprung (auch als Host oder Domain bezeichnet) wie AEM beliefert wird.

| Client-Typ | [Single Page App (SPA)](../spa.md) | [Web-Komponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS-Konfiguration erforderlich | ✔ | ✔ | ✘ | ✘ |

## AEM Author

Die Aktivierung von CORS im AEM-Author-Service unterscheidet sich von AEM Publish- und AEM-Vorschaudiensten. AEM-Author-Service-Autorendienst erfordert, dass eine OSGi-Konfiguration zum Ausführungsmodusordner des AEM-Author-Services hinzugefügt wird, und verwendet keine Dispatcher-Konfiguration.

### OSGi-Konfiguration

Die AEM-CORS-OSGi-Konfigurations-Factory definiert die Zulassungskriterien für die Annahme von CORS-HTTP-Anfragen.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS-OSGi-Konfiguration erforderlich | ✔ | ✘ | ✘ |


Im folgenden Beispiel wird eine OSGi-Konfiguration für AEM Author (`../config.author/..`) so festgelegt, sodass sie nur im AEM-Author-Service aktiv ist.

Die wichtigsten Konfigurationseigenschaften lauten wie folgt:

+ `alloworigin` und/oder `alloworiginregexp` spezifizieren die Ursprünge, über die der Client, der eine AEM-Web-Verbindung herstellt, ausgeführt wird.
+ `allowedpaths` spezifiziert die URL-Pfadmuster, die in den spezifizierten Ursprüngen zulässig sind.
   + Um AEM GraphQL-persistierte Abfragen zu unterstützen, fügen Sie das folgende Muster hinzu: `/graphql/execute.json.*`
   + Um Experience Fragments zu unterstützen, fügen Sie das folgende Muster hinzu: `/content/experience-fragments/.*`
+ `supportedmethods` spezifiziert die zulässigen HTTP-Methoden für die CORS-Anfragen. Fügen Sie `GET` hinzu, um persistierte AEM GraphQL-Anfragen (und Erlebnisfragmente) zu unterstützen.
+ `supportedheaders` enthält `"Authorization"`, da Anfragen an AEM Author genehmigt werden sollten.
+ `supportscredentials` ist auf `true` gesetzt, da Anfrage an AEM Author autorisiert werden sollten.

[Hier finden Sie weitere Informationen zur CORS-OSGi-Konfiguration.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=de)

Im folgenden Beispiel wird die Verwendung persistierter AEM GraphQL-Anfragen in AEM Author unterstützt. Um Client-definierte GraphQL-Abfragen zu verwenden, fügen Sie eine GraphQL-Endpunkt-URL in `allowedpaths` und `POST` zu `supportedmethods` hinzu.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
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
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### Beispiel einer OSGi-Konfiguration

+ [Ein Beispiel für die OSGi-Konfiguration finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM Publish

Die Aktivierung von CORS für AEM Publish- (und Vorschau-)Dienste unterscheidet sich vom AEM-Author-Service. Der AEM-Publish-Service erfordert, dass zur Dispatcher-Konfiguration von AEM Publish eine AEM Dispatcher-Konfiguration hinzugefügt wird. AEM Publish verwendet keine [OSGi-Konfiguration](#osgi-configuration).

Stellen Sie beim Konfigurieren von CORS für AEM Publish Folgendes sicher:

+ Der `Origin`-HTTP-Anfrage-Header kann nicht an den AEM-Publish-Service gesendet werden, indem der `Origin`-Header (falls zuvor hinzugefügt) aus der Datei `clientheaders.any` des AEM Dispatcher-Projekts gelöscht wird. Alle `Access-Control-`-Header sollten aus der Datei `clientheaders.any` entfernt werden. Sie werden vom Dispatcher verwaltet und nicht vom AEM-Publish-Service.
+ Wenn [CORS OSGi-Konfigurationen](#osgi-configuration) in Ihrem AEM-Publish-Service aktiviert sind, müssen Sie sie entfernen und ihre Konfigurationen in die [Dispatcher-Vhost-Konfiguration](#set-cors-headers-in-vhost) migrieren, wie unten beschrieben.

### Dispatcher-Konfiguration

Der Dispatcher des AEM-Veröffentlichungs-Service (und der Vorschau) muss zur Unterstützung von CORS konfiguriert werden.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher-CORS-Konfiguration erforderlich | ✘ | ✔ | ✔ |

#### Festlegen von CORS-Headern in vhost

1. Öffnen Sie die vhost-Konfigurationsdatei für den AEM-Publish-Service in Ihrem Dispatcher-Konfigurationsprojekt, normalerweise unter `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Kopieren Sie den Inhalt des Blocks `<IfDefine ENABLE_CORS>...</IfDefine>` unten in Ihre aktivierte vhost-Konfigurationsdatei.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch '%{REQUEST_SCHEME}://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Passen Sie den gewünschten Ursprung an, indem Sie auf Ihren AEM-Publish-Service zugreifen und den regulären Ausdruck in der folgenden Zeile aktualisieren. Wenn mehrere Ursprünge erforderlich sind, duplizieren Sie diese Zeile und aktualisieren Sie sie für jeden Ursprung/jedes Ursprungsmuster.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + So aktivieren Sie beispielsweise den CORS-Zugriff über den Ursprung:

      + Jede Subdomain auf `https://example.com`
      + Ein beliebiger Port unter `http://localhost`

     Ersetzen Sie die Zeile durch die beiden folgenden Zeilen:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Beispiel einer Dispatcher-Konfiguration

+ [Ein Beispiel für die Dispatcher-Konfiguration finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
