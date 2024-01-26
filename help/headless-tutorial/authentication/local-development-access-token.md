---
title: Lokales Entwicklungs-Zugriffstoken
description: Lokale AEM-Entwicklungs-Zugriffstoken werden verwendet, um die Entwicklung von Integrationen mit AEM as a Cloud Service zu beschleunigen, die programmgesteuert über HTTP mit AEM-Autoren- oder Veröffentlichungs-Services interagieren.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 603
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 100%

---

# Lokales Entwicklungs-Zugriffstoken

Entwickelnde, die Integrationen erstellen, für die ein programmatischer Zugriff auf AEM as a Cloud Service erforderlich ist, müssen temporäre Zugriffstoken für AEM einfach und schnell beziehen können, um lokale Entwicklungsaktivitäten zu erleichtern. Hierzu ermöglicht AEM Developer Console es Entwickelnden, selbst temporäre Zugriffstoken für den programmatischen Zugriff auf AEM zu generieren.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Generieren eines lokalen Entwicklungs-Zugriffstoken

![Abrufen eines lokalen Entwicklungs-Zugriffstoken](assets/local-development-access-token/getting-a-local-development-access-token.png)

Das lokale Entwicklungs-Zugriffstoken bietet Zugriff auf die AEM-Autoren- und Veröffentlichungs-Services als die Person, die das Token generiert hat, sowie auf deren Berechtigungen. Geben Sie dieses Token nicht frei oder speichern Sie es nicht in der Quell-Code-Verwaltung, auch wenn es sich um ein Entwicklungs-Token handelt.

