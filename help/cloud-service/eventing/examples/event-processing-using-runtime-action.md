---
title: Verarbeitung AEM Ereignissen mithilfe der Adobe I/O Runtime-Aktion
description: Erfahren Sie, wie Sie mit der Adobe I/O Runtime-Aktion empfangene AEM Ereignisse verarbeiten.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---


# Verarbeitung AEM Ereignissen mithilfe der Adobe I/O Runtime-Aktion

Erfahren Sie, wie Sie empfangene AEM mithilfe von [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Aktion. Dieses Beispiel verbessert das frühere Beispiel [Adobe I/O Runtime-Aktionen und AEM](runtime-action.md), stellen Sie sicher, dass Sie es abgeschlossen haben, bevor Sie mit diesem Vorgang fortfahren.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

In diesem Beispiel speichert die Ereignisverarbeitung die ursprünglichen Ereignisdaten und das empfangene Ereignis als Aktivitätsnachricht im Adobe I/O Runtime-Speicher. Wenn das Ereignis jedoch _Inhaltsfragment geändert_ eingeben, dann wird auch AEM Autorendienst aufgerufen, um die Änderungsdetails zu finden. Schließlich werden die Ereignisdetails in einer Einzelseitenanwendung (SPA) angezeigt.

## Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie:

- AEM as a Cloud Service Umgebung mit [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Außerdem das Beispiel [WKND-Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) auf dem Projekt bereitgestellt werden.

- Zugriff auf [Adobe Developer-Konsole](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) auf Ihrem lokalen Computer installiert.

- Lokal initialisiertes Projekt aus dem vorherigen Beispiel [Adobe I/O Runtime-Aktionen und AEM](./runtime-action.md#initialize-project-for-local-development).


## Aktion AEM Ereignisprozessors

In diesem Beispiel wird der Ereignisprozessor [action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/?lang=de) führt die folgenden Aufgaben aus:

- Analysiert das empfangene Ereignis in eine Aktivitätsnachricht.
- Wenn das empfangene Ereignis _Inhaltsfragment geändert_ type, rufen Sie zurück zu AEM Autorendienst, um die Änderungsdetails zu finden.
- Behält die ursprünglichen Ereignisdaten, Aktivitätsnachrichten und gegebenenfalls Änderungsdetails im Adobe I/O Runtime-Speicher bei.

Beginnen wir mit dem Hinzufügen einer Aktion zum Projekt, dem Entwickeln von JavaScript-Modulen zur Durchführung der oben genannten Aufgaben und schließlich dem Aktualisieren des Aktionscodes, um die entwickelten Module zu verwenden.

Siehe Anhang [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) -Datei für den vollständigen Code und im folgenden Abschnitt werden die Schlüsseldateien hervorgehoben.

### Aktion hinzufügen

- Um eine Aktion hinzuzufügen, führen Sie den folgenden Befehl aus:

  ```bash
  aio app add action
  ```

- Auswählen `@adobe/generator-add-action-generic` als Aktionsvorlage verwenden, benennen Sie die Aktion als `aem-event-processor`.

  ![Aktion hinzufügen](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### JavaScript-Module entwickeln

Um die oben genannten Aufgaben durchzuführen, entwickeln wir die folgenden JavaScript-Module.

- Die `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` Modul bestimmt, ob das empfangene Ereignis _Inhaltsfragment geändert_ Typ.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- Die `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` -Modul ruft AEM Autorendienst auf, um die Änderungsdetails zu finden.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Siehe Abschnitt [Tutorial AEM Service Credentials](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) , um mehr darüber zu erfahren. Außerdem wird die [App Builder-Konfigurationsdateien](https://developer.adobe.com/app-builder/docs/guides/configuration/) für die Verwaltung von Geheimnissen und Aktionsparametern.

- Die `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` -Modul speichert die ursprünglichen Ereignisdaten, Aktivitätsnachrichten und (falls vorhanden) Änderungsdetails im Adobe I/O Runtime-Speicher.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Aktionscode aktualisieren

Aktualisieren Sie abschließend den Aktionscode unter `src/dx-excshell-1/actions/aem-event-processor/index.js` um die entwickelten Module zu verwenden.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Zusätzliche Ressourcen

- Die `src/dx-excshell-1/actions/model` Ordner enthält `aemEvent.js` und `errors.js` -Dateien, die von der -Aktion verwendet werden, um das empfangene Ereignis zu analysieren und Fehler zu beheben.
- Die `src/dx-excshell-1/actions/load-processed-aem-events` -Ordner den Aktionscode enthält, wird diese Aktion vom SPA verwendet, um die verarbeiteten AEM-Ereignisse aus dem Adobe I/O Runtime-Speicher zu laden.
- Die `src/dx-excshell-1/web-src` enthält den SPA-Code, der die verarbeiteten AEM-Ereignisse anzeigt.
- Die `src/dx-excshell-1/ext.config.yaml` -Datei enthält die Aktionskonfiguration und -parameter.

## Konzept und wichtige Schritte

Die Anforderungen an die Ereignisverarbeitung unterscheiden sich von Projekt zu Projekt. Die wichtigsten Schritte in diesem Beispiel sind jedoch:

- Die Ereignisverarbeitung kann mit der Adobe I/O Runtime-Aktion durchgeführt werden.
- Die Laufzeitaktion kann mit Systemen wie Ihren internen Anwendungen, Lösungen von Drittanbietern und Adobe-Lösungen kommunizieren.
- Die Laufzeitaktion dient als Einstiegspunkt für einen Geschäftsprozess, der auf eine Inhaltsänderung ausgerichtet ist.





