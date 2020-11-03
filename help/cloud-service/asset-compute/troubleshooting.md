---
title: Fehlerbehebung bei der Erweiterbarkeit von Asset Compute für AEM Assets
description: Im Folgenden finden Sie eine Übersicht über häufig auftretende Probleme und Fehler sowie die entsprechenden Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset Compute-Mitarbeitern für AEM Assets auftreten können.
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Fehlerbehebung bei der Erweiterbarkeit von Asset Compute

Im Folgenden finden Sie eine Übersicht über häufig auftretende Probleme und Fehler sowie die entsprechenden Lösungen, die bei der Entwicklung und Bereitstellung von benutzerdefinierten Asset Compute-Mitarbeitern für AEM Assets auftreten können.

## Entwicklung{#develop}

### Darstellung wird teilweise gezeichnet/beschädigt zurückgegeben{#rendition-returned-partially-drawn-or-corrupt}

+ __Fehler__: Die Darstellung wird unvollständig (wenn ein Bild beschädigt ist) oder beschädigt dargestellt und kann nicht geöffnet werden.

   ![Darstellung wird teilweise gezeichnet zurückgegeben](./assets/troubleshooting/develop__await.png)

+ __Ursache__: Die `renditionCallback` Funktion des Arbeitnehmers wird beendet, bevor die Darstellung vollständig in geschrieben werden kann `rendition.path`.
+ __Auflösung__: Überprüfen Sie den benutzerdefinierten Worker-Code und stellen Sie sicher, dass alle asynchronen Aufrufe synchron mit `await`erfolgen.

## Entwicklungstool{#development-tool}

### Falscher YAML-Einzug in manifest.yml{#incorrect-yaml-indentation}

+ __Fehler:__ YAMLException: Ungültiger Einzug eines Zuordnungseintrags in Zeile X, Spalte Y:(über `aio app run` Befehl &quot;Standard ab&quot;)
+ __Ursache:__ Yaml-Dateien haben weiße Abstände, der Einzug ist wahrscheinlich nicht korrekt.
+ __Lösung:__ Überprüfen Sie Ihren Einzug `manifest.yml` und stellen Sie sicher, dass er korrekt ist.

### memorySize limit ist zu niedrig eingestellt{#memorysize-limit-is-set-too-low}

+ __Fehler:__  Local Dev Server OpenWhiskError: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true HTTP 400 (Ungültige Anforderung) zurückgegeben —> &quot;Der Anforderungsinhalt war fehlerhaft:Anforderung fehlgeschlagen: Speicher 64 MB unter dem zulässigen Schwellenwert von 134217728 B&quot;
+ __Ursache:__ Ein `memorySize` Grenzwert für den Arbeiter im `manifest.yml` wurde unter dem erlaubten Mindestschwellenwert festgelegt, der von der Fehlermeldung in Byte angegeben wurde.
+ __Lösung:__  Überprüfen Sie die `memorySize` Grenzwerte in der `manifest.yml` und stellen Sie sicher, dass sie alle größer als der erlaubte Mindestschwellenwert sind.

### Entwicklungstool kann aufgrund fehlender privater.key-Elemente nicht Beginn werden{#missing-private-key}

+ __Fehler:__ Local Dev ServerError: Erforderliche Dateien bei validatePrivateKeyFile fehlen.... (über &quot;Standard out from `aio app run` command&quot;)
+ __Ursache:__ Der `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der `.env` Datei verweist nicht auf den aktuellen Benutzer `private.key` `private.key` oder ist für diesen nicht lesbar.
+ __Lösung:__ Überprüfen Sie den `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der `.env` Datei und stellen Sie sicher, dass er den vollständigen, absoluten Pfad zum `private.key` Dateisystem enthält.

### Dropdown-Liste der Quelldateien falsch{#source-files-dropdown-incorrect}

Das Asset Compute Development Tool kann einen Status eingeben, in dem statische Daten abgerufen werden, und ist im Dropdown-Menü __Quelldatei__ mit falschen Elementen am auffälligsten.

+ __Fehler:__ Das Dropdown-Menü Quelldatei enthält falsche Elemente.
+ __Ursache:__ Der Status &quot;Nicht im Cache gespeicherter Browser&quot;verursacht
+ __Lösung:__ In Ihrem Browser löschen Sie vollständig den &quot;Anwendungszustand&quot; des Browser-Tab, den Browser-Cache, die lokale Datenspeicherung und den Service-Mitarbeiter.

### Fehlender oder ungültiger Parameter für die devToolToken-Abfrage{#missing-or-invalid-devtooltoken-query-parameter}

