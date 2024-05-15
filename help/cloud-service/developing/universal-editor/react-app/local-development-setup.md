---
title: Lokale Entwicklungseinrichtung
description: Erfahren Sie, wie Sie eine lokale Entwicklungsumgebung für den Universal Editor einrichten, damit Sie den Inhalt einer React-Beispielanwendung bearbeiten können.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 3%

---

# Lokale Entwicklungseinrichtung

Erfahren Sie, wie Sie eine lokale Entwicklungsumgebung einrichten, um den Inhalt einer React-App mit dem AEM Universal Editor zu bearbeiten.

## Voraussetzungen

Für dieses Tutorial sind folgende Dinge erforderlich:

- Grundlegende HTML- und JavaScript-Kenntnisse.
- Die folgenden Tools müssen lokal installiert sein:
   - [Node.js](https://nodejs.org/de/download/)
   - [Git](https://git-scm.com/downloads)
   - IDE oder Code-Editor, z. B. [Visual Studio-Code](https://code.visualstudio.com/)
- Laden Sie Folgendes herunter und installieren Sie es:
   - [AEM AS A CLOUD SERVICE SDK](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): Enthält die Schnellstart-JAR-Datei, die zum lokalen Ausführen AEM Autoren- und Veröffentlichungsvorgangs für Entwicklungszwecke verwendet wird.
   - [Universal Editor-Dienst](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home): Eine lokale Kopie des Universal Editor-Dienstes, der eine Untergruppe von Funktionen aufweist und vom Software Distribution-Portal heruntergeladen werden kann.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): Ein einfacher lokaler SSL-HTTP-Proxy, der ein selbstsigniertes Zertifikat für die lokale Entwicklung verwendet. Der universelle Editor AEM benötigt die HTTPS-URL der React-App, um sie im Editor zu laden.

## Lokales Setup

Gehen Sie wie folgt vor, um die lokale Entwicklungsumgebung einzurichten:

### AEM SDK

Um die Inhalte für die WKND Teams React-App bereitzustellen, installieren Sie die folgenden Pakete im lokalen AEM SDK.

- [WKND-Teams - Inhaltspaket](./assets/basic-tutorial-solution.content.zip): Enthält die Inhaltsfragmentmodelle, Inhaltsfragmente und persistenten GraphQL-Abfragen.
- [WKND-Teams - Konfigurationspaket](./assets/basic-tutorial-solution.ui.config.zip): Enthält die Konfigurationen Cross-Origin Resource Sharing (CORS) und Token Authentication Handler . Das CORS ermöglicht es Nicht-AEM-Webeigenschaften, browserbasierte clientseitige Aufrufe an AEM GraphQL-APIs durchzuführen, und der Token-Authentifizierungs-Handler wird verwendet, um jede Anfrage an AEM zu authentifizieren.

  ![WKND-Teams - Pakete](./assets/wknd-teams-packages.png)

### React-App

Gehen Sie wie folgt vor, um die WKND Teams React-App einzurichten:

1. Klonen Sie die [WKND Teams React App](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) aus dem `basic-tutorial` Lösungsverzweigung.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigieren Sie zum `basic-tutorial` und öffnen Sie es in Ihrem Code-Editor.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Installieren Sie die Abhängigkeiten und starten Sie die React-App.

   ```bash
   $ npm install
   $ npm start
   ```

1. Öffnen Sie die React-App der WKND-Teams in Ihrem Browser unter [http://localhost:3000](http://localhost:3000). Es wird eine Liste der Teammitglieder und ihre Details angezeigt. Der Inhalt für die React-App wird vom lokalen AEM SDK mithilfe von GraphQL-APIs bereitgestellt (`/graphql/execute.json/my-project/all-teams`), die Sie auf der Registerkarte &quot;Netzwerk&quot;des Browsers überprüfen können.

   ![WKND-Teams - React-App](./assets/wknd-teams-react-app.png)

### Universal Editor Service

So richten Sie die **lokal** Gehen Sie wie folgt vor:

1. Laden Sie die neueste Version des Universal Editor-Dienstes vom [Software Distribution-Portal](https://experience.adobe.com/downloads).

   ![Software Distribution - Universal Editor Service](./assets/universal-editor-service.png)

1. Extrahieren Sie die heruntergeladene ZIP-Datei und kopieren Sie die `universal-editor-service.cjs` in ein neues Verzeichnis mit dem Namen `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Erstellen `.env` in der Datei `universal-editor-service` und fügen Sie die folgenden Umgebungsvariablen hinzu:

   ```bash
   # The port on which the Universal Editor service runs
   EXPRESS_PORT=8000
   # Disable SSL verification
   NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

1. Starten Sie den lokalen Universal Editor-Dienst.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

Der obige Befehl startet den Universal Editor-Dienst am Port `8000` und Sie sollten die folgende Ausgabe sehen:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Lokaler SSL-HTTP-Proxy

Der universelle Editor AEM erfordert, dass die React-App über HTTPS bereitgestellt wird. Richten wir einen lokalen SSL-HTTP-Proxy ein, der ein selbstsigniertes Zertifikat für die lokale Entwicklung verwendet.

Gehen Sie wie folgt vor, um den lokalen SSL-HTTP-Proxy einzurichten und den AEM SDK- und Universal Editor-Dienst über HTTPS bereitzustellen:

1. Installieren Sie die `local-ssl-proxy` global verfügbar.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Starten Sie zwei Instanzen des lokalen SSL-HTTP-Proxys für die folgenden Dienste:

   - SDK-lokaler SSL-HTTP-Proxy auf Port AEM `8443`.
   - lokaler SSL-HTTP-Proxy des Universal Editor-Dienstes auf Port `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### React-App für die Verwendung von HTTPS aktualisieren

Gehen Sie wie folgt vor, um HTTPS für die WKND Teams React-App zu aktivieren:

1. Stoppen Sie den React durch Drücken der Taste `Ctrl + C` im Terminal.
1. Aktualisieren Sie die `package.json` -Datei, die eingeschlossen werden soll `HTTPS=true` Umgebungsvariable in `start` Skript.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Aktualisieren Sie die `REACT_APP_HOST_URI` im `.env.development` -Datei, um das HTTPS-Protokoll und den lokalen SSL-HTTP-Proxy-Port des AEM SDK zu verwenden.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Aktualisieren Sie die `../src/proxy/setupProxy.auth.basic.js` Datei, um entspannte SSL-Einstellungen mit `secure: false` -Option.

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Starten Sie die React-App.

   ```bash
   $ npm start
   ```

## Überprüfen des Setups

Nachdem wir die lokale Entwicklungsumgebung mit den oben genannten Schritten eingerichtet haben, überprüfen wir die Einrichtung.

### Lokale Überprüfung

Stellen Sie sicher, dass die folgenden Dienste lokal über HTTPS ausgeführt werden. Möglicherweise müssen Sie die Sicherheitswarnung im Browser für das selbstsignierte Zertifikat akzeptieren:

1. WKND Teams React-App auf [https://localhost:3000](https://localhost:3000)
1. AEM SDK on [https://localhost:8443](https://localhost:8443)
1. Universal Editor-Dienst auf [https://localhost:8001](https://localhost:8001)

### WKND-Teams React-App im universellen Editor laden

Laden wir die React-App &quot;WKND-Teams&quot;in den Universal Editor, um die Einrichtung zu überprüfen:

1. Öffnen Sie den Universal Editor https://experience.adobe.com/#/aem/editor in Ihrem Browser. Melden Sie sich bei entsprechender Aufforderung mit Ihrem Adobe ID an.

1. Geben Sie die React-App-URL der WKND-Teams in das Eingabefeld Site-URL des Universal Editors ein und klicken Sie auf `Open`.

   ![Universal Editor - Site-URL](./assets/universal-editor-site-url.png)

1. Die WKND-Teams-React-App wird im universellen Editor geladen **aber Sie können den Inhalt noch nicht bearbeiten**. Sie müssen die React-App instrumentieren, um die Inhaltsbearbeitung mit dem universellen Editor zu aktivieren.

   ![Universal Editor - WKND Teams React App](./assets/universal-editor-wknd-teams.png)


## Nächster Schritt

Erfahren Sie, wie [Instrumentieren der React-App zum Bearbeiten des Inhalts](./instrument-to-edit-content.md).
