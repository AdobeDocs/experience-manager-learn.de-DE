---
title: Asset compute Worker debuggen
description: asset compute-Sekundäre können auf verschiedene Weise debuggt werden, von einfachen Debug-Protokollanweisungen über angehängten VS-Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM as a Cloud Service initiiert wurden.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# Asset compute Worker debuggen

asset compute-Sekundäre können auf verschiedene Weise debuggt werden, von einfachen Debug-Protokollanweisungen über angehängten VS-Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM as a Cloud Service initiiert wurden.

## Protokollierung

Die einfachste Form des Debuggens von Asset compute-Sekundären verwendet herkömmliche `console.log(..)` -Anweisungen im Worker-Code. Die `console` JavaScript-Objekt ist ein implizites, globales Objekt, sodass es nicht importiert werden muss oder erforderlich ist, da es immer in allen Kontexten vorhanden ist.

Diese Protokollanweisungen sind je nach Ausführung des Asset compute Worker unterschiedlich für die Überprüfung verfügbar:

+ Von `aio app run`, druckt so aus, dass der Standard-Prozess [Entwicklungs-Tool](../develop/development-tool.md) Aktivierungsprotokolle
   ![Ausführen der aio-App console.log(..)](./assets/debug/console-log__aio-app-run.png)
+ Von `aio app test`, protokolliert drucken in `/build/test-results/test-worker/test.log`
   ![aio app test console.log(..)](./assets/debug/console-log__aio-app-test.png)
+ Verwenden `wskdebug`, Protokollanweisungen in die VS Code Debug Console (Ansicht > Debug Console) drucken, Standard-Out
   ![wskdebug console.log(..)](./assets/debug/console-log__wskdebug.png)
+ Verwenden `aio app logs`, Protokollanweisungen werden in der Aktivierungsprotokollausgabe gedruckt.

## Remote-Debugging über angehängten Debugger

>[!WARNING]
>
>Verwenden von Microsoft Visual Studio Code 1.48.0 oder höher für Kompatibilität mit wskdebug

Die [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm-Modul unterstützt das Anhängen eines Debuggers an Asset compute-Sekundäre, einschließlich der Möglichkeit, Haltepunkte im VS-Code festzulegen und den Code schrittweise zu durchlaufen.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Clickthrough des Debuggens eines Asset compute Sekundärs mit wskdebug (kein Audio)_

1. Sichern [wskdebug](../set-up/development-environment.md#wskdebug) und [ngrok](../set-up/development-environment.md#ngork) npm-Module sind installiert
1. Sichern [Docker Desktop und die unterstützenden Docker-Bilder](../set-up/development-environment.md#docker) installiert und ausgeführt werden
1. Schließen Sie alle aktiven laufenden Instanzen des Entwicklungstools.
1. Stellen Sie den neuesten Code mit `aio app deploy`  und notieren Sie den Namen der bereitgestellten Aktion (Name zwischen dem `[...]`). Damit wird die `launch.json` in Schritt 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Starten Sie eine neue Instanz des Asset compute-Entwicklungstools mit dem -Befehl. `npx adobe-asset-compute devtool`
1. Tippen Sie im VS-Code im linken Navigationsbereich auf das Symbol Debuggen .
   + Wenn Sie dazu aufgefordert werden, tippen Sie auf __Datei launch.json erstellen > Node.js__ , um eine neue `launch.json` -Datei.
   + Andernfalls tippen Sie auf die __Fanggerät__ rechts neben dem __Launch-Programm__ Dropdown zum Öffnen der vorhandenen `launch.json` im Editor.
1. Fügen Sie die folgende JSON-Objektkonfiguration zum `configurations` array:

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. Wählen Sie das neue __wskdebug__ aus der Dropdown-Liste
1. Tippen Sie auf Grün __Ausführen__ Schaltfläche links von __wskdebug__ Dropdown
1. Öffnen `/actions/worker/index.js` und tippen Sie links neben den Zeilennummern auf , um die Umbruchpunkte 1 hinzuzufügen. Navigieren Sie zum Webbrowser-Fenster des Asset compute Development Tool , das in Schritt 6 geöffnet wurde.
1. Tippen Sie auf __Ausführen__ Schaltfläche zum Ausführen des Sekundärs
1. Navigieren Sie zurück zu VS-Code, um `/actions/worker/index.js` und schrittweise durch den Code
1. Um das Debuggable Development Tool zu beenden, tippen Sie auf `Ctrl-C` im Terminal, das ausgeführt wurde `npx adobe-asset-compute devtool` Befehl in Schritt 6

## Zugriff auf Protokolle aus Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service nutzt Asset compute-Sekundäre über Verarbeitungsprofile](../deploy/processing-profiles.md) durch direkten Aufruf in Adobe I/O Runtime. Da diese Aufrufe keine lokale Entwicklung erfordern, können ihre Ausführungen nicht mit lokalen Tools wie Asset compute Development Tool oder wskdebug debuggt werden. Stattdessen kann die Adobe I/O-CLI verwendet werden, um Protokolle vom Worker abzurufen, der in einem bestimmten Arbeitsbereich in Adobe I/O Runtime ausgeführt wird.

1. Stellen Sie sicher, dass [Workspace-spezifische Umgebungsvariablen](../deploy/runtime.md) festgelegt über `AIO_runtime_namespace` und `AIO_runtime_auth`, basierend auf dem Arbeitsbereich, der Debugging erfordert.
1. Führen Sie in der Befehlszeile `aio app logs`
   + Wenn der Arbeitsbereich einen hohen Traffic aufweist, erweitern Sie die Anzahl der Aktivierungsprotokolle über die `--limit` Markierung:
      `$ aio app logs --limit=25`
1. Die neueste (bis zum bereitgestellten `--limit`) Aktivierungsprotokolle werden als Ausgabe des Befehls zur Überprüfung zurückgegeben.

   ![aio app logs](./assets/debug/aio-app-logs.png)

## Fehlerbehebung

+ [Debugger wird nicht angehängt](../troubleshooting.md#debugger-does-not-attach)
+ [Haltepunkte werden nicht angehalten](../troubleshooting.md#breakpoints-no-pausing)
+ [VS-Code-Debugger nicht angehängt](../troubleshooting.md#vs-code-debugger-not-attached)
+ [VS-Code-Debugger nach Beginn der Ausführung des Workflows angehängt](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker-Timeout beim Debugging](../troubleshooting.md#worker-times-out-while-debugging)
+ [Debug-Prozess kann nicht beendet werden](../troubleshooting.md#cannot-terminate-debugger-process)
