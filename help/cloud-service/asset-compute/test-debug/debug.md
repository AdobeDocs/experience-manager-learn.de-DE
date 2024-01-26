---
title: Debugging eines Asset Compute-Sekundärs
description: Zum Debugging von Asset Compute-Sekundären gibt es verschiedene Möglichkeiten – angefangen bei einfachen Debugging-Protokollanweisungen über das Anhängen von VS Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM as a Cloud Service initiiert wurden.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
duration: 268
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 100%

---

# Debugging eines Asset Compute-Sekundärs

Zum Debugging von Asset Compute-Sekundären gibt es verschiedene Möglichkeiten – angefangen bei einfachen Debugging-Protokollanweisungen über das Anhängen von VS Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM as a Cloud Service initiiert wurden.

## Protokollierung

Die einfachste Form zum Debugging von Asset Compute-Sekundären besteht darin, herkömmliche `console.log(..)`-Anweisungen im Code des Sekundärs zu verwenden. Das JavaScript-Objekt `console` ist ein implizites, globales Objekt, sodass es nicht importiert oder angefordert werden muss, da es immer in allen Kontexten vorhanden ist.

Diese Protokollanweisungen können je nach Ausführung des Asset Compute-Sekundärs auf unterschiedliche Weise eingesehen werden:

+ Von `aio app run` werden Protokolle in die Standardausgabe und die Aktivierungsprotokolle des [Entwicklungs-Tools](../develop/development-tool.md) gedruckt.
  ![aio app run console.log(…)](./assets/debug/console-log__aio-app-run.png)
+ Von `aio app test` werden Protokolle unter `/build/test-results/test-worker/test.log` gedruckt.
  ![aio app test console.log(…)](./assets/debug/console-log__aio-app-test.png)
+ Bei Verwendung von `wskdebug` werden Protokollanweisungen in die Standardausgabe der VS Code-Debugging-Konsole („Ansicht“ > „Debugging-Konsole“) gedruckt.
  ![wskdebug console.log(…)](./assets/debug/console-log__wskdebug.png)
+ Bei Verwendung von `aio app logs` werden Protokollanweisungen in die Aktivierungsprotokollausgabe gedruckt.

## Remote-Debugging über angehängten Debugger

>[!WARNING]
>
>Verwenden Sie Microsoft Visual Studio Code 1.48.0 oder höher, um die Kompatibilität mit wskdebug zu erreichen. 

Das npm-Modul [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) unterstützt das Anhängen eines Debuggers an Asset Compute-Sekundäre, einschließlich der Möglichkeit, Breakpoints in VS Code festzulegen und den Code schrittweise zu durchlaufen.

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_Clickthrough beim Debugging eines Asset Compute-Sekundärs mit wskdebug (kein Audio)_

1. Stellen Sie sicher, dass die npm-Module [wskdebug](../set-up/development-environment.md#wskdebug) und [ngrok](../set-up/development-environment.md#ngork) installiert sind.
1. Stellen Sie sicher, dass [Docker Desktop und die unterstützenden Docker-Bilder](../set-up/development-environment.md#docker) installiert sind und ausgeführt werden.
1. Schließen Sie alle aktiven, laufenden Instanzen des Entwicklungs-Tools.
1. Stellen Sie den neuesten Code mit `aio app deploy` bereit und notieren Sie sich den Namen der bereitgestellten Aktion (Name zwischen `[...]`). Damit wird `launch.json` in Schritt 8 aktualisiert.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. Starten Sie eine neue Instanz des Asset Compute-Entwicklungs-Tools mit dem Befehl `npx adobe-asset-compute devtool`.
1. Klicken Sie in VS Code im linken Navigationsbereich auf das Debugging-Symbol.
   + Klicken Sie bei entsprechender Aufforderung auf __launch.json-Datei erstellen > Node.js__, um eine neue `launch.json`-Datei zu erstellen.
   + Klicken Sie andernfalls auf das __Zahnradsymbol__ rechts neben dem Dropdown-Menü __Startprogramm__, um die vorhandene `launch.json`-Datei im Editor zu öffnen.
1. Fügen Sie die folgende JSON-Objektkonfiguration zum Array `configurations` hinzu:

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

1. Wählen Sie den neuen Eintrag __wskdebug__ aus der Dropdown-Liste aus.
1. Klicken Sie auf die grüne Schaltfläche __Ausführen__ links vom Dropdown-Menü __wskdebug__.
1. Öffnen Sie `/actions/worker/index.js` und klicken Sie links neben den Zeilennummern, um Breakpoint 1 hinzuzufügen. Navigieren Sie zu dem in Schritt 6 geöffneten Webbrowser-Fenster des Asset Compute-Entwicklungs-Tools.
1. Klicken Sie auf die Schaltfläche __Ausführen__, um den Sekundär auszuführen.
1. Navigieren Sie zurück zu `/actions/worker/index.js` in VS Code und gehen Sie den Code schrittweise durch.
1. Um das Debugging-fähige Entwicklungs-Tool zu beenden, klicken Sie auf `Ctrl-C` in dem Terminal, das in Schritt 6 den Befehl `npx adobe-asset-compute devtool` ausgeführt hat.

## Zugreifen auf Protokolle über Adobe I/O Runtime{#aio-app-logs}

[AEM as a Cloud Service nutzt Asset Compute-Sekundäre über Verarbeitungsprofile](../deploy/processing-profiles.md), indem diese direkt in Adobe I/O Runtime aufgerufen werden. Da diese Aufrufe keine lokale Entwicklung erfordern, können ihre Ausführungen nicht mit lokalen Tools wie dem Asset Compute-Entwicklungs-Tool oder wskdebug debuggt werden. Stattdessen kann die Adobe I/O-CLI verwendet werden, um Protokolle von dem in einem bestimmten Arbeitsbereich in Adobe I/O Runtime ausgeführten Sekundär abzurufen.

1. Stellen Sie sicher, dass die [arbeitsbereichsspezifischen Umgebungsvariablen](../deploy/runtime.md) über `AIO_runtime_namespace` und `AIO_runtime_auth` festgelegt sind, und zwar basierend auf dem Arbeitsbereich, für den ein Debugging erforderlich ist.
1. Führen Sie in der Befehlszeile `aio app logs` aus.
   + Wenn der Arbeitsbereich einen hohen Traffic aufweist, erweitern Sie die Anzahl der Aktivierungsprotokolle über das Flag `--limit`:
     `$ aio app logs --limit=25`
1. Die neuesten Aktivierungsprotokolle (bis zum angegebenen `--limit`) werden als Ausgabe des Befehls zur Überprüfung zurückgegeben.

   ![aio app logs](./assets/debug/aio-app-logs.png)

## Fehlerbehebung

+ [Debugger nicht angehängt](../troubleshooting.md#debugger-does-not-attach)
+ [Breakpoints werden nicht angehalten](../troubleshooting.md#breakpoints-no-pausing)
+ [VS-Code-Debugger nicht angehängt](../troubleshooting.md#vs-code-debugger-not-attached)
+ [VS-Code-Debugger nach Beginn der Sekundär-Ausführung angehängt](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Sekundär-Timeout beim Debugging](../troubleshooting.md#worker-times-out-while-debugging)
+ [Debugger-Prozess kann nicht beendet werden](../troubleshooting.md#cannot-terminate-debugger-process)
