---
title: HTTP/HTTPS-Verbindungen auf Nicht-Standard-Ports für einen flexiblen Port-Ausgang
description: Erfahren Sie, wie Sie HTTP/HTTPS-Anfragen von AEM as a Cloud Service an externe Web-Dienste stellen, die auf Nicht-Standard-Ports ausgeführt werden, um einen flexiblen Port-Ausgang zu ermöglichen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '239'
ht-degree: 100%

---

# HTTP/HTTPS-Verbindungen auf Nicht-Standard-Ports für einen flexiblen Port-Ausgang

HTTP/HTTPS-Verbindungen auf Nicht-Standard-Ports (nicht 80/443) müssen aus AEM as a Cloud Service weitergeleitet werden. Sie benötigen jedoch keine speziellen `portForwards`-Regeln und können den erweiterten Netzwerk-`AEM_PROXY_HOST` von AEM sowie einen reservierten Proxy-Port `AEM_HTTP_PROXY_PORT` oder `AEM_HTTPS_PROXY_PORT` verwenden, abhängig davon, ob das Ziel HTTP/HTTPS ist.

## Erweiterte Netzwerkunterstützung

Das folgende Code-Beispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie sicher, dass die [geeignete](../advanced-networking.md#advanced-networking) erweiterte Netzwerkkonfiguration eingerichtet wurde, bevor Sie dieses Tutorial durchführen. 

| Keine erweiterten Netzwerkfunktionen | [Flexibler Port-Ausgang](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Dieses Code-Beispiel gilt nur für einen [flexiblen Port-Ausgang](../flexible-port-egress.md). Ein ähnliches, aber anderes Code-Beispiel ist für [HTTP/HTTPS-Verbindungen auf Nicht-Standard-Ports für eine dedizierte Ausgangs-IP-Adresse und VPN](./http-dedicated-egress-ip-vpn.md) verfügbar.

## Code-Beispiel

Dieses Java™-Code-Beispiel zeigt einen OSGi-Dienst, der in AEM as a Cloud Service ausgeführt werden kann und eine HTTP-Verbindung zu einem externen Web-Server auf 8080 herstellt. Verbindungen zu HTTPS-Webservern verwenden die Umgebungsvariablen `AEM_PROXY_HOST` und `AEM_HTTPS_PROXY_PORT` (Standardeinstellung `proxy.tunnel:3128` in AEM-Versionen vor 6094).

>[!NOTE]
> Es wird empfohlen, die [Java™ 11 HTTP-APIs](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) zu verwenden, um HTTP/HTTPS-Aufrufe von AEM durchzuführen.

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
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

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
