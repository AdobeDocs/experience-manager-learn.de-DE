---
title: AEM Assets-Ereignisse für die PIM-Integration
description: Erfahren Sie, wie Sie AEM Assets und Systeme für die Produktdatenverwaltung (Product Information Management, PIM) für Asset-Metadaten-Aktualisierungen integrieren.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 761
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
exl-id: 070cbe54-2379-448b-bb7d-3756a60b65f0
source-git-commit: 99aa43460a76460175123a5bfe5138767491252b
workflow-type: tm+mt
source-wordcount: '1489'
ht-degree: 31%

---

# AEM Assets-Ereignisse für die PIM-Integration

>[!IMPORTANT]
>
>In diesem Tutorial werden OpenAPI-basierte AEM-APIs verwendet. Sie sind im Rahmen eines Early-Access-Programms verfügbar. Wenn Sie daran interessiert sind, auf sie zuzugreifen, empfehlen wir Ihnen, eine E-Mail an [aem-apis@adobe.com](mailto:aem-apis@adobe.com) mit einer Beschreibung Ihres Anwendungsfalls zu senden.

Erfahren Sie, wie Sie ein AEM-Ereignis empfangen und darauf reagieren können, um den Inhaltsstatus in AEM mithilfe der OpenAPI-basierten Assets Author-API zu aktualisieren.

Wie das empfangene Ereignis verarbeitet wird, hängt von den Geschäftsanforderungen ab. Beispielsweise können die Ereignisdaten verwendet werden, um das Drittanbietersystem, die AEM oder beides zu aktualisieren.

Dieses Beispiel zeigt, wie ein Drittanbietersystem, z. B. ein PIM-System (Product Information Management), in AEM as a Cloud Service Assets integriert werden kann. Nach Erhalt eines AEM Assets-Ereignisses wird es verarbeitet, um zusätzliche Metadaten aus dem PIM-System abzurufen und die Asset-Metadaten in AEM zu aktualisieren. Die aktualisierten Asset-Metadaten können zusätzliche Informationen wie SKU, Lieferantenname oder andere Produktdetails enthalten.

Zum Empfang und zur Verarbeitung des AEM Assets-Ereignisses wird die [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)-Plattform ohne Server verwendet. Es können jedoch auch andere Ereignisverarbeitungssysteme wie Webhook in Ihrem Drittanbietersystem oder Amazon EventBridge verwendet werden.

Die Integration verläuft auf hoher Ebene wie folgt:

