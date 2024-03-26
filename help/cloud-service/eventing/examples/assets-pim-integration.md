---
title: AEM Assets-Ereignisse für die PIM-Integration
description: Erfahren Sie, wie Sie AEM Assets und Systeme für die Produktdatenverwaltung (Product Information Management, PIM) für Asset-Metadaten-Aktualisierungen integrieren.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
exl-id: 070cbe54-2379-448b-bb7d-3756a60b65f0
source-git-commit: 08ad6e3e6db6940f428568c749901b0b3c6ca171
workflow-type: ht
source-wordcount: '1116'
ht-degree: 100%

---

# AEM Assets-Ereignisse für die PIM-Integration

>[!IMPORTANT]
>
>In diesem Tutorial werden experimentelle AEM as a Cloud Service-APIs verwendet. Um Zugriff auf diese APIs zu erhalten, müssen Sie eine Pre-Release-Software-Vereinbarung akzeptieren und diese APIs manuell durch Adobe Engineering für Ihre Umgebung aktivieren lassen. Wenden Sie sich an den Adobe-Support, um einen solchen Zugriff anzufordern.

Erfahren Sie, wie Sie AEM Assets in ein Drittanbietersystem, z. B. ein System für die Produktdatenverwaltung (Product Information Management, PIM) oder für die Produktlinienverwaltung (Product Line Management, PLM), integrieren, um Asset-Metadaten **mithilfe nativer AEM-E/A-Ereignisse** zu aktualisieren. Nach Erhalt eines AEM Assets-Ereignisses können die Asset-Metadaten je nach Geschäftsanforderungen in AEM, im PIM-System oder in beiden Systemen aktualisiert werden. Dieses Beispiel zeigt jedoch, wie die Asset-Metadaten in AEM aktualisiert werden.

