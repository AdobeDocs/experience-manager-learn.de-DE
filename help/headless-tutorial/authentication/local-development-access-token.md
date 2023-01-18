---
title: Lokales Entwicklungs-Zugriffstoken
description: AEM Zugriffstoken für die lokale Entwicklung werden verwendet, um die Entwicklung von Integrationen mit AEM as a Cloud Service zu beschleunigen, die programmgesteuert über HTTP mit AEM Author- oder Publish-Diensten interagieren.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: 8b6d8d99c806e782a1ddce2b300211f8d4c9da56
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Lokales Entwicklungs-Zugriffstoken

Entwickler, die Integrationen erstellen, für die programmatischer Zugriff auf AEM as a Cloud Service erforderlich ist, benötigen eine einfache, schnelle Möglichkeit, temporäre Zugriffstoken für AEM zu erhalten, um lokale Entwicklungsaktivitäten zu erleichtern. Um dies zu erreichen, ermöglicht AEM Developer Console es Entwicklern, selbst temporäre Zugriffstoken zu generieren, die für den programmgesteuerten Zugriff auf AEM verwendet werden können.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Lokales Entwicklungs-Zugriffstoken generieren

![Abrufen eines Zugriffs-Tokens für lokale Entwicklung](assets/local-development-access-token/getting-a-local-development-access-token.png)

Das Zugriffstoken für lokale Entwicklung bietet Zugriff auf die Autoren- und Veröffentlichungsdienste von AEM als Benutzer, der das Token generiert hat, sowie auf dessen Berechtigungen. Geben Sie dieses Token nicht als Entwicklungstoken frei oder speichern Sie es in der Quell-Code-Verwaltung.

