---
title: Fehlerbehebung bei der Asset Compute-Erweiterung für AEM Assets
description: Im Folgenden finden Sie ein Verzeichnis häufiger Probleme und Fehler sowie Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset Compute-Sekundären für AEM Assets auftreten können.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 355
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 100%

---

# Fehlerbehebung bei der Asset Compute-Erweiterung

Im Folgenden finden Sie ein Verzeichnis häufiger Probleme und Fehler sowie Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset Compute-Sekundären für AEM Assets auftreten können.

## Entwickeln{#develop}

### Ausgabedarstellung wird teilweise/beschädigt zurückgegeben{#rendition-returned-partially-drawn-or-corrupt}

+ __Fehler__: Die Ausgabedarstellung wird (bei einem Bild) nicht vollständig gerendert oder ist beschädigt und kann nicht geöffnet werden.

  ![Die Ausgabedarstellung wird nur teilweise gezeichnet zurückgegeben](./assets/troubleshooting/develop__await.png)

+ __Ursache__: Die `renditionCallback`-Funktion des Sekundärs wird beendet, bevor die Ausgabedarstellung vollständig in `rendition.path` geschrieben wird.
+ __Lösung__: Überprüfen Sie den benutzerdefinierten Sekundär-Code und stellen Sie sicher, dass alle asynchronen Aufrufe mithilfe von `await` synchron gemacht werden.

## Entwicklungs-Tool{#development-tool}

### Datei „Console.json“ fehlt im Asset Compute-Projekt{#missing-console-json}

+ __Fehler:__ Error: Missing required files at validate (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) at async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY).
+ __Ursache:__ Die Datei `console.json` fehlt im Stammverzeichnis des Asset Compute-Projekts.
+ __Lösung:__ Laden Sie ein neues `console.json`-Formular aus Ihrem Adobe I/O-Projekt herunter:
   1. Öffnen Sie in console.adobe.io das Adobe I/O-Projekt, für das das Asset Compute-Projekt konfiguriert ist.
   1. Klicken Sie oben rechts auf die Schaltfläche __Herunterladen__.
   1. Speichern Sie die heruntergeladene Datei unter dem Dateinamen `console.json` im Stammverzeichnis Ihres Asset Compute-Projekts.

### Falscher YAML-Einzug in manifest.yml{#incorrect-yaml-indentation}

+ __Fehler:__ YAMLException: bad indentation of a mapping entry at line X, column Y: (über die Standardausgabe des Befehls `aio app run`).
+ __Ursache:__ Bei YAML-Dateien werden Leerzeichen berücksichtigt. Der Einzug ist wahrscheinlich falsch.
+ __Lösung:__ Überprüfen Sie die Datei `manifest.yml` und stellen Sie sicher, dass alle Einzüge korrekt sind.

### memorySize-Limit ist zu niedrig eingestellt{#memorysize-limit-is-set-too-low}

+ __Fehler:__ Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Returned HTTP 400 (Bad Request) --> &quot;The request content was malformed:requirement failed: memory 64 MB below allowed threshold of 134217728 B&quot;.
+ __Ursache:__ Ein `memorySize`-Limit für den Sekundär in der Datei `manifest.yml` wurde unterhalb des erlaubten Mindestschwellenwerts gesetzt, wie von der Fehlermeldung in Byte gemeldet.
+ __Lösung:__ Überprüfen Sie die `memorySize`-Limits in der Datei `manifest.yml` und stellen sicher, dass alle den erlaubten Mindestschwellenwert überschreiten.

### Entwicklungs-Tool kann nicht gestartet werden, da der private Schlüssel fehlt{#missing-private-key}

+ __Fehler:__ Local Dev ServerError: Missing required files at validatePrivateKeyFile… (über Standardausgabe des Befehls `aio app run`).
+ __Ursache:__ Der Wert `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` in der `.env`-Datei verweist nicht auf `private.key` oder `private.key` ist von der aktuellen Benutzerin bzw. dem aktuellen Benutzer nicht lesbar.
+ __Lösung:__ Überprüfen Sie den Wert `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` in der `.env`-Datei und stellen Sie sicher, dass er den vollständigen, absoluten Pfad zu `private.key` auf Ihrem Dateisystem enthält.

### Dropdown-Liste mit den Quelldateien falsch{#source-files-dropdown-incorrect}

Das Asset Compute-Entwicklungs-Tool kann in einen Status eintreten, in dem veraltete Daten abgerufen werden. Deutlich erkennbar wird dies dadurch, dass in der Dropdown-Liste mit den __Quelldateien__ falsche Elemente angezeigt werden.

+ __Fehler:__ Das Dropdown-Menü mit den Quelldateien zeigt falsche Elemente an.
+ __Ursache:__ Dies ist auf einen veralteten, im Cache gespeicherten Browser-Status zurückzuführen.
+ __Lösung:__ Löschen Sie in Ihrem Browser vollständig den „Anwendungsstatus“ der Browser-Registerkarte, den Browser-Cache, den lokalen Speicher und den Dienst-Sekundär.