>[!VIDEO](https://video.tv.adobe.com/v/3427592?quality=12&learn=on)

Zum Ausführen des Asset-Metadaten-Aktualisierungs-Codes **außerhalb von AEM** wird die Server-lose Plattform [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) verwendet.

Der Ablauf der Ereignisverarbeitung ist wie folgt:

![AEM Assets-Ereignisse für die PIM-Integration](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. Der AEM Author-Service löst ein Ereignis _Asset-Verarbeitung abgeschlossen_ aus, wenn ein Asset-Upload beendet ist sowie alle Asset-Verarbeitungsaktivitäten abgeschlossen sind. Wird auf den Abschluss des Verarbeitungsvorgangs gewartet, stellt dies sicher, dass die standardmäßige Verarbeitung, z. B. die Metadatenextraktion, abgeschlossen ist.
1. Das Ereignis wird an den Dienst [Adobe I/O-Ereignisse](https://developer.adobe.com/events/) gesendet.
1. Der Dienst „Adobe I/O-Ereignisse“ übergibt das Ereignis zur Verarbeitung an die [Adobe I/O Runtime-Aktion](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/).
1. Die Adobe I/O Runtime-Aktion ruft die API des PIM-Systems auf, um zusätzliche Metadaten wie SKU, Lieferanteninformationen oder andere Details abzurufen.
1. Die vom PIM-System abgerufenen zusätzlichen Metadaten werden dann in AEM Assets mithilfe der [Asset Author-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) aktualisiert.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung, in der [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) ist. Außerdem muss das [WKND-Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)-Beispielprojekt darin bereitgestellt sein.

- Zugriff auf [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer-CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), die auf Ihrem lokalen Computer installiert ist.

## Entwicklungsschritte

Die allgemeinen Entwicklungsschritte lauten:

1. [Erstellen eines Projekts in Adobe Developer Console (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Initialisieren des Projekts für die lokale Entwicklung](./runtime-action.md#initialize-project-for-local-development)
1. Konfigurieren des Projekts in ADC
1. Konfigurieren des AEM Author-Service, um die ADC-Projektkommunikation zu aktivieren
1. Entwickeln einer Runtime-Aktion, die das Abrufen und Aktualisieren von Metadaten orchestriert
1. Hochladen eines Assets in den AEM Author-Service und Überprüfen, ob die Metadaten aktualisiert wurden

Weitere Informationen zu den ersten beiden Schritten finden Sie im Beispiel [Adobe I/O Runtime-Aktionen und AEM-Ereignisse](./runtime-action.md#). Näheres zu den Schritten 3 bis 6 erfahren Sie in den folgenden Abschnitten.

### Konfigurieren des Projekts in Adobe Developer Console (ADC)

Um AEM Assets-Ereignisse zu empfangen und die im vorherigen Schritt erstellte Adobe I/O Runtime-Aktion auszuführen, konfigurieren Sie das Projekt in ADC.

- Navigieren Sie in ADC zum [Projekt](https://developer.adobe.com/console/projects). Wählen Sie den `Stage`-Arbeitsbereich. Hier wurde die Runtime-Aktion bereitgestellt.

- Klicken Sie auf die Schaltfläche **Dienst hinzufügen** und wählen Sie die Option **Ereignis** aus. Wählen Sie im Dialogfeld **Ereignisse hinzufügen** **Experience Cloud** > **AEM Assets** und klicken Sie auf **Weiter**. Führen Sie zusätzliche Konfigurationsschritte aus und wählen Sie die AEMCS-Instanz, das Ereignis _Asset-Verarbeitung abgeschlossen_, den OAuth-Server-zu-Server-Authentifizierungstyp und andere Details aus.

  ![AEM Assets-Ereignis – Hinzufügen eines Ereignisses](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Erweitern Sie schließlich im Schritt **Empfangen von Ereignissen** die Option **Runtime-Aktion** und wählen Sie die _generische_ Aktion aus, die im vorherigen Schritt erstellt wurde. Klicken Sie auf **Konfigurierte Ereignisse speichern**.

  ![AEM Assets-Ereignis – Empfangen von Ereignissen](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Klicken Sie wieder auf die Schaltfläche **Dienst hinzufügen** und wählen Sie die Option **API** aus. Wählen Sie im Modal **API hinzufügen** **Experience Cloud** > **AEM as a Cloud Service-API** und klicken Sie auf **Weiter**.

  ![Hinzufügen einer AEM as a Cloud Service-API – Konfigurieren eines Projekts](../assets/examples/assets-pim-integration/add-aem-api.png)

- Wählen Sie anschließend **OAuth-Server-zu-Server** für den Authentifizierungstyp und klicken Sie auf **Weiter**.

- Wählen Sie dann das Produktprofil **AEM-Administratoren – XXX** und klicken Sie auf **Konfigurierte API speichern**. Um das betreffende Asset zu aktualisieren, muss das ausgewählte Produktprofil mit der AEM Assets-Umgebung verknüpft sein, aus der das Ereignis erstellt wird, und über ausreichende Zugriffsrechte verfügen, um Assets dort zu aktualisieren.

  ![Hinzufügen einer AEM as a Cloud Service-API – Konfigurieren eines Projekts](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### Konfigurieren des AEM-Author-Service, um die Kommunikation mit ADC-Projekten zu aktivieren

Um in AEM die Asset-Metadaten des obigen ADC-Projekts zu aktualisieren, konfigurieren Sie den AEM-Author-Service mit der Client-ID des ADC-Projekts. Die _Client-ID_ wird über die [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=de#add-variables)-Benutzeroberfläche als Umgebungsvariable hinzugefügt.

- Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) an und wählen Sie **Programm** > **Umgebung** > **Auslassungspunkte** > **Details anzeigen** > Registerkarte **Konfiguration**.

  ![Adobe Cloud Manager – Umgebungskonfiguration](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Klicken Sie anschließend auf die Schaltfläche **Konfiguration hinzufügen** und geben Sie die Variablendetails ein als

  | Name | Wert | AEM-Service | Typ |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_PROVIDED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Author | Variable |

  ![Adobe Cloud Manager – Umgebungskonfiguration](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Klicken Sie auf **Hinzufügen** und **speichern** Sie die Konfiguration.

### Entwickeln der Runtime-Aktion

Um das Abrufen und Aktualisieren der Metadaten durchzuführen, aktualisieren Sie zunächst den automatisch erstellten _generischen_ Aktions-Code im Ordner `src/dx-excshell-1/actions/generic`.

Im Anhang [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) finden Sie den vollständigen Code und im folgenden Abschnitt werden die Schlüsseldateien hervorgehoben.

- Die Datei `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` ahmt den PIM-API-Aufruf nach, um zusätzliche Metadaten wie Produktnummer und Anbietername abzurufen. Diese Datei wird für Demozwecke verwendet. Sobald der durchgängige Fluss funktioniert, ersetzen Sie diese Funktion durch einen Aufruf an Ihr echtes PIM-System, um Metadaten für das Asset abzurufen.

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- Die Datei `src/dx-excshell-1/actions/generic/aemCommunicator.js` aktualisiert die Asset-Metadaten in AEM mit der [Asset-Author-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  Die Datei `.env` speichert die OAuth-Server-zu-Server-Anmeldeinformationen des ADC-Projekts. Sie werden mithilfe der Datei `ext.config.yaml` als Parameter an die Aktion übergeben. Informationen zum Verwalten von Geheimnissen und Aktionsparametern finden Sie unter [App Builder-Konfigurationsdateien](https://developer.adobe.com/app-builder/docs/guides/configuration/).

- Der Ordner `src/dx-excshell-1/actions/model` enthält die Dateien `aemAssetEvent.js` und `errors.js`, die von der Aktion verwendet werden, um das empfangene Ereignis zu parsen bzw. Fehler zu beheben.

- Die Datei `src/dx-excshell-1/actions/generic/index.js` verwendet die oben genannten Module, um das Abrufen und Aktualisieren der Metadaten zu organisieren.

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

Stellen Sie die aktualisierte Aktion mit dem folgenden Befehl in Adobe I/O Runtime bereit:

```bash
$ aio app deploy
```

### Asset-Upload und Metadatenüberprüfung

Gehen Sie wie folgt vor, um die AEM Assets- und PIM-Integration zu überprüfen:

- Um die von PIM bereitgestellten nachgeahmten Metadaten wie Produktnummer und Anbietername anzuzeigen, müssen Sie ein Metdadatenschema in AEM Assets erstellen. Im [Metadatenschema](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html?lang=de), werden die Metadateneigenschaften von Produktnummer und Anbietername angezeigt.

- Laden Sie ein Asset in den AEM-Author-Service hoch und überprüfen Sie die Metadaten-Aktualisierung.

  ![AEM Assets – Metadaten-Aktualisierung](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Konzept und wichtige Schritte

In Unternehmen ist häufig die Synchronisierung der Asset-Metadaten zwischen AEM und anderen Systemen wie PIM erforderlich. Mit AEM Eventing können solche Anforderungen erfüllt werden.

- Der Abruf-Code für Asset-Metadaten wird außerhalb von AEM ausgeführt, wodurch die Belastung des AEM-Author-Service vermieden wird, was zu einer ereignisgesteuerten Architektur führt, die sich unabhängig skalieren lässt.
- Die neu eingeführte Asset-Author-API wird verwendet, um die Asset-Metadaten in AEM zu aktualisieren.
- Die API-Authentifizierung verwendet OAuth-Server-zu-Server (auch Client-Anmeldedatenfluss genannt), siehe [Implementierungshandbuch für OAuth-Server-zu-Server-Anmeldedaten](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- Anstelle von Adobe I/O Runtime-Aktionen können andere Webhooks oder Amazon EventBridge verwendet werden, um das AEM Assets-Ereignis zu empfangen und die Metadaten-Aktualisierung zu verarbeiten.
- Asset-Ereignisse über AEM Eventing ermöglichen es Unternehmen, kritische Prozesse zu automatisieren und zu optimieren, wodurch Effizienz und Kohärenz im gesamten Inhaltsökosystem gefördert werden.
