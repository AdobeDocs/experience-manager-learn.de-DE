---
title: Testen eines Asset Computing-Mitarbeiters
description: Das Asset Compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von Tests von Asset Compute-Mitarbeitern.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6284
thumbnail: KT-6284.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '631'
ht-degree: 0%

---


# Testen eines Asset Computing-Mitarbeiters

Das Asset Compute-Projekt definiert ein Muster für das einfache Erstellen und Ausführen von [Tests von Asset Compute-Mitarbeitern](https://docs.adobe.com/content/help/en/asset-compute/using/extend/test-custom-application.html).

## Anatomie eines Arbeitertests

Die Tests von Mitarbeitern der Asset Compute werden in Test-Suites unterteilt. Innerhalb jeder Testsuite werden ein oder mehrere Testfälle durchgeführt, in denen eine Testbedingung bestätigt wird.

Die Struktur der Tests in einem Asset Compute-Projekt lautet wie folgt:

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
   + Zu testende Quelldatei (Erweiterung kann alles außer `.link`)
   + Erforderlich
+ `rendition.<extension>`
   + Erwartete Darstellung
   + Erforderlich, außer bei Fehlertests
+ `params.json`
   + JSON-Anweisungen für die einzelne Darstellung
   + Optional
+ `validate`
   + Ein Skript, das erwartete und tatsächliche Pfade für die Darstellungsdatei als Argumente erhält und bei einem positiven Ergebnis den Exitcode 0 zurückgeben muss, oder ein Exitcode, wenn die Überprüfung oder der Vergleich fehlgeschlagen ist.
   + Optional, standardmäßig der `diff` Befehl
   + Verwenden Sie ein Shell-Skript, das einen Befehl zum Ausführen des Dockers für die Verwendung verschiedener Validierungstools umschließt
+ `mock-<host-name>.json`
   + JSON formatierte HTTP-Antworten zur [Verfolgung externer Dienstaufrufe](https://www.mock-server.com/mock_server/creating_expectations.html).
   + Optional, wird nur verwendet, wenn der Worker-Code HTTP-Anforderungen eigenständig macht

## Schreiben eines Testfalls

Dieser Test-Fall bestätigt, dass die parametrisierte Eingabe (`params.json`) für die Eingabedatei (`file.jpg`) die erwartete PNG-Darstellung (`rendition.png`) generiert.

1. Löschen Sie zunächst den automatisch generierten `simple-worker` Testfall, `/test/asset-compute/simple-worker` da dieser ungültig ist, da unser Mitarbeiter die Quelle nicht mehr einfach in die Darstellung kopiert.
1. Erstellen Sie einen neuen Ordner mit Testfällen, `/test/asset-compute/worker/success-parameterized` um eine erfolgreiche Ausführung des Workers zu testen, der eine PNG-Darstellung generiert.
1. Fügen Sie im `success-parameterized` Ordner die Test- [Eingabedatei](./assets/test/success-parameterized/file.jpg) für diesen Testfall hinzu und geben Sie ihm einen Namen `file.jpg`.
1. Fügen Sie im `success-parameterized` Ordner eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Workers definiert:

   ```json
   { 
       "size": "400",
       "contrast": "0.25",
       "brightness": "-0.50"
   }
   ```
   Dabei handelt es sich um die gleichen Schlüssel/Werte, die in die Asset Compute-Profil-Definition [des](../develop/development-tool.md)Entwicklungstools übernommen werden, abzüglich des `worker` Schlüssels.
1. hinzufügen Sie die erwartete [Darstellungsdatei](./assets/test/success-parameterized/rendition.png) in diesen Testfall und geben Sie ihm einen Namen `rendition.png`. Diese Datei stellt die erwartete Ausgabe des Workers für die angegebene Eingabe dar `file.jpg`.
1. Führen Sie in der Befehlszeile die Tests des Projektstamms durch, indem Sie `aio app test`
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) - und unterstützende Docker-Bilder installiert und gestartet wurden
   + Beenden Sie alle ausgeführten Instanzen des Entwicklungstools

![Test - Erfolg ](./assets/test/success-parameterized/result.png)

## Schreiben eines Testfalls für die Fehlerprüfung

In diesem Test wird überprüft, ob der Worker den entsprechenden Fehler auslöst, wenn der `contrast` Parameter auf einen ungültigen Wert eingestellt ist.

1. Erstellen Sie einen neuen Ordner mit Testfällen, `/test/asset-compute/worker/error-contrast` um eine fehlerhafte Ausführung des Workers aufgrund eines ungültigen `contrast` Parameterwerts zu testen.
1. Fügen Sie im `error-contrast` Ordner die Test- [Eingabedatei](./assets/test/error-contrast/file.jpg) für diesen Testfall hinzu und geben Sie ihm einen Namen `file.jpg`. Der Inhalt dieser Datei ist für diesen Test unwesentlich, es muss nur vorhanden sein, um die Prüfung &quot;Korrupte Quelle&quot; zu überwinden, um die `rendition.instructions` Validitätsüberprüfung zu erreichen, dass dieser Testfall validiert.
1. Fügen Sie im `error-contrast` Ordner eine neue Datei mit dem Namen `params.json` hinzu, die die Eingabeparameter des Workers mit dem Inhalt definiert:

   ```json
   {
       "contrast": "10",
       "errorReason": "rendition_instructions_error"
   }
   ```

   + Legen Sie `contrast` Parameter auf `10`, einen ungültigen Wert, da der Kontrast zwischen -1 und 1 liegen muss, um einen `RenditionInstructionsError`Wert auszulösen.
   + Stellen Sie sicher, dass der entsprechende Fehler in Tests ausgegeben wird, indem Sie den `errorReason` Schlüssel auf den mit dem erwarteten Fehler verbundenen &quot;Grund&quot;setzen. Dieser ungültige Kontrastparameter gibt den [benutzerdefinierten Fehler](../develop/worker.md#errors)aus. `RenditionInstructionsError`Legen Sie daher den `errorReason` Grund für diesen Fehler fest oder`rendition_instructions_error` stellen Sie fest, dass er ausgegeben wird.

1. Da während einer fehlerhaften Ausführung keine Darstellung generiert werden sollte, ist keine `rendition.<extension>` Datei erforderlich.
1. Führen Sie die Test Suite aus dem Stammverzeichnis des Projekts aus, indem Sie den Befehl `aio app test`
   + Stellen Sie sicher, dass [Docker Desktop](../set-up/development-environment.md#docker) - und unterstützende Docker-Bilder installiert und gestartet wurden
   + Beenden Sie alle ausgeführten Instanzen des Entwicklungstools

![Test - Fehlerkontrast](./assets/test/error-contrast/result.png)

## Testfälle bei Github

Die abschließenden Testfälle sind auf Github unter folgender Adresse abrufbar:

+ [aem-guides-work-asset-compute/test/asset-compute/Worker](https://github.com/adobe/aem-guides-wknd-asset-compute/tree/master/test/asset-compute/worker)

## Fehlerbehebung

+ [Keine Darstellung während der Testausführung generiert](../troubleshooting.md#test-no-rendition-generated)
+ [Test generiert falsche Darstellung](../troubleshooting.md#tests-generates-incorrect-rendition)