### Fehlender oder ungültiger devToolToken-Abfrageparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fehler:__ Benachrichtigung „Nicht autorisiert“ im Asset Compute-Entwicklungs-Tool.
+ __Ursache:__ `devToolToken` fehlt oder ist ungültig.
+ __Lösung:__ Schließen Sie das Browser-Fenster des Asset Compute-Entwicklungs-Tools, beenden Sie alle laufenden Entwicklungs-Tool-Prozesse, die über den Befehl `aio app run` initiiert wurden, und starten Sie das Entwicklungs-Tool neu (mithilfe von `aio app run`).

### Quelldateien können nicht entfernt werden{#unable-to-remove-source-files}

+ __Fehler:__ Es gibt keine Möglichkeit, hinzugefügte Quelldateien aus der Benutzeroberfläche der Entwicklungs-Tools zu entfernen.
+ __Ursache:__ Diese Funktionalität wurde nicht implementiert.
+ __Lösung:__ Melden Sie sich mit den in `.env` definierten Anmeldeinformationen bei Ihrem Cloud-Speicheranbieter an. Suchen Sie den Container, der von den Entwicklungs-Tools verwendet wird (ebenfalls angegeben unter `.env`), navigieren Sie zum __Quellordner__ und löschen Sie alle Quellbilder. Ggf. müssen Sie die unter [Dropdown-Liste mit den Quelldateien falsch](#source-files-dropdown-incorrect) beschriebenen Schritte ausführen, wenn die gelöschten Quelldateien weiterhin im Dropdown-Menü angezeigt werden, da sie lokal im „Anwendungsstatus“ der Entwicklungs-Tools zwischengespeichert sein können.

  ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testen{#test}

### Keine Ausgabedarstellung während der Testausführung generiert{#test-no-rendition-generated}

+ __Fehler:__ Keine Ausgabedarstellung generiert.
+ __Ursache:__ Der Sekundär konnte aufgrund eines unerwarteten Fehlers wie einem JavaScript-Syntaxfehler keine Ausgabedarstellung generieren.
+ __Lösung:__ Überprüfen Sie nach der Testausführung `test.log` unter `/build/test-results/test-worker/test.log`. Suchen Sie den Abschnitt in dieser Datei, der dem fehlerhaften Testfall entspricht, und überprüfen Sie auf Fehler.

  ![Fehlerbehebung – keine Ausgabedarstellung generiert](./assets/troubleshooting/test__no-rendition-generated.png)

### Test generiert falsche Ausgabedarstellung, wodurch der Test fehlschlägt{#tests-generates-incorrect-rendition}

+ __Fehler:__ Failure: Rendition &#39;rendition.xxx&#39; not as expected.
+ __Ursache:__ Der Sekundär gibt eine Ausgabedarstellung aus, die nicht mit `rendition.<extension>` des Testfalls übereinstimmt.
   + Wenn die erwartete Datei `rendition.<extension>` nicht exakt auf die gleiche Weise erstellt wird wie die lokal generierte Ausgabedarstellung im Testfall, kann der Test fehlschlagen, da es einen gewissen Bit-Unterschied geben kann. Wenn der Asset Compute-Sekundär beispielsweise den Kontrast mithilfe von APIs ändert und das erwartete Ergebnis durch Anpassung des Kontrasts in Adobe Photoshop CC erstellt wird, können die Dateien zwar gleich aussehen, sich aber durch geringfügige Bit-Varianten unterscheiden.
+ __Lösung:__ Überprüfen Sie die Ausgabedarstellung vom Test, indem Sie zu `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` navigieren und mit der erwarteten Ausgabedarstellungsdatei im Testfall vergleichen. Wählen Sie eine der folgenden Möglichkeiten, um ein Asset zu erstellen, das genau den Erwartungen entspricht:
   + Verwenden Sie das Entwicklungs-Tool, um eine Ausgabedarstellung zu generieren, sie zu überprüfen und als erwartete Ausgabedarstellungsdatei zu verwenden.
   + Oder validieren Sie die durch Tests generierte Datei unter `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, überprüfen Sie, ob sie korrekt ist, und verwenden Sie diese als erwartete Ausgabedarstellungsdatei.

## Debuggen

### Debugger nicht angehängt{#debugger-does-not-attach}

+ __Fehler__: Error processing launch: Error: Could not connect ot debug target at…
+ __Ursache__: Docker Desktop wird nicht auf dem lokalen System ausgeführt. Überprüfen Sie dies, indem Sie die VS Code-Debugging-Konsole („Ansicht“ > „Debugging-Konsole“) überprüfen und bestätigen, dass dieser Fehler gemeldet wird.
+ __Lösung__: Starten Sie Docker Desktop und [bestätigen Sie, dass die erforderlichen Docker-Bilder installiert sind](./set-up/development-environment.md#docker).

### Breakpoints werden nicht angehalten{#breakpoints-no-pausing}

+ __Fehler__: Beim Ausführen des Asset Compute-Sekundär über das Debug-fähige Entwicklungs-Tool wird VS-Code nicht an Breakpoints angehalten.

#### VS-Code-Debugger nicht angehängt{#vs-code-debugger-not-attached}

+ __Ursache:__ Der VS-Code-Debugger wurde angehalten/getrennt.
+ __Lösung:__ Starten Sie den VS-Code-Debugger neu und überprüfen Sie, ob er verbunden wird, indem Sie die Konsole für das Debugging der VS-Code-Ausgabe („Ansicht“ > „Debugging-Konsole“) aufrufen.

#### VS-Code-Debugger nach Beginn der Sekundär-Ausführung angehängt{#vs-code-debugger-attached-after-worker-execution-began}

+ __Ursache:__ Der VS-Code-Debugger wurde nicht vor dem Klicken auf __Ausführen__ im Entwicklungs-Tool angehängt.
+ __Lösung:__ Stellen Sie sicher, dass der Debugger angehängt ist, indem Sie die VS-Code-Debugging-Konsole („Ansicht“ > „Debugging-Konsole“) aufrufen und dann den Asset Compute-Sekundär über das Entwicklungs-Tool erneut ausführen.

### Sekundär-Timeout beim Debugging{#worker-times-out-while-debugging}

+ __Fehler__: Die Debugging-Konsole meldet „Action will timeout in -XXX milliseconds“ oder die Ausgabedarstellungsvorschau des [Asset Compute-Entwicklungs-Tools](./develop/development-tool.md) läuft in einer Endlosschleife.
+ __Ursache__: Das Sekundär-Timeout gemäß Definition in [manifest.yml](./develop/manifest.md) wird beim Debugging überschritten.
+ __Lösung__: Erhöhen Sie vorübergehend den Wert für das Sekundär-Timeout in [manifest.yml](./develop/manifest.md) oder beschleunigen Sie das Debugging von Aktivitäten.

### Debugger-Prozess kann nicht beendet werden{#cannot-terminate-debugger-process}

+ __Fehler__: Der Debugger-Prozess lässt sich nicht durch `Ctrl-C` in der Befehlszeile beenden (`npx adobe-asset-compute devtool`).
+ __Ursache__: Ein Fehler in `@adobe/aio-cli-plugin-asset-compute` 1.3.x führt dazu, dass `Ctrl-C` nicht als Befehl zum Beenden erkannt wird.
+ __Lösung__: Aktualisieren Sie `@adobe/aio-cli-plugin-asset-compute` auf Version 1.4.1+.

  ```
  $ aio update
  ```

  ![Fehlerbehebung – aio-Aktualisierung](./assets/troubleshooting/debug__terminate.png)

## Bereitstellen{#deploy}

### Benutzerdefinierte Ausgabedarstellung fehlt in einem Asset in AEM{#custom-rendition-missing-from-asset}

+ __Fehler:__ Neue und erneut verarbeitete Assets werden erfolgreich verarbeitet, aber die benutzerdefinierte Ausgabe fehlt.

#### Das Verarbeitungsprofil wird nicht auf den Vorgängerordner angewendet

+ __Ursache:__ Das Asset ist nicht unter einem Ordner mit dem Verarbeitungsprofil vorhanden, das den benutzerdefinierten Sekundär verwendet.
+ __Lösung:__ Wenden Sie das Verarbeitungsprofil auf einen Vorgängerordner des Assets an.

#### Das Verarbeitungsprofil wird durch ein niedrigeres Verarbeitungsprofil ersetzt

+ __Ursache:__ Das Asset befindet sich unter einem Ordner, auf den das Verarbeitungsprofil mit benutzerdefiniertem Sekundär angewendet wurde. Zwischen diesem Ordner und dem Asset wurde jedoch ein anderes Verarbeitungsprofil angewendet, das den benutzerdefinierten Sekundär nicht verwendet.
+ __Lösung:__ Kombinieren Sie die beiden Verarbeitungsprofile oder stimmen Sie sie anderweitig ab und entfernen Sie das Zwischen-Verarbeitungsprofil.

### Asset-Verarbeitung schlägt in AEM fehl{#asset-processing-fails}

+ __Fehler:__ Ein Badge „Asset-Verarbeitung fehlgeschlagen“ wird für das Asset angezeigt.
+ __Ursache:__ Bei der Ausführung des benutzerdefinierten Sekundärs ist ein Fehler aufgetreten.
+ __Lösung:__ Befolgen Sie die Anweisungen zum [Debugging von Adobe I/O Runtime-Aktivierungen](./test-debug/debug.md#aio-app-logs) mithilfe von `aio app logs`.
