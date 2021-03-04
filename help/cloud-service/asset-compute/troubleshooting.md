---
title: Fehlerbehebung bei der Asset compute-Erweiterbarkeit für AEM Assets
description: Im Folgenden finden Sie eine Übersicht über häufig auftretende Probleme und Fehler sowie die entsprechenden Lösungen, die beim Entwickeln und Bereitstellen von benutzerdefinierten Asset compute-Workern für AEM Assets auftreten können.
feature: asset compute Microservices
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1249'
ht-degree: 0%

---


# Fehlerbehebung bei Asset compute-Erweiterbarkeit

Im Folgenden finden Sie eine Übersicht über häufig auftretende Probleme und Fehler sowie die entsprechenden Lösungen, die beim Entwickeln und Bereitstellen von benutzerdefinierten Asset compute-Workern für AEM Assets auftreten können.

## Entwicklung{#develop}

### Die Darstellung wird teilweise gezeichnet/beschädigt zurückgegeben{#rendition-returned-partially-drawn-or-corrupt}

+ __Fehler__: Die Darstellung wird unvollständig (wenn ein Bild beschädigt ist) oder beschädigt dargestellt und kann nicht geöffnet werden.

   ![Darstellung wird teilweise gezeichnet zurückgegeben](./assets/troubleshooting/develop__await.png)

+ __Ursache__: Die  `renditionCallback` Funktion des Arbeitnehmers wird beendet, bevor die Darstellung vollständig in geschrieben werden kann  `rendition.path`.
+ __Auflösung__: Überprüfen Sie den benutzerdefinierten Worker-Code und stellen Sie sicher, dass alle asynchronen Aufrufe synchron mit  `await`erfolgen.

## Entwicklungstool{#development-tool}

### Console.json-Datei fehlt im Asset compute-Projekt{#missing-console-json}

+ __Fehler:__ Fehler: Fehlende erforderliche Dateien bei der Überprüfung (...)/node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY) bei async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __Ursache:__ Die  `console.json` Datei fehlt im Stammverzeichnis des Asset compute-Projekts
+ __Lösung:__ Laden Sie ein neues  `console.json` Formular für Ihr Adobe I/O-Projekt herunter
   1. Öffnen Sie in console.adobe.io das Adobe I/O-Projekt, für das das Asset compute konfiguriert ist,
   1. Tippen Sie oben rechts auf die Schaltfläche __Download__
   1. Speichern Sie die heruntergeladene Datei unter dem Dateinamen `console.json` im Stammverzeichnis Ihres Asset compute-Projekts.

### Falscher YAML-Einzug in manifest.yml{#incorrect-yaml-indentation}

+ __Fehler:__ YAMLException: Ungültiger Einzug eines Zuordnungseintrags in Zeile X, Spalte Y:(über Standard ab  `aio app run` Befehl)
+ __Ursache:__ Bei Yaml-Dateien wird der Einzug in weißen Abständen angezeigt. Der Einzug ist wahrscheinlich nicht korrekt.
+ __Lösung:__ Überprüfen Sie Ihren Einzug  `manifest.yml` und stellen Sie sicher, dass er korrekt ist.

### memorySize limit ist auf low{#memorysize-limit-is-set-too-low} eingestellt

+ __Fehler:OpenWhiskError für__  lokalen Dev-Server: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 (Ungültige Anforderung) zurückgegeben —> &quot;Der Anforderungsinhalt war fehlerhaft:Anforderung fehlgeschlagen: Speicher 64 MB unter dem zulässigen Schwellenwert von 134217728 B&quot;
+ __Ursache:__ Eine  `memorySize` Beschränkung für den Arbeiter im  `manifest.yml` wurde unter dem erlaubten Mindestschwellenwert festgelegt, wie in der Fehlermeldung in Byte angegeben.
+ __Lösung:__  Überprüfen Sie die  `memorySize` Grenzwerte in der  `manifest.yml` und stellen Sie sicher, dass sie alle den erlaubten Mindestschwellenwert überschreiten.

### Das Entwicklungstool kann aufgrund fehlender privater.key{#missing-private-key}-Zeichenfolge nicht Beginn werden

