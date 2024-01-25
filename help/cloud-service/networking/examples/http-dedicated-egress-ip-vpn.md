---
title: HTTP/HTTPS-Verbindungen für dedizierte Ausgangs-IP-Adressen und VPN
description: Erfahren Sie, wie Sie HTTP/HTTPS-Anfragen von AEM as a Cloud Service zu externen Web-Diensten für dedizierte Ausgangs-IP-Adressen und VPN ausführen
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 92
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 100%

---

# HTTP/HTTPS-Verbindungen für dedizierte Ausgangs-IP-Adressen und VPN

HTTP/HTTPS-Verbindungen werden automatisch aus AEM as a Cloud Service mit dedizierter Ausgangs-IP-Adresse oder VPN bereitgestellt und benötigen keine speziellen `portForwards`-Regeln.

## Erweiterte Netzwerkunterstützung

Das folgende Code-Beispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Vergewissern Sie sich, dass die erweiterte Netzwerkkonfiguration ([ dedizierte Ausgangs-IP-Adresse oder VPN](../advanced-networking.md#advanced-networking)) eingerichtet wurde, bevor Sie dieses Tutorial durchführen.

| Keine erweiterten Netzwerkfunktionen | [Flexibler Port-Ausgang](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Dieses Code-Beispiel gilt nur für [dedizierte Ausgangs-IP-Adressen](../dedicated-egress-ip-address.md) und [VPN](../vpn.md). Ein ähnliches, aber anderes Code-Beispiel ist für [HTTP/HTTPS-Verbindungen auf nicht standardmäßigen Ports für flexible Port-Ausgänge](./http-on-non-standard-ports-flexible-port-egress.md) verfügbar.

## Code-Beispiel

Dieses Java™-Code-Beispiel zeigt einen OSGi-Dienst, der in AEM as a Cloud Service ausgeführt werden kann und eine HTTP-Verbindung zu einem externen Web-Server auf 8080 herstellt. Die HTTPS- (oder HTTP-) Verbindungen werden von AEM as a Cloud Service automatisch bereitgestellt und erfordern keine spezielle Entwicklung.

>[!NOTE]
> Es wird empfohlen, die [Java™ 11-HTTP-APIs](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) zu verwenden, um HTTP/HTTPS-Aufrufe von AEM aus durchzuführen.

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