1. In [Adobe Admin Console](https://adminconsole.adobe.com/) Stellen Sie sicher, dass Sie, der Entwickler, Mitglied von sind:
   + __Cloud Manager - Entwickler__ IMS-Produktprofil (gewährt Zugriff auf AEM Developer Console)
   + Entweder das __AEM Administratoren__ oder __AEM__ IMS-Produktprofil für den Dienst der AEM Umgebung, in den das Zugriffstoken integriert ist
   + Sandbox AEM as a Cloud Service Umgebung erfordert nur eine Mitgliedschaft in __AEM Administratoren__ oder __AEM__ Produktprofil
1. Anmelden bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Öffnen Sie das Programm mit der AEM as a Cloud Service Umgebung, in die integriert werden soll.
1. Tippen Sie auf __Ellipse__ neben der Umgebung in __Umgebungen__ und wählen Sie __Entwicklerkonsole__
1. Tippen Sie im __Integrationen__ tab
1. Tippen Sie auf __Lokales Token__ tab
1. Tippen __Abrufen des lokalen Entwicklungstokens__ button
1. Tippen Sie auf die __Download-Schaltfläche__ in der linken oberen Ecke zum Herunterladen der JSON-Datei mit `accessToken` und speichern Sie die JSON-Datei an einem sicheren Speicherort auf Ihrem Entwicklungscomputer.
   + Dies ist Ihr 24-Stunden-Entwickler-Zugriffstoken für die AEM as a Cloud Service Umgebung.

![AEM Developer Console - Integrationen - Abrufen eines lokalen Entwicklungstokens](./assets/local-development-access-token/developer-console.png)

## Verwenden des Zugriffstokens auf die lokale Entwicklung{#use-local-development-access-token}

![Lokales Entwicklungs-Zugriffstoken - Externe Anwendung](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Laden Sie das temporäre Zugriffstoken für lokale Entwicklung von AEM Developer Console herunter.
   + Das Zugriffstoken für die lokale Entwicklung läuft alle 24 Stunden ab, sodass Entwickler täglich neue Zugriffstoken herunterladen müssen
1. Es wird eine externe Anwendung entwickelt, die programmgesteuert mit AEM as a Cloud Service interagiert
1. Die externe Anwendung liest im Zugriffstoken für die lokale Entwicklung
1. Die externe Anwendung erstellt HTTP-Anfragen für AEM as a Cloud Service und fügt das Lokale Entwicklungs-Zugriffstoken als Trägertoken zum Autorisierungs-Header der HTTP-Anforderungen hinzu.
1. AEM as a Cloud Service empfängt die HTTP-Anforderung, authentifiziert die Anforderung, führt die von der HTTP-Anforderung angeforderten Arbeiten aus und gibt eine HTTP-Antwort zurück an die externe Anwendung

### Die Beispiel-externe Anwendung

Wir erstellen eine einfache externe JavaScript-Anwendung, um zu veranschaulichen, wie mithilfe des lokalen Zugriffstokens für Entwickler programmgesteuert auf AEM über HTTPS zugegriffen werden kann. Dies zeigt, wie _any_ -Anwendung oder -System, die außerhalb von AEM ausgeführt wird, kann unabhängig von Framework oder Sprache das Zugriffstoken verwenden, um sich programmatisch bei AEM as a Cloud Service zu authentifizieren und darauf zuzugreifen. Im [nächster Abschnitt](./service-credentials.md), aktualisieren wir diesen Anwendungs-Code, um den Ansatz zum Generieren eines Tokens für die Produktionsverwendung zu unterstützen.

Diese Beispielanwendung wird über die Befehlszeile ausgeführt und aktualisiert AEM Asset-Metadaten mithilfe von AEM Assets-HTTP-APIs mithilfe des folgenden Flusses:

1. Liest in Parametern über die Befehlszeile (`getCommandLineParams()`)
1. Ruft das Zugriffstoken ab, das für die Authentifizierung bei AEM as a Cloud Service (`getAccessToken(...)`)
1. Listet alle Assets in einem AEM Asset-Ordner auf, der in Befehlszeilenparametern (`listAssetsByFolder(...)`)
1. Aktualisieren Sie die Metadaten der aufgelisteten Assets mit den in Befehlszeilenparametern (`updateMetadata(...)`)

Das Schlüsselelement bei der programmgesteuerten Authentifizierung für AEM mithilfe des Zugriffstokens ist das Hinzufügen eines Autorisierungs-HTTP-Anforderungsheaders zu allen an AEM gerichteten HTTP-Anfragen im folgenden Format:

+ `Authorization: Bearer ACCESS_TOKEN`

## Ausführen der externen Anwendung

1. Stellen Sie sicher, dass [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) auf Ihrem lokalen Entwicklungscomputer installiert ist, auf dem die externe Anwendung ausgeführt wird
1. Laden Sie die [Beispiel für eine externe Anwendung](./assets/aem-guides_token-authentication-external-application.zip)
1. Führen Sie in der Befehlszeile im Ordner dieses Projekts `npm install`
1. Kopieren Sie die [hat das Zugriffstoken für lokale Entwicklung heruntergeladen](#download-local-development-access-token) in eine Datei mit dem Namen `local_development_token.json` im Stammverzeichnis des Projekts
   + Aber denken Sie daran, nie irgendwelche Anmeldedaten zu Git zu übertragen!
1. Öffnen `index.js` und überprüfen Sie den Code und die Kommentare der externen Anwendung.

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

   Überprüfen Sie die `fetch(..)` -Aufrufe in der `listAssetsByFolder(...)` und `updateMetadata(...)`, und Hinweis `headers` definieren `Authorization` HTTP-Anforderungsheader mit dem Wert `Bearer ACCESS_TOKEN`. So authentifiziert sich die HTTP-Anforderung, die von der externen Anwendung stammt, AEM as a Cloud Service.

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

   Alle HTTP-Anfragen an AEM as a Cloud Service müssen das Trägerzugriffs-Token im Autorisierungs-Header festlegen. Beachten Sie, dass jede AEM as a Cloud Service Umgebung ein eigenes Zugriffstoken erfordert. Das Zugriffstoken der Entwicklung funktioniert nicht in der Staging- oder Produktionsumgebung, das der Staging-Umgebung funktioniert nicht in der Entwicklung oder Produktion und das der Produktion funktioniert nicht in der Entwicklungs- oder Staging-Umgebung!

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

   + `aem`: Das Schema und der Hostname der AEM as a Cloud Service Umgebung, mit der die Anwendung interagiert (z. B. `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Der Asset-Ordnerpfad, dessen Assets mit dem `propertyValue`; Fügen Sie NICHT die `/content/dam` Präfix (z. B. `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: Der zu aktualisierende Asset-Eigenschaftsname relativ zu `[dam:Asset]/jcr:content` (z. B. `metadata/dc:rights`).
   + `propertyValue`: Der Wert, der festgelegt werden soll `propertyName` nach; -Werte mit Leerzeichen müssen mit `"` (z. B. `"WKND Limited Use"`)
   + `file`: Der relative Dateipfad zur JSON-Datei, die von AEM Developer Console heruntergeladen wurde.

   Bei erfolgreicher Ausführung der Anwendung werden die Ergebnisse für jedes aktualisierte Asset ausgegeben:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Überprüfen der Metadaten-Aktualisierung in AEM

Stellen Sie sicher, dass die Metadaten aktualisiert wurden, indem Sie sich bei der AEM as a Cloud Service Umgebung anmelden (stellen Sie sicher, dass derselbe Host an die `aem` auf den Befehlszeilenparameter zugegriffen wird).

1. Melden Sie sich bei der AEM as a Cloud Service Umgebung an, mit der die externe Anwendung interagiert hat (verwenden Sie denselben Host, der in der `aem` Befehlszeilenparameter)
1. Navigieren Sie zum __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch die `folder` Befehlszeilenparameter, z. B. __WKND__ > __englisch__ > __Abenteuer__ > __Napa Weinverkostung__
1. Öffnen Sie die __Eigenschaften__ für alle (Nicht-Inhaltsfragment)-Assets im Ordner
1. Tippen Sie auf die __Erweitert__ tab
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__ , die der aktualisierten `metadata/dc:rights` JCR-Eigenschaft, die den Wert widerspiegelt, der im `propertyValue` -Parameter, z. B. __WKND Eingeschränkte Verwendung__

![WKND - Aktualisierung von Metadaten für eingeschränkte Verwendung](./assets/local-development-access-token/asset-metadata.png)

## Nächste Schritte

Jetzt, da wir programmatisch mit dem lokalen Entwicklungstoken auf AEM as a Cloud Service zugegriffen haben. Als Nächstes müssen wir die Anwendung aktualisieren, damit sie mit Service Credentials umgeht, damit diese Anwendung in einem Produktionskontext verwendet werden kann.

+ [Verwendung von Dienstanmeldeinformationen](./service-credentials.md)
