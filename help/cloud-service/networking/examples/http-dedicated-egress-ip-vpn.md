---
title: HTTP/HTTPS-Verbindungen für dedizierte Ausgangs-IP-Adresse und VPN
description: Erfahren Sie, wie Sie HTTP/HTTPS-Anfragen von AEM as a Cloud Service zu externen Webdiensten ausführen, die für dedizierte Egress-IP-Adresse und VPN ausgeführt werden.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: aa2d0d4d6e0eb429baa37378907a9dd53edd837d
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# HTTP/HTTPS-Verbindungen für dedizierte Ausgangs-IP-Adresse und VPN

HTTP/HTTPS-Verbindungen müssen von AEM as a Cloud Service bereitgestellt werden, sie benötigen jedoch keine speziellen `portForwards` Regeln und kann AEM `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`und `AEM_HTTPS_PROXY_PORT`.

## Erweiterte Netzwerkunterstützung

Das folgende Codebeispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie die [geeignete](../advanced-networking.md#advanced-networking) Vor diesem Tutorial wurde eine erweiterte Netzwerkkonfiguration eingerichtet.

| Kein erweitertes Netzwerk | [Flexibles Port-Egress](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ms | ms |

>[!CAUTION]
>
> Dieses Codebeispiel ist nur für [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) und [VPN](../vpn.md). Ein ähnliches, aber anderes Codebeispiel ist für verfügbar. [HTTP/HTTPS-Verbindungen auf nicht standardmäßigen Ports für flexiblen Port-Ausgang](./http-on-non-standard-ports-flexible-port-egress.md).

## Codebeispiel

Dieses Java™-Codebeispiel ist ein OSGi-Dienst, der as a Cloud Service ausgeführt werden kann, AEM eine HTTP-Verbindung zu einem externen Webserver unter 8080 herstellt. Verbindungen zu HTTPS-Webservern verwenden die `AEM_HTTPS_PROXY_HOST` und `AEM_HTTPS_PROXY_PORT` anstelle von  `AEM_HTTP_PROXY_HOST` und `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> Es wird empfohlen, die [Java™ 11 HTTP-APIs](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) werden verwendet, um HTTP-/HTTPS-Aufrufe von AEM durchzuführen.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
