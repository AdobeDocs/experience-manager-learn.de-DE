---
title: Generieren eines Zugriffs-Tokens
description: Beispiel-Code zum Generieren eines Zugriffs-Tokens zum Aufrufen der Forms-Dokumentendienste-API
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 100cab4b-16bd-4358-b0f0-61dbfd64d412
source-git-commit: 1a76256677d06aaffd142c46dc9167a669ac6455
workflow-type: ht
source-wordcount: '180'
ht-degree: 100%

---

# Generieren eines Zugriffs-Tokens

Ein Zugriffs-Token besteht aus Anmeldedaten und wird zum Authentifizieren und Autorisieren von Anfragen an eine REST-API verwendet. Es wird normalerweise von einem Authentifizierungs-Server (z. B. OAuth 2.0 oder OpenID Connect) ausgegeben, nachdem sich eine Person oder eine Anwendung erfolgreich angemeldet hat. Beim Aufrufen einer gesicherten Forms-Dokumentendienste-API wird ein Zugriffs-Token in die Anfrage-Kopfzeile eingefügt, um die Identität des Clients zu verifizieren.
Im Folgenden finden Sie eine Beispielanfrage zum Senden eines Zugriffs-Tokens.

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

Der folgende Code wurde zum Generieren des Zugriff-Tokens verwendet.

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

Die Klasse „AccessTokenService“ ist für das Abrufen eines OAuth-Zugriffs-Tokens aus dem IMS-Authentifizierungsdienst von Adobe verantwortlich. Sie liest Client-Anmeldeinformationen aus einer JSON-Datei (server_credentials.json), erstellt eine Authentifizierungsanfrage und sendet sie an den Token-Endpunkt von Adobe. Die Antwort wird dann analysiert, um das Zugriffs-Token zu extrahieren, das für sichere API-Aufrufe verwendet wird. Die Klasse umfasst eine ordnungsgemäße Protokollierung und Fehlerverarbeitung, um Zuverlässigkeit und einfacheres Debuggen zu gewährleisten.

## Nächste Schritte

[Tätigen von API-Aufrufen](./make-api-calls.md)
