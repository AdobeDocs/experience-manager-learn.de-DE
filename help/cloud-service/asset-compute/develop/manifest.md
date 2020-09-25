---
title: manifest.yml eines Asset Compute-Projekts konfigurieren
description: In der Datei "manifest.yml"des Projekts "Asset Compute"werden alle bereitzustellenden Arbeiter in dieser Anwendung beschrieben.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 0%

---


# manifest.yml konfigurieren

In `manifest.yml`der Datei im Stammverzeichnis des Projekts &quot;Asset Compute&quot;werden alle in diesem Projekt bereitzustellenden Arbeiter beschrieben.

![manifest.yml](./assets/manifest/manifest.png)

## Standardarbeitsdefinition

Arbeitnehmer werden als Adobe I/O Runtime-Aktionseinträge unter definiert `actions`und bestehen aus einer Gruppe von Konfigurationen.

Arbeiter, die auf andere Adoben-E/A-Integrationen zugreifen, müssen die `annotations -> require-adobe-auth` Eigenschaft auf einstellen, `true` da dadurch die Adoben-E/A-Anmeldeinformationen [des Workers über das](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) Objekt `params.auth` verfügbar gemacht werden. Dies ist in der Regel erforderlich, wenn der Mitarbeiter Adobe-I/O-APIs wie die Adobe Photoshop-, Lightroom- oder Sensei-APIs abruft und pro Mitarbeiter umgeschaltet werden kann.

1. Öffnen und überprüfen Sie den automatisch generierten Arbeiter `manifest.yml`. Projekte, die mehrere Asset Compute-Mitarbeiter enthalten, müssen einen Eintrag für jeden Arbeitnehmer unter dem `actions` Array definieren.

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

## Grenzen definieren

Jeder Mitarbeiter kann die [Beschränkungen](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) für seinen Ausführungskontext in Adobe I/O Runtime konfigurieren. Diese Werte sollten so eingestellt werden, dass eine optimale Größenanpassung für den Arbeiter möglich ist, basierend auf der Menge, der Rate und der Art der zu berechnenden Assets sowie der Art der von ihm ausgeführten Arbeit.

Überprüfen Sie die Anleitung zur Größenanpassung der [Adobe](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) , bevor Sie Grenzwerte festlegen. Mitarbeiter von Asset Compute können bei der Verarbeitung von Assets nicht über genügend Arbeitsspeicher verfügen, was dazu führt, dass die Ausführung von Adobe I/O Runtime abgebrochen wird. So wird sichergestellt, dass die Größe des Arbeitnehmers für die Verarbeitung aller zu prüfenden Assets angemessen ist.

1. hinzufügen Sie einen `inputs` Abschnitt zum neuen `wknd-asset-compute` Aktionseintrag. Dies ermöglicht die Abstimmung der Gesamtleistung und Ressourcenzuordnung des Asset Compute-Mitarbeiters.

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

Das endgültige Ergebnis `manifest.yml` sieht wie folgt aus:

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

## Validieren der Datei manifest.yml

Führen Sie nach der Aktualisierung des erstellten Asset-Compute das lokale Entwicklungstool aus und stellen Sie sicher, dass die Beginn die aktualisierten `manifest.yml` `manifest.yml` Einstellungen erfolgreich verwenden.

So Beginn Asset Compute Development Tool for the Asset Compute project:

1. Öffnen Sie eine Befehlszeile im Projektstamm &quot;Asset Compute&quot;(im VS-Code kann diese über Terminal > New Terminal direkt in der IDE geöffnet werden) und führen Sie den Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset Compute Development Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![App-Ausführung](assets/environment-variables/aio-app-run.png)

1. Beobachten Sie die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen während der Initialisierung des Entwicklungstools.
1. Um das Asset Compute Development Tool zu beenden, tippen Sie `Ctrl-C` im Fenster, das ausgeführt wurde, auf , um den Prozess `aio app run` zu beenden.

## Fehlerbehebung

### Falscher YAML-Einzug

+ __Fehler:__ YAMLException: Ungültiger Einzug eines Zuordnungseintrags in Zeile X, Spalte Y:(über `aio app run` Befehl &quot;Standard ab&quot;)
+ __Ursache:__ Bei den Yaml-Dateien wird zwischen weißen Abständen unterschieden. Der Einzug ist wahrscheinlich nicht korrekt.
+ __Lösung:__ Überprüfen Sie Ihren Einzug `manifest.yml` und stellen Sie sicher, dass er korrekt ist.

### memorySize limit ist zu niedrig eingestellt

+ __Fehler:__  Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 (Ungültige Anforderung) zurückgegeben —> &quot;Der Anforderungsinhalt war fehlerhaft:Anforderung fehlgeschlagen: Speicher 64 MB unter dem zulässigen Schwellenwert von 134217728 B&quot;
+ __Ursache:__ Eine `memorySize` Beschränkung im Manifest wurde unter dem erlaubten Mindestschwellenwert festgelegt, wie von der Fehlermeldung in Byte berichtet.
+ __Lösung:__  Überprüfen Sie die `memorySize` Grenzwerte in der `manifest.yml` und stellen Sie sicher, dass sie alle größer als der erlaubte Mindestschwellenwert sind.