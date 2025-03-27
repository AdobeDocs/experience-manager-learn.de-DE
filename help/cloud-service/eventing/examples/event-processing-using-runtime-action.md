---
title: Verarbeiten von AEM-Ereignissen mithilfe einer Adobe I/O Runtime-Aktion
description: Erfahren Sie, wie Sie empfangene AEM-Ereignisse mit der Adobe I/O Runtime-Aktion verarbeiten.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '548'
ht-degree: 100%

---

# Verarbeiten von AEM-Ereignissen mithilfe einer Adobe I/O Runtime-Aktion

Erfahren Sie, wie Sie empfangene AEM-Ereignisse mithilfe der [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)-Aktion verarbeiten. Dieses Beispiel ist eine Erweiterung für das vorherige Beispiel [Adobe I/O Runtime-Aktionen und AEM-Ereignisse](runtime-action.md). Stellen Sie daher sicher, dass Sie dieses abgeschlossen haben, bevor Sie mit diesem Vorgang fortfahren.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

In diesem Beispiel speichert die Ereignisverarbeitung die ursprünglichen Ereignisdaten und das empfangene Ereignis als Aktivitätsmeldung im Adobe I/O Runtime-Speicher. Wenn das Ereignis jedoch vom Typ _Inhaltsfragment geändert_ ist, dann wird auch der AEM Author-Service aufgerufen, um die Änderungsdetails aufzuspüren. Schließlich werden die Ereignisdetails in einer Single Page Application (SPA) angezeigt.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung, in der [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) ist. Außerdem muss das [WKND-Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project)-Beispielprojekt darin bereitgestellt sein.

- Zugriff auf [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer-CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), die auf Ihrem lokalen Computer installiert ist.

- Lokal initialisiertes Projekt aus dem vorherigen Beispiel [Adobe I/O Runtime-Aktionen und AEM-Ereignisse](./runtime-action.md#initialize-project-for-local-development).

## AEM-Ereignisprozessor „action“

In diesem Beispiel führt der Ereignisprozessor [action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) die folgenden Aufgaben aus:

- Das empfangene Ereignis wird in eine Aktivitätsmeldung geparst.
- Wenn das empfangene Ereignis vom Typ _Inhaltsfragment geändert_ ist, erfolgt ein Rückruf an den AEM Author-Service, um die Änderungsdetails aufzuspüren.
- Die ursprünglichen Ereignisdaten, die Aktivitätsmeldung und etwaige Änderungsdetails werden im Adobe I/O Runtime-Speicher beibehalten.

Um die oben beschriebenen Aufgaben durchführen zu können, müssen wir dem Projekt zunächst eine Aktion hinzufügen, JavaScript-Module entwickeln und schließlich den Aktions-Code aktualisieren, um die entwickelten Module zu verwenden.

Die Archivdatei [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) enthält den vollständigen Code. Die wichtigsten Dateien daraus werden im folgenden Abschnitt vorgestellt.

### Hinzufügen einer Aktion

- Führen Sie den folgenden Befehl aus, um eine Aktion hinzuzufügen:

  ```bash
  aio app add action
  ```

- Wählen Sie `@adobe/generator-add-action-generic` als Aktionsvorlage aus und benennen Sie die Aktion mit `aem-event-processor`.

  ![Hinzufügen einer Aktion](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Entwickeln von JavaScript-Modulen

Um die oben genannten Aufgaben durchzuführen, müssen wir die folgenden JavaScript-Module entwickeln.

- Das Modul `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` bestimmt, ob das empfangene Ereignis vom Typ _Inhaltsfragment geändert_ ist.

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

- Das Modul `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` ruft den AEM Author-Service auf, um die Änderungsdetails aufzuspüren.

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

  Weitere Informationen dazu finden Sie im [Tutorial zu den AEM Service-Anmeldeinformationen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=de). Unter [App Builder-Konfigurationsdateien](https://developer.adobe.com/app-builder/docs/guides/configuration/) finden Sie zudem weitere Erklärungen zur Verwaltung von Geheimnissen und Aktionsparametern.

- Das Modul `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` speichert die ursprünglichen Ereignisdaten, die Aktivitätsmeldung und etwaige Änderungsdetails im Adobe I/O Runtime-Speicher.

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

### Aktualisieren des Aktions-Codes

Aktualisieren Sie abschließend den Aktions-Code unter `src/dx-excshell-1/actions/aem-event-processor/index.js`, um die entwickelten Module verwenden zu können.

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

- Der Ordner `src/dx-excshell-1/actions/model` enthält die Dateien `aemEvent.js` und `errors.js`, die von der Aktion verwendet werden, um das empfangene Ereignis zu parsen und Fehler zu beheben.
- Der Ordner `src/dx-excshell-1/actions/load-processed-aem-events` enthält den Aktions-Code. Diese Aktion wird von der SPA verwendet, um die verarbeiteten AEM-Ereignisse aus dem Adobe I/O Runtime-Speicher zu laden.
- Der Ordner `src/dx-excshell-1/web-src` enthält den SPA-Code, um die verarbeiteten AEM-Ereignisse anzuzeigen.
- Die Datei `src/dx-excshell-1/ext.config.yaml` enthält die Aktionskonfiguration und -parameter.

## Konzept und wichtige Schritte

Die Anforderungen an die Ereignisverarbeitung unterscheiden sich von Projekt zu Projekt. Die wichtigsten Schritte in diesem Beispiel sind jedoch:

- Die Ereignisverarbeitung kann mit der Adobe I/O Runtime-Aktion durchgeführt werden.
- Die Runtime-Aktion kann mit Systemen wie Ihren internen Anwendungen, Lösungen von Drittanbietern und Adobe-Lösungen kommunizieren.
- Die Runtime-Aktion dient als Einstiegspunkt für einen Geschäftsprozess, der auf eine Inhaltsänderung ausgerichtet ist.
