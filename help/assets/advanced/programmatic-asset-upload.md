---
title: Programmgesteuerter Asset-Upload in AEM as a Cloud Service
description: Erfahren Sie, wie Sie Assets mithilfe der @adobe/aem-upload Node.js-Bibliothek in AEM as a Cloud Service hochladen.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 2%

---


# Programmgesteuerter Asset-Upload in AEM as a Cloud Service

Erfahren Sie, wie Sie Assets mithilfe des Client-Programms, das die Bibliothek [aem-upload](https://github.com/adobe/aem-upload) Node.js verwendet, in die AEM as a Cloud Service-Umgebung hochladen.


>[!VIDEO](https://video.tv.adobe.com/v/3476961?captions=ger&quality=12&learn=on)


## Lerninhalt

In diesem Tutorial erfahren Sie mehr über:

+ Verwendung des Ansatzes _direkter binärer Upload_ zum Hochladen von Assets in die AEM as a Cloud Service-Umgebung (RDE, Dev, Staging, Prod) mithilfe der [aem-upload](https://github.com/adobe/aem-upload) Node.js-Bibliothek.
+ Konfigurieren und Ausführen der Anwendung &quot;[-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) zum Hochladen von Assets in die AEM as a Cloud Service-Umgebung.
+ Überprüfen Sie den Beispiel-Anwendungs-Code und verstehen Sie die Implementierungsdetails.
+ Machen Sie sich mit den Best Practices für den programmgesteuerten Asset-Upload in die AEM as a Cloud Service-Umgebung vertraut.

## Grundlegendes zum Ansatz _direkter binärer Upload_

Mit dem _direkten binären Upload_-Ansatz können Sie Dateien aus Ihrem Quellsystem _direkt in den Cloud-Speicher)_ AEM as a Cloud Service-Umgebung mithilfe einer _vordefinierten URL_ hochladen. Binärdaten müssen nicht mehr durch die Java-Prozesse von AEM geroutet werden, was zu schnelleren Uploads und geringerer Serverlast führt.

Bevor wir die Beispielanwendung ausführen, sollten wir den direkten binären Upload-Fluss verstehen.

Im direkten binären Upload-Fluss werden die Binärdaten mit vordefinierten URLs direkt in den Cloud-Speicher hochgeladen. Die AEM as a Cloud Service ist für eine schlanke Verarbeitung verantwortlich, z. B. für das Generieren der vordefinierten URLs und die Benachrichtigung des AEM Asset Compute-Service über den Abschluss des Uploads. Das folgende logische Flussdiagramm veranschaulicht den Fluss des direkten binären Uploads.

![Fluss des direkten binären Uploads](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### Die aem-upload-Bibliothek

Die Bibliothek [aem-upload](https://github.com/adobe/aem-upload) Node.js abstrahiert die Implementierungsdetails des Ansatzes _direkter binärer Upload_. Es stehen zwei Klassen zur Orchestrierung des Upload-Prozesses zur Verfügung:

+ **FileSystemUpload** - Wird beim Hochladen von Dateien aus dem lokalen Dateisystem verwendet, einschließlich Unterstützung für Verzeichnisstrukturen
+ **DirectBinaryUpload** - Damit können Sie den binären Upload-Prozess genauer steuern, z. B. das Hochladen aus Streams oder Puffern

>[!CAUTION]
>
>Es gibt KEINE Entsprechung zur Bibliothek [aem-upload](https://github.com/adobe/aem-upload) in Java. Die Client-Anwendung muss in Node.js geschrieben sein, damit der Ansatz des _direkten binären Uploads_ verwendet werden kann. Weitere Informationen finden Sie auf der Seite [Experience Manager Assets-APIs und -Vorgänge](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis) .

## Beispielanwendung

Verwenden Sie das Programm [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip), um mehr über den programmgesteuerten Upload-Prozess für Assets zu erfahren. Die Beispielanwendung zeigt die Verwendung von `FileSystemUpload` und `DirectBinaryUpload` Klassen aus der Bibliothek [aem-upload](https://github.com/adobe/aem-upload).

### Voraussetzungen

Bevor Sie die Beispielanwendung ausführen, stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllen:

+ AEM as a Cloud Service-Autorenumgebung, z. B. schnelle Entwicklungsumgebung (RDE), Entwicklungsumgebung usw.
+ Node.js (neueste LTS-Version)
+ Grundlegendes zu Node.js und npm

>[!CAUTION]
>
> Sie können AEM as a Cloud Service SDK (auch als lokale AEM-Instanz bezeichnet) NICHT zum Testen des programmgesteuerten Asset-Upload-Prozesses verwenden. Sie müssen eine AEM as a Cloud Service-Umgebung wie die schnelle Entwicklungsumgebung (RDE), die Entwicklungsumgebung usw. verwenden.

### Beispielanwendung herunterladen

1. Laden Sie die ZIP-Datei [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) des Programms herunter und extrahieren Sie sie.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Öffnen Sie den extrahierten Ordner in Ihrem bevorzugten Code-Editor.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. Installieren Sie mithilfe des Code-Editor-Terminals die Abhängigkeiten.

   ```bash
   $ npm install
   ```

   ![Beispielanwendung](./assets/programmatic-asset-upload/install-dependencies.png)

### Beispielanwendung konfigurieren

Bevor Sie die Beispielanwendung ausführen, müssen Sie sie mit den erforderlichen AEM as a Cloud Service-Umgebungsdetails wie AEM-Autoren-URL, _Authentifizierungsmethode_ und Asset-Ordnerpfad konfigurieren.

Es _(mehrere Authentifizierungsmethoden),_ von der Bibliothek [aem-upload](https://github.com/adobe/aem-upload) Node.js unterstützt werden. In der folgenden Tabelle sind die unterstützten _Authentifizierungsmethoden_ und ihr Zweck aufgeführt.

| | Standardauthentifizierung | [Lokales Entwicklungstoken](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [Service-Anmeldeinformationen](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [OAuth-Web-App](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [OAuth SPA](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| Wird unterstützt? | &check; | &check; | &check; | &cross; | &cross; | &cross; |
| Zweck | Lokale Entwicklung | Lokale Entwicklung | Produktion | Nicht zutreffend | Nicht zutreffend | Nicht zutreffend |

Gehen Sie wie folgt vor, um die Beispielanwendung zu konfigurieren:

1. Kopieren Sie die `env.example` in `.env` Datei .

   ```bash
   $ cp env.example .env
   ```

1. Öffnen Sie die `.env` und aktualisieren Sie die `AEM_URL` Umgebungsvariable mit der AEM as a Cloud Service-Autoren-URL.

1. Wählen Sie die Authentifizierungsmethode aus den folgenden Optionen aus und aktualisieren Sie die entsprechenden Umgebungsvariablen.

>[!BEGINTABS]

>[!TAB Standardauthentifizierung]

Um die Standardauthentifizierung zu verwenden, müssen Sie einen Benutzer in der AEM as a Cloud Service-Umgebung erstellen.

1. Melden Sie sich bei Ihrer AEM as a Cloud Service-Umgebung an.

1. Navigieren Sie zu **Tools** > **Sicherheit** > **Benutzer** und klicken Sie auf die Schaltfläche **Erstellen**.

   ![Benutzer erstellen](./assets/programmatic-asset-upload/create-user.png)

1. Benutzerdetails eingeben

   ![Benutzerdetails](./assets/programmatic-asset-upload/user-details.png)

1. Fügen Sie auf **Registerkarte** die Gruppe **DAM-Benutzer** hinzu. Klicken Sie auf **Schaltfläche „Speichern und schließen**.

   ![DAM-Benutzergruppe hinzufügen](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. Aktualisieren Sie die Umgebungsvariablen `AEM_USERNAME` und `AEM_PASSWORD` mit dem Benutzernamen und Kennwort des erstellten Benutzers.

>[!TAB Lokales Entwicklungstoken]

Um das lokale Entwicklungs-Token abzurufen, müssen Sie die **AEM**-Developer Console verwenden. Das generierte Token ist vom Typ JSON Web Token (JWT).

1. Melden Sie sich bei [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) an und gehen Sie zur gewünschten Detailseite **Umgebung**. Klicken Sie auf die **&quot;…“** und wählen Sie **Developer Console**.

   ![Entwicklerkonsole](./assets/programmatic-asset-upload/developer-console.png)

1. Melden Sie sich bei AEM Developer Console an und verwenden Sie die Schaltfläche _Neue Konsole_, um zur neueren Konsole zu wechseln.

1. Wählen Sie im Abschnitt **Tools** die Option **Integrationen** und klicken Sie auf die Schaltfläche **Lokales Token**.

   ![Lokales Token abrufen](./assets/programmatic-asset-upload/get-local-token.png)

1. Kopieren Sie den Token-Wert und aktualisieren Sie die `AEM_BEARER_TOKEN` Umgebungsvariable mit dem Token-Wert.

Beachten Sie, dass das lokale Entwicklungs-Token 24 Stunden lang gültig ist und für den Benutzer ausgestellt wird, der das Token generiert hat.

>[!TAB Service-Anmeldeinformationen]

Um die Dienstanmeldeinformationen abzurufen, müssen Sie die **AEM**-Developer Console verwenden. Sie wird verwendet, um das Token vom Typ JSON Web Token (JWT) mit dem npm-Modul [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) zu generieren.

1. Melden Sie sich bei [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) an und gehen Sie zur gewünschten Detailseite **Umgebung**. Klicken Sie auf die **&quot;…“** und wählen Sie **Developer Console**.

   ![Entwicklerkonsole](./assets/programmatic-asset-upload/developer-console.png)

1. Melden Sie sich bei AEM Developer Console an und verwenden Sie die Schaltfläche _Neue Konsole_, um zur neueren Konsole zu wechseln.

1. Wählen Sie im Abschnitt **Tools** die Option **Integrationen** und klicken Sie auf die Schaltfläche **Neues technisches Konto erstellen**.

   ![Service-Anmeldeinformationen abrufen](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Klicken Sie auf **Option „Anzeigen**, um die JSON-Dienstanmeldeinformationen zu kopieren.

   ![Service-Anmeldeinformationen](./assets/programmatic-asset-upload/service-credentials.png)

1. Erstellen Sie eine `service-credentials.json` Datei im Stammverzeichnis der Beispielanwendung und fügen Sie die JSON-Datei mit den Dienstanmeldeinformationen in die Datei ein.

1. Aktualisieren Sie die Umgebungsvariable `AEM_SERVICE_CREDENTIALS_FILE` mit dem Pfad zur Datei service-credentials.json.

1. Stellen Sie sicher, dass der Benutzer mit den Service-Anmeldeinformationen über die erforderlichen Berechtigungen zum Hochladen von Assets in die AEM as a Cloud Service-Umgebung verfügt. Weitere Informationen finden Sie auf [&#x200B; Seite „Zugriff in AEM &#x200B;](https://experienceleague.adobe.com/de/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem).

>[!ENDTABS]

Im Folgenden finden Sie eine vollständige `.env` mit allen drei konfigurierten Authentifizierungsmethoden.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### Ausführen der Beispielanwendung

Das Beispielprogramm zeigt drei verschiedene Möglichkeiten, Beispiel-Assets in eine AEM as a Cloud Service-Umgebung hochzuladen.

1. **FileSystemUpload** - Hochladen von Dateien aus einem lokalen Dateisystem mit Verzeichnisstrukturunterstützung und automatischer Ordnererstellung
1. **DirectBinaryUpload** - Lädt eine [Remote-Datei](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets) hoch. Die Binärdatei wird vor dem Hochladen in die AEM as a Cloud Service-Umgebung im Speicher gepuffert.
1. **Batch-Upload** - Lädt mehrere Dateien aus einem lokalen Dateisystem in Batches mit automatischer Wiederholungslogik und Fehlerbehebung hoch. Im Hintergrund verwendet sie die `FileSystemUpload`-Klasse zum Hochladen von Dateien aus dem lokalen Dateisystem.

Zu ladende Assets befinden sich im Ordner `sample-assets` und enthalten `img`, `video` und `doc` Unterordner, die jeweils einige Beispiel-Assets enthalten.

1. Um die Beispielanwendung auszuführen, verwenden Sie den folgenden Befehl:

```bash
$ npm start
```

1. Geben Sie die gewünschte Option _Zahl_ aus den folgenden Auswahlmöglichkeiten ein:

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

Auf den folgenden Registerkarten werden die Ausführung der Beispielanwendung, ihre Ausgabe und hochgeladene Assets in der AEM as a Cloud Service-Umgebung für jede Upload-Methode angezeigt.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

1. Die Beispielanwendungsausgabe für `FileSystemUpload` Option:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets mit `FileSystemUpload` Option in AEM as a Cloud Service-Umgebung hochgeladen:

   ![Hochgeladene Assets in der AEM as a Cloud Service-Umgebung mit der FileSystemUpload-Klasse](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. Die Beispielanwendungsausgabe für `DirectBinaryUpload` Option:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets mit `DirectBinaryUpload` Option in AEM as a Cloud Service-Umgebung hochgeladen:

![Hochgeladene Assets in der AEM as a Cloud Service-Umgebung mit der DirectBinaryUpload-Klasse](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB Batch-Upload]

1. Die Beispielanwendungsausgabe für `Batch Upload` Option:

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets mit `Batch Upload` Option in AEM as a Cloud Service-Umgebung hochgeladen:

![Hochgeladene Assets in der AEM as a Cloud Service-Umgebung mit der BatchUpload-Klasse](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## Überprüfen des Beispiel-Anwendungs-Codes

Der Haupteinstiegspunkt der Beispielanwendung ist die `index.js`. Sie enthält die `promptUser`, die den Benutzer zur Auswahl auffordert und das ausgewählte Beispiel ausführt.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Den vollständigen Code finden Sie in der `index.js` aus der Beispielanwendung.

Auf den folgenden Registerkarten werden die Implementierungsdetails der einzelnen Upload-Methoden angezeigt.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

Die `FileSystemUpload`-Klasse wird verwendet, um Dateien aus dem lokalen Dateisystem mit Unterstützung der Verzeichnisstruktur und automatischer Ordnererstellung hochzuladen.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Den vollständigen Code finden Sie in der `examples/filesystem-upload.js` aus der Beispielanwendung.

>[!TAB DirectBinaryUpload]

Die `DirectBinaryUpload`-Klasse wird zum Hochladen einer Remote-Datei in die AEM as a Cloud Service-Umgebung verwendet.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Den vollständigen Code finden Sie in der `examples/direct-binary-upload.js` aus der Beispielanwendung.

>[!TAB Batch-Upload]

Die Dateien werden in Batches aufgeteilt und mit automatischer Wiederholungslogik und Fehlerbehebung in Batches hochgeladen. Im Hintergrund verwendet sie die `FileSystemUpload`-Klasse zum Hochladen von Dateien aus dem lokalen Dateisystem.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Den vollständigen Code finden Sie in der `examples/batch-upload.js` aus der Beispielanwendung.

>[!ENDTABS]

Außerdem enthält die `README.md` aus der Beispielanwendung die ausführliche Dokumentation für die Beispielanwendung.

## Best Practices

1. **Wählen Sie die richtige Authentifizierungsmethode aus:**
Verwenden Sie nur Service-Anmeldeinformationen für Produktionsumgebungen, ein lokales Entwicklungstoken und die Standardauthentifizierung für Entwicklung/Tests. Stellen Sie sicher, dass der Benutzer mit den Service-Anmeldeinformationen über die erforderlichen Berechtigungen zum Hochladen von Assets in die AEM as a Cloud Service-Umgebung verfügt.

1. **Wählen Sie die richtige Upload-Methode aus:**
Verwenden Sie FileSystemUpload für lokale Dateien mit automatischer Ordnererstellung, DirectBinaryUpload für Streams/Puffer/Remote-URLs mit feinabgestimmter Steuerung und Batch-Upload-Muster für Produktionsumgebungen mit mehr als 1000 Dateien, die eine Wiederholungslogik erfordern.

1. **DirectBinaryUpload-Dateiobjekte korrekt strukturieren**
Verwenden Sie die Blob-Eigenschaft (nicht den Puffer) mit den erforderlichen Feldern: { fileName, fileSize, blob: buffer, targetFolder } und denken Sie daran, dass DirectBinaryUpload Ordner NICHT automatisch erstellt.

1. **Beispielanwendung als Referenz:**
Die Beispielanwendung ist eine gute Referenz für die Implementierungsdetails des programmgesteuerten Asset-Upload-Prozesses. Sie können sie als Ausgangspunkt für Ihre eigene Implementierung verwenden.
