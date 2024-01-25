---
title: Testen eines Asset Compute-Sekundärs
description: Das Asset Compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von Tests von Asset Compute-Sekundären.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6284
thumbnail: KT-6284.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 04992caf-b715-4701-94a8-6257e9bd300c
duration: 175
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 100%

---

# Testen eines Asset Compute-Sekundärs

Das Asset Compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von [Tests von Asset Compute-Sekundären](https://experienceleague.adobe.com/docs/asset-compute/using/extend/test-custom-application.html?lang=de).

## Anatomie eines Sekundär-Tests

Asset Compute-Sekundär-Tests sind in Test-Suites unterteilt, und in jeder Test-Suite werden ein oder mehrere Testfälle durchgeführt, in denen eine Testbedingung angegeben wird.

Die Teststruktur in einem Asset Compute-Projekt sieht wie folgt aus:

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

Jeder Testentwurf kann die folgenden Dateien enthalten:

+ `file.<extension>`
   + Zu testende Quelldatei (Erweiterung kann alles außer `.link`)
   + Erforderlich
+ `rendition.<extension>`
   + Erwartete Ausgabedarstellung
   + Erforderlich, außer für Fehlertests
+ `params.json`
   + JSON-Anweisungen für einzelne Ausgabedarstellungen
   + Optional
+ `validate`
   + Ein Skript, das erwartete und tatsächliche Dateipfade von Ausgabedarstellungen als Argumente abruft und Exitcode 0 zurückgeben muss, wenn das Ergebnis in Ordnung ist, oder einen Exitcode ungleich null, wenn die Überprüfung oder der Vergleich fehlgeschlagen ist.
   + Optional, Standardwert ist der Befehl `diff`
   + Verwenden Sie ein Shell-Skript, das einen Docker-Ausführungsbefehl für die Verwendung verschiedener Validierungs-Tools umschließt
+ `mock-<host-name>.json`
   + JSON-formatierte HTTP-Antworten für [Mocking-Aufrufe externer Dienste](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optional, nur verwendet, wenn der Sekundär-Code eigene HTTP-Anfragen sendet

## Schreiben eines Testfalls

Dieser Testfall bestätigt, dass die parametrisierte Eingabe (`params.json`) für die Eingabedatei (`file.jpg`) die erwartete PNG-Wiedergabe (`rendition.png`) erzeugt.

1. Löschen Sie zunächst den automatisch generierten `simple-worker`-Testfall unter `/test/asset-compute/simple-worker`, da dieser ungültig ist, da unser Sekundär nicht mehr einfach die Quelle in die Wiedergabeversion kopiert.
1. Erstellen Sie einen neuen Testfallordner unter `/test/asset-compute/worker/success-parameterized`, um die erfolgreiche Ausführung des Sekundärs zu testen, der eine PNG-Wiedergabe erzeugt.
1. Fügen Sie im Ordner `success-parameterized` die Test-[Eingabedatei](./assets/test/success-parameterized/file.jpg) für diesen Testfall hinzu und nennen Sie sie `file.jpg`.
1. Fügen Sie im Ordner `success-parameterized` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Sekundär definiert:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Dies sind dieselben Schlüssel/Werte, die in die [Definition des Asset Compute-Profils des Entwicklungstools](../develop/development-tool.md) eingegeben werden, abzüglich des `worker`-Schlüssels.

1. Fügen Sie die erwartete [Ausgabedarstellungsdatei](./assets/test/success-parameterized/rendition.png) zu diesem Testfall hinzu und nennen Sie sie `rendition.png`. Diese Datei stellt die erwartete Ausgabe des Sekundärs für die gegebene Eingabe `file.jpg` dar.
1. Führen Sie in der Befehlszeile die Tests für den Projektstamm durch, indem Sie `aio app test` ausführen
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet sind
   + Beenden Sie alle laufenden Entwicklungs-Tool-Instanzen

![Test – Erfolgreich](./assets/test/success-parameterized/result.png)

## Schreiben eines Testfalls zur Fehlerprüfung

Dieser Testfall prüft, ob der Sekundär den entsprechenden Fehler auslöst, wenn der Parameter `contrast` auf einen ungültigen Wert gesetzt wird.

1. Erstellen Sie einen neuen Testfallordner unter `/test/asset-compute/worker/error-contrast`, um eine fehlerhafte Ausführung des Sekundärs aufgrund eines ungültigen `contrast`-Parameterwertes zu testen.
1. Fügen Sie im Ordner `error-contrast` die Test-[Eingabedatei](./assets/test/error-contrast/file.jpg) für diesen Testfall hinzu und nennen Sie sie `file.jpg`. Der Inhalt dieser Datei ist für diesen Test unerheblich, sie muss nur vorhanden sein, um die Prüfung „Korrupte Quelle“ zu überstehen, damit die `rendition.instructions`-Gültigkeitsprüfungen erreicht werden können, die in diesem Testfall überprüft werden.
1. Fügen Sie im Ordner `error-contrast` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Sekundärs mit dem Inhalt definiert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Setzen Sie den Parameter `contrast` auf `10`, einen ungültigen Wert, da der Kontrast zwischen -1 und 1 liegen muss, um einen `RenditionInstructionsError` auszulösen.
   + Stellen Sie sicher, dass der entsprechende Fehler in Tests ausgelöst wird, indem Sie den Schlüssel `errorReason` auf den mit dem erwarteten Fehler verbundenen „Grund“ setzen. Dieser ungültige Kontrast-Parameter löst den [benutzerdefinierten Fehler](../develop/worker.md#errors), `RenditionInstructionsError`, aus, daher setzen Sie `errorReason` auf den Grund dieses Fehlers, oder `rendition_instructions_error`, um zu bestätigen, dass er ausgelöst wird.

1. Da bei einer fehlerhaften Ausführung keine Ausgabedarstellung generiert werden sollte, ist keine `rendition.<extension>`-Datei erforderlich.
1. Führen Sie die Test-Suite aus dem Stammverzeichnis des Projekts aus, indem Sie den folgenden Befehl ausführen `aio app test`
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet sind
   + Beenden Sie alle laufenden Entwicklungs-Tool-Instanzen

![Test – Fehlerkontrast](./assets/test/error-contrast/result.png)

## Testfälle bei GitHub

Die endgültigen Testfälle sind auf Github verfügbar unter:

+ [aem-guides-wknd-asset-compute/test/asset-compute/worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Fehlerbehebung

+ [Keine Ausgabedarstellung während der Testausführung generiert](../troubleshooting.md#test-no-rendition-generated)
+ [Test generiert falsche Ausgabedarstellung](../troubleshooting.md#tests-generates-incorrect-rendition)
