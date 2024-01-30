---
title: Adobe I/O Runtime-Aktionen und AEM
description: Erfahren Sie, wie Sie AEM Ereignisse mit der Adobe I/O Runtime-Aktion empfangen und die Ereignisdetails wie Payload, Kopfzeilen und Metadaten überprüfen.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 1%

---


# Adobe I/O Runtime-Aktionen und AEM

Erfahren Sie, wie Sie AEM Ereignisse mit [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Aktion und Überprüfen Sie die Ereignisdetails wie Payload, Header und Metadaten.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Die Adobe I/O Runtime ist eine Server-lose Plattform, die die Codeausführung als Reaktion auf Adobe I/O-Ereignisse ermöglicht. So können Sie ereignisgesteuerte Anwendungen erstellen, ohne sich um die Infrastruktur zu sorgen.

In diesem Beispiel erstellen Sie eine Adobe I/O Runtime [Aktion](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/?lang=de) , das AEM Ereignisse erhält und die Ereignisdetails protokolliert.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

Die allgemeinen Schritte sind:

- Projekt in der Adobe Developer-Konsole erstellen
- Projekt für lokale Entwicklung initialisieren
- Konfigurieren eines Projekts in der Adobe Developer Console
- Trigger AEM Ereignisausführung überprüfen

## Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie:

- AEM as a Cloud Service Umgebung mit [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Zugriff auf [Adobe Developer-Konsole](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) auf Ihrem lokalen Computer installiert.

## Projekt in der Adobe Developer-Konsole erstellen

Gehen Sie wie folgt vor, um ein Projekt in der Adobe Developer Console zu erstellen:

- Navigieren Sie zu [Adobe Developer-Konsole](https://developer.adobe.com/) und klicken **Konsole** Schaltfläche.

- Im **Schnellstart** Abschnitt, klicken Sie auf **Projekt aus Vorlage erstellen**. Dann in der **Vorlagen durchsuchen** Dialogfeld auswählen **App Builder** Vorlage.

- Aktualisieren Sie bei Bedarf den Titel des Projekts, den App-Namen und den Arbeitsbereich hinzufügen . Klicken Sie anschließend auf **Speichern**.

  ![Projekt in der Adobe Developer-Konsole erstellen](../assets/examples/runtime-action/create-project.png)


## Projekt für lokale Entwicklung initialisieren

Um Adobe I/O Runtime Action zum Projekt hinzuzufügen, müssen Sie das Projekt für die lokale Entwicklung initialisieren. Navigieren Sie auf dem lokalen Computer zum Terminal, an dem Sie das Projekt initialisieren möchten, und führen Sie die folgenden Schritte aus:

- Initialisieren des Projekts durch Ausführen

  ```bash
  aio app init
  ```

- Wählen Sie die `Organization`, die `Project` die Sie im vorherigen Schritt erstellt haben, und den Arbeitsbereich. In `What templates do you want to search for?` Schritt auswählen `All Templates` -Option.

  ![Organisations-Projekt-Auswahl - Projekt initialisieren](../assets/examples/runtime-action/all-templates.png)

- Wählen Sie in der Liste &quot;Vorlagen&quot;die Option `@adobe/generator-app-excshell` -Option.

  ![Erweiterungsvorlage - Projekt initialisieren](../assets/examples/runtime-action/extensibility-template.png)

- Öffnen Sie das Projekt in Ihrer bevorzugten IDE, z. B. VSCode.

- Die ausgewählte _Erweiterungsvorlage_ (`@adobe/generator-app-excshell`) eine generische Laufzeitaktion bereitstellt, ist der Code in `src/dx-excshell-1/actions/generic/index.js` -Datei. Aktualisieren wir es, um es einfach zu halten, protokollieren Sie die Ereignisdetails und geben Sie eine Erfolgsantwort zurück. Im nächsten Beispiel wird es jedoch verbessert, die empfangenen AEM Ereignisse zu verarbeiten.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Stellen Sie abschließend die aktualisierte Aktion auf Adobe I/O Runtime bereit, indem Sie sie ausführen.

  ```bash
  aio app deploy
  ```

## Konfigurieren eines Projekts in der Adobe Developer Console

Um AEM Ereignisse zu erhalten und die im vorherigen Schritt erstellte Adobe I/O Runtime-Aktion auszuführen, konfigurieren Sie das Projekt in der Adobe Developer-Konsole.

- Navigieren Sie in der Adobe Developer-Konsole zum [Projekt](https://developer.adobe.com/console/projects) im vorherigen Schritt erstellt wurde, und klicken Sie auf , um ihn zu öffnen. Wählen Sie die `Stage` Arbeitsbereich, hier wurde die Aktion bereitgestellt.

- Klicks **Dienst hinzufügen** Schaltfläche und wählen Sie **API** -Option. Im **API hinzufügen** modal, wählen Sie **Adobe Services** > **I/O-Management-API** und klicken **Nächste** führen Sie weitere Konfigurationsschritte aus und klicken Sie auf **Konfigurierte API speichern**.

  ![Dienst hinzufügen - Projekt konfigurieren](../assets/examples/runtime-action/add-io-management-api.png)

- Klicken Sie auf **Dienst hinzufügen** Schaltfläche und wählen Sie **Ereignis** -Option. Im **Ereignisse hinzufügen** Dialogfeld auswählen **Experience Cloud** > **AEM Sites** und klicken Sie auf **Nächste**. Führen Sie zusätzliche Konfigurationsschritte aus, wählen Sie AEMCS-Instanz, Ereignistypen und andere Details aus.

- Schließlich wird im **Empfangen von Ereignissen** Schritt, erweitern **Laufzeitaktion** und wählen Sie die _generisch_ Aktion, die im vorherigen Schritt erstellt wurde. Klicks **Konfigurierte Ereignisse speichern**.

  ![Laufzeitaktion - Projekt konfigurieren ](../assets/examples/runtime-action/select-runtime-action.png)

- Überprüfen Sie die Details zur Ereignisregistrierung, auch die **Debug-Verfolgung** und überprüfen Sie die **Challenge Probe** Anfrage und Antwort.

  ![Details zur Ereignisregistrierung](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Trigger AEM Ereignisse

Gehen Sie wie folgt vor, um AEM Ereignisse aus Ihrer AEM as a Cloud Service Umgebung Trigger, die im obigen Adobe Developer Console-Projekt registriert wurde:

- Greifen Sie über auf Ihre AEM as a Cloud Service Autorenumgebung zu und melden Sie sich dort an [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Je nach **Abonnierte Ereignisse**, erstellen, aktualisieren, löschen, veröffentlichen oder Veröffentlichung eines Inhaltsfragments rückgängig machen.

## Ereignisdetails überprüfen

Nach Abschluss der oben genannten Schritte sollten die AEM Ereignisse angezeigt werden, die an die allgemeine Aktion gesendet werden.

Sie können die Ereignisdetails im Abschnitt **Debug-Verfolgung** auf der Registerkarte Ereignisregistrierungsdetails.

![AEM Ereignisdetails](../assets/examples/runtime-action/aem-event-details.png)


## Nächste Schritte

Im nächsten Beispiel möchten wir diese Aktion erweitern, um AEM Ereignisse zu verarbeiten, AEM Autorendienst zurückrufen, um Inhaltsdetails abzurufen, Details im Adobe I/O Runtime-Speicher zu speichern und über Einzelseiten-Apps (SPA) anzuzeigen.