+ __Fehler:__ Local Dev ServerError: Erforderliche Dateien bei validatePrivateKeyFile fehlen.... (über den Standardabgleich von `aio app run`-Befehl)
+ __Ursache:__ Der  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der  `.env` Datei verweist nicht auf  `private.key` oder  `private.key` ist vom aktuellen Benutzer nicht lesbar.
+ __Auflösung:__ Überprüfen Sie den  `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der  `.env` Datei und stellen Sie sicher, dass er den vollständigen, absoluten Pfad zum  `private.key` Dateisystem enthält.

### Dropdown-Liste der Quelldateien falsch{#source-files-dropdown-incorrect}

asset compute Development Tool kann einen Status eingeben, in dem statische Daten abgerufen werden. Am auffälligsten ist dies bei der Dropdown-Liste __Quelldatei__, in der falsche Elemente angezeigt werden.

+ __Fehler: Im Dropdown-Menü__ Quelldatei werden falsche Elemente angezeigt.
+ __Ursache:Der__ Status &quot;Blocker Cache&quot;verursacht die
+ __Lösung:__ Im Browser müssen Sie den &quot;Anwendungszustand&quot;, den Browser-Cache, die lokale Datenspeicherung und den Service-Mitarbeiter der Browser-Registerkarte vollständig löschen.

### Fehlender oder ungültiger Parameter für die devToolToken-Abfrage{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fehler:__  Benachrichtigung &quot;Nicht autorisiert&quot;im Asset compute Development Tool
+ __Ursache:__ `devToolToken` fehlt oder ist ungültig
+ __Lösung:__ Schließen Sie das Browserfenster des Asset compute Development Tool, beenden Sie alle laufenden Entwicklungstool-Prozesse, die über den  `aio app run` Befehl initiiert wurden, und starten Sie das Development Tool erneut (unter Verwendung  `aio app run`).

### Quelldateien können nicht entfernt werden{#unable-to-remove-source-files}

+ __Fehler:__ Es gibt keine Möglichkeit, hinzugefügte Quelldateien aus der Benutzeroberfläche der Entwicklungstools zu entfernen
+ __Ursache:__ Diese Funktion wurde nicht implementiert
+ __Lösung:__ Melden Sie sich mit den in  `.env`definierten Anmeldeinformationen bei Ihrem Cloud-Datenspeicherung-Provider an. Suchen Sie den Container, der von den Entwicklungstools verwendet wird (auch in `.env` angegeben), navigieren Sie zum Ordner __source__ und löschen Sie alle Quellbilder. Möglicherweise müssen Sie die unter [Dropdown-Liste &quot;Quelldateien&quot;beschriebenen Schritte ausführen, die nicht korrekt](#source-files-dropdown-incorrect) sind, wenn die gelöschten Quelldateien weiterhin im Dropdown-Menü angezeigt werden, da sie lokal im &quot;Anwendungszustand&quot;der Entwicklungstools zwischengespeichert werden können.

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Keine Darstellung während der Testausführung generiert{#test-no-rendition-generated}

+ __Fehler:__ Fehler: Keine Darstellung generiert.
+ __Ursache:__ Der Arbeiter konnte aufgrund eines unerwarteten Fehlers wie einem JavaScript-Syntaxfehler keine Darstellung erstellen.
+ __Lösung:__ Überprüfen Sie die Testausführung  `test.log` unter  `/build/test-results/test-worker/test.log`. Suchen Sie den Abschnitt in dieser Datei, der dem fehlerhaften Testfall entspricht, und überprüfen Sie, ob Fehler vorliegen.

   ![Fehlerbehebung - Keine Darstellung generiert](./assets/troubleshooting/test__no-rendition-generated.png)

### Test generiert eine falsche Darstellung, wodurch der Test fehlschlägt{#tests-generates-incorrect-rendition}

+ __Fehler:__ Fehler: Darstellung &#39;rendition.xxx&#39; nicht wie erwartet.
+ __Ursache:__ Der Arbeiter gibt eine Darstellung aus, die nicht mit der im Testfall  `rendition.<extension>` bereitgestellten identisch war.
   + Wenn die erwartete `rendition.<extension>`-Datei nicht genau so erstellt wird wie die lokal generierte Darstellung im Testfall, schlägt der Test möglicherweise fehl, da die Bits etwas voneinander abweichen können. Wenn der Asset compute-Worker beispielsweise den Kontrast mithilfe von APIs ändert und das erwartete Ergebnis durch Anpassen des Kontrasts in Adobe Photoshop CC erstellt wird, können die Dateien gleich aussehen, aber kleinere Varianten der Bits können unterschiedlich sein.
+ __Auflösung:__ Überprüfen Sie die Ausgabe der Darstellung vom Test, indem Sie zu navigieren  `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`und sie mit der erwarteten Darstellungsdatei im Testfall vergleichen. So erstellen Sie ein exakt erwartetes Asset:
   + Verwenden Sie das Entwicklungstool, um eine Darstellung zu erstellen, zu überprüfen, ob sie korrekt ist, und verwenden Sie diese als erwartete Darstellungsdatei
   + Oder überprüfen Sie die testgenerierte Datei bei `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, überprüfen Sie sie auf ihre Richtigkeit und verwenden Sie sie als erwartete Darstellungsdatei.

## Debug

### Debugger wird nicht angehängt{#debugger-does-not-attach}

