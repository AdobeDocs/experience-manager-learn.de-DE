---
title: Webhook-Verifizierung von Github.com
description: Erfahren Sie, wie Sie eine Webhook-Anfrage von Github.com in einer App-Entwicklungs-Aktion verifizieren.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '363'
ht-degree: 100%

---

# Webhook-Verifizierung von Github.com

Mit Webhooks können Sie Integrationen erstellen oder einrichten, die bestimmte Ereignisse auf GitHub.com abonnieren. Wenn eines dieser Ereignisse ausgelöst wird, sendet GitHub eine HTTP-POST-Payload an die konfigurierte URL des Webhooks. Aus Sicherheitsgründen ist es jedoch wichtig zu überprüfen, ob die eingehende Webhook-Anfrage tatsächlich von GitHub stammt und nicht aus einer böswilligen Quelle. In diesem Tutorial werden Sie durch die Schritte geführt, die zum Überprüfen einer Webhook-Anfrage von GitHub.com in einer Aktion der Adobe-App-Entwicklung mit einem gemeinsam genutzten geheimen Schlüssel durchgeführt werden.

## Einrichten des GitHub-Geheimnisses in der App-Entwicklung

1. **Fügen Sie ein Geheimnis zur `.env`-Datei hinzu:**

   Fügen Sie in der `.env`-Datei des App-Entwicklungs-Projekts einen benutzerdefinierten Schlüssel für das Webhook-Geheimnis von GitHub.com hinzu:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Aktualisieren Sie die Datei `ext.config.yaml`:**

   Die Datei `ext.config.yaml` muss aktualisiert werden, um die Webhook-Anfrage bei GitHub.com zu verifizieren.

   - Legen Sie die AppBuilder-Aktionskonfiguration `web` auf `raw` fest, um den rohen Anfragetext von GitHub.com zu erhalten.
   - Fügen Sie unter `inputs` in der AppBuilder-Aktionskonfiguration den Schlüssel `GITHUB_SECRET` hinzu und ordnen Sie ihn dem Feld `.env` zu, das das Geheimnis enthält. Der Wert dieses Schlüssels ist der Feldname `.env` mit dem Präfix `$`.
   - Legen Sie die Anmerkung `require-adobe-auth` in der AppBuilder-Aktionskonfiguration auf `false` fest, damit die Aktion ohne eine Adobe-Authentifizierung aufgerufen werden kann.

   Die sich daraus ergebende Datei `ext.config.yaml` sollte in etwa so aussehen:

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

## Hinzufügen von Verifizierungs-Code zur AppBuilder-Aktion

Als nächstes fügen Sie den unten angegebenen JavaScript-Code (kopiert aus der Dokumentation von [GitHub.com](https://docs.github.com/de/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) zu Ihrer AppBuilder-Aktion hinzu. Achten Sie darauf, die Funktion `verifySignature` zu exportieren.

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

## Implementieren der Verifizierung in der AppBuilder-Aktion

Vergewissern Sie sich anschließend, ob die Anfrage von GitHub kommt, indem Sie die Signatur im Anfrage-Header mit der von der Funktion `verifySignature` generierten Signatur vergleichen.

Fügen Sie in der `index.js` der AppBuilder-Aktion den folgenden Code zur Funktion `main` hinzu:


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

## Konfigurieren des Webhooks in GitHub

Geben Sie in GitHub.com bei der Erstellung des Webhooks denselben geheimen Wert für GitHub.com ein. Verwenden Sie den geheimen Wert, der im Schlüssel `GITHUB_SECRET` Ihrer Datei `.env` angegeben ist.

Wechseln Sie in GitHub.com zu den Repository-Einstellungen und bearbeiten Sie den Webhook. Geben Sie in den Webhook-Einstellungen den geheimen Wert in das Feld `Secret` ein. Klicken Sie unten auf __Webhook aktualisieren__, um die Änderungen zu speichern.

![Github-Webhook-Geheimnis](./assets/github-webhook-verification/github-webhook-settings.png)

Mit diesen Schritten stellen Sie sicher, dass Ihre App Builder-Aktion zuverlässig überprüfen kann, ob eingehende Webhook-Anfragen tatsächlich von Ihrem GitHub.com-Webhook stammen.
