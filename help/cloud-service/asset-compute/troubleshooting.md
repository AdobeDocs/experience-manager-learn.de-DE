---
title: Fehlerbehebung bei der Asset compute-Erweiterbarkeit für AEM Assets
description: Im Folgenden finden Sie einen Index häufiger Probleme und Fehler sowie Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset compute-Sekundären für AEM Assets auftreten können.
feature: asset compute Microservices
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 0%

---


# Fehlerbehebung bei der Asset compute-Erweiterbarkeit

Im Folgenden finden Sie einen Index häufiger Probleme und Fehler sowie Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset compute-Sekundären für AEM Assets auftreten können.

## Entwickeln{#develop}

### Das Ausgabeformat wird teilweise gezeichnet/beschädigt{#rendition-returned-partially-drawn-or-corrupt} zurückgegeben

+ __Fehler__: Die Ausgabedarstellung wird nicht vollständig (wenn ein Bild beschädigt ist) oder nicht geöffnet.

   ![Ausgabedarstellung wird teilweise gezeichnet zurückgegeben](./assets/troubleshooting/develop__await.png)

+ __Ursache__: Die  `renditionCallback` Funktion des Sekundärs wird beendet, bevor die Ausgabedarstellung vollständig in  `rendition.path`geschrieben werden kann.
+ __Auflösung__: Überprüfen Sie den benutzerdefinierten Worker-Code und stellen Sie sicher, dass alle asynchronen Aufrufe mit synchronisiert werden  `await`.

## Entwicklungstool{#development-tool}

### Datei &quot;Console.json&quot;fehlt im Asset compute-Projekt{#missing-console-json}

+ __Fehler:__ Fehler: Fehlende erforderliche Dateien bei Validierung (.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) bei async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __Ursache:__ Die  `console.json` Datei fehlt im Stammverzeichnis des Asset compute-Projekts
+ __Lösung:__ Laden Sie ein neues  `console.json` Formular für Ihr Adobe I/O-Projekt herunter.
   1. Öffnen Sie in console.adobe.io das Adobe I/O-Projekt, für das das Asset compute-Projekt konfiguriert ist.
   1. Tippen Sie oben rechts auf die Schaltfläche __Download__ .
   1. Speichern Sie die heruntergeladene Datei im Stammverzeichnis Ihres Asset compute-Projekts unter Verwendung des Dateinamens `console.json`.

### Falscher YAML-Einzug in manifest.yml{#incorrect-yaml-indentation}

+ __Fehler:__ YAMLException: schlechter Einzug eines Zuordnungseintrags in Zeile X, Spalte Y: (standardmäßig aus  `aio app run` Befehl)
+ __Ursache:__ Bei Yaml-Dateien wird der weiße Abstand berücksichtigt. Wahrscheinlich ist der Einzug falsch.
+ __Lösung:__ Überprüfen Sie Ihre  `manifest.yml` und stellen Sie sicher, dass alle Einzüge korrekt sind.

### memorySize limit ist auf low{#memorysize-limit-is-set-too-low} gesetzt

+ __Fehler:__  Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Zurückgegeben HTTP 400 (Ungültige Anfrage) —> &quot;Der Anfrageinhalt war fehlerhaft:Anforderung fehlgeschlagen: Arbeitsspeicher 64 MB unter dem zulässigen Schwellenwert von 134217728 B&quot;
+ __Ursache:__ Eine  `memorySize` Begrenzung für den Worker in  `manifest.yml` wurde unter dem erlaubten Mindestschwellenwert festgelegt, der von der Fehlermeldung in Byte gemeldet wird.
+ __Lösung:__  Prüfen Sie die  `memorySize` Grenzwerte in der  `manifest.yml` und stellen Sie sicher, dass alle über dem erlaubten Mindestschwellenwert liegen.

### Das Entwicklungstool kann nicht gestartet werden, da private.key{#missing-private-key} fehlt.

+ __Fehler:__ Local Dev ServerError: Fehlende erforderliche Dateien bei validatePrivateKeyFile... (über den Standardbefehl `aio app run` )
+ __Ursache:__ Der  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der  `.env` Datei verweist nicht auf  `private.key` oder  `private.key` kann vom aktuellen Benutzer nicht gelesen werden.
+ __Lösung:__ Überprüfen Sie den  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der  `.env` Datei und stellen Sie sicher, dass er den vollständigen, absoluten Pfad zum  `private.key` in Ihrem Dateisystem enthält.

### Dropdown-Liste der Quelldateien falsch{#source-files-dropdown-incorrect}

Das asset compute Development Tool kann einen Status eingeben, in dem veraltete Daten abgerufen werden. Am auffälligsten ist dies im Dropdown-Menü __Quelldatei__, in dem falsche Elemente angezeigt werden.

