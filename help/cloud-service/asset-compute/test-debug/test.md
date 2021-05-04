---
title: Testen eines Asset compute-Workers
description: Das Asset compute-Projekt definiert ein Muster für die einfache Erstellung und Durchführung von Tests von Asset compute-Workern.
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
translation-type: tm+mt
source-git-commit: 2efb7050b0b0c783c5f34c1f2e375cf21fa7a6cd
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 1%

---


# Testen eines Asset compute-Workers

Das Asset compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von [Tests von Asset compute Worker](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomie eines Arbeitertests

asset compute-Worker-Tests werden in Test-Suites unterteilt. Innerhalb jeder Testsuite werden ein oder mehrere Testfälle durchgeführt, die eine Testbedingung bestätigen.

Die Teststruktur in einem Asset compute-Projekt ist wie folgt:

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
   + Erwartete Darstellung
   + Erforderlich, außer bei Fehlertests
+ `params.json`
   + JSON-Anweisungen für die einzelne Darstellung
   + Optional
+ `validate`
   + Ein Skript, das erwartete und tatsächliche Pfade für die Darstellungsdatei als Argumente erhält und bei einem positiven Ergebnis den Exitcode 0 zurückgeben muss, oder ein Exitcode, wenn die Überprüfung oder der Vergleich fehlgeschlagen ist.
   + Optional, standardmäßig der Befehl `diff`
   + Verwenden Sie ein Shell-Skript, das einen Befehl zum Ausführen des Dockers für die Verwendung verschiedener Validierungstools umschließt
+ `mock-<host-name>.json`
   + JSON-formatierte HTTP-Antworten für [Verspotten externer Dienstaufrufe](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optional, wird nur verwendet, wenn der Worker-Code HTTP-Anforderungen eigenständig macht

## Schreiben eines Testfalls

Dieser Test-Fall bestätigt, dass die parametrisierte Eingabe (`params.json`) für die Eingabedatei (`file.jpg`) die erwartete PNG-Darstellung (`rendition.png`) generiert.

1. Löschen Sie zunächst den automatisch generierten Test-Vorgang `simple-worker` unter `/test/asset-compute/simple-worker`, da dieser ungültig ist, da unser Mitarbeiter die Quelle nicht mehr einfach in die Darstellung kopiert.
1. Erstellen Sie unter `/test/asset-compute/worker/success-parameterized` einen neuen Ordner für Testfälle, um eine erfolgreiche Ausführung des Workers zu testen, der eine PNG-Darstellung generiert.
1. Fügen Sie im Ordner `success-parameterized` die Eingabedatei [für diesen Testfall und den Namen `file.jpg` hinzu.](./assets/test/success-parameterized/file.jpg)
1. Fügen Sie im Ordner `success-parameterized` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Workers definiert:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```

   Dabei handelt es sich um die gleichen Schlüssel/Werte, die an die Asset compute-Profil-Definition des [Entwicklungstools](../develop/development-tool.md), abzüglich der `worker`-Taste übergeben werden.

1. hinzufügen Sie die erwartete Datei [Darstellung](./assets/test/success-parameterized/rendition.png) in diesen Testfall und geben Sie `rendition.png` einen Namen. Diese Datei stellt die erwartete Ausgabe des Workers für die angegebene Eingabe dar.`file.jpg`
1. Führen Sie in der Befehlszeile die Tests des Projektstamms durch, indem Sie `aio app test`
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet wurden.
   + Beenden Sie alle ausgeführten Instanzen des Entwicklungstools

![Test - Erfolg  ](./assets/test/success-parameterized/result.png)

## Schreiben eines Testfalls für die Fehlerprüfung

In diesem Test wird überprüft, ob der Worker den entsprechenden Fehler auslöst, wenn der Parameter `contrast` auf einen ungültigen Wert eingestellt ist.

1. Erstellen Sie unter `/test/asset-compute/worker/error-contrast` einen neuen Ordner für Testfälle, um eine fehlerhafte Ausführung des Workers aufgrund eines ungültigen Parameters `contrast` zu testen.
1. Fügen Sie im Ordner `error-contrast` die Eingabedatei [für diesen Testfall und den Namen `file.jpg` hinzu. ](./assets/test/error-contrast/file.jpg) Der Inhalt dieser Datei ist für diesen Test nicht wesentlich. Er muss nur vorhanden sein, um die Prüfung &quot;Korrupte Quelle&quot;zu überwinden, damit die `rendition.instructions` Gültigkeitsprüfung erreicht wird, die dieser Testfall validiert.
1. Fügen Sie im Ordner `error-contrast` eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Workers mit dem Inhalt definiert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Setzen Sie die Parameter `contrast` auf `10`, einen ungültigen Wert, da der Kontrast zwischen -1 und 1 liegen muss, um ein `RenditionInstructionsError` auszulösen.
   + Stellen Sie sicher, dass der entsprechende Fehler in Tests ausgegeben wird, indem Sie die `errorReason`-Taste auf den mit dem erwarteten Fehler verknüpften &quot;Grund&quot;setzen. Dieser ungültige Kontrastparameter gibt den [benutzerspezifischen Fehler](../develop/worker.md#errors), `RenditionInstructionsError` aus. Legen Sie deshalb `errorReason` auf den Grund dieses Fehlers fest, oder`rendition_instructions_error`, um zu behaupten, dass der Fehler ausgegeben wird.

1. Da während einer fehlerhaften Ausführung keine Darstellung generiert werden sollte, ist keine `rendition.<extension>`-Datei erforderlich.
1. Führen Sie die Test Suite aus dem Stammverzeichnis des Projekts aus, indem Sie den Befehl `aio app test`
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) und unterstützende Docker-Bilder installiert und gestartet wurden.
   + Beenden Sie alle ausgeführten Instanzen des Entwicklungstools

![Test - Fehlerkontrast](./assets/test/error-contrast/result.png)

## Testfälle bei Github

Die abschließenden Testfälle sind auf Github unter folgender Adresse abrufbar:

+ [aem-guides-work-asset-compute/test/asset-compute/Worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Fehlerbehebung

+ [Keine Darstellung während der Testausführung generiert](../troubleshooting.md#test-no-rendition-generated)
+ [Test generiert falsche Darstellung](../troubleshooting.md#tests-generates-incorrect-rendition)
