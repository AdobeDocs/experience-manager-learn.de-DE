---
title: Zugriffstoken in App Builder-Aktion generieren
description: Erfahren Sie, wie Sie ein Zugriffstoken mit JWT-Anmeldeinformationen generieren, um sie in einer App Builder-Aktion zu verwenden.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 0990fc230e2a36841380b5b0c6cd94dca24614fa
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 2%

---


# Zugriffstoken in App Builder-Aktion generieren

App Builder-Aktionen müssen möglicherweise mit Adobe-APIs interagieren, die mit Adobe Developer Console-Projekten verknüpft sind, in denen die App Builder-App bereitgestellt wird.

Dies erfordert möglicherweise die App Builder-Aktion, um ein eigenes Zugriffstoken zu generieren, das mit dem gewünschten Adobe Developer Console-Projekt verknüpft ist.

>[!IMPORTANT]
>
> Überprüfen [Sicherheitsdokumentation für App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) , um zu verstehen, wann Zugriffstoken generiert werden sollten, im Gegensatz zur Verwendung bereitgestellter Zugriffstoken.
>
> Die benutzerdefinierte Aktion muss möglicherweise eigene Sicherheitsprüfungen durchführen, um sicherzustellen, dass nur zugelassene Kunden auf die App Builder-Aktion und die Adobe-Dienste dahinter zugreifen können.


## .env-Datei

Im App Builder-Projekt `.env` -Datei, hängen Sie benutzerdefinierte Schlüssel für die JWT-Anmeldeinformationen jedes Adobe Developer Console-Projekts an. Die JWT-Berechtigungswerte können aus dem Adobe Developer Console-Projekt abgerufen werden. __Anmeldeinformationen__ > __Dienstkonto (JWT)__ für einen bestimmten Arbeitsbereich.

![Adobe Developer Console-JWT-Dienstanmeldeinformationen](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Die Werte für `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` kann direkt aus dem Bildschirm &quot;JWT-Anmeldedaten&quot;des Adobe Developer Console-Projekts kopiert werden.

### Metaskope

Bestimmen Sie die Adobe-APIs und ihre Metriken, mit denen die App Builder-Aktion interagiert. Auflisten von Metriken mit Kommagetrennzeichen im `JWT_METASCOPES` Schlüssel. Gültige Metriken sind in [Dokumentation zu JWT-Metascope in Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Beispielsweise könnte der folgende Wert zum `JWT_METASCOPES` Schlüssel in der `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Privater Schlüssel

Die `JWT_PRIVATE_KEY` muss besonders formatiert sein, da es sich um einen nativen mehrzeiligen Wert handelt, der in `.env` Dateien. Die einfachste Möglichkeit besteht darin, den privaten Schlüssel mit base64 zu kodieren. Base64-Kodierung des privaten Schlüssels (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) kann mit nativen Tools durchgeführt werden, die von Ihrem Betriebssystem bereitgestellt werden.

>[!BEGINTABS]

>[!TAB macOS]

1. Öffnen Sie `Terminal`
1. `$ base64 -i /path/to/private.key | pbcopy`

Die base64-Ausgabe wird automatisch in die Zwischenablage kopiert

>[!TAB Windows]



1. Öffnen Sie `Command Prompt`
1. `$ certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Kopieren Sie den Inhalt von `encoded-private.key` in die Zwischenablage

>[!TAB Linux®]

1. Terminal öffnen
1. `$ base64 private.key`
1. Die base64-Ausgabe in die Zwischenablage kopieren

>[!ENDTABS]

Beispielsweise könnte der folgende Wert zum `JWT_PRIVATE_KEY` Schlüssel in der `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Erweiterungskonfiguration

Mit dem JWT-Berechtigungswert, der im `.env` -Datei, müssen sie den AppBuilder-Aktionseingaben zugeordnet werden, damit sie in der Aktion selbst gelesen werden können. Fügen Sie dazu Einträge für jede Variable im `ext.config.yaml` action `inputs` im Format: `INPUT_NAME=$ENV_KEY`.

Beispiel:

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
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

Die unter `inputs` finden Sie im `params` -Objekt, das für die App Builder-Aktion bereitgestellt wird.


## Konvertieren von JWT-Berechtigungen in Zugriffstoken

In der App Builder-Aktion sind die JWT-Anmeldeinformationen im `params` -Objekt und verwenden Sie [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) , um ein Zugriffstoken zu generieren, das wiederum auf andere Adobe-APIs und -Dienste zugreifen kann.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
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
