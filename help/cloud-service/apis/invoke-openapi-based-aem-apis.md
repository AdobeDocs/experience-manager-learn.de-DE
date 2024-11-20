---
title: Aufrufen von OpenAPI-basierten AEM-APIs
description: Erfahren Sie, wie Sie über Ihre Anwendung OpenAPI-basierte AEM-APIs aufrufen.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
source-git-commit: 6b8a8dc5cdcddfa2d8572bfd195bc67906882f67
workflow-type: tm+mt
source-wordcount: '1751'
ht-degree: 1%

---


# Aufrufen von OpenAPI-basierten AEM-APIs{#invoke-openapi-based-aem-apis}

Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs in AEM as a Cloud Service aus benutzerdefinierten Anwendungen aufrufen.

>[!AVAILABILITY]
>
>OpenAPI-basierte AEM-APIs sind im Rahmen eines Programms für frühzeitigen Zugriff verfügbar. Wenn Sie daran interessiert sind, darauf zuzugreifen, empfehlen wir Ihnen, [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls per E-Mail zu versenden.

In diesem Tutorial lernen Sie Folgendes:

- Aktivieren Sie den Zugriff auf OpenAPI-basierte AEM-APIs für Ihre AEM as a Cloud Service-Umgebung.
- Erstellen und konfigurieren Sie ein Adobe Developer Console-Projekt (ADC), um mithilfe der OAuth-Server-zu-Server-Authentifizierung auf AEM APIs zuzugreifen.
- Entwickeln Sie eine NodeJS-Beispielanwendung, die die Assets Author-API aufruft, um Metadaten für ein bestimmtes Asset abzurufen.

Bevor Sie beginnen, überprüfen Sie den Abschnitt [Zugriff auf Adobe-APIs und zugehörige Konzepte](overview.md#accessing-adobe-apis-and-related-concepts) .

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Modernisierte AEM as a Cloud Service-Umgebung mit folgenden Funktionen:
   - AEM Release `2024.10.18459.20241031T210302Z` oder höher.
   - Neue Produktprofile im Stil (wenn die Umgebung vor November 2024 erstellt wurde)

- Das Beispielprojekt [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) muss darauf bereitgestellt werden.

- Zugriff auf den [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- Installieren Sie [Node.js](https://nodejs.org/de/) auf Ihrem lokalen Computer, um die Beispiel-NodeJS-Anwendung auszuführen.

## Entwicklungsschritte

Die allgemeinen Entwicklungsschritte lauten:

1. Modernisierung der AEM as a Cloud Service-Umgebung.
1. Aktivieren Sie den Zugriff auf AEM APIs.
1. Erstellen Sie ein Adobe Developer Console-Projekt (ADC).
1. Konfigurieren des ADC-Projekts
   1. Hinzufügen AEM APIs
   1. Authentifizierung konfigurieren
   1. Verknüpfen des Produktprofils mit der Authentifizierungskonfiguration
1. Konfigurieren Sie die AEM-Instanz, um die Kommunikation mit dem ADC-Projekt zu aktivieren
1. Entwickeln einer NodeJS-Beispielanwendung
1. Durchsatz überprüfen

## Modernisierung der AEM as a Cloud Service-Umgebung

Beginnen wir mit der Modernisierung der AEM as a Cloud Service-Umgebung. Dieser Schritt ist nur erforderlich, wenn die Umwelt nicht modernisiert wird.

Die Modernisierung der AEM as a Cloud Service-Umgebung erfolgt in zwei Schritten:

- Aktualisierung auf die neueste AEM Version
- Fügen Sie neue Produktprofile hinzu.

### AEM aktualisieren

Um die AEM-Instanz zu aktualisieren, wählen Sie im Abschnitt &quot;_Umgebungen_&quot;des Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)&quot;das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen aus und wählen Sie die Option **Aktualisieren** aus.

![AEM Instanz aktualisieren](assets/update-aem-instance.png)

Klicken Sie dann auf die Schaltfläche **Senden** und führen Sie die vorgeschlagene Fullstack-Pipeline aus.

![Wählen Sie die neueste AEM Version aus](assets/select-latest-aem-release.png)

In meinem Fall lautet der Name der Fullstack-Pipeline _Dev :: Fullstack-Deploy_ und der Name der AEM Umgebung lautet _wknd-program-dev_. Er kann in Ihrem Fall variieren.

### Neue Produktprofile hinzufügen

Um der AEM neue Produktprofile hinzuzufügen, wählen Sie im Abschnitt _Umgebungen_ des Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) das Symbol mit den Auslassungspunkten _neben dem Umgebungsnamen aus und wählen Sie die Option **Produktprofile hinzufügen**aus._

![Hinzufügen neuer Produktprofile](assets/add-new-product-profiles.png)

Sie können die neu hinzugefügten Produktprofile überprüfen, indem Sie auf das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen klicken und **Zugriff verwalten** > **Autorenprofile** auswählen.

Im Fenster _Admin Console_ werden die neu hinzugefügten Produktprofile angezeigt.

![Neue Produktprofile überprüfen](assets/review-new-product-profiles.png)

Die oben genannten Schritte schließen die Modernisierung der AEM as a Cloud Service-Umgebung ab.

## Zugriff auf AEM APIs aktivieren

Neue Produktprofile ermöglichen den OpenAPI-basierten AEM-API-Zugriff in der Adobe Developer Console (ADC).

Die neu hinzugefügten Produktprofile sind mit den _Services_ verknüpft, die AEM Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (ACLs) darstellen. Die _Dienste_ werden verwendet, um den Grad des Zugriffs auf die AEM-APIs zu steuern.

Sie können auch die mit dem Produktprofil verknüpften _Dienste_ auswählen oder deaktivieren, um den Zugriffsgrad zu reduzieren oder zu erhöhen.

Überprüfen Sie die Zuordnung, indem Sie auf das Symbol _Details anzeigen_ neben dem Produktprofilnamen klicken.

![Überprüfen der mit dem Produktprofil verknüpften Dienste](assets/review-services-associated-with-product-profile.png)

Standardmäßig ist der Dienst **AEM Assets API Users** mit keinem Produktprofil verknüpft. Verknüpfen wir ihn mit dem neu hinzugefügten Produktprofil **AEM Administratoren - Autor - Programm XXX - Umgebung XXX** . Nach dieser Zuordnung kann die _Asset-Autor-API_ des ADC-Projekts die OAuth-Server-zu-Server-Authentifizierung einrichten und das Authentifizierungskonto mit dem Produktprofil verknüpfen.

![Verknüpfen des AEM Assets API-Benutzerdienstes mit dem Produktprofil](assets/associate-aem-assets-api-users-service-with-product-profile.png)

Beachten Sie, dass vor der Modernisierung in AEM Autoreninstanz zwei Produktprofile verfügbar waren: **AEM Administratoren-XXX** und **AEM Benutzer-XXX**. Es ist auch möglich, diese vorhandenen Produktprofile mit den neuen Diensten zu verknüpfen.

## Adobe Developer Console (ADC)-Projekt erstellen

Erstellen Sie anschließend ein ADC-Projekt, um auf AEM APIs zuzugreifen.

1. Melden Sie sich mit Ihrer Adobe ID bei [Adobe Developer Console](https://developer.adobe.com/console) an.

   ![Adobe-Entwicklerkonsole](assets/adobe-developer-console.png)

1. Klicken Sie im Abschnitt _Schnellstart_ auf die Schaltfläche **Neues Projekt erstellen** .

   ![Neues Projekt erstellen](assets/create-new-project.png)

1. Es wird ein neues Projekt mit dem Standardnamen erstellt.

   ![Neues Projekt erstellt](assets/new-project-created.png)

1. Bearbeiten Sie den Projektnamen, indem Sie oben rechts auf die Schaltfläche **Projekt bearbeiten** klicken. Geben Sie einen aussagekräftigen Namen ein und klicken Sie auf **Speichern**.

   ![Projektname bearbeiten](assets/edit-project-name.png)

## Konfigurieren des ADC-Projekts

Konfigurieren Sie anschließend das ADC-Projekt, um AEM APIs hinzuzufügen, die Authentifizierung zu konfigurieren und das Produktprofil zu verknüpfen.

1. Um AEM APIs hinzuzufügen, klicken Sie auf die Schaltfläche **API hinzufügen** .

   ![API hinzufügen](assets/add-api.png)

1. Filtern Sie im Dialogfeld _API hinzufügen_ nach _Experience Cloud_, wählen Sie die Karte **AEM Assets-Autoren-API** aus und klicken Sie auf **Weiter**.

   ![AEM API hinzufügen](assets/add-aem-api.png)

1. Wählen Sie anschließend im Dialogfeld _API konfigurieren_ die Authentifizierungsoption **Server-zu-Server** und klicken Sie auf **Weiter**. Die Server-zu-Server-Authentifizierung eignet sich ideal für Backend-Dienste, die ohne Benutzerinteraktion API-Zugriff benötigen.

   ![Select authentication](assets/select-authentication.png)

1. Benennen Sie die Berechtigung zur leichteren Identifizierung um (falls erforderlich) und klicken Sie auf **Weiter**. Zu Demozwecken wird der Standardname verwendet.

   ![Berechtigung umbenennen](assets/rename-credential.png)

1. Wählen Sie das Produktprofil &quot;**AEM Administratoren - Autor - Programm XXX - Umgebung XXX**&quot;aus und klicken Sie auf &quot;**Speichern**&quot;. Wie Sie sehen, ist nur das Produktprofil verfügbar, das mit dem AEM Assets API-Benutzerdienst verknüpft ist.

   ![Profil auswählen](assets/select-product-profile.png)

1. Überprüfen Sie die AEM-API und die Authentifizierungskonfiguration.

   ![AEM API-Konfiguration](assets/aem-api-configuration.png)

   ![Authentifizierungskonfiguration](assets/authentication-configuration.png)


## AEM Instanz konfigurieren, um die Kommunikation mit ADC-Projekten zu aktivieren

Damit die OAuth Server-zu-Server-Anmeldedaten des ADC-Projekts mit der AEM-Instanz kommunizieren können, müssen Sie die AEM-Instanz konfigurieren.

Dies geschieht durch die Definition der Konfiguration in der Datei `config.yaml` im AEM Projekt. Stellen Sie dann die Datei &quot;`config.yaml`&quot;mithilfe der Config Pipeline in Cloud Manager bereit.

1. Suchen oder erstellen Sie in AEM Projekt die Datei &quot;`config.yaml`&quot; aus dem Ordner &quot;`config`&quot;.

   ![Suchen Sie config YAML](assets/locate-config-yaml.png)

1. Fügen Sie der Datei `config.yaml` die folgende Konfiguration hinzu.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's OAuth Server-to-Server credential ClientID>"
   ```

   Ersetzen Sie `<ADC Project's OAuth Server-to-Server credential ClientID>` durch die tatsächliche ClientID der OAuth Server-zu-Server-Anmeldedaten des ADC-Projekts. Der in diesem Tutorial verwendete API-Endpunkt ist nur auf der Autorenstufe verfügbar. Bei anderen APIs kann die YAML-Konfiguration jedoch auch einen Knoten _publish_ oder _preview_ aufweisen.

1. Übertragen Sie die Konfigurationsänderungen in das Git-Repository und übertragen Sie die Änderungen in das Remote-Repository.

1. Stellen Sie die oben genannten Änderungen mithilfe der Config Pipeline in Cloud Manager bereit. Beachten Sie, dass die Datei `config.yaml` auch in einem RDE installiert werden kann, indem die Befehlszeilenwerkzeuge verwendet werden.

   ![Bereitstellen von config.yaml](assets/config-pipeline.png)

## Entwickeln einer NodeJS-Beispielanwendung

Entwickeln wir eine Beispiel-NodeJS-Anwendung, die die Assets Author-API aufruft.

Sie können andere Programmiersprachen wie Java, Python usw. verwenden, um die Anwendung zu entwickeln.

Zu Testzwecken können Sie den [Postman](https://www.postman.com/), den [curl](https://curl.se/) oder einen anderen REST-Client verwenden, um die AEM-APIs aufzurufen.

### API überprüfen

Bevor Sie die Anwendung entwickeln, überprüfen wir, ob Sie [den Metadaten-Endpunkt des angegebenen Assets bereitstellen](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) von der _Assets-Autoren-API_. Die API-Syntax lautet:

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

Um die Metadaten eines bestimmten Assets abzurufen, benötigen Sie die Werte `bucket` und `assetId`. Der `bucket` ist der Name der AEM Instanz ohne den Adobe-Domänennamen (.adobeaemcloud.com), z. B. `author-p63947-e1420428`.

Die `assetId` ist die JCR-UUID des Assets mit dem Präfix `urn:aaid:aem:`, z. B. `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Es gibt mehrere Möglichkeiten, den `assetId` zu erhalten:

- Hängen Sie die Erweiterung AEM Asset-Pfad `.json` an, um die Asset-Metadaten abzurufen. Beispiel: `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` und suchen Sie nach der Eigenschaft `jcr:uuid` .

- Alternativ können Sie den Wert &quot;`assetId`&quot;abrufen, indem Sie das Asset im Elementinspektor des Browsers überprüfen. Suchen Sie nach dem Attribut `data-id="urn:aaid:aem:..."` .

  ![Inspect-Asset](assets/inspect-asset.png)

### API über den Browser aufrufen

Bevor wir die Anwendung entwickeln, rufen wir die API mithilfe der Funktion **Versuchen Sie es** in der [API-Dokumentation](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) auf.

1. Öffnen Sie die Dokumentation zur Assets-Autoren-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author) im Browser.[

1. Erweitern Sie den Abschnitt _Metadaten_ und klicken Sie auf die Option **Metadaten des angegebenen Assets bereitstellen** .

1. Klicken Sie im rechten Bereich auf die Schaltfläche **Testen** .
   ![API-Dokumentation](assets/api-documentation.png)

1. Geben Sie die folgenden Werte ein:
   1. Der Wert `bucket` ist der Name der AEM Instanz ohne den Adobe-Domänennamen (.adobeaemcloud.com), z. B. `author-p63947-e1420428`.

   1. Die Werte **Sicherheit** im Zusammenhang mit `Bearer Token` und `X-Api-Key` werden aus den OAuth Server-zu-Server-Anmeldedaten des ADC-Projekts abgerufen. Klicken Sie auf **Zugriffstoken generieren** , um den `Bearer Token` -Wert abzurufen und den `ClientID` -Wert als `X-Api-Key` zu verwenden.
      ![Zugriffstoken generieren](assets/generate-access-token.png)

   1. Der **Parameter** -Abschnittswert, der `assetId` zugeordnet ist, ist die eindeutige Kennung für das Asset in AEM. Der `X-Adobe-Accept-Experimental`-Wert ist auf 1 gesetzt.

      ![API aufrufen - Eingabewerte](assets/invoke-api-input-values.png)

1. Klicken Sie auf **Senden** , um die API aufzurufen.

1. Überprüfen Sie die Registerkarte **Antwort** , um die API-Antwort anzuzeigen.

   ![API aufrufen - response](assets/invoke-api-response.png)

Die oben genannten Schritte bestätigen die Modernisierung der AEM as a Cloud Service-Umgebung und ermöglichen den Zugriff auf AEM APIs. Er bestätigt auch die erfolgreiche Konfiguration des ADC-Projekts und die Kommunikation zwischen Server und Server mit der OAuth-Server-Anmeldedaten ClientID mit der AEM Autoreninstanz.

### Beispielhafte NodeJS-Anwendung

Entwickeln wir eine NodeJS-Beispielanwendung.

Um die Anwendung zu entwickeln, können Sie entweder die Anweisungen _Run-the-sample-application_ oder _Schrittweise Entwicklung_ verwenden.


>[!BEGINTABS]

>[!TAB Ausführen der Beispielanwendung]

1. Laden Sie die Beispieldatei [demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) der Anwendung herunter und extrahieren Sie sie.

1. Navigieren Sie zum extrahierten Ordner und installieren Sie die Abhängigkeiten.

   ```bash
   $ npm install
   ```

1. Ersetzen Sie die Platzhalter in der Datei &quot;`.env`&quot;durch die tatsächlichen Werte aus den OAuth Server-zu-Server-Anmeldedaten des ADC-Projekts.

1. Ersetzen Sie die Werte `<BUCKETNAME>` und `<ASSETID>` in der Datei `src/index.js` durch die tatsächlichen Werte.

1. Führen Sie die NodeJS-Anwendung aus.

   ```bash
   $ node src/index.js
   ```

>[!TAB Schrittweise Entwicklung]

1. Erstellen Sie ein neues NodeJS-Projekt.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. Installieren Sie die Bibliothek _fetch_ und _dotenv_ , um HTTP-Anforderungen zu stellen und die Umgebungsvariablen zu lesen.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Öffnen Sie das Projekt in Ihrem bevorzugten Code-Editor und aktualisieren Sie die Datei `package.json` , um die Datei `type` zu `module` hinzuzufügen.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Erstellen Sie die Datei `.env` und fügen Sie die folgende Konfiguration hinzu. Ersetzen Sie die Platzhalter durch die tatsächlichen Werte aus den OAuth Server-zu-Server-Anmeldedaten des ADC-Projekts.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Erstellen Sie die Datei &quot;`src/index.js`&quot;, fügen Sie den folgenden Code hinzu und ersetzen Sie die Werte &quot;`<BUCKETNAME>`&quot; und &quot;`<ASSETID>`&quot; durch die tatsächlichen Werte.

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. Führen Sie die NodeJS-Anwendung aus.

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### API-Antwort

Bei erfolgreicher Ausführung wird die API-Antwort in der Konsole angezeigt. Die Antwort enthält die Metadaten des angegebenen Assets.

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

Herzlichen Glückwunsch! Sie haben die OpenAPI-basierten AEM-APIs mithilfe der OAuth Server-zu-Server-Authentifizierung von Ihrer benutzerdefinierten Anwendung aus erfolgreich aufgerufen.

### Überprüfen des Anwendungs-Codes

Die Schlüssel-Legenden aus dem Beispiel-NodeJS-Anwendungscode sind:

1. **IMS-Authentifizierung**: Ruft ein Zugriffstoken mithilfe der OAuth-Server-zu-Server-Anmeldedaten ab, die im ADC-Projekt eingerichtet wurden.

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **API-Aufruf**: Ruft die Assets-Autoren-API auf, Metadaten für ein bestimmtes Asset abzurufen, indem das Zugriffstoken zur Autorisierung bereitgestellt wird.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie OpenAPI-basierte AEM-APIs aus benutzerdefinierten Anwendungen aufrufen. Sie haben den Zugriff auf AEM APIs aktiviert, ein Adobe Developer Console-Projekt (ADC) erstellt und konfiguriert.
Im ADC-Projekt haben Sie die AEM-APIs hinzugefügt, den Authentifizierungstyp konfiguriert und das Produktprofil zugeordnet. Sie haben auch die AEM-Instanz konfiguriert, um die Kommunikation mit ADC-Projekten zu aktivieren, und eine NodeJS-Beispielanwendung entwickelt, die die Assets-Autoren-API aufruft.

