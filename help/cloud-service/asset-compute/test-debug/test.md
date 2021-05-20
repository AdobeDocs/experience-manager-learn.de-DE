---
title: Asset compute Worker testen
description: Das Asset compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von Tests von Asset compute-Workern.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 1%

---


# Asset compute Worker testen

Das Asset compute-Projekt definiert ein Muster zum einfachen Erstellen und Ausführen von [Tests von Asset compute-Sekundären](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomie eines Worker-Tests

asset compute-Worker-Tests sind in Test-Suites unterteilt, und in jeder Test-Suite werden ein oder mehrere Testfälle durchgeführt, in denen eine Testbedingung bestätigt wird.

Die Teststruktur in einem Asset compute-Projekt sieht wie folgt aus:

```
/actions/<worker-name>/index.js
...
/test/
  asset-compute/
    <worker-name>/           <--- Test suite for the worker, must match the yaml key for this worker in manifest.yml
        <test-case-1>/       <--- Specific test case 
            file.jpg         <--- Input file (ie. `source.path` or `source.url`)
            params.json      <--- Parameters (ie. `rendition.instructions`)
            rendition.png    <--- Expected output file (ie. `rendition.path`)
        <test-case-2>/       <--- Another specific test case for this worker
            ...
```

Jeder Testcast kann die folgenden Dateien enthalten:

+ `file.<extension>`
   + Zu testende Quelldatei (Erweiterung kann alles außer `.link` sein)
   + Erforderlich
+ `rendition.<extension>`
   + Erwartete Ausgabedarstellung
   + Erforderlich, außer für Fehlertests
+ `params.json`
   + JSON-Anweisungen für einzelne Ausgabedarstellungen
   + Optional
+ `validate`
   + Ein Skript, das erwartete und tatsächliche Ausgabedarstellungs-Dateipfade als Argumente abruft und Exitcode 0 zurückgeben muss, wenn das Ergebnis in Ordnung ist, oder einen Exitcode ungleich null, wenn die Überprüfung oder der Vergleich fehlgeschlagen ist.
   + Optional, standardmäßig der Befehl `diff`
   + Verwenden Sie ein Shell-Skript, das einen Docker-Ausführungsbefehl für die Verwendung verschiedener Validierungs-Tools umbricht
+ `mock-<host-name>.json`
   + JSON-formatierte HTTP-Antworten für [das Nachahmen externer Dienstaufrufe](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optional, nur verwendet, wenn der Worker-Code eigene HTTP-Anforderungen sendet

## Schreiben eines Testfalls

In diesem Testfall wird die parametrisierte Eingabe (`params.json`) für die Eingabedatei (`file.jpg`) bestätigt, die die erwartete PNG-Wiedergabe (`rendition.png`) generiert.

1. Löschen Sie zunächst den automatisch generierten Testfall `simple-worker` unter `/test/asset-compute/simple-worker`, da dieser ungültig ist, da unser Worker die Quelle nicht mehr einfach in die Ausgabedarstellung kopiert.
1. Erstellen Sie unter `/test/asset-compute/worker/success-parameterized` einen neuen Ordner für Testfälle, um eine erfolgreiche Ausführung des Sekundärs zu testen, der eine PNG-Ausgabedarstellung generiert.
1. Fügen Sie im Ordner `success-parameterized` die Eingabedatei [test ](./assets/test/success-parameterized/file.jpg) für diesen Testfall hinzu und nennen Sie sie `file.jpg`.
1. Fügen Sie im Ordner `success-parameterized` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Sekundärs definiert:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Hierbei handelt es sich um dieselben Schlüssel/Werte, die an die Asset compute-Profildefinition des [Entwicklungs-Tools](../develop/development-tool.md) übergeben werden, abzüglich des Schlüssels `worker`.

1. Fügen Sie diesem Testfall die erwartete [Ausgabedarstellungsdatei](./assets/test/success-parameterized/rendition.png) hinzu und nennen Sie sie `rendition.png`. Diese Datei stellt die erwartete Ausgabe des Sekundärs für die angegebene Eingabe dar `file.jpg`.
1. Führen Sie in der Befehlszeile die Tests für den Projektstamm durch, indem Sie `aio app test` ausführen.
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet sind.
   + Beenden laufender Entwicklungstool-Instanzen

![Test - Erfolg  ](./assets/test/success-parameterized/result.png)

## Schreiben eines Testfalls zur Fehlerprüfung

In diesem Testfall wird geprüft, ob der Worker den entsprechenden Fehler ausgibt, wenn der Parameter `contrast` auf einen ungültigen Wert gesetzt ist.

1. Erstellen Sie einen neuen Ordner für Testfälle unter `/test/asset-compute/worker/error-contrast` , um eine fehlerhafte Ausführung des Sekundärs aufgrund eines ungültigen `contrast`-Parameterwerts zu testen.
1. Fügen Sie im Ordner `error-contrast` die Eingabedatei [test ](./assets/test/error-contrast/file.jpg) für diesen Testfall hinzu und nennen Sie sie `file.jpg`. Der Inhalt dieser Datei ist für diesen Test nicht wesentlich. Er muss nur vorhanden sein, um die Prüfung &quot;Beschädigte Quelle&quot;zu beenden, damit die `rendition.instructions`-Validierungsprüfungen, die dieser Testfall validiert, erreicht werden können.
1. Fügen Sie im Ordner `error-contrast` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Sekundärs mit dem Inhalt definiert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Setzen Sie die Parameter `contrast` auf `10`, einen ungültigen Wert, da der Kontrast zwischen -1 und 1 liegen muss, um einen `RenditionInstructionsError`-Wert zu erzeugen.
   + Stellen Sie sicher, dass der entsprechende Fehler in Tests ausgegeben wird, indem Sie die `errorReason`-Taste auf den &quot;Grund&quot;setzen, der dem erwarteten Fehler zugeordnet ist. Dieser ungültige Kontrastparameter gibt den benutzerdefinierten Fehler [a1/>, `RenditionInstructionsError` aus. Setzen Sie daher `errorReason` auf den Grund dieses Fehlers, oder`rendition_instructions_error`, um zu bestätigen, dass er ausgegeben wird.](../develop/worker.md#errors)

1. Da während einer fehlerhaften Ausführung keine Ausgabedarstellung generiert werden sollte, ist keine `rendition.<extension>`-Datei erforderlich.
1. Führen Sie die Test-Suite aus dem Stammverzeichnis des Projekts aus, indem Sie den Befehl `aio app test` ausführen.
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet sind.
   + Beenden laufender Entwicklungstool-Instanzen

![Test - Fehlerkontrast](./assets/test/error-contrast/result.png)

## Testfälle bei GitHub

Die endgültigen Testfälle sind auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Fehlerbehebung

+ [Keine Ausgabedarstellung während der Testausführung generiert](../troubleshooting.md#test-no-rendition-generated)
+ [Test generiert falsche Ausgabedarstellung](../troubleshooting.md#tests-generates-incorrect-rendition)
