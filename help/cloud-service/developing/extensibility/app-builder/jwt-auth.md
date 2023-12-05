---
title: Generieren von Zugriffs-Token in App-Entwicklungs-Aktion
description: Erfahren Sie, wie Sie ein Zugriffs-Token mit JWT-Anmeldeinformationen generieren, um sie in einer App-Entwicklungs-Aktion zu verwenden.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 229
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 100%

---

# Generieren von Zugriffs-Token in App-Entwicklungs-Aktion

App-Entwicklungs-Aktionen müssen möglicherweise mit Adobe-APIs interagieren, die mit Adobe Developer Console-Projekten verknüpft sind, in denen die App-Entwicklungs-App bereitgestellt wird.

Dies kann erfordern, dass die App-Entwicklungs-Aktion ein eigenes Zugriffs-Token generiert, das mit dem gewünschten Adobe Developer Console-Projekt verknüpft ist.

>[!IMPORTANT]
>
> Lesen Sie in der [Sicherheitsdokumentation für die App-Entwicklung](https://developer.adobe.com/app-builder/docs/guides/security/) nach, um zu verstehen, wann es angemessen ist, Zugriffs-Token zu generieren bzw. bereitgestellte Zugriffs-Token zu verwenden.
>
> Die benutzerdefinierte Aktion muss möglicherweise eigene Sicherheitsprüfungen enthalten, um sicherzustellen, dass nur zugelassene Kundinnen und Kunden auf die App-Entwicklungs-Aktion und die Adobe-Dienste dahinter zugreifen können.


## .env-Datei

Hängen Sie in der `.env`-Datei des App-Entwicklungs-Projekts benutzerdefinierte Schlüssel für jede JWT-Anmeldeinformation des Adobe Developer Console-Projekts an. Die Werte der JWT-Anmeldedaten können über __Anmeldedaten__ > __Dienstkonto (JWT)__ vom Adobe Developer Console-Projekt für einen bestimmten Arbeitsbereich abgerufen werden.

![Anmeldedaten für den JWT-Dienst von Adobe Developer Console](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Die Werte für `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` können direkt aus dem Bildschirm „JWT-Anmeldedaten“ des Adobe Developer Console-Projekts kopiert werden.

### Metaskopen

Bestimmen Sie die Adobe-APIs und ihre Metaskopen, mit denen die App-Entwicklungs-Aktion interagiert. Listen Sie Metaskopen mit Kommatrennzeichen im Schlüssel `JWT_METASCOPES` auf. Gültige Metaskopen sind in der [Dokumentation zu JWT-Metaskopen in Adobe](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/) aufgeführt.


Beispielsweise könnte der folgende Wert zum Schlüssel `JWT_METASCOPES` in `.env` hinzugefügt werden:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Privater Schlüssel

Der `JWT_PRIVATE_KEY` muss besonders formatiert sein, da es sich um einen nativen mehrzeiligen Wert handelt, der in `.env`-Dateien nicht unterstützt wird. Die einfachste Möglichkeit besteht darin, den privaten Schlüssel mit base64 zu codieren. Das Base64-Codieren des privaten Schlüssels (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) kann mit nativen Tools durchgeführt werden, die von Ihrem Betriebssystem bereitgestellt werden.

>[!BEGINTABS]

>[!TAB macOS]

1. Öffnen Sie ein `Terminal`.
1. Führen Sie den Befehl `base64 -i /path/to/private.key | pbcopy` aus.
1. Die base64-Ausgabe wird automatisch in die Zwischenablage kopiert.
1. Fügen Sie sie in `.env` als Wert des entsprechenden Schlüssels ein.

>[!TAB Windows]

1. Öffnen Sie `Command Prompt`.
1. Führen Sie den Befehl `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key` aus.
1. Führen Sie den Befehl `findstr /v CERTIFICATE C:\path\to\encoded-private.key` aus.
1. Kopieren Sie die base64-Ausgabe in die Zwischenablage.
1. Fügen Sie sie in `.env` als Wert des entsprechenden Schlüssels ein.

>[!TAB Linux®]

1. Öffnen Sie ein Terminal. 
1. Führen Sie den Befehl `base64 private.key` aus.
1. Kopieren Sie die base64-Ausgabe in die Zwischenablage.
1. Fügen Sie sie in `.env` als Wert des entsprechenden Schlüssels ein.

>[!ENDTABS]

Beispielsweise kann der folgende base64-codierte private Schlüssel zum Schlüssel `JWT_PRIVATE_KEY` in `.env` hinzugefügt werden:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Zuordnung der Eingaben

Mit dem in der `.env`-Datei festgelegten Wert der JWT-Anmeldedaten müssen sie den App-Entwicklungs-Aktionseingaben zugeordnet werden, damit sie in der Aktion selbst gelesen werden können. Fügen Sie dazu Einträge für jede Variable in der `ext.config.yaml`-Aktion `inputs` im folgenden Format hinzu: `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

Die unter `inputs` definierten Schlüssel sind verfügbar im `params`-Objekt, das für die App-Entwicklungs-Aktion bereitgestellt wird.


## JWT-Anmeldedaten zum Zugriffs-Token

In der App-Entwicklungs-Aktion sind die JWT-Anmeldedaten im `params`-Objekt verfügbar und verwendbar durch [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth), um ein Zugriffs-Token zu generieren, das wiederum auf andere Adobe-APIs und -Dienste zugreifen kann.

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
