---
title: Zugriffstoken von Server zu Server in App Builder-Aktion generieren
description: Erfahren Sie, wie Sie ein Zugriffstoken mithilfe von OAuth Server-zu-Server-Anmeldeinformationen für die Verwendung in einer App Builder-Aktion generieren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: null
source-git-commit: c77dd9c2872e7e43863d83837cedbff50a7d3c1a
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 8%

---

# Zugriffstoken von Server zu Server in App Builder-Aktion generieren

App Builder-Aktionen müssen ggf. mit Adobe-APIs interagieren, die **OAuth Server-zu-Server-Anmeldedaten** und sind mit Adobe Developer Console-Projekten verknüpft, die die App Builder-App bereitgestellt wird.

In diesem Handbuch wird erläutert, wie Sie mithilfe von _OAuth Server-zu-Server-Anmeldedaten_ zur Verwendung in einer App Builder-Aktion.

>[!IMPORTANT]
>
> Die Anmeldedaten für Dienstkonten (JWT) werden nicht mehr für die OAuth Server-zu-Server-Anmeldedaten unterstützt. Es gibt jedoch noch einige Adobe-APIs, die nur Service Account (JWT)-Anmeldedaten unterstützen und die Migration zu OAuth Server-to-Server ist im Gange. Lesen Sie die Adobe API-Dokumentation , um zu erfahren, welche Anmeldeinformationen unterstützt werden.

## Adobe Developer Console-Projektkonfigurationen

Beim Hinzufügen der gewünschten Adobe-API zum Adobe Developer Console-Projekt im _API konfigurieren_ Schritt, wählen Sie die **OAuth Server-zu-Server** Authentifizierungstyp.

![Adobe Developer Console - OAuth Server-to-Server](./assets/s2s-auth/oauth-server-to-server.png)

Um das oben genannte automatisch erstellte Integrationsdienstkonto zuzuweisen, wählen Sie das gewünschte Produktprofil aus. Somit werden über das Produktprofil die Berechtigungen für Dienstkonten gesteuert.

![Adobe Developer Console - Produktprofil](./assets/s2s-auth/select-product-profile.png)

## .env-Datei

Im App Builder-Projekt `.env` -Datei, hängen Sie benutzerdefinierte Schlüssel für die OAuth Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts an. Die OAuth-Server-zu-Server-Anmeldewerte können vom Adobe Developer Console-Projekt abgerufen werden. __Anmeldeinformationen__ > __OAuth Server-zu-Server__ für einen bestimmten Arbeitsbereich.

![OAuth Server-zu-Server-Anmeldedaten für die Adobe Developer Console](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Die Werte für `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` kann direkt aus dem Bildschirm &quot;OAuth Server-to-Server-Anmeldedaten&quot;des Adobe Developer Console-Projekts kopiert werden.

## Zuordnung der Eingaben

Mit dem OAuth-Server-zu-Server-Berechtigungswert, der im `.env` -Datei, müssen sie den AppBuilder-Aktionseingaben zugeordnet werden, damit sie in der Aktion selbst gelesen werden können. Fügen Sie dazu Einträge für jede Variable in der `ext.config.yaml`-Aktion `inputs` im folgenden Format hinzu: `PARAMS_INPUT_NAME: $ENV_KEY`.

Zum Beispiel:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

Die unter `inputs` definierten Schlüssel sind verfügbar im `params`-Objekt, das für die App-Entwicklungs-Aktion bereitgestellt wird.

## OAuth Server-zu-Server-Anmeldedaten für den Zugriff auf Token

In der App Builder-Aktion sind die OAuth Server-zu-Server-Anmeldedaten im `params` -Objekt. Mithilfe dieser Anmeldeinformationen kann das Zugriffstoken mithilfe von [OAuth 2.0-Bibliotheken](https://oauth.net/code/). Sie können auch die [Knotenabruf-Bibliothek](https://www.npmjs.com/package/node-fetch) , um eine POST-Anfrage an den Adobe IMS-Token-Endpunkt zu senden und das Zugriffstoken abzurufen.

Das folgende Beispiel zeigt die Verwendung der `node-fetch` -Bibliothek, um eine POST-Anfrage an den Adobe IMS-Token-Endpunkt zu senden, um das Zugriffstoken abzurufen.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = responseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const res = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
