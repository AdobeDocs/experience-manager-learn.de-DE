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
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: 181023c9584bcd5084778ebf00d34f8ecaa74524
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 42%

---

# Cross-Origin Resource Sharing (CORS)

Die Cross-Origin Resource Sharing (CORS) von Adobe Experience Manager as a Cloud Service erleichtert nicht AEM Webeigenschaften die Durchführung browserbasierter clientseitiger Aufrufe an GraphQL-APIs und andere AEM Headless-Ressourcen.

>[!TIP]
>
> Im Folgenden finden Sie Beispiele für Konfigurationen. Passen Sie diese an die Anforderungen Ihres Projekts an.

## CORS-Anforderung

CORS ist für Browser-basierte Verbindungen zu AEM GraphQL-APIs erforderlich, wenn der Client, der eine Verbindung zu AEM herstellt, NICHT vom selben Ursprung (auch als Host oder Domain bezeichnet) wie AEM beliefert wird.

| Client-Typ | [Single Page App (SPA)](../spa.md) | [Web-Komponente/JS](../web-component.md) | [Mobilgerät](../mobile.md) | [Server-zu-Server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| CORS-Konfiguration erforderlich | ✔ | ✔ | ✘ | ✘ |

## AEM Author

Die Aktivierung von CORS im AEM-Autorendienst unterscheidet sich von den AEM-Veröffentlichungs- und AEM-Vorschaudiensten. Für den AEM-Autorendienst muss eine OSGi-Konfiguration zum Ausführungsmodusordner des AEM-Autorendienstes hinzugefügt werden. Er verwendet keine Dispatcher-Konfiguration.

### OSGi-Konfiguration

Die AEM-CORS-OSGi-Konfigurations-Factory definiert die Zulassungskriterien für die Annahme von CORS-HTTP-Anfragen.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| CORS-OSGi-Konfiguration erforderlich | ✔ | ✘ | ✘ |


Im folgenden Beispiel wird eine OSGi-Konfiguration für die AEM-Autoreninstanz (`../config.author/..`), sodass sie nur im AEM-Autorendienst aktiv ist.

Die wichtigsten Konfigurationseigenschaften lauten wie folgt:

+ `alloworigin` und/oder `alloworiginregexp` spezifizieren die Ursprünge, über die der Client, der eine AEM-Web-Verbindung herstellt, ausgeführt wird.
+ `allowedpaths` spezifiziert die URL-Pfadmuster, die in den spezifizierten Ursprüngen zulässig sind.
   + Um AEM GraphQL-persistierte Abfragen zu unterstützen, fügen Sie das folgende Muster hinzu: `/graphql/execute.json.*`
   + Um Experience Fragments zu unterstützen, fügen Sie das folgende Muster hinzu: `/content/experience-fragments/.*`
+ `supportedmethods` spezifiziert die zulässigen HTTP-Methoden für die CORS-Anfragen. Um AEM von GraphQL gespeicherten Abfragen (und Experience Fragments) zu unterstützen, fügen Sie `GET` .
+ `supportedheaders` include `"Authorization"` , da Anfragen an die AEM-Autoreninstanz autorisiert werden sollten.
+ `supportscredentials` auf `true` als Anfrage an die AEM-Autoreninstanz autorisiert werden.

[Hier finden Sie weitere Informationen zur CORS-OSGi-Konfiguration.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=de)

Im folgenden Beispiel wird die Verwendung AEM von GraphQL gespeicherten Abfragen in der AEM-Autoreninstanz unterstützt. Um Client-definierte GraphQL-Abfragen zu verwenden, fügen Sie eine GraphQL-Endpunkt-URL in `allowedpaths` und `POST` zu `supportedmethods` hinzu.

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

Die Aktivierung von CORS für AEM-Veröffentlichungs- (und Vorschau-)Dienste unterscheidet sich vom AEM-Autorendienst. Für den AEM-Veröffentlichungsdienst muss der Dispatcher-Konfiguration der AEM-Veröffentlichungsinstanz eine AEM hinzugefügt werden. AEM Publish verwendet keine [OSGi-Konfiguration](#osgi-configuration).

Stellen Sie beim Konfigurieren von CORS für AEM Publish Folgendes sicher:

+ Die `Origin` HTTP-Anforderungsheader kann nicht an den AEM-Veröffentlichungsdienst gesendet werden, indem der `Origin` -Kopfzeile (falls zuvor hinzugefügt) aus dem AEM Dispatcher-Projekt `clientheaders.any` -Datei. Alle `Access-Control-` -Kopfzeilen sollten aus der `clientheaders.any` -Datei und Dispatcher verwaltet sie, nicht aber den AEM-Veröffentlichungsdienst.
+ Wenn eine [CORS OSGi-Konfigurationen](#osgi-configuration) die für Ihren AEM-Veröffentlichungsdienst aktiviert ist, müssen Sie sie entfernen und ihre Konfigurationen in die [Dispatcher-Vhost-Konfiguration](#set-cors-headers-in-vhost) unten beschrieben.

### Dispatcher-Konfiguration

Der Dispatcher des AEM-Veröffentlichungs-Service (und der Vorschau) muss zur Unterstützung von CORS konfiguriert werden.

| Client-Verbindung zu | AEM Author | AEM Publish | AEM-Vorschau |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher-CORS-Konfiguration erforderlich | ✘ | ✔ | ✔ |

#### Umgebungsvariable Dispatcher festlegen

1. Öffnen Sie die Datei global.vars für AEM Dispatcher-Konfiguration, normalerweise unter `dispatcher/src/conf.d/variables/global.vars`.
2. Fügen Sie der Datei Folgendes hinzu:

   ```
   # Enable CORS handling in the dispatcher
   #
   # By default, CORS is handled by the AEM publish server.
   # If you uncomment and define the ENABLE_CORS variable, then CORS will be handled in the dispatcher.
   # See the default.vhost file for a suggested dispatcher configuration. Note that:
   #   a. You will need to adapt the regex from default.vhost to match your CORS domains
   #   b. Remove the "Origin" header (if it exists) from the clientheaders.any file
   #   c. If you have any CORS domains configured in your AEM publish server origin, you have to move those to the dispatcher
   #       (i.e. accordingly update regex in default.vhost to match those domains)
   #
   Define ENABLE_CORS
   ```

#### Festlegen von CORS-Kopfzeilen in vhost

1. Öffnen Sie die Vhost-Konfigurationsdatei für den AEM Publish-Dienst in Ihrem Dispatcher-Konfigurationsprojekt, normalerweise unter `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Kopieren Sie den Inhalt der `<IfDefine ENABLE_CORS>...</IfDefine>` unten in Ihre aktivierte vhost-Konfigurationsdatei ein.

   ```{line-numbers="true"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
       ...
       <IfDefine ENABLE_CORS>
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
   
         # Remove Origin before sending to AEM Publish
         RequestHeader unset Origin
   
         ################## End of CORS configuration ##################
       </IfDefine>
       ...
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Passen Sie den gewünschten Ursprung an, indem Sie auf Ihren AEM-Veröffentlichungsdienst zugreifen, indem Sie den regulären Ausdruck in der folgenden Zeile aktualisieren. Wenn mehrere Ursprünge erforderlich sind, duplizieren Sie diese Zeile und aktualisieren Sie sie für jedes Ursprungs-/Ursprungsmuster.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + So aktivieren Sie beispielsweise den CORS-Zugriff von Herkunft aus:

      + Jede Subdomäne auf `https://example.com`
      + Ein beliebiger Anschluss `http://localhost`

     Ersetzen Sie die Zeile durch die beiden folgenden Zeilen:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Beispiel einer Dispatcher-Konfiguration

+ [Ein Beispiel für die Dispatcher-Konfiguration finden Sie im WKND-Projekt.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
