---
title: Aufrufen von OpenAPI-basierten AEM-APIs für die Server-zu-Server-Authentifizierung
description: Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs für AEM as a Cloud Service aus benutzerdefinierten Programmen mithilfe der OAuth-Server-zu-Server-Authentifizierung konfigurieren und aufrufen.
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
exl-id: 24c641e7-ab4b-45ee-bbc7-bf6b88b40276
source-git-commit: b3d053a09dfc8989441a21bf0d8c4771d816106f
workflow-type: tm+mt
source-wordcount: '1855'
ht-degree: 10%

---

# Aufrufen von OpenAPI-basierten AEM-APIs für die Server-zu-Server-Authentifizierung{#invoke-openapi-based-aem-apis}

Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs für AEM as a Cloud Service aus benutzerdefinierten Programmen mithilfe der _OAuth-Server-zu-Server_-Authentifizierung konfigurieren und aufrufen.

Die OAuth Server-zu-Server-Authentifizierung ist ideal für Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. Sie verwendet den Grant-Typ OAuth _.0_ client_credentials) für die Authentifizierung des Client-Programms.

>[!AVAILABILITY]
>
>OpenAPI-basierte AEM-APIs sind als Teil eines Early-Access-Programms verfügbar. Wenn Sie daran interessiert sind, darauf zuzugreifen, empfehlen wir Ihnen, eine E-Mail an [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls zu senden.

In diesem Tutorial erfahren Sie, wie Sie:

- Aktivieren Sie den OpenAPI-basierten Zugriff auf AEM-APIs für Ihre AEM as a Cloud Service-Umgebung.
- Erstellen und konfigurieren Sie ein Adobe Developer Console-Projekt (ADC) für den Zugriff auf AEM-APIs mithilfe _OAuth-Server-zu-Server-Authentifizierung_.
- Entwickeln Sie eine NodeJS-Beispielanwendung, die die Assets-Autoren-API aufruft, um Metadaten für ein bestimmtes Asset abzurufen.

Bevor Sie beginnen, lesen Sie den Abschnitt [Zugriff auf Adobe-APIs und zugehörige ](overview.md#accessing-adobe-apis-and-related-concepts)&quot;.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- Modernisierte AEM as a Cloud Service-Umgebung mit folgenden Neuerungen:
   - AEM-Version `2024.10.18459.20241031T210302Z` oder höher.
   - Neue Stil-Produktprofile (wenn die Umgebung vor November 2024 erstellt wurde)

- Das Beispielprojekt [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) muss darin bereitgestellt werden.

- Rufen Sie die [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/) auf.

- Installieren Sie [Node.js](https://nodejs.org/de/) auf Ihrem lokalen Computer, um die NodeJS-Beispielanwendung auszuführen.

## Entwicklungsschritte

Die allgemeinen Entwicklungsschritte lauten:

1. Modernisierung der AEM as a Cloud Service-Umgebung.
1. Aktivieren des Zugriffs auf AEM-APIs.
1. Erstellen Sie ein Adobe Developer Console-Projekt (ADC).
1. ADC-Projekt konfigurieren
   1. Hinzufügen der gewünschten AEM-APIs
   1. Konfigurieren der Authentifizierung
   1. Verknüpfen des Produktprofils mit der Authentifizierungskonfiguration
1. Konfigurieren der AEM-Instanz, um die ADC-Projektkommunikation zu aktivieren
1. Entwickeln einer NodeJS-Beispielanwendung
1. Überprüfen des End-to-End-Flusses

## Modernisierung der AEM as a Cloud Service-Umgebung

Beginnen wir mit der Modernisierung der AEM as a Cloud Service-Umgebung. Dieser Schritt ist nur erforderlich, wenn die Umgebung nicht modernisiert wird.

Die Modernisierung der AEM as a Cloud Service-Umgebung erfolgt in zwei Schritten:

- Aktualisierung auf die neueste AEM-Release-Version
- Fügen Sie neue Produktprofile hinzu.

### AEM-Instanz aktualisieren

Um die AEM-Instanz zu aktualisieren, klicken Sie im Abschnitt _Umgebungen_ von Adobe {0](https://my.cloudmanager.adobe.com/)Cloud Manager auf das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen und wählen Sie die Option **Aktualisieren** aus.[

![AEM-Instanz aktualisieren](assets/update-aem-instance.png)

Klicken Sie dann auf **Senden** und führen Sie die vorgeschlagene Full-Stack-Pipeline aus.

![Wählen Sie die neueste AEM-Release-Version aus](assets/select-latest-aem-release.png)

In meinem Fall lautet der Name der Full-Stack-Pipeline _dev :: fullstack-Deploy_ und der Name der AEM-Umgebung lautet _wknd-program-dev_ Er kann in Ihrem Fall variieren.

### Hinzufügen neuer Produktprofile

Um der AEM-Instanz neue Produktprofile hinzuzufügen, klicken Sie im Abschnitt _Umgebungen_ von Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) auf das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen und wählen Sie die Option **Produktprofile hinzufügen** aus.

![Hinzufügen neuer Produktprofile](assets/add-new-product-profiles.png)

Sie können die neu hinzugefügten Produktprofile überprüfen, indem Sie auf das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen klicken und **Zugriff verwalten** > **Autorenprofile** auswählen.

Im Fenster _Admin Console_ werden die neu hinzugefügten Produktprofile angezeigt.

![Neue Produktprofile überprüfen](assets/review-new-product-profiles.png)

Mit den oben genannten Schritten wird die Modernisierung der AEM as a Cloud Service-Umgebung abgeschlossen.

## Zugriff auf AEM-APIs aktivieren

Neue Produktprofile ermöglichen den OpenAPI-basierten Zugriff auf AEM-APIs in Adobe Developer Console (ADC).

Die neu hinzugefügten Produktprofile sind mit den _Services_ verknüpft, die AEM-Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (ACLs) darstellen. Die _Services_ werden verwendet, um die Zugriffsebene auf die AEM-APIs zu steuern.

Sie können auch die mit dem Produktprofil verknüpften _Services_ auswählen oder die Auswahl aufheben, um die Zugriffsebene zu reduzieren oder zu erhöhen.

Überprüfen Sie die Verknüpfung, indem Sie auf das Symbol _Details anzeigen_ neben dem Namen des Produktprofils klicken.

![Mit dem Produktprofil verknüpfte Services überprüfen](assets/review-services-associated-with-product-profile.png)

Standardmäßig ist der Service **AEM Assets API Users** mit keinem Produktprofil verknüpft. Verknüpfen wir sie mit dem neu hinzugefügten Produktprofil **AEM-Administratoren - Autor - Programm XXX - Umgebung XXX**. Nach dieser Verknüpfung kann die _Asset-Autoren-API_ des ADC-Projekts die OAuth-Server-zu-Server-Authentifizierung einrichten und das Authentifizierungskonto mit dem Produktprofil verknüpfen.

![Verknüpfen des AEM Assets-API-Benutzerdiensts mit dem Produktprofil](assets/associate-aem-assets-api-users-service-with-product-profile.png)

Beachten Sie, dass vor der Modernisierung in der AEM-Autoreninstanz zwei Produktprofile verfügbar waren: **AEM Administrators-XXX** und **AEM Users-XXX**. Es ist auch möglich, diese vorhandenen Produktprofile mit den neuen Services zu verknüpfen.

## Erstellen eines Adobe Developer Console-Projekts (ADC)

Erstellen Sie anschließend ein ADC-Projekt, um auf AEM-APIs zuzugreifen.

1. Melden Sie sich mit Ihrer Adobe ID bei ](https://developer.adobe.com/console) [Adobe Developer Console an.

   ![Adobe-Entwicklerkonsole](assets/adobe-developer-console.png)

1. Klicken Sie _Abschnitt „Schnellstart_ auf die Schaltfläche **Neues Projekt erstellen**.

   ![Neues Projekt erstellen](assets/create-new-project.png)

1. Es wird ein neues Projekt mit dem Standardnamen erstellt.

   ![Neues Projekt erstellt](assets/new-project-created.png)

1. Bearbeiten Sie den Projektnamen, indem Sie auf die **Projekt bearbeiten** oben rechts klicken. Geben Sie einen aussagekräftigen Namen ein und klicken Sie auf **Speichern**.

   ![Projektname bearbeiten](assets/edit-project-name.png)

## ADC-Projekt konfigurieren

Konfigurieren Sie anschließend das ADC-Projekt, um AEM-APIs hinzuzufügen, seine Authentifizierung zu konfigurieren und das Produktprofil zu verknüpfen.

1. Um AEM-APIs hinzuzufügen, klicken Sie auf die Schaltfläche **API hinzufügen**.

   ![API hinzufügen](assets/add-api.png)

1. Filtern Sie _Dialogfeld &quot;_ hinzufügen“ nach _Experience Cloud_ und wählen Sie die Karte **AEM Assets Author API** aus und klicken Sie auf **Weiter**.

   ![AEM-API hinzufügen](assets/add-aem-api.png)

1. Wählen Sie anschließend im Dialogfeld _API konfigurieren_ die Option **Server-zu-Server**-Authentifizierung aus und klicken Sie auf **Weiter**. Die Server-zu-Server-Authentifizierung ist ideal für Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen.

   ![Authentifizierung auswählen](assets/select-authentication.png)

1. Benennen Sie die Berechtigung um (falls erforderlich) und klicken Sie auf **Weiter**. Zu Demozwecken wird der Standardname verwendet.

   ![Berechtigung umbenennen](assets/rename-credential.png)

1. Wählen Sie das Produktprofil **AEM-Administratoren - Autor - Programm XXX - Umgebung XXX** und klicken Sie auf **Speichern**. Wie zu sehen ist, steht nur das mit dem AEM Assets-API-Benutzerdienst verknüpfte Produktprofil zur Auswahl.

   ![Profil auswählen](assets/select-product-profile.png)

   >[!CAUTION]
   >
   >    Beachten Sie, dass der Benutzer des Service-Kontos (auch als technisches Konto bezeichnet) VOLLZUGRIFF erhält, da er mit dem Produktprofil **AEM-Administratoren - XX - XX** verknüpft ist.


1. Überprüfen Sie die AEM-API und die Authentifizierungskonfiguration.

   ![AEM-API-Konfiguration](assets/aem-api-configuration.png)

   ![Authentifizierungskonfiguration](assets/authentication-configuration.png)

## Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation

Um die Client-ID der OAuth-Server-zu-Server-Anmeldeinformationen des ADC-Projekts für die Kommunikation mit der AEM-Instanz zu aktivieren, müssen Sie die AEM-Instanz konfigurieren.

Definieren Sie dazu im AEM-Projekt die Konfiguration in der Datei `config.yaml`. Stellen Sie dann die Datei `config.yaml` mithilfe der Konfigurations-Pipeline in Cloud Manager bereit.

1. Suchen Sie im AEM-Projekt im Ordner `config` nach der Datei `config.yaml` oder erstellen Sie diese.

   ![Suchen nach der Datei „config.yaml“](assets/locate-config-yaml.png)

1. Fügen Sie der Datei `config.yaml` die folgende Konfiguration hinzu:

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

   Ersetzen Sie `<ADC Project's OAuth Server-to-Server credential ClientID>` durch die tatsächliche Client-ID der OAuth Server-zu-Server-Anmeldeinformationen des ADC-Projekts. Der in diesem Tutorial verwendete API-Endpunkt ist nur auf der Autorenebene verfügbar, aber für andere APIs kann die YAML-Konfiguration auch über einen Knoten _publish_ oder _preview_ verfügen.

   >[!CAUTION]
   >
   > Zu Demozwecken wird für alle Umgebungen dieselbe Client-ID verwendet. Es wird empfohlen, für mehr Sicherheit und Kontrolle eine separate Client-ID pro Umgebung (Entwicklung, Staging, Produktion) zu verwenden.

1. Übertragen Sie die Konfigurationsänderungen per Commit an das Git-Repository und per Push an das Remote-Repository.

1. Stellen Sie die oben genannten Änderungen mithilfe der Konfigurations-Pipeline in Cloud Manager bereit. Beachten Sie, dass die Datei `config.yaml` mithilfe von Befehlszeilenprogrammen auch in einer schnellen Entwicklungsumgebung installiert werden kann.

   ![Bereitstellen der Datei „config.yaml“](assets/config-pipeline.png)

## Entwickeln einer NodeJS-Beispielanwendung

Entwickeln wir ein NodeJS-Beispielprogramm, das die Assets Author-API aufruft.

Sie können andere Programmiersprachen wie Java, Python usw. verwenden, um die Anwendung zu entwickeln.

Zu Testzwecken können Sie den [Postman](https://www.postman.com/), [curl](https://curl.se/) oder einen anderen REST-Client verwenden, um die AEM-APIs aufzurufen.

### Überprüfen der API

Bevor wir das Programm entwickeln, sollten wir den Endpunkt [Bereitstellen der Metadaten des angegebenen Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) über die _Assets Author-API_ überprüfen. Die API-Syntax lautet:

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

Um die Metadaten eines bestimmten Assets abzurufen, benötigen Sie die `bucket` und `assetId` Werte. Der `bucket` ist der AEM-Instanzname ohne den Adobe-Domain-Namen (.adobeaemcloud.com), z. B. `author-p63947-e1420428`.

Der `assetId` ist die JCR-UUID des Assets mit dem `urn:aaid:aem:` Präfix, z. B. `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`. Es gibt mehrere Möglichkeiten, die `assetId` zu erhalten:

- Hängen Sie die Erweiterung AEM Asset Path `.json` an, um die Asset-Metadaten abzurufen. `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json` Sie beispielsweise und suchen Sie nach der Eigenschaft `jcr:uuid` .

- Alternativ können Sie die `assetId` abrufen, indem Sie das Asset im Element-Inspektor des Browsers überprüfen. Suchen Sie nach dem Attribut `data-id="urn:aaid:aem:..."` .

  ![Überprüfen von Assets](assets/inspect-asset.png)

### Aufrufen der API über den Browser

Bevor wir das Programm entwickeln, rufen wir die API mithilfe der Funktion &quot;**&quot;** der [API-Dokumentation](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) auf.

1. Öffnen Sie die Dokumentation zur [Assets Author](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author)API im Browser.

1. Erweitern Sie den Abschnitt _Metadaten_ und klicken Sie auf die Option **Übermittelt die Metadaten des angegebenen Assets** .

1. Klicken Sie im rechten Bereich auf die Schaltfläche **Probieren Sie es aus**.
   ![API-Dokumentation](assets/api-documentation.png)

1. Geben Sie die folgenden Werte ein:
   1. Der `bucket` ist der AEM-Instanzname ohne den Adobe-Domain-Namen (.adobeaemcloud.com), z. B. `author-p63947-e1420428`.

   1. Die **Sicherheit** Abschnittsbezogenen `Bearer Token`- und `X-Api-Key` werden aus den OAuth-Server-zu-Server-Anmeldeinformationen des ADC-Projekts abgerufen. Klicken Sie **Zugriffs-Token generieren**, um den `Bearer Token` Wert abzurufen und den `ClientID` Wert als `X-Api-Key` zu verwenden.
      ![Zugriffstoken generieren](assets/generate-access-token.png)

   1. Der `assetId` Wert **Parameter** im Abschnitt ist die eindeutige Kennung für das Asset in AEM. Der `X-Adobe-Accept-Experimental` ist auf 1 gesetzt.

      ![API aufrufen - Eingabewerte](assets/invoke-api-input-values.png)

1. Klicken Sie auf **Senden**, um die API aufzurufen.

1. Überprüfen Sie die **Antwort**, um die API-Antwort anzuzeigen.

   ![API aufrufen - Antwort](assets/invoke-api-response.png)

Die oben genannten Schritte bestätigen die Modernisierung der AEM as a Cloud Service-Umgebung und ermöglichen den Zugriff auf AEM-APIs. Außerdem wird die erfolgreiche Konfiguration des ADC-Projekts und die Kommunikation der OAuth-Server-zu-Server-Anmeldeinformationen mit der ClientID mit der AEM-Autoreninstanz bestätigt.

### Beispielhafte NodeJS-Anwendung

Entwickeln wir ein NodeJS-Beispielprogramm.

Zur Entwicklung der Anwendung können Sie entweder die _Run-the-sample_ application) oder _Step-by-Step-Development_ Anweisungen verwenden.


>[!BEGINTABS]

>[!TAB Run-the-sample-application]

1. Laden Sie die ZIP-Datei [ Beispielanwendung „demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) herunter und extrahieren Sie sie.

1. Navigieren Sie zum extrahierten Ordner und installieren Sie die Abhängigkeiten.

   ```bash
   $ npm install
   ```

1. Ersetzen Sie die Platzhalter in der `.env`-Datei durch die tatsächlichen Werte aus den OAuth Server-zu-Server-Anmeldeinformationen des ADC-Projekts.

1. Ersetzen Sie `<BUCKETNAME>` und `<ASSETID>` in der `src/index.js`-Datei durch die tatsächlichen Werte.

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

1. Installieren Sie die _fetch_ und _dotenv_-Bibliothek, um HTTP-Anfragen durchzuführen bzw. die Umgebungsvariablen zu lesen.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. Öffnen Sie das Projekt in Ihrem bevorzugten Code-Editor und aktualisieren Sie die `package.json`, um die `type` zu `module` hinzuzufügen.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. Erstellen Sie `.env` Datei und fügen Sie die folgende Konfiguration hinzu. Ersetzen Sie die Platzhalter durch die tatsächlichen Werte aus den OAuth Server-zu-Server-Anmeldeinformationen des ADC-Projekts.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. Erstellen Sie `src/index.js` Datei , fügen Sie den folgenden Code hinzu und ersetzen Sie die `<BUCKETNAME>` und `<ASSETID>` durch die tatsächlichen Werte.

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

Nach erfolgreicher Ausführung wird die API-Antwort in der Konsole angezeigt. Die Antwort enthält die Metadaten des angegebenen Assets.

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

Herzlichen Glückwunsch! Sie haben die OpenAPI-basierten AEM-APIs erfolgreich aus Ihrem benutzerdefinierten Programm mithilfe der OAuth-Server-zu-Server-Authentifizierung aufgerufen.

### Überprüfen des Anwendungs-Codes

Die wichtigsten Hinweise aus dem Beispiel-Code der NodeJS-Anwendung sind:

1. **IMS-Authentifizierung**: Ruft ein Zugriffstoken mithilfe der OAuth-Server-zu-Server-Anmeldedaten-Einrichtung im ADC-Projekt ab.

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

1. **API-Aufruf**: Ruft die Assets-Autoren-API auf, um Metadaten für ein bestimmtes Asset abzurufen, indem das Zugriffstoken zur Autorisierung angegeben wird.

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

In diesem Tutorial haben Sie erfahren, wie Sie OpenAPI-basierte AEM-APIs aus benutzerdefinierten Programmen aufrufen. Sie haben APIs für AEM aktiviert, um auf ein Adobe Developer Console-Projekt (ADC) zuzugreifen, es zu erstellen und zu konfigurieren.
Im ADC-Projekt haben Sie die AEM-APIs hinzugefügt, ihren Authentifizierungstyp konfiguriert und das Produktprofil zugeordnet. Sie haben auch die AEM-Instanz konfiguriert, um ADC Project-Kommunikation zu aktivieren, und eine NodeJS-Beispielanwendung entwickelt, die die Assets-Autoren-API aufruft.