+ __Fehler:__ Benachrichtigung &quot;Nicht autorisiert&quot;im Asset Computing Development Tool
+ __Ursache:__ `devToolToken` fehlt oder ist ungültig
+ __Lösung:__ Schließen Sie das Browserfenster des Tools für die Asset-Berechnung, beenden Sie alle laufenden Entwicklungstool-Prozesse, die über den `aio app run` Befehl initiiert wurden, und starten Sie das Beginn-Entwicklungstool (unter Verwendung `aio app run`).

### Quelldateien können nicht entfernt werden{#unable-to-remove-source-files}

+ __Fehler:__ Es gibt keine Möglichkeit, hinzugefügte Quelldateien aus der Benutzeroberfläche der Entwicklungstools zu entfernen
+ __Ursache:__ Diese Funktion wurde nicht implementiert.
+ __Lösung:__ Melden Sie sich bei Ihrem Cloud-Anbieter für Datenspeicherung mit den unter `.env`&quot;Anmeldeinformationen&quot;definierten Anmeldeinformationen an. Suchen Sie den Container, der von den Entwicklungstools verwendet wird (siehe auch `.env`), navigieren Sie zum __Quellordner__ und löschen Sie alle Quellbilder. Möglicherweise müssen Sie die in der Dropdown-Liste [Quelldateien beschriebenen Schritte falsch](#source-files-dropdown-incorrect) ausführen, wenn die gelöschten Quelldateien weiterhin im Dropdown-Menü angezeigt werden, da sie lokal im &quot;Anwendungszustand&quot;der Entwicklungstools zwischengespeichert werden.

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## Test{#test}

### Keine Darstellung während der Testausführung generiert{#test-no-rendition-generated}

+ __Fehler:__ Fehler: Keine Darstellung generiert.
+ __Ursache:__ Der Arbeiter konnte aufgrund eines unerwarteten Fehlers, z. B. eines JavaScript-Syntaxfehlers, keine Darstellung erstellen.
+ __Lösung:__ Überprüfen Sie die Testausführung `test.log` unter `/build/test-results/test-worker/test.log`. Suchen Sie den Abschnitt in dieser Datei, der dem fehlerhaften Testfall entspricht, und überprüfen Sie, ob Fehler vorliegen.

   ![Fehlerbehebung - Keine Darstellung generiert](./assets/troubleshooting/test__no-rendition-generated.png)

### Test generiert eine falsche Darstellung, wodurch der Test fehlschlägt{#tests-generates-incorrect-rendition}

+ __Fehler:__ Fehler: Darstellung &#39;rendition.xxx&#39; nicht wie erwartet.
+ __Ursache:__ Der Worker gibt eine Darstellung aus, die nicht mit der im Testfall `rendition.<extension>` bereitgestellten identisch war.
   + Wenn die erwartete `rendition.<extension>` Datei nicht genau so erstellt wird wie die lokal generierte Darstellung im Testfall, schlägt der Test möglicherweise fehl, da die Bits etwas voneinander abweichen können. Wenn beispielsweise der Asset Compute-Mitarbeiter den Kontrast mithilfe von APIs ändert und das erwartete Ergebnis durch Anpassen des Kontrasts in Adobe Photoshop CC erstellt wird, können die Dateien gleich aussehen, aber kleinere Variationen der Bits können unterschiedlich sein.
+ __Lösung:__ Überprüfen Sie die Ausgabe der Darstellung aus dem Test, indem Sie zu dieser Datei navigieren `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`und sie mit der erwarteten Darstellungsdatei im Testfall vergleichen. So erstellen Sie ein exakt erwartetes Asset:
   + Verwenden Sie das Entwicklungstool, um eine Darstellung zu erstellen, zu überprüfen, ob sie korrekt ist, und verwenden Sie diese als erwartete Darstellungsdatei
   + Oder überprüfen Sie die testgenerierte Datei auf `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`, überprüfen Sie sie auf ihre Richtigkeit und verwenden Sie sie als erwartete Darstellungsdatei.

## Debug


### Debugger wird nicht angehängt{#debugger-does-not-attach}

+ __Fehler__: Fehlerverarbeitungsstart: Fehler: Verbindung zur Debug-Zielgruppe konnte nicht hergestellt werden...
+ __Ursache__: Docker Desktop wird nicht auf dem lokalen System ausgeführt. Überprüfen Sie dies, indem Sie die VS-Code-Debug-Konsole (Ansicht > Debug-Konsole) überprüfen und bestätigen, dass dieser Fehler gemeldet wird.
+ __Auflösung__: Beginn [Docker Desktop und überprüfen Sie, ob die erforderlichen Docker-Bilder installiert](./set-up/development-environment.md#docker)sind.

### Haltepunkte werden nicht angehalten{#breakpoints-no-pausing}

+ __Fehler__: Beim Ausführen des Assets Compute-Workers über das debug-fähige Entwicklungstool wird der VS-Code an Haltepunkten nicht angehalten.

#### VS-Code-Debugger nicht angehängt{#vs-code-debugger-not-attached}

+ __Ursache:__ Der VS-Code-Debugger wurde angehalten/getrennt.
+ __Lösung:__ Starten Sie den VS-Code-Debugger neu und überprüfen Sie, ob er angehängt wird, indem Sie die Konsole &quot;VS-Code-Debug-Ausgabe&quot;(&quot;Ansicht&quot;> &quot;Debug-Konsole&quot;) aufrufen.

#### VS-Code-Debugger nach Beginn der Arbeitsausführung angehängt{#vs-code-debugger-attached-after-worker-execution-began}

+ __Ursache:__ Der VS-Code-Debugger wurde nicht angehängt, bevor auf In Entwicklungstool __ausführen__ getippt wurde.
+ __Lösung:__ Vergewissern Sie sich, dass der Debugger angehängt wurde, indem Sie die Debug-Konsole des VS-Codes (Ansicht > Debug-Konsole) überprüfen und dann den Asset-Compute-Mitarbeiter aus dem Entwicklungstool erneut ausführen.

### Zeitüberschreitung beim Debugging{#worker-times-out-while-debugging}

+ __Fehler__: Debug-Konsolenberichte &quot;Action will timeout in -XXX Millisekunden&quot;oder [Asset Compute Development Tool&#39;s](./develop/development-tool.md) Darstellungs-Vorschau dreht sich unendlich oder
+ __Ursache__: Der in der Datei [manifest.yml](./develop/manifest.md) definierte Worker-Timeout wird während des Debuggens überschritten.
+ __Auflösung__: Erhöhen Sie vorübergehend den Timeout des Workers in der [Datei manifest.yml](./develop/manifest.md) oder beschleunigen Sie das Debugging-Aktivitäten.

### Debug-Prozess kann nicht beendet werden{#cannot-terminate-debugger-process}

+ __Fehler__: `Ctrl-C` in der Befehlszeile wird der Debugger-Prozess nicht beendet (`npx adobe-asset-compute devtool`).
+ __Ursache__: Ein Fehler in `@adobe/aio-cli-plugin-asset-compute` 1.3.x führt dazu, dass `Ctrl-C` er nicht als beendet erkannt wird.
+ __Auflösung__: Update `@adobe/aio-cli-plugin-asset-compute` auf Version 1.4.1+

   ```
   $ aio update
   ```

   ![Fehlerbehebung - Aktualisierung](./assets/troubleshooting/debug__terminate.png)

## Bereitstellen{#deploy}

### Benutzerdefinierte Darstellung fehlt im Asset in AEM{#custom-rendition-missing-from-asset}

+ __Fehler:__ Neue und erneut verarbeitete Assets werden erfolgreich verarbeitet, die benutzerdefinierte Darstellung fehlt jedoch

#### Verarbeitendes Profil, das nicht auf übergeordneten Ordner angewendet wird

+ __Ursache:__ Das Asset ist nicht in einem Ordner mit dem Verarbeitungsordner vorhanden, das den benutzerdefinierten Worker verwendet
+ __Lösung:__ Anwenden des Profils &quot;Verarbeitung&quot;auf einen übergeordneten Ordner des Assets

#### Profil wird durch Profil der unteren Verarbeitung ersetzt

+ __Ursache:__ Das Asset befindet sich unter einem Ordner, auf den das benutzerdefinierte Worker Processing-Profil angewendet wurde. Zwischen diesem Ordner und dem Asset wurde jedoch ein anderes Profil für die Verarbeitung angewendet, das den Kundenarbeiter nicht verwendet.
+ __Lösung:__ Kombinieren oder auf andere Weise abgleichen Sie die beiden VerarbeitungsProfil und entfernen Sie das Profil für die Zwischenverarbeitung

### Die Asset-Verarbeitung schlägt in AEM fehl{#asset-processing-fails}

+ __Fehler:__ Asset-Verarbeitung Fehlgeschlagene Markierung für Asset angezeigt
+ __Ursache:__ Bei der Ausführung des benutzerdefinierten Workers ist ein Fehler aufgetreten
+ __Lösung:__ Befolgen Sie die Anweisungen zum [Debugging von Adobe I/O Runtime-Aktivierungen](./test-debug/debug.md#aio-app-logs) mit `aio app logs`.


