---
title: Generieren von Server-zu-Server-Zugriffs-Token in einer App-Entwicklungs-Aktion
description: Erfahren Sie, wie Sie ein Zugriffs-Token mit OAuth-Server-zu-Server-Anmeldedaten generieren, um sie in einer App-Entwicklungs-Aktion zu verwenden.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 100%

---

# Generieren von Server-zu-Server-Zugriffs-Token in einer App-Entwicklungs-Aktion

App-Entwicklungs-Aktionen müssen möglicherweise mit Adobe-APIs interagieren, die **OAuth-Server-zu-Server-Anmeldedaten** unterstützen und mit Adobe Developer Console-Projekten verknüpft sind, in denen die App-Entwicklungs-App bereitgestellt wird.

In diesem Handbuch wird erklärt, wie Sie ein Zugriffs-Token mit _OAuth-Server-zu-Server-Anmeldedaten_ generieren, um sie in einer App-Entwicklungs-Aktion zu verwenden.

>[!IMPORTANT]
>
> Die Anmeldedaten für Dienstkonten (JWT) wurden zugunsten der OAuth-Server-zu-Server-Anmeldeinformationen eingestellt. Es gibt jedoch noch einige Adobe-APIs, die nur Anmeldedaten für Dienstkonten (JWT) unterstützen und wo die Migration zu OAuth-Server-zu-Server im Gange ist. Lesen Sie die Adobe API-Dokumentation, um zu erfahren, welche Anmeldedaten unterstützt werden.

## Konfigurationen des Adobe Developer Console-Projekts

Wählen Sie beim Hinzufügen der gewünschten Adobe-API zum Adobe Developer Console-Projekt im Schritt _API konfigurieren_ den Authentifizierungstyp **OAuth-Server-zu-Server** aus.

![Adobe Developer Console – OAuth-Server-to-Server](./assets/s2s-auth/oauth-server-to-server.png)

Wählen Sie das gewünschte Produktprofil aus, um das oben genannte automatisch erstellte Integrationsdienstkonto zuzuweisen.  Hierdurch werden die Berechtigungen für Dienstkonten über das Produktprofil gesteuert.

![Adobe Developer Console – Produktprofil](./assets/s2s-auth/select-product-profile.png)

## .env-Datei

Hängen Sie in der `.env`-Datei des App-Entwicklungs-Projekts benutzerdefinierte Schlüssel für alle OAuth-Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts an. Die Werte der OAuth-Server-zu-Server-Anmeldedaten können vom Adobe Developer Console-Projekt über __Anmeldedaten__ > __OAuth-Server-zu-Server__ für einen bestimmten Arbeitsbereich abgerufen werden.

![OAuth-Server-zu-Server-Anmeldedaten von der Adobe Developer Console](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Die Werte für `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` können direkt aus dem Bildschirm „OAuth-Server-zu-Server-Anmeldedaten“ des Adobe Developer Console-Projekts kopiert werden.

## Zuordnung der Eingaben

Mit dem in der `.env`-Datei festgelegten Wert der OAuth-Server-zu-Server-Anmeldedaten müssen sie den App-Entwicklungs-Aktionseingaben zugeordnet werden, damit sie in der Aktion selbst gelesen werden können. Fügen Sie dazu Einträge für jede Variable in der `ext.config.yaml`-Aktion `inputs` im folgenden Format hinzu: `PARAMS_INPUT_NAME: $ENV_KEY`.

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

## OAuth-Server-zu-Server-Anmeldedaten zum Zugriffs-Token

In der App-Entwicklungs-Aktion sind die OAuth-Server-zu-Server-Anmeldedaten im `params`-Objekt verfügbar. Mithilfe dieser Anmeldedaten kann das Zugriffs-Token mithilfe von [OAuth 2.0-Bibliotheken](https://oauth.net/code/) generiert werden. Sie können auch die [Knotenabruf-Bibliothek](https://www.npmjs.com/package/node-fetch) verwenden, um eine POST-Anfrage an den Adobe IMS-Token-Endpunkt zu senden und das Zugriffs-Token abzurufen.

Das folgende Beispiel zeigt die Verwendung der `node-fetch`-Bibliothek, um eine POST-Anfrage an den Adobe IMS-Token-Endpunkt zu senden, um das Zugriffs-Token abzurufen.

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
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