![AEM Assets-Ereignisse für die PIM-Integration](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. Der AEM-Autoren-Service Trigger ein _Asset-Verarbeitung abgeschlossen_, wenn ein Asset-Upload abgeschlossen ist und alle Asset-Verarbeitungsaktivitäten ebenfalls abgeschlossen sind. Wenn Sie auf den Abschluss der Asset-Verarbeitung warten, wird sichergestellt, dass alle vordefinierten Verarbeitungsschritte, z. B. die Extraktion von Metadaten, abgeschlossen wurden.
1. Das Ereignis wird an den Dienst [Adobe I/O-Ereignisse](https://developer.adobe.com/events/) gesendet.
1. Der Dienst „Adobe I/O-Ereignisse“ übergibt das Ereignis zur Verarbeitung an die [Adobe I/O Runtime-Aktion](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/).
1. Die Adobe I/O Runtime-Aktion ruft die API des PIM-Systems auf, um zusätzliche Metadaten wie SKU, Lieferanteninformationen oder andere Details abzurufen.
1. Die zusätzlichen Metadaten, die vom PIM abgerufen werden, werden dann in AEM Assets mithilfe der OpenAPI-basierten [Assets Author API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) aktualisiert.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung, in der [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) ist. Außerdem muss das [WKND-Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)-Beispielprojekt darin bereitgestellt sein.

- Zugriff auf die [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer-CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), die auf Ihrem lokalen Computer installiert ist.

## Entwicklungsschritte

Die allgemeinen Entwicklungsschritte lauten:

1. [Modernisierung der AEM as a Cloud Service-Umgebung](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis#modernization-of-aem-as-a-cloud-service-environment)
1. [AEM-API-Zugriff aktivieren](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis#enable-aem-apis-access)
1. [Erstellen eines Projekts in Adobe Developer Console (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Initialisieren des Projekts für die lokale Entwicklung](./runtime-action.md#initialize-project-for-local-development)
1. Konfigurieren des Projekts in ADC
1. Konfigurieren des AEM Author-Service, um die ADC-Projektkommunikation zu aktivieren
1. Entwickeln einer Laufzeitaktion zur Orchestrierung
   1. Metadatenabruf aus dem PIM-System
   1. Aktualisierung von Metadaten in AEM Assets mithilfe der Assets-Autoren-API
1. Erstellen und Anwenden eines Asset-Metadatenschemas
1. Verifizierung des Asset-Uploads und der Metadatenaktualisierung

Weitere Informationen zu den Schritten 1-2 finden Sie im Handbuch [OpenAPI-basierte AEM-APIs aufrufen](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) und in den Schritten 3-4 finden Sie das Beispiel für [Adobe I/O Runtime-Aktion und -AEM](./runtime-action.md#)Ereignisse. Die Schritte 5 bis 9 finden Sie in den folgenden Abschnitten.

### Konfigurieren des Projekts in Adobe Developer Console (ADC)

Um AEM Assets-Ereignisse zu empfangen und die im vorherigen Schritt erstellte Adobe I/O Runtime-Aktion auszuführen, konfigurieren Sie das Projekt in ADC.

- Navigieren Sie in ADC zu dem [Projekt](https://developer.adobe.com/console/projects) das Sie in Schritt-3 erstellt haben. Wählen Sie aus diesem Projekt den Arbeitsbereich `Stage` aus, in dem die Laufzeitaktion bereitgestellt wird, wenn Sie `aio app deploy` als Teil der Schritt-4-Anweisungen ausführen.

- Klicken Sie auf die Schaltfläche **Dienst hinzufügen** und wählen Sie die Option **Ereignis** aus. Wählen **Dialogfeld „Ereignisse**&quot; die Option **Experience Cloud** > **AEM Assets** und klicken Sie auf **Weiter**.
  ![AEM Assets-Ereignis – Hinzufügen eines Ereignisses](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Wählen **Schritt „Ereignisregistrierung konfigurieren** die gewünschte AEMCS-Instanz, dann das Ereignis _Asset-Verarbeitung abgeschlossen_ und den Authentifizierungstyp OAuth Server-zu-Server .

  ![AEM Assets-Ereignis - Ereignis konfigurieren](../assets/examples/assets-pim-integration/configure-aem-assets-event.png)

- Erweitern Sie abschließend im Schritt **So erhalten Sie** Ereignisse“ die Option **Laufzeitaktion** und wählen Sie die im vorherigen Schritt erstellte _generische_ Aktion aus. Klicken Sie auf **Konfigurierte Ereignisse speichern**.

  ![AEM Assets-Ereignis – Empfangen von Ereignissen](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Klicken Sie wieder auf die Schaltfläche **Dienst hinzufügen** und wählen Sie die Option **API** aus. Wählen Sie im **API hinzufügen**-Modal **Experience Cloud** > **AEM Assets Author API** und klicken Sie auf **Weiter**.

  ![AEM Assets Author-API hinzufügen - Projekt konfigurieren](../assets/examples/assets-pim-integration/add-aem-api.png)

- Wählen Sie anschließend **OAuth-Server-zu-Server** für den Authentifizierungstyp und klicken Sie auf **Weiter**.

- Wählen Sie dann das richtige **Produktprofil** aus, das mit der AEM Assets-Umgebung verknüpft ist, aus der das Ereignis erzeugt wird, und verfügen Sie über genügend Zugriffsrechte, um dort Assets zu aktualisieren. Klicken Sie abschließend auf die Schaltfläche **Konfigurierte API speichern**.

  ![AEM Assets Author-API hinzufügen - Projekt konfigurieren](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

  In meinem Fall ist das Produktprofil _AEM-Administratoren - Autor - Programm XXX - Umgebung JJJJ_ ausgewählt. Es ist der Service **AEM Assets API-Benutzer** aktiviert.

  ![AEM Assets Author-API hinzufügen - Beispiel-Produktprofil](../assets/examples/assets-pim-integration/aem-api-product-profile.png)

## Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation

Um die ClientID der OAuth-Server-zu-Server-Anmeldedaten des ADC-Projekts für die Kommunikation mit der AEM-Instanz zu aktivieren, müssen Sie die AEM-Instanz konfigurieren.

Definieren Sie dazu die Konfiguration in der `config.yaml` im AEM-Projekt. Stellen Sie dann die `config.yaml` mithilfe der Konfigurations-Pipeline in der Cloud Manager bereit.

- Suchen oder erstellen Sie im AEM-Projekt die `config.yaml`-Datei aus dem `config`.

  ![Finden Sie die Konfiguration YAML](../assets/examples/assets-pim-integration/locate-config-yaml.png)

- Fügen Sie der `config.yaml`-Datei die folgende Konfiguration hinzu.

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

  Ersetzen Sie `<ADC Project's OAuth Server-to-Server credential ClientID>` durch die tatsächliche Client-ID der OAuth Server-zu-Server-Anmeldeinformationen des ADC-Projekts.

- Übertragen Sie die Konfigurationsänderungen in das Git-Repository und übertragen Sie die Änderungen in das Remote-Repository.

- Stellen Sie die oben genannten Änderungen mithilfe der Konfigurations-Pipeline in der Cloud Manager bereit. Beachten Sie, dass die `config.yaml`-Datei auch mithilfe von Befehlszeilen-Tools in einer RDE installiert werden kann.

  ![Bereitstellen von config.yaml](../assets/examples/assets-pim-integration/config-pipeline.png)

### Entwickeln der Runtime-Aktion

Um das Abrufen und Aktualisieren der Metadaten durchzuführen, aktualisieren Sie zunächst den automatisch erstellten _generischen_ Aktions-Code im Ordner `src/dx-excshell-1/actions/generic`.

Den vollständigen Code finden Sie in [ angehängten Datei „WKND-Assets-PIM-Integration](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip)zip“. Im folgenden Abschnitt werden die Schlüsseldateien hervorgehoben.

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

  Die `.env`-Datei speichert die Details der OAuth Server-zu-Server-Anmeldeinformationen des ADC-Projekts, und sie werden mithilfe `ext.config.yaml` -Datei als Parameter an die Aktion übergeben. Informationen zum Verwalten von Geheimnissen und Aktionsparametern finden Sie unter [App Builder-Konfigurationsdateien](https://developer.adobe.com/app-builder/docs/guides/configuration/).
- Der Ordner `src/dx-excshell-1/actions/model` enthält die Dateien `aemAssetEvent.js` und `errors.js`, die von der Aktion verwendet werden, um das empfangene Ereignis zu parsen bzw. Fehler zu beheben.
- Die `src/dx-excshell-1/actions/generic/index.js`-Datei verwendet die oben genannten Module, um das Abrufen und Aktualisieren von Metadaten zu orchestrieren.

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

- Stellen Sie die aktualisierte Aktion mit dem folgenden Befehl in Adobe I/O Runtime bereit:

  ```bash
  $ aio app deploy
  ```

### Erstellen und Anwenden eines Asset-Metadatenschemas

Standardmäßig verfügt das WKND Sites-Projekt nicht über das Asset-Metadatenschema zum Anzeigen der PIM-spezifischen Metadaten wie SKU, Lieferantenname usw. Erstellen wir nun das Asset-Metadatenschema und wenden es auf einen Asset-Ordner in der AEM-Instanz an.

1. Melden Sie sich bei der AEM as a Cloud Service Asset-Instanz an und befinden Sie sich in der [Asset-](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/authoring/switch-views).

   ![AEM Assets-Ansicht](../assets/examples/assets-pim-integration/aem-assets-view.png)

1. Navigieren Sie in der **Leiste zur Option** Einstellungen **> Metadaten-Forms** und klicken Sie auf die Schaltfläche **Erstellen**. Geben **im Dialogfeld „Metadatenformular erstellen** die folgenden Details ein und klicken Sie auf **Erstellen**.
   - Name: `PIM`
   - Vorhandene Formularstruktur als Vorlage verwenden: `Check`
   - Wählen Sie aus: `default`

   ![Erstellen eines Metadatenformulars](../assets/examples/assets-pim-integration/create-metadata-form.png)

1. Klicken Sie auf das Symbol **+** , um eine neue Registerkarte **PIM** hinzuzufügen und Komponenten **Einzeiliger Text** hinzuzufügen.

   ![Registerkarte „PIM hinzufügen“](../assets/examples/assets-pim-integration/add-pim-tab.png)

   In der folgenden Tabelle sind die Metadateneigenschaften und die entsprechenden Felder aufgeführt.

   | Bezeichnung | Platzhalter | Metadaten-Eigenschaft |
   | --- | --- | --- |
   | SKU | Automatisch über die AEM-Ereignisintegration ausgefüllt | `wknd-skuid` |
   | Lieferantenname | Automatisch über die AEM-Ereignisintegration ausgefüllt | `wknd-suppliername` |

1. Klicken Sie auf **Speichern** und **Schließen**, um das Metadatenformular zu speichern.

1. Wenden Sie abschließend das **PIM**-Metadatenschema auf den **PIM**-Ordner an.

   ![Metadatenschema anwenden](../assets/examples/assets-pim-integration/apply-metadata-schema.png)

Mit den oben genannten Schritten können die Assets aus dem Ordner **Adventures** die PIM-spezifischen Metadaten wie SKU, Lieferantenname usw. anzeigen.

### Asset-Upload und Metadatenüberprüfung

Laden Sie zum Überprüfen der AEM Assets- und PIM-Integration ein Asset in den Ordner **Adventures** in AEM Assets hoch. Auf der Registerkarte „PIM“ auf der Seite mit Asset-Details sollten die Metadaten „SKU“ und „Lieferantenname“ angezeigt werden.

![AEM Assets-Upload](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Konzept und wichtige Schritte

In Unternehmen ist häufig die Synchronisierung der Asset-Metadaten zwischen AEM und anderen Systemen wie PIM erforderlich. Mit AEM Eventing können solche Anforderungen erfüllt werden.

- Der Code zum Abrufen von Asset-Metadaten wird außerhalb von AEM ausgeführt, wodurch die Belastung des AEM-Autoren-Services vermieden wird. Daher ist die ereignisgesteuerte Architektur unabhängig skalierbar.
- Die neu eingeführte Asset-Author-API wird verwendet, um die Asset-Metadaten in AEM zu aktualisieren.
- Die API-Authentifizierung verwendet OAuth-Server-zu-Server (auch Client-Anmeldedatenfluss genannt), siehe [Implementierungshandbuch für OAuth-Server-zu-Server-Anmeldedaten](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- Anstelle von Adobe I/O Runtime-Aktionen können andere Webhooks oder Amazon EventBridge verwendet werden, um das AEM Assets-Ereignis zu empfangen und die Metadaten-Aktualisierung zu verarbeiten.
- Asset Events über AEM Eventing ermöglichen es Unternehmen, wichtige Prozesse zu automatisieren und zu optimieren und so die Effizienz und Kohärenz im gesamten Content-Ökosystem zu fördern.
