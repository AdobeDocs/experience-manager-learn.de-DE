---
title: Lokales Entwicklungs-Zugriffstoken
description: AEM Zugriffstoken für die lokale Entwicklung werden verwendet, um die Entwicklung von Integrationen mit AEM als Cloud Service zu beschleunigen, der programmgesteuert mit AEM Author- oder Publish-Diensten über HTTP interagiert.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrationen
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---


# Lokales Entwicklungs-Zugriffstoken

Entwickler, die Integrationen erstellen, für die programmatischer Zugriff auf AEM als Cloud Service erforderlich ist, benötigen eine einfache, schnelle Möglichkeit, temporäre Zugriffstoken für AEM zu erhalten, um lokale Entwicklungsaktivitäten zu erleichtern. Um dies zu erreichen, ermöglicht AEM Developer Console es Entwicklern, selbst temporäre Zugriffstoken zu generieren, die für den programmgesteuerten Zugriff auf AEM verwendet werden können.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Lokales Entwicklungs-Zugriffstoken generieren

![Abrufen eines Zugriffs-Tokens für lokale Entwicklung](assets/local-development-access-token/getting-a-local-development-access-token.png)

Das Zugriffstoken für lokale Entwicklung bietet Zugriff auf die Autoren- und Veröffentlichungsdienste von AEM als Benutzer, der das Token generiert hat, sowie auf dessen Berechtigungen. Geben Sie dieses Token nicht als Entwicklungstoken frei oder speichern Sie es in der Quell-Code-Verwaltung.

