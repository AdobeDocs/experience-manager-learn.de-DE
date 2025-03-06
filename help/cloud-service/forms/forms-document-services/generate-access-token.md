---
title: Generieren eines Zugriffs-Tokens
description: Beispielcode zum Generieren eines Zugriffstokens zum Aufrufen der Forms Document Services-API
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 7%

---

# Generieren eines Zugriffs-Tokens

Ein Zugriffs-Token ist eine Berechtigung, die zum Authentifizieren und Autorisieren von Anfragen an eine REST-API verwendet wird. Sie wird normalerweise von einem Authentifizierungs-Server (z. B. OAuth 2.0 oder OpenID Connect) ausgestellt, nachdem sich ein Benutzer oder eine Anwendung erfolgreich angemeldet hat. Beim Aufrufen einer gesicherten Forms Document Services-API wird ein Zugriffstoken in den Anfrage-Header eingefügt, um die Identität des Clients zu überprüfen.
Im Folgenden finden Sie eine Beispielanfrage zum Senden eines Zugriffs-Tokens

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

Der folgende Code wurde verwendet, um das Zugriffstoken zu generieren

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

Die AccessTokenService-Klasse ist für das Abrufen eines OAuth-Zugriffstokens aus dem IMS-Authentifizierungsdienst von Adobe verantwortlich. Er liest Client-Anmeldeinformationen aus einer JSON-Datei (server_credentials.json), erstellt eine Authentifizierungsanfrage und sendet sie an den Adobe-Token-Endpunkt. Die Antwort wird dann geparst, um das Zugriffstoken zu extrahieren, das für sichere API-Aufrufe verwendet wird. Die -Klasse umfasst eine ordnungsgemäße Protokollierung und Fehlerbehandlung, um Zuverlässigkeit und einfacheres Debuggen zu gewährleisten.

## Nächste Schritte

[Durchführen von API-Aufrufen](./make-api-calls.md)