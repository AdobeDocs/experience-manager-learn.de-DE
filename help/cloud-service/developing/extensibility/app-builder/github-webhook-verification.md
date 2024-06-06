---
title: Github.com Webhook-Verifizierung
description: Erfahren Sie, wie Sie in einer App Builder-Aktion eine Webhook-Anfrage von Github.com überprüfen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Github.com Webhook-Verifizierung

Mit Webhooks können Sie Integrationen erstellen oder einrichten, die bestimmte Ereignisse auf GitHub.com abonnieren. Wenn eines dieser Ereignisse ausgelöst wird, sendet GitHub eine HTTP-POST-Payload an die konfigurierte URL des Webhooks. Aus Sicherheitsgründen ist es jedoch wichtig zu überprüfen, ob die eingehende Webhook-Anfrage tatsächlich von GitHub stammt und nicht von einem böswilligen Akteur. In diesem Tutorial werden Sie durch die Schritte geführt, die zum Überprüfen einer GitHub.com Webhook-Anfrage in einer Adobe App Builder-Aktion mit einem gemeinsam genutzten geheimen Schlüssel durchgeführt werden.

## GitHub-Geheimnis in AppBuilder einrichten

1. **Geheimnis hinzufügen zu `.env` Datei:**

   Im App Builder-Projekt `.env` -Datei einen benutzerdefinierten Schlüssel für das GitHub.com Webhook-Geheimnis hinzufügen:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Aktualisieren `ext.config.yaml` Datei:**

   Die `ext.config.yaml` muss aktualisiert werden, um die GitHub.com Webhook-Anfrage zu überprüfen.

   - AppBuilder-Aktion festlegen `web` Konfiguration auf `raw` um den rohen Anfragetext von GitHub.com zu erhalten.
   - under `inputs` Fügen Sie in der AppBuilder-Aktionskonfiguration die `GITHUB_SECRET` Schlüssel, Zuordnung zum `.env` -Feld, das den geheimen Schlüssel enthält. Der Wert dieses Schlüssels ist die `.env` Feldname mit dem Präfix `$`.
   - Legen Sie die `require-adobe-auth` Anmerkung in der AppBuilder-Aktionskonfiguration zu `false` , damit die Aktion aufgerufen werden kann, ohne dass eine Adobe-Authentifizierung erforderlich ist.

   Das Ergebnis `ext.config.yaml` -Datei sollte ungefähr so aussehen:

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## Hinzufügen von Überprüfungscode zur AppBuilder-Aktion

Fügen Sie als Nächstes den unten bereitgestellten JavaScript-Code hinzu (kopiert von [Dokumentation zu GitHub.com](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) zu Ihrer AppBuilder-Aktion hinzu. Achten Sie darauf, die `verifySignature` -Funktion.

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## Implementierung der Verifizierung in der AppBuilder-Aktion

Vergewissern Sie sich anschließend, dass die Anfrage von GitHub stammt, indem Sie die Signatur im Anforderungsheader mit der von der `verifySignature` -Funktion.

In der AppBuilder-Aktion `index.js`Fügen Sie den folgenden Code zum `main` Funktion:


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## Webhook in GitHub konfigurieren

Geben Sie in GitHub.com bei der Erstellung des Webhooks denselben geheimen Wert für GitHub.com ein. Verwenden Sie den in Ihrer `.env` -Datei `GITHUB_SECRET` Schlüssel.

Wechseln Sie in GitHub.com zu den Repository-Einstellungen und bearbeiten Sie den Webhook. Geben Sie in den Webhook-Einstellungen den geheimen Wert im `Secret` -Feld. Klicks __Webhook aktualisieren__ unten, um die Änderungen zu speichern.

![Github-Webhook-Geheimnis](./assets/github-webhook-verification/github-webhook-settings.png)

Mit diesen Schritten stellen Sie sicher, dass Ihre App Builder-Aktion sicher überprüfen kann, ob eingehende Webhook-Anfragen tatsächlich von Ihrem GitHub.com Webhook stammen.