1. Stellen Sie in [Adobe Admin Console](https://adminconsole.adobe.com/) sicher, dass Sie, der Entwickler, Mitglied von sind:
   + __Cloud Manager -__ Entwickler-IMS-Produktprofil (gewährt Zugriff auf AEM Developer Console)
   + Entweder die __AEM Administratoren__ oder __AEM Benutzer__ IMS-Produktprofil für den Dienst der AEM Umgebung, in den das Zugriffstoken integriert wird
   + Sandbox-AEM als Cloud Service-Umgebung erfordert nur eine Mitgliedschaft im __AEM-Administrator__ oder __AEM-Profil für Benutzer__
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm, das die AEM als Cloud Service-Umgebung enthält, in die integriert werden soll.
1. Tippen Sie im Abschnitt __Umgebungen__ neben der Umgebung auf die Auslassungszeichen __und wählen Sie__ Entwicklerkonsole __aus.__
1. Tippen Sie auf der Registerkarte __Integrationen__ .
1. Tippen Sie auf die Schaltfläche __Lokales Entwicklungstoken abrufen__ .
1. Tippen Sie oben links auf die Schaltfläche __Herunterladen__ , um die JSON-Datei mit dem Wert `accessToken` herunterzuladen und die JSON-Datei an einem sicheren Speicherort auf Ihrem Entwicklungscomputer zu speichern.
   + Dies ist Ihr 24-Stunden-Entwicklerzugriffstoken auf die AEM als Cloud Service-Umgebung.

![AEM Developer Console - Integrationen - Abrufen eines lokalen Entwicklungstokens](./assets/local-development-access-token/developer-console.png)

## Verwenden Sie das Zugriffstoken für die lokale Entwicklung{#use-local-development-access-token}

![Lokales Entwicklungs-Zugriffstoken - Externe Anwendung](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Laden Sie das temporäre Zugriffstoken für lokale Entwicklung von AEM Developer Console herunter.
   + Das Zugriffstoken für die lokale Entwicklung läuft alle 24 Stunden ab, sodass Entwickler täglich neue Zugriffstoken herunterladen müssen
1. Es wird eine externe Anwendung entwickelt, die programmgesteuert mit AEM als Cloud Service interagiert
1. Die externe Anwendung liest im Zugriffstoken für die lokale Entwicklung
1. Die externe Anwendung erstellt HTTP-Anfragen an AEM als Cloud Service und fügt das Lokale Entwicklungs-Zugriffstoken als Trägertoken zum Autorisierungs-Header der HTTP-Anforderungen hinzu.
1. AEM als Cloud Service die HTTP-Anforderung erhält, die Anfrage authentifiziert, die von der HTTP-Anforderung angeforderten Arbeiten durchführt und eine HTTP-Antwort an die externe Anwendung zurückgibt

### Die Beispiel-externe Anwendung

Wir erstellen eine einfache externe JavaScript-Anwendung, die veranschaulicht, wie mithilfe des lokalen Zugriffstokens für Entwickler programmgesteuert auf AEM als Cloud Service über HTTPS zugegriffen werden kann. Dies zeigt, wie _alle_ Anwendungen oder Systeme, die außerhalb von AEM ausgeführt werden, unabhängig von Framework oder Sprache das Zugriffstoken verwenden können, um sich programmatisch bei und auf AEM als Cloud Service zu authentifizieren. Im [nächsten Abschnitt](./service-credentials.md) werden wir diesen Anwendungs-Code aktualisieren, um den Ansatz zum Generieren eines Tokens für die Verwendung in der Produktion zu unterstützen.

Diese Beispielanwendung wird über die Befehlszeile ausgeführt und aktualisiert AEM Asset-Metadaten mithilfe von AEM Assets-HTTP-APIs mithilfe des folgenden Flusses:

1. Liest in Parametern über die Befehlszeile (`getCommandLineParams()`)
1. Ruft das Zugriffstoken ab, das für die Authentifizierung bei AEM als Cloud Service (`getAccessToken(...)`) verwendet wird
1. Listet alle Assets in einem AEM Asset-Ordner auf, der in einem Befehlszeilenparameter (`listAssetsByFolder(...)`) angegeben ist.
1. Aktualisieren der Metadaten der aufgelisteten Assets mit den in Befehlszeilenparametern (`updateMetadata(...)`) angegebenen Werten

Das Schlüsselelement bei der programmgesteuerten Authentifizierung für AEM mithilfe des Zugriffstokens ist das Hinzufügen eines Autorisierungs-HTTP-Anforderungsheaders zu allen an AEM gerichteten HTTP-Anfragen im folgenden Format:

+ `Authorization: Bearer ACCESS_TOKEN`

## Ausführen der externen Anwendung

1. Stellen Sie sicher, dass [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) auf Ihrem lokalen Entwicklungscomputer installiert ist, der zum Ausführen der externen Anwendung verwendet wird.
1. Laden Sie die externe Beispielanwendung [herunter und entpacken Sie sie.](./assets/aem-guides_token-authentication-external-application.zip)
1. Führen Sie in der Befehlszeile im Ordner dieses Projekts `npm install` aus.
1. Kopieren Sie das [heruntergeladene Lokales Entwicklungs-Zugriffstoken](#download-local-development-access-token) in eine Datei mit dem Namen `local_development_token.json` im Stammverzeichnis des Projekts.
   + Aber denken Sie daran, nie irgendwelche Anmeldedaten zu Git zu übertragen!
1. Öffnen Sie `index.js` und überprüfen Sie den Code und die Kommentare der externen Anwendung.

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   Überprüfen Sie die `fetch(..)` -Aufrufe in den `listAssetsByFolder(...)` und `updateMetadata(...)` und beachten Sie `headers`, dass Sie den `Authorization` HTTP-Anforderungsheader mit dem Wert `Bearer ACCESS_TOKEN` definieren. So authentifiziert sich die HTTP-Anforderung, die von der externen Anwendung stammt, als Cloud Service AEM.

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

   Alle HTTP-Anfragen, die als Cloud Service AEM werden, müssen das Trägerzugriffs-Token im Autorisierungs-Header festlegen. Beachten Sie, dass jede AEM als Cloud Service-Umgebung das eigene Zugriffstoken erfordert. Das Zugriffstoken der Entwicklung funktioniert nicht auf der Staging- oder Produktionsumgebung, Staging-Workflows funktionieren nicht in der Entwicklung oder Produktion und das der Produktion funktioniert nicht in der Entwicklungs- oder Staging-Umgebung!

1. Führen Sie über die Befehlszeile aus dem Stammverzeichnis des Projekts die Anwendung aus und übergeben Sie die folgenden Parameter:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Die folgenden Parameter werden übergeben:

   + `aem`: Das Schema und der Hostname des AEM als Cloud Service-Umgebung, mit dem die Anwendung interagiert (z. B.  `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Der Asset-Ordnerpfad, dessen Assets mit dem  `propertyValue`aktualisiert werden; Fügen Sie NICHT das  `/content/dam` Präfix hinzu (z. B.  `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`: Der Asset-Eigenschaftsname, der aktualisiert werden soll, relativ zu  `[dam:Asset]/jcr:content` (z. B.  `metadata/dc:rights`).
   + `propertyValue`: Der Wert,  `propertyName` auf den festgelegt werden soll. -Werte mit Leerzeichen müssen mit  `"` (z. B.  `"WKND Limited Use"`)
   + `file`: Der relative Dateipfad zur JSON-Datei, die von AEM Developer Console heruntergeladen wurde.

   Bei erfolgreicher Ausführung der Anwendung werden die Ergebnisse für jedes aktualisierte Asset ausgegeben:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Überprüfen der Metadaten-Aktualisierung in AEM

Überprüfen Sie, ob die Metadaten aktualisiert wurden, indem Sie sich bei der AEM als Cloud Service-Umgebung anmelden (stellen Sie sicher, dass auf denselben Host zugegriffen wird, der an den Befehlszeilenparameter `aem` übergeben wurde).

1. Melden Sie sich bei der AEM als Cloud Service-Umgebung an, mit der die externe Anwendung interagiert hat (verwenden Sie denselben Host, der im Befehlszeilenparameter `aem` bereitgestellt wird).
1. Navigieren Sie zu __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch den Befehlszeilenparameter `folder` angegeben wird, z. B. __WKND__ > __Englisch__ > __Abenteuer__ > __Napa Wine Testing__
1. Öffnen Sie die __Properties__ für alle (Nicht-Inhaltsfragment)-Assets im Ordner
1. Tippen Sie auf die Registerkarte __Erweitert__ .
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__, der der aktualisierten `metadata/dc:rights` JCR-Eigenschaft zugeordnet ist, die den im Parameter `propertyValue` bereitgestellten Wert widerspiegelt, z. B. __WKND Limited Use__

![WKND - Aktualisierung von Metadaten für eingeschränkte Verwendung](./assets/local-development-access-token/asset-metadata.png)

## Nächste Schritte

Nachdem wir nun mithilfe des lokalen Entwicklungstokens programmatisch auf AEM als Cloud Service zugegriffen haben, müssen wir die Anwendung aktualisieren, damit sie mit Dienstanmeldeinformationen umgeht, damit diese Anwendung in einem Produktionskontext verwendet werden kann.

+ [Verwendung von Dienstanmeldeinformationen](./service-credentials.md)
