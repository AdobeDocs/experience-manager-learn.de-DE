---
title: Exportieren von Assets
description: Erfahren Sie, wie Sie Assets stapelweise exportieren und auf Ihren lokalen Computer herunterladen können.
feature: Asset Management
version: Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2024-04-08T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
source-git-commit: 681263a2f4008980fc3119e88d34b73da23eb260
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---


# Exportieren von Assets

Erfahren Sie, wie Sie Assets mithilfe eines anpassbaren Node.js-Skripts auf Ihren lokalen Computer exportieren. Dieses Exportskript enthält ein Beispiel dafür, wie Assets mithilfe von programmgesteuert von AEM heruntergeladen werden können. [AEM Assets-HTTP-APIs](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), mit besonderem Fokus auf die Original-Ausgabedarstellungen, um die höchste Qualität zu gewährleisten. Sie wurde entwickelt, um die Ordnerstruktur von AEM Assets auf Ihrem lokalen Laufwerk zu replizieren, sodass Assets einfach gesichert oder migriert werden können.

Das -Skript lädt nur die Original-Ausgabedarstellungen des Assets herunter, ohne die zugehörigen Metadaten, es sei denn, diese Metadaten wurden als XMP in das Asset eingebettet. Das bedeutet, dass alle beschreibenden Informationen, Kategorisierungen oder Tags, die in AEM gespeichert, aber nicht in die Asset-Dateien integriert sind, nicht im Download enthalten sind. Sie können auch andere Ausgabedarstellungen herunterladen, indem Sie das Skript ändern, um sie einzuschließen. Stellen Sie sicher, dass genügend Speicherplatz zum Speichern der exportierten Assets vorhanden ist.

Das Skript wird normalerweise für AEM Author ausgeführt, kann jedoch auch für AEM Publish ausgeführt werden, solange die AEM Assets-HTTP-API-Endpunkte und Asset-Ausgabedarstellungen über den Dispatcher zugänglich sind.

Bevor Sie das Skript ausführen, müssen Sie es mit Ihrer AEM-Instanz-URL, den Benutzeranmeldeinformationen (Zugriffstoken) und dem Pfad zum zu exportierenden Ordner konfigurieren.

## Skript exportieren

Das als JavaScript-Modul erstellte Skript ist Teil eines Node.js-Projekts, da es eine Abhängigkeit von aufweist. `node-fetch`. Sie können [Projekt als ZIP-Datei herunterladen](./assets/export/export-aem-assets-script.zip)oder kopieren Sie das unten stehende Skript in ein leeres Node.js-Projekt vom Typ . `module`, und ausführen `npm install node-fetch` , um die Abhängigkeit zu installieren.

Dieses Skript führt Sie durch die Ordnerstruktur von AEM Assets und lädt Assets und Ordner in einen lokalen Ordner auf Ihrem Computer herunter. Es verwendet die [AEM Assets-HTTP-API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) um die Ordner- und Asset-Daten abzurufen und die Original-Ausgabedarstellungen der Assets herunterzuladen.

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        // Always allow processing /content/dam 
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }

    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns 
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns the JSON response of the AEM Assets HTTP API
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry
 * @param {Object} options the options for downloading assets
 * @param {String} options.folderUrl the URL of the AEM folder to download
 * @param {String} options.localPath the local path to save the downloaded assets
 * @param {String} options.aemPath the AEM path of the folder to download
 */
async function downloadAssets({apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam'}) {    
    if (!apiUrl) {
        // Handle the initial call to the script, which should just provide the AEM path
        // Construct the proper AEM Assets HTTP API URL as it uses a truncated AEM path
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // Process folders first
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });
    
        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href, 
            localPath: newLocalPath, 
            aemPath: newAemPath
        });
    }

    let downloads = [];

    // Process assets
    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(downloadFile(getLink(asset, 'content')?.href, assetLocalPath));
        }

        // Process in batches of MAX_CONCURRENT_DOWNLOADS
        if (downloads.length >= MAX_CONCURRENT_DOWNLOADS) {
            await Promise.all(downloads);
            downloads = [];
        }
    }

    // Wait for the remaining downloads to finish
    await Promise.all(downloads);
    downloads = [];

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER, 
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## Konfigurieren des Exports

Nachdem das Skript heruntergeladen wurde, aktualisieren Sie die Konfigurationsvariablen unten im Skript.

Die `AEM_ACCESS_TOKEN` können mit den Schritten in der [Token-basierte Authentifizierung für AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview) Tutorial. In vielen Fällen ist das 24-Stunden-Entwickler-Token ausreichend, solange der Export weniger als 24 Stunden dauert und die Person, die das Token generiert, Lesezugriff auf die zu exportierenden Assets hat.

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## Exportieren der Assets

Führen Sie das Skript mit Node.js aus, um die Assets auf Ihren lokalen Computer zu exportieren.

Abhängig von der Anzahl der Assets und ihrer Größe kann es einige Zeit dauern, bis das Skript abgeschlossen ist. Während das Skript ausgeführt wird, [Protokolliert den Fortschritt](#output) in die Konsole.

```shell
$ node export-assets.js
```

## Ausgabe exportieren

Das Exportskript protokolliert den Fortschritt in der Konsole und zeigt die Assets an, die heruntergeladen werden. Nach Abschluss des Skripts werden die Assets in dem in der Konfiguration angegebenen lokalen Ordner gespeichert und das Protokoll wird mit der Gesamtzeit für das Herunterladen der Assets abgeschlossen.

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

Die exportierten Assets befinden sich in dem in der Konfiguration angegebenen lokalen Ordner `LOCAL_DOWNLOAD_FOLDER`. Die Ordnerstruktur spiegelt die AEM Assets-Ordnerstruktur wider, wobei die Assets in die entsprechenden Unterordner heruntergeladen werden. Diese Dateien können hochgeladen werden in [Unterstützte Cloud-Speicheranbieter](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view), für [Massenimport](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/migration/bulk-import) in anderen AEM-Instanzen oder zu Sicherungszwecken.

![Exportierte Assets](./assets/export/exported-assets.png)