+ __Fehler:__ Das Dropdown-Menü Quelldatei zeigt falsche Elemente an.
+ __Ursache:__ Der veraltete zwischengespeicherte Browserstatus verursacht die
+ __Lösung:__ Löschen Sie in Ihrem Browser den &quot;Anwendungsstatus&quot; der Browser-Registerkarte, den Browsercache, den lokalen Speicher und den Service Worker vollständig.

### Fehlender oder ungültiger devToolToken-Abfrageparameter{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fehler:__  Benachrichtigung &quot;Nicht autorisiert&quot;im Asset compute Development Tool
+ __Ursache:__ `devToolToken` fehlt oder ist ungültig
+ __Lösung:__ Schließen Sie das Browserfenster des Asset compute Development Tool, beenden Sie alle laufenden Entwicklungs-Tool-Prozesse, die über den  `aio app run` Befehl initiiert werden, und starten Sie das Entwicklungstool neu (unter Verwendung von  `aio app run`).

### Quelldateien können nicht entfernt werden{#unable-to-remove-source-files}

+ __Fehler:__ Es gibt keine Möglichkeit, hinzugefügte Quelldateien aus der Benutzeroberfläche der Entwicklungs-Tools zu entfernen
+ __Ursache:__ Diese Funktion wurde nicht implementiert
+ __Lösung:__ Melden Sie sich mit den in  `.env`definierten Anmeldeinformationen bei Ihrem Cloud-Speicher-Provider an. Suchen Sie den von den Entwicklungs-Tools verwendeten Container (ebenfalls angegeben in `.env`), navigieren Sie zum Ordner __source__ und löschen Sie alle Quellbilder. Möglicherweise müssen Sie die im Dropdown-Menü [Quelldateien, die falsch](#source-files-dropdown-incorrect) angezeigt werden, ausführen, wenn die gelöschten Quelldateien weiterhin im Dropdown-Menü angezeigt werden, da sie lokal im &quot;Anwendungsstatus&quot;der Entwicklungs-Tools zwischengespeichert werden können.

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Testen{#test}

### Keine Wiedergabe während der Testausführung generiert{#test-no-rendition-generated}

+ __Fehler:__ Fehler: Keine Ausgabedarstellung generiert.
+ __Ursache:__ Der Worker konnte aufgrund eines unerwarteten Fehlers wie einem JavaScript-Syntaxfehler keine Ausgabedarstellung generieren.
+ __Lösung:__ Überprüfen Sie die Testausführung  `test.log` unter  `/build/test-results/test-worker/test.log`. Suchen Sie den Abschnitt in dieser Datei, der dem fehlerhaften Testfall entspricht, und überprüfen Sie auf Fehler.

   ![Fehlerbehebung - Keine Ausgabedarstellung generiert](./assets/troubleshooting/test__no-rendition-generated.png)

### Test generiert eine falsche Ausgabedarstellung, wodurch der Test fehlschlägt{#tests-generates-incorrect-rendition}

+ __Fehler:__ Fehler: Ausgabe &quot;rendition.xxx&quot;nicht erwartungsgemäß.
+ __Ursache:__ Der Worker gibt eine Ausgabedarstellung aus, die nicht mit der im Testfall  `rendition.<extension>` bereitgestellten übereinstimmt.
   + Wenn die erwartete `rendition.<extension>`-Datei im Testfall nicht genau so wie die lokal generierte Ausgabedarstellung erstellt wird, schlägt der Test möglicherweise fehl, da die Bit möglicherweise etwas voneinander abweichen. Wenn der Asset compute Worker beispielsweise den Kontrast mithilfe von APIs ändert und das erwartete Ergebnis durch Anpassung des Kontrasts in Adobe Photoshop CC erstellt wird, können die Dateien gleich aussehen, doch können sich die Bit-Varianten unterscheiden.
+ __Lösung:__ Überprüfen Sie die Ausgabedarstellung des Tests, indem Sie zu  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` navigieren und sie im Testfall mit der erwarteten Ausgabedarstellungsdatei vergleichen. So erstellen Sie ein exakt erwartetes Asset:
   + Verwenden Sie das Entwicklungstool, um eine Ausgabedarstellung zu generieren, sie zu überprüfen und sie als erwartete Ausgabedarstellungsdatei zu verwenden.
   + Oder validieren Sie die test-generierte Datei unter `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, überprüfen Sie sie auf ihre Richtigkeit und verwenden Sie sie als erwartete Ausgabedarstellungsdatei.

## Debug

### Debugger hängt nicht an{#debugger-does-not-attach}

+ __Fehler__: Start der Fehlerverarbeitung: Fehler: Verbindung zum Debug-Ziel konnte nicht hergestellt werden unter..
+ __Ursache__: Docker Desktop wird nicht auf dem lokalen System ausgeführt. Überprüfen Sie dies, indem Sie die VS-Code-Debug-Konsole (Ansicht > Debug-Konsole) überprüfen und bestätigen, dass dieser Fehler gemeldet wird.
+ __Auflösung__: Starten Sie  [Docker Desktop und überprüfen Sie, ob die erforderlichen Docker-Bilder installiert](./set-up/development-environment.md#docker) sind.

### Haltepunkte werden nicht angehalten{#breakpoints-no-pausing}

+ __Fehler__: Beim Ausführen des Asset compute Worker über das debug-fähige Entwicklungstool wird VS Code an Haltepunkten nicht angehalten.

#### VS-Code-Debugger nicht angehängt{#vs-code-debugger-not-attached}

+ __Ursache:__ Der VS-Code-Debugger wurde angehalten/getrennt.
+ __Lösung:__ Starten Sie den VS-Code-Debugger neu und überprüfen Sie die Anhänge, indem Sie die VS Code Debug Output Console (Ansicht > Debug-Konsole) ansehen.

#### VS-Code-Debugger hinzugefügt, nachdem die Ausführung des Workflows gestartet wurde{#vs-code-debugger-attached-after-worker-execution-began}

+ __Ursache:__ Der VS-Code-Debugger wurde nicht vor dem Tippen auf das  ____ Runin Development Tool angehängt.
+ __Lösung:__ Stellen Sie sicher, dass der Debugger angehängt ist, indem Sie die Debug-Konsole von VS Code (Ansicht > Debug-Konsole) überprüfen und dann den Asset compute Worker über das Entwicklungstool erneut ausführen.

### Worker-Timeout beim Debugging{#worker-times-out-while-debugging}

+ __Fehler__: Debug Console meldet &quot;Action will timeout in -XXX Millisekunden&quot;oder die Ausgabevorschau-Rotation des  [Asset compute Development Tool für ](./develop/development-tool.md) die Wiedergabe auf unbestimmte Zeit oder
+ __Ursache__: Das Worker-Timeout, wie im  [manifest.](./develop/manifest.md) ymlis definiert, wurde beim Debugging überschritten.
+ __Auflösung__: Erhöhen Sie vorübergehend die Zeitüberschreitung des Sekundärs im  [manifest.](./develop/manifest.md) ymlor beschleunigen Sie die Debugging-Aktivitäten.

### Debug-Prozess kann nicht beendet werden{#cannot-terminate-debugger-process}

+ __Fehler__:  `Ctrl-C` in der Befehlszeile beendet den Debugger-Prozess nicht (`npx adobe-asset-compute devtool`).
+ __Ursache__: Ein Fehler in  `@adobe/aio-cli-plugin-asset-compute` Version 1.3.x führt dazu, dass  `Ctrl-C` kein Befehl zum Beenden erkannt wird.
+ __Auflösung__: Aktualisierung  `@adobe/aio-cli-plugin-asset-compute` auf Version 1.4.1+

   ```
   $ aio update
   ```

   ![Fehlerbehebung - aio-Aktualisierung](./assets/troubleshooting/debug__terminate.png)

## Bereitstellen{#deploy}

### Benutzerdefinierte Ausgabedarstellung fehlt im Asset in AEM{#custom-rendition-missing-from-asset}

+ __Fehler:__ Neue und erneut verarbeitete Assets werden erfolgreich verarbeitet, aber die benutzerdefinierte Ausgabe fehlt

#### Verarbeitungsprofil wird nicht auf Vorgängerordner angewendet

+ __Ursache:__  Das Asset ist nicht in einem Ordner mit dem Verarbeitungsprofil vorhanden, der den benutzerdefinierten Worker verwendet
+ __Lösung:__ Wenden Sie das Verarbeitungsprofil auf einen Vorgängerordner des Assets an

#### Verarbeitungsprofil wird durch ein niedrigeres Verarbeitungsprofil ersetzt

+ __Ursache:__ Das Asset befindet sich unter einem Ordner, auf den das benutzerdefinierte Worker-Verarbeitungsprofil angewendet wurde. Zwischen diesem Ordner und dem Asset wurde jedoch ein anderes Verarbeitungsprofil angewendet, das den Kunden nicht verwendet.
+ __Lösung:__ Kombinieren oder anderweitig abstimmen Sie die beiden Verarbeitungsprofile und entfernen Sie das Zwischen-Verarbeitungsprofil.

### Die Asset-Verarbeitung schlägt AEM{#asset-processing-fails} fehl

+ __Fehler:__ Badge zur Asset-Verarbeitung fehlgeschlagen wird auf Asset angezeigt
+ __Ursache:__ Bei der Ausführung des benutzerdefinierten Sekundärs ist ein Fehler aufgetreten.
+ __Lösung:__ Befolgen Sie die Anweisungen zum  [Debugging von Adobe I/O Runtime-](./test-debug/debug.md#aio-app-logs) Aktivierungen mit  `aio app logs`.


