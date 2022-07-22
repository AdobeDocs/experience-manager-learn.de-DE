---
title: HTTP/HTTPS-Verbindungen auf nicht standardmäßigen Ports für flexible Port-Auslösung
description: Erfahren Sie, wie Sie HTTP-/HTTPS-Anfragen von AEM as a Cloud Service zu externen Webdiensten durchführen, die auf nicht standardmäßigen Ports ausgeführt werden, um eine flexible Port-Erweiterung zu ermöglichen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: 8e8991d128ff40f7873dd8666460e761356c2dd9
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# HTTP/HTTPS-Verbindungen auf nicht standardmäßigen Ports für flexible Port-Auslösung

HTTP/HTTPS-Verbindungen an nicht standardmäßigen Ports (nicht 80/443) müssen von AEM as a Cloud Service bereitgestellt werden, sie benötigen jedoch keine speziellen `portForwards` Regeln und kann AEM `AEM_PROXY_HOST` und einen reservierten Proxyanschluss `AEM_HTTP_PROXY_PORT` oder `AEM_HTTPS_PROXY_PORT` abhängig davon, ob das Ziel HTTP/HTTPS ist.

## Erweiterte Netzwerkunterstützung

Das folgende Codebeispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie die [geeignete](../advanced-networking.md#advanced-networking) Vor diesem Tutorial wurde eine erweiterte Netzwerkkonfiguration eingerichtet.

| Kein erweitertes Netzwerk | [Flexibles Port-Egress](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ms | ✘ | ✘ |

>[!CAUTION]
>
> Dieses Codebeispiel ist nur für [Flexibler Hafenausbau](../flexible-port-egress.md). Ein ähnliches, aber anderes Codebeispiel ist für verfügbar. [HTTP/HTTPS-Verbindungen auf nicht standardmäßigen Ports für dedizierte Egress-IP-Adresse und VPN](./http-dedicated-egress-ip-vpn.md).

## Codebeispiel

Dieses Java™-Codebeispiel ist ein OSGi-Dienst, der as a Cloud Service ausgeführt werden kann, AEM eine HTTP-Verbindung zu einem externen Webserver unter 8080 herstellt. Verbindungen zu HTTPS-Webservern verwenden die Umgebungsvariablen `AEM_PROXY_HOST` und `AEM_HTTPS_PROXY_PORT` (Standardeinstellung ist `proxy.tunnel:3128` in AEM Versionen &lt; 6094).

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
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
            // The explicit fallback of 3128 will be obsoleted in Jan 2022, and only the AEM_HTTP_PROXY_PORT/AEM_HTTPS_PROXY_PORT variable will be required
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", "3128"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
