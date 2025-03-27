---
title: Exportieren von Assets
description: Erfahren Sie, wie Sie Assets in großen Mengen exportieren und auf Ihren lokalen Computer herunterladen können.
feature: Asset Management
version: Experience Manager as a Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2024-04-08T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '517'
ht-degree: 100%

---

# Exportieren von Assets

Erfahren Sie, wie Sie Assets mithilfe eines benutzerdefinierten Node.js-Skripts auf Ihren lokalen Computer exportieren können. Dieses Exportskript bietet ein Beispiel für das programmgesteuerte Herunterladen von Assets von AEM mithilfe von [AEM Assets HTTP-APIs](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), wobei der Schwerpunkt auf den Original-Ausgabedarstellungen liegt, um die höchste Qualität zu gewährleisten. Es wurde entwickelt, um die Ordnerstruktur von AEM-Assets auf Ihrem lokalen Laufwerk zu replizieren, wodurch Assets einfach gesichert oder migriert werden können.

Das Skript lädt nur die Original-Ausgabedarstellungen des Assets ohne zugehörige Metadaten herunter, es sei denn, diese Metadaten wurden als XMP in das Asset eingebettet. Das bedeutet, dass alle beschreibenden Informationen, Kategorien oder Tags, die in AEM gespeichert, aber nicht in die Asset-Dateien integriert sind, nicht im Download enthalten sind. Andere Ausgabedarstellungen können ebenfalls heruntergeladen werden, indem das Skript so geändert wird, dass sie eingeschlossen werden. Stellen Sie sicher, dass Sie genügend Platz zum Speichern der exportierten Assets haben.

Das Skript wird in der Regel mit AEM Author ausgeführt, kann aber auch mit AEM Publish ausgeführt werden, sofern über den Dispatcher auf die AEM Assets-HTTP-API-Endpunkte und Asset-Ausgabedarstellungen zugegriffen werden kann.

Bevor Sie das Skript ausführen, müssen Sie es mit Ihrer AEM-Instanz-URL, den Benutzeranmeldeinformationen (Zugriffs-Token) und dem Pfad zum Ordner konfigurieren, den Sie exportieren möchten.

## Exportieren eines Skripts

Das als JavaScript-Modul geschriebene Skript ist Teil eines Node.js-Projekts, da es eine Abhängigkeit von `node-fetch` hat. Sie können [das Projekt als ZIP-Datei herunterladen](./assets/export/export-aem-assets-script.zip), oder Sie kopieren das unten stehende Skript in ein leeres Node.js-Projekt vom Typ `module` und führen `npm install node-fetch` aus, um die Abhängigkeit zu installieren.

Dieses Skript durchläuft die Ordnerstruktur von AEM Assets und lädt Assets und Ordner in einen lokalen Ordner auf Ihrem Computer herunter. Es verwendet die [AEM Assets-HTTP-API](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), um den Ordner und die Asset-Daten abzurufen, und lädt die Original-Ausgabeformate der Assets herunter.

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

Aktualisieren Sie bei heruntergeladenem Skript die Konfigurationsvariablen am unteren Rand des Skripts.

Das `AEM_ACCESS_TOKEN` können Sie mithilfe der Schritte in der Anleitung [Token-basierte Authentifizierung für AEM as a Cloud Service](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview) erhalten. Häufig ist das 24-Stunden-Entwickler-Token ausreichend, solange der Export weniger als 24 Stunden dauert und die Person, die das Token generiert, Lesezugriff auf die Assets hat, die exportiert werden sollen.

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

Je nach Anzahl der Assets und ihrer Größe kann es einige Zeit dauern, bis das Skript abgeschlossen ist. Wenn das Skript ausgeführt wird, [protokolliert es den Fortschritt](#output) in der Konsole.

```shell
$ node export-assets.js
```

## Exportieren der Ausgabe

Das Exportskript protokolliert den Fortschritt in der Konsole und gibt die Assets an, die heruntergeladen werden. Wenn das Skript abgeschlossen ist, werden die Assets in dem in der Konfiguration angegebenen lokalen Ordner gespeichert und das Protokoll schließt mit der Gesamtzeit, die zum Herunterladen der Assets benötigt wurde.

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

Die exportierten Assets befinden sich in dem in der Konfiguration angegebenen lokalen Ordner `LOCAL_DOWNLOAD_FOLDER`. Die Ordnerstruktur spiegelt die AEM-Assets-Ordnerstruktur wider, wobei die Assets in die entsprechenden Unterordner heruntergeladen werden. Diese Dateien können zu [unterstützten Cloud-Speicheranbietern](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view) zum [Massenimport](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/migration/bulk-import) in andere AEM-Instanzen oder zu Sicherungszwecken hochgeladen werden.

![Exportierte Assets](./assets/export/exported-assets.png)