+ __Fehler__: Fehlerverarbeitungsstart: Fehler: Verbindung zur Debug-Zielgruppe konnte nicht hergestellt werden...
+ __Ursache__: Docker Desktop wird nicht auf dem lokalen System ausgeführt. Überprüfen Sie dies, indem Sie die VS-Code-Debug-Konsole (Ansicht > Debug-Konsole) überprüfen und bestätigen, dass dieser Fehler gemeldet wird.
+ __Auflösung__: Beginn  [Docker Desktop und überprüfen Sie, ob die erforderlichen Docker-Bilder installiert](./set-up/development-environment.md#docker) sind.

### Haltepunkte werden nicht angehalten{#breakpoints-no-pausing}

+ __Fehler__: Beim Ausführen des Asset compute-Workers über das debug-fähige Entwicklungstool wird der VS-Code an Haltepunkten nicht angehalten.

#### VS-Code-Debugger nicht angehängt{#vs-code-debugger-not-attached}

+ __Ursache:__ Der VS-Code-Debugger wurde angehalten/getrennt.
+ __Auflösung:__ Starten Sie den VS-Code-Debugger neu und überprüfen Sie, ob er angehängt wird, indem Sie die Konsole &quot;VS-Code-Debug-Ausgabe&quot;(Ansicht > Debug-Konsole)

#### VS-Code-Debugger angehängt, nachdem die Ausführung des Arbeitnehmers begonnen hat{#vs-code-debugger-attached-after-worker-execution-began}

+ __Ursache:__ Der VS-Code-Debugger wurde nicht vor dem Tippen auf das  ____ Runin Development Tool angehängt.
+ __Lösung:__ Vergewissern Sie sich, dass der Debugger angehängt wurde, indem Sie die Debug-Konsole des VS-Codes (Ansicht > Debug-Konsole) überprüfen und dann den Asset compute-Worker aus dem Entwicklungstool erneut ausführen.

### Worker-Timeout beim Debugging{#worker-times-out-while-debugging}

+ __Fehler__: Debug-Konsolenberichte &quot;Action will timeout in -XXX Millisekunden&quot;oder die  [Ausgabeformations-Vorschau des ](./develop/development-tool.md) Asset compute-Entwicklungstools werden unendlich oder
+ __Ursache__: Das Zeitlimit des Workers, wie in  [manifest.](./develop/manifest.md) ymlis definiert, wurde während des Debuggens überschritten.
+ __Auflösung__: Erhöhen Sie vorübergehend die Zeitüberschreitung des Workers in den  [Aktivitäten manifest.](./develop/manifest.md) ymlor, um das Debugging zu beschleunigen.

### Debug-Prozess{#cannot-terminate-debugger-process} kann nicht beendet werden

+ __Fehler__:  `Ctrl-C` in der Befehlszeile wird der Debugger-Prozess nicht beendet (`npx adobe-asset-compute devtool`).
+ __Ursache__: Ein Fehler in  `@adobe/aio-cli-plugin-asset-compute` 1.3.x führt dazu, dass der Befehl zum Beenden  `Ctrl-C` nicht erkannt wird.
+ __Auflösung__: Aktualisierung  `@adobe/aio-cli-plugin-asset-compute` auf Version 1.4.1+

   ```
   $ aio update
   ```

   ![Fehlerbehebung - Aktualisierung](./assets/troubleshooting/debug__terminate.png)

## Bereitstellen{#deploy}

### Benutzerdefinierte Darstellung fehlt im Asset in AEM{#custom-rendition-missing-from-asset}

+ __Fehler:__ Neue und erneut verarbeitete Assets werden erfolgreich verarbeitet, aber die benutzerdefinierte Darstellung fehlt

#### Verarbeitendes Profil, das nicht auf übergeordneten Ordner angewendet wird

+ __Ursache:__ Das Asset ist nicht in einem Ordner mit dem Verarbeitungsordner vorhanden, in dem der benutzerdefinierte Worker verwendet wird
+ __Auflösung:__ Wenden Sie das Profil &quot;Verarbeitung&quot;auf einen übergeordneten Ordner des Assets an

#### Profil wird durch Profil der unteren Verarbeitung ersetzt

+ __Ursache:__ Das Asset befindet sich unter einem Ordner, auf den das benutzerdefinierte Worker Processing-Profil angewendet wurde. Es wurde jedoch ein anderes Verarbeitungsordner angewendet, das den Kundendienstmitarbeiter nicht verwendet, und das Asset wurde mit diesem Profil verknüpft.
+ __Auflösung:__ Kombinieren oder anderweitig abgleichen Sie die beiden verarbeitenden Profil und entfernen Sie das Profil für die Zwischenverarbeitung

### Die Elementverarbeitung schlägt bei AEM{#asset-processing-fails} fehl

+ __Fehler: Fehler bei der__ Asset-Verarbeitung: Markierung für Asset fehlgeschlagen
+ __Ursache:__ Bei der Ausführung des benutzerdefinierten Workers ist ein Fehler aufgetreten
+ __Lösung:__ Befolgen Sie die Anweisungen zum  [Debugging von Adobe I/O Runtime-](./test-debug/debug.md#aio-app-logs) Aktivierungen  `aio app logs`.