1. Stellen Sie in [Adobe Admin Console](https://adminconsole.adobe.com/) sicher, dass Sie als Entwicklungsperson Mitglied von Folgendem sind:
   + __Cloud Manager – Entwickler__-IMS-Produktprofil (gewährt Zugriff auf AEM Developer Console).
   + __AEM-Administrator__- oder __AEM__-IMS-Produktprofil für den Service der AEM-Umgebung, in den das Zugriffstoken integriert ist.
   + Die Sandbox AEM as a Cloud Service-Umgebung erfordert nur eine Mitgliedschaft im __AEM-Administrator__- oder __AEM__-Produktprofil.
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm mit der AEM as a Cloud Service Umgebung, mit der integriert werden soll
1. Tippen Sie auf das __Auslassungszeichen__ neben der Umgebung im Abschnitt __Umgebungen__ und wählen Sie __Developer Console__ aus.
1. Tippen Sie auf die Registerkarte __Integrationen__.
1. Tippen Sie auf die Registerkarte __Lokales Token__.
1. Tippen Sie auf die Schaltfläche __Lokales Entwicklungs-Token abrufen__.
1. Tippen Sie oben links auf die __Download-Schaltfläche__ zum Herunterladen der JSON-Datei mit dem `accessToken`-Wert und speichern Sie die JSON-Datei an einem sicheren Speicherort auf Ihrem Entwicklungsrechner.
   + Dies ist Ihr 24 Stunden lang gültiges Entwickler-Zugriffstoken für die AEM as a Cloud Service-Umgebung.

![AEM Developer Console – Integrationen – Abrufen eines lokalen Entwicklungs-Token](./assets/local-development-access-token/developer-console.png)

## Verwenden des lokalen Entwicklungs-Zugriffstoken{#use-local-development-access-token}

![Lokales Entwicklungs-Zugriffstoken – Externe Anwendung](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Laden Sie das temporäre lokale Entwicklungs-Zugriffstoken von AEM Developer Console herunter.
   + Das lokale Entwicklungs-Zugriffstoken läuft alle 24 Stunden ab, sodass Entwickelnde täglich neue Zugriffstoken herunterladen müssen.
1. Es wird eine externe Anwendung entwickelt, die programmatisch mit AEM as a Cloud Service interagiert.
1. Die externe Anwendung liest das lokale Entwicklungs-Zugriffstoken ein.
1. Die externe Anwendung erstellt HTTP-Anfragen für AEM as a Cloud Service und fügt das lokale Entwicklungs-Zugriffstoken als Bearer-Token zum Autorisierungs-Header der HTTP-Anfragen hinzu.
1. AEM as a Cloud Service empfängt die HTTP-Anfrage, authentifiziert sie, führt die von ihr angeforderte Aufgabe aus und gibt eine HTTP-Antwort an die externe Anwendung zurück.

### Beispielhafte externe Anwendung

Wir erstellen eine einfache externe JavaScript-Anwendung, um zu veranschaulichen, wie mithilfe des lokalen Entwicklungs-Zugriffstoken über HTTPS programmatisch auf AEM as a Cloud Service zugegriffen werden kann. Dies zeigt, dass _alle_ außerhalb von AEM ausgeführten Anwendungen oder Systeme – unabhängig von Framework oder Sprache – das Zugriffstoken verwenden können, um sich programmatisch bei AEM as a Cloud Service zu authentifizieren und darauf zuzugreifen. Im [nächsten Abschnitt](./service-credentials.md) aktualisieren wir diesen Anwendungs-Code, um den Ansatz zum Generieren eines Token für die Produktion zu unterstützen.

Diese Beispielanwendung wird über die Befehlszeile ausgeführt und aktualisiert AEM Asset-Metadaten mithilfe von AEM Assets-HTTP-APIs wie folgt:

1. Die Parameter werden über die Befehlszeile eingelesen (`getCommandLineParams()`).
1. Das Zugriffstoken, das für die Authentifizierung bei AEM as a Cloud Service verwendet wird, wird abgerufen (`getAccessToken(...)`).
1. Alle Assets in einem AEM Asset-Ordner, der in Befehlszeilenparametern angegeben ist, werden aufgelistet (`listAssetsByFolder(...)`).
1. Die Metadaten der aufgelisteten Assets werden mit den in Befehlszeilenparametern angegebenen Werten aktualisiert (`updateMetadata(...)`).

Entscheidend zur programmatischen Authentifizierung bei AEM mit dem Zugriffstoken ist, einen Autorisierungs-HTTP-Anfrage-Header zu allen an AEM gerichteten HTTP-Anfragen im folgenden Format hinzuzufügen:

+ `Authorization: Bearer ACCESS_TOKEN`

## Ausführen der externen Anwendung

1. Stellen Sie sicher, dass [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) auf Ihrem lokalen Entwicklungsrechner installiert ist, auf dem die externe Anwendung ausgeführt wird
1. Laden Sie die [beispielhafte externe Anwendung](./assets/aem-guides_token-authentication-external-application.zip) herunter und entzippen Sie diese.
1. Führen Sie `npm install` in der Befehlszeile im Ordner dieses Projekts aus.
1. Kopieren Sie das [heruntergeladende lokale Entwicklungs-Zugriffstoken](#download-local-development-access-token) in eine Datei mit dem Namen `local_development_token.json` im Stammverzeichnis des Projekts.
   + Aber denken Sie daran, niemals Anmeldeinformationen auf Git zu übertragen.
1. Öffnen Sie `index.js` und überprüfen Sie den Code und die Kommentare für die externe Anwendung.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Überprüfen Sie die `fetch(..)`-Aufrufe in `listAssetsByFolder(...)` und `updateMetadata(...)`, wobei `headers` die `Authorization`-HTTP-Anfrage-Header mit dem Wert `Bearer ACCESS_TOKEN` definiert. So wird die von der externen Anwendung stammende HTTP-Anfrage bei AEM as a Cloud Service authentifiziert.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   In allen HTTP-Anfragen an AEM as a Cloud Service muss das Bearer-Zugriffsoken im Autorisierungs-Header festgelegt sein. Denken Sie daran, dass jede AEM as a Cloud Service-Umgebung ein eigenes Zugriffstoken erfordert. Das Zugriffstoken der Entwicklung funktioniert nicht in der Staging- oder Produktionsumgebung, das der Staging-Umgebung funktioniert nicht in der Entwicklung oder Produktion und das der Produktion funktioniert nicht in der Entwicklungs- oder Staging-Umgebung!

1. Führen Sie über die Befehlszeile aus dem Stammverzeichnis des Projekts die Anwendung aus und übergeben Sie die folgenden Parameter:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Die folgenden Parameter werden übergeben:

   + `aem`: Das Schema und der Hostname der AEM as a Cloud Service-Umgebung, mit der die Anwendung interagiert (z. B. `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Der Pfad des Asset-Ordners, dessen Assets mit `propertyValue` aktualisiert werden; fügen Sie NICHT das Präfix `/content/dam` hinzu (z. B. `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: Der zu aktualisierende Asset-Eigenschaftsname relativ zu `[dam:Asset]/jcr:content` (z. B. `metadata/dc:rights`).
   + `propertyValue`: Der Wert, auf den der `propertyName` gesetzt werden soll. Werte mit Leerzeichen müssen mit `"` eingekapselt werden (z.B. `"WKND Limited Use"`).
   + `file`: Der relative Dateipfad zur JSON-Datei, die von der AEM Developer Console heruntergeladen wurde.

   Bei erfolgreicher Ausführung der Anwendung werden die Ergebnisse für jedes aktualisierte Asset ausgegeben:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Überprüfen der Metadaten-Aktualisierung in AEM

Stellen Sie sicher, dass die Metadaten aktualisiert wurden, indem Sie sich bei der AEM as a Cloud Service-Umgebung anmelden (stellen Sie sicher, dass auf denselben Host zugegriffen wird, der im Befehlszeilenparameter `aem` angegeben ist).

1. Melden Sie sich bei der AEM as a Cloud Service-Umgebung an, mit der die externe Anwendung interagiert hat (verwenden Sie denselben Host, der im `aem`-Befehlszeilenparameter angegeben ist)
1. Navigieren Sie zu __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch die `folder`-Befehlszeilenparameter, z. B. __WKND__ > __English__ > __Adventure__ > __Napa Wine Tasting__ festgelegt wurde
1. Öffnen Sie die __Eigenschaften__ für alle (Nicht-Inhaltsfragment)-Assets im Ordner
1. Wechseln Sie zur Registerkarte __Erweitert__
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__, der der aktualisierten `metadata/dc:rights` JCR-Eigenschaft zugeordnet ist, die den im Parameter `propertyValue` angegebenen Wert widerspiegelt, z. B. __WKND Eingeschränkte Verwendung__.

![WKND Metadaten-Aktualisierung für eingeschränkte Verwendung](./assets/local-development-access-token/asset-metadata.png)

## Nächste Schritte

Jetzt, da wir programmatisch mit dem lokalen Entwicklungstoken auf AEM as a Cloud Service zugegriffen haben. Als Nächstes müssen wir die Anwendung aktualisieren, um die Verwendung von Dienstanmeldeinformationen zu ermöglichen, damit diese Anwendung in einem Produktionskontext verwendet werden kann.

+ [Verwendung von Service-Anmeldeinformationen](./service-credentials.md)
