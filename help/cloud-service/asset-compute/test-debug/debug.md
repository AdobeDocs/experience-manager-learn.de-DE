---
title: Debuggen eines Asset Computing-Mitarbeiters
description: Asset Compute-Mitarbeiter können auf verschiedene Weise debuggt werden, von einfachen Debug-Protokollanweisungen über angehängten VS-Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM als Cloud Service initiiert wurden.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# Debuggen eines Asset Computing-Mitarbeiters

Asset Compute-Mitarbeiter können auf verschiedene Weise debuggt werden, von einfachen Debug-Protokollanweisungen über angehängten VS-Code als Remote-Debugger bis hin zum Abruf von Protokollen für Aktivierungen in Adobe I/O Runtime, die von AEM als Cloud Service initiiert wurden.

## Protokollierung

Die grundlegendste Form des Debuggens von Asset Compute-Mitarbeitern verwendet traditionelle `console.log(..)` Anweisungen im Arbeitscode. Das `console` JavaScript-Objekt ist ein implizites, globales Objekt, sodass es nicht importiert werden muss oder erforderlich ist, da es immer in allen Kontexten vorhanden ist.

Diese Protokollanweisungen stehen je nach Ausführung des Workers &quot;Asset Compute&quot;unterschiedlich zur Überprüfung zur Verfügung:

+ Von `aio app run`den Protokollen print bis standard out und die [Aktivierungen des](../develop/development-tool.md) Entwicklungstools
   ![Die App führt console.log(...) aus.](./assets/debug/console-log__aio-app-run.png)
+ Protokolle drucken `aio app test`zu `/build/test-results/test-worker/test.log`
   ![aio app test console.log(...)](./assets/debug/console-log__aio-app-test.png)
+ Protokolle drucken `wskdebug`die Anweisungen in der VS-Code-Debug-Konsole (Ansicht > Debug-Konsole), Standard-Out
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ Protokollanweisungen `aio app logs`werden in der Protokollausgabe der Aktivierung gedruckt

## Remote-Debugging über angehängten Debugger

>[!WARNING]
>
>Verwenden Sie Microsoft Visual Studio Code 1.48.0 oder höher zur Kompatibilität mit wskdebug

Das Modul [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm unterstützt das Anhängen eines Debuggers an Asset Compute-Mitarbeiter, einschließlich der Möglichkeit, Haltepunkte im VS-Code festzulegen und den Code schrittweise zu durchlaufen.

>[!VIDEO](https://video.tv.adobe.com/v/40383/?quality=12&learn=on)

_Clickthrough zum Debugging eines Asset Compute-Workers mit wskdebug (kein Audio)_

1. Stellen Sie sicher, dass die [Module &quot;wskdebug](../set-up/development-environment.md#wskdebug) &quot;und [ngrok](../set-up/development-environment.md#ngork) npm installiert sind
1. Stellen Sie sicher, dass [Docker Desktop und die zugehörigen Docker-Bilder](../set-up/development-environment.md#docker) installiert und ausgeführt sind
1. Schließen Sie alle aktiven Instanzen von Development Tool.
1. Stellen Sie den neuesten Code mit dem Namen der bereitgestellten Aktion bereit `aio app deploy` und zeichnen Sie ihn auf (Name zwischen dem `[...]`). Dies wird verwendet, um die `launch.json` in Schritt 8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```
1. Beginn einer neuen Instanz des Asset Compute Development Tool mit dem Befehl `npx adobe-asset-compute devtool`
1. Tippen Sie im VS-Code auf das Debug-Symbol in der linken Navigation
   + Wenn Sie dazu aufgefordert werden, tippen Sie auf Datei &quot;launch.json&quot;> &quot;Node.js __&quot;, um eine neue__ `launch.json` Datei zu erstellen.
   + Tippen Sie andernfalls auf das Symbol &quot; __Gear__ &quot;rechts neben dem Dropdown-Menü &quot; __Launch-Programm__ &quot;, um die vorhandenen Elemente `launch.json` im Editor zu öffnen.
1. hinzufügen Sie die folgende JSON-Objektkonfiguration für das `configurations` Array:

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

1. Wählen Sie das neue __wskdebug__ aus der Dropdownliste
1. Tippen Sie auf die grüne __Schaltfläche &quot;Ausführen__ &quot;links neben dem Dropdown-Menü &quot; __wskdebug__ &quot;.
1. Öffnen Sie `/actions/worker/index.js` und tippen Sie auf die linke Seite der Zeilennummern, um Umbruchpunkte 1 hinzuzufügen. Navigieren Sie zum Webbrowser des Asset Compute Development Tool, das in Schritt 6 geöffnet wurde.
1. Tippen Sie auf die Schaltfläche __Ausführen__ , um den Arbeiter auszuführen
1. Navigieren Sie zurück zum VS-Code, zum Code `/actions/worker/index.js` und durchlaufen Sie ihn.
1. Um das debug-fähige Entwicklungstool zu beenden, tippen Sie `Ctrl-C` im Terminal, das den `npx adobe-asset-compute devtool` Befehl in Schritt 6 ausgeführt hat

## Zugriff auf Protokolle von Adobe I/O Runtime{#aio-app-logs}

[AEM als Cloud Service nutzt Asset-Compute-Mitarbeiter über die Verarbeitung von Profilen](../deploy/processing-profiles.md) , indem sie sie direkt in Adobe I/O Runtime aufrufen. Da diese Aufrufe keine lokale Entwicklung beinhalten, können ihre Ausführung nicht mit lokalen Werkzeugen wie Asset Compute Development Tool oder wskdebug debuggt werden. Stattdessen kann die Adobe-I/O-CLI verwendet werden, um Protokolle vom Worker abzurufen, der in einem bestimmten Arbeitsbereich in Adobe I/O Runtime ausgeführt wird.

1. Vergewissern Sie sich, dass die [Workspace-spezifischen Variablen](../deploy/runtime.md) für die Umgebung über `AIO_runtime_namespace` und `AIO_runtime_auth`, basierend auf dem zu debuggenden Arbeitsbereich, festgelegt werden.
1. Führen Sie in der Befehlszeile die `aio app logs`
   + Wenn der Arbeitsbereich stark frequentiert wird, erweitern Sie die Anzahl der Aktivierungen-Protokolle über das `--limit` Flag:
      `$ aio app logs --limit=25`
1. Die letzten (bis zu den angegebenen `--limit`) Aktivierungen-Protokolle werden als Ausgabe des Befehls zur Überprüfung zurückgegeben.

   ![App-Protokolle](./assets/debug/aio-app-logs.png)

## Fehlerbehebung

+ [Debugger wird nicht angehängt](../troubleshooting.md#debugger-does-not-attach)
+ [Haltepunkte werden nicht angehalten](../troubleshooting.md#breakpoints-no-pausing)
+ [VS-Code-Debugger nicht angehängt](../troubleshooting.md#vs-code-debugger-not-attached)
+ [VS-Code-Debugger nach Beginn der Arbeitsausführung angehängt](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Zeitüberschreitung beim Debugging](../troubleshooting.md#worker-times-out-while-debugging)
+ [Debug-Prozess kann nicht beendet werden](../troubleshooting.md#cannot-terminate-debugger-process)
