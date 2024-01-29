---
title: Konfigurieren der manifest.yml eines Asset Compute-Projekts
description: Die manifest.yml des Asset Compute-Projekts beschreibt alle zu verteilenden Sekundäre in diesem Projekt.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 128
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '401'
ht-degree: 100%

---

# Konfigurieren von manifest.yml

Die Datei `manifest.yml`, die sich im Stammverzeichnis des Asset Compute-Projekts befindet, beschreibt alle Sekundäre in diesem Projekt, die eingesetzt werden sollen.

![manifest.yml](./assets/manifest/manifest.png)

## Standardsekundärdefinition

Sekundäre werden als Adobe I/O Runtime-Aktionseinträge unter `actions` definiert und bestehen aus einer Reihe von Konfigurationen.

Sekundäre, die auf andere Adobe I/O-Integrationen zugreifen, müssen die `annotations -> require-adobe-auth`-Eigenschaft auf `true` setzen, da dies [die Adobe I/O-Anmeldeinformationen](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=de#access-adobe-apis) des Sekundärs über das `params.auth`-Objekt offenlegt.  Dies ist in der Regel erforderlich, wenn der Sekundär Adobe I/O-APIs wie die Adobe Photoshop-, Lightroom- oder Sensei-APIs aufruft. Dies kann zudem für jeden Sekundär umgeschaltet werden.

1. Öffnen und überprüfen Sie den automatisch generierten Sekundär `manifest.yml`. Projekte, die mehrere Asset Compute-Sekundäre enthalten, müssen für jeden Sekundär einen Eintrag unter dem Array `actions` definieren.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Definieren von Limits

Jeder Sekundär kann die [Limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) für seinen Ausführungskontext in Adobe I/O Runtime konfigurieren.  Diese Werte sollten angepasst werden, um eine optimale Skalierung des Sekundärs zu ermöglichen, basierend auf der Menge, Rate und Art der zu berechnenden Assets sowie der Art der von ihm durchgeführten Arbeit.

Prüfen Sie die [Adobe-Größenrichtlinien](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=de#sizing-workers), bevor Sie Grenzen festlegen.  Asset Compute-Sekundäre können bei der Verarbeitung von Assets nicht genügend Arbeitsspeicher haben, was dazu führt, dass die Adobe I/O Runtime-Ausführung beendet wird. Stellen Sie daher sicher, dass die Größe des Sekundärs für die Verarbeitung aller Kandidaten-Assets angemessen ist.

1. Fügen Sie einen `inputs`-Abschnitt zu dem neuen Aktionseintrag in `wknd-asset-compute` hinzu.  Dadurch können die Gesamtleistung und die Ressourcenzuweisung des Asset Compute-Sekundärs optimiert werden.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## Die fertige manifest.yml

Die endgültige `manifest.yml` sieht wie folgt aus:

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml auf Github

Die endgültige `.manifest.yml` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validieren der manifest.yml

Sobald die generierte `manifest.yml` für Asset Compute aktualisiert ist, führen Sie das lokale Entwicklungs-Tool aus und stellen Sie sicher, dass es erfolgreich mit den aktualisierten Einstellungen der `manifest.yml` startet.

So starten Sie das Asset Compute-Entwicklungs-Tool für das Asset Compute-Projekt:

1. Öffnen Sie eine Befehlszeile im Asset Compute-Projektstammverzeichnis (in VS Code kann dies direkt in der IDE über „Terminal“ > „Neues Terminal“ geöffnet werden) und führen Sie den folgenden Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset Compute-Entwicklungs-Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. Beobachten Sie die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen während der Initialisierung des Entwicklungs-Tools.
1. Um das Asset Compute-Entwicklungs-Tool zu beenden, wählen Sie `Ctrl-C` in dem Fenster aus, in dem `aio app run` ausgeführt wurde.

## Fehlerbehebung

+ [Falscher YAML-Einzug](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize-Limit ist zu niedrig eingestellt](../troubleshooting.md#memorysize-limit-is-set-too-low)
