---
title: Konfigurieren von manifest.yml eines Asset compute-Projekts
description: In der Datei "manifest.yml"des Asset compute-Projekts werden alle bereitzustellenden Sekundäre in diesem Projekt beschrieben.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Konfigurieren von manifest.yml

Das `manifest.yml` im Stammverzeichnis des Asset compute-Projekts beschreibt alle bereitzustellenden Arbeiter in diesem Projekt.

![manifest.yml](./assets/manifest/manifest.png)

## Standardarbeitsdefinition

Arbeitnehmer werden als Adobe I/O Runtime-Aktionseinträge unter `actions` definiert und bestehen aus einer Reihe von Konfigurationen.

Arbeitnehmer, die auf andere Adobe I/O-Integrationen zugreifen, müssen die `annotations -> require-adobe-auth` -Eigenschaft auf `true` setzen, da [die Anmeldeinformationen der Worker für die Adobe I/O](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) über das `params.auth` -Objekt verfügbar macht. Dies ist normalerweise erforderlich, wenn der Worker Adobe I/O-APIs wie die Adobe Photoshop-, Lightroom- oder Sensei-APIs abruft und pro Worker umgeschaltet werden kann.

1. Öffnen und überprüfen Sie den automatisch generierten Worker `manifest.yml`. Projekte, die mehrere Asset compute-Sekundäre enthalten, müssen einen Eintrag für jeden Worker unter dem Array `actions` definieren.

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

## Definieren von Beschränkungen

Jeder Worker kann die [limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) für den Ausführungskontext in Adobe I/O Runtime konfigurieren. Diese Werte sollten angepasst werden, um eine optimale Skalierung für den Worker zu ermöglichen, basierend auf der Menge, Rate und Art der zu berechnenden Assets sowie der Art der von ihm durchgeführten Arbeit.

Lesen Sie [Anleitung zur Dimensionierung von Adoben](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) , bevor Sie Begrenzungen festlegen. asset compute-Sekundäre können bei der Verarbeitung von Assets nicht genügend Arbeitsspeicher haben, was dazu führt, dass die Adobe I/O Runtime-Ausführung beendet wird. Stellen Sie daher sicher, dass die Größe des Sekundärs für die Verarbeitung aller Kandidaten-Assets angemessen ist.

1. Fügen Sie dem neuen Aktionseintrag `wknd-asset-compute` den Abschnitt `inputs` hinzu. Dies ermöglicht die Abstimmung der Gesamtleistung und Ressourcenzuordnung des Asset compute Worker.

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

Das endgültige `manifest.yml` sieht wie folgt aus:

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

Das endgültige `.manifest.yml` ist auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validieren von manifest.yml

Nachdem die generierte Asset compute `manifest.yml` aktualisiert wurde, führen Sie das lokale Entwicklungstool aus und stellen Sie sicher, dass die aktualisierten `manifest.yml`-Einstellungen erfolgreich verwendet werden.

So starten Sie das Asset compute Development Tool für das Asset compute-Projekt:

1. Öffnen Sie eine Befehlszeile im Asset compute-Projektstamm (in VS Code kann dies direkt in der IDE über Terminal > Neues Terminal geöffnet werden) und führen Sie den Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset compute Development Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. Sehen Sie sich die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen an, wenn das Entwicklungstool initialisiert wird.
1. Um das Asset compute Development Tool zu stoppen, tippen Sie im Fenster, das `aio app run` ausgeführt hat, auf `Ctrl-C` , um den Vorgang zu beenden.

## Fehlerbehebung

+ [Falscher YAML-Einzug](../troubleshooting.md#incorrect-yaml-indentation)
+ [Speichergrößenlimit ist zu niedrig eingestellt](../troubleshooting.md#memorysize-limit-is-set-too-low)
