---
title: Asset Compute-Entwicklungs-Tool
description: Das Asset Compute-Entwicklungs-Tool ist eine lokale Web-Anwendung, mit der Entwicklungspersonen Asset-Computer-Sekundäre lokal konfigurieren und ausführen können, und zwar außerhalb des Kontexts des AEM SDK für die Asset Compute-Ressourcen in Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 190
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# Asset Compute-Entwicklungs-Tool

Das Asset Compute-Entwicklungs-Tool ist eine lokale Web-Anwendung, mit der Entwicklungspersonen Asset-Computer-Sekundäre lokal konfigurieren und ausführen können, und zwar außerhalb des Kontexts des AEM SDK für die Asset Compute-Ressourcen in Adobe I/O Runtime.

## Verwenden des Asset Compute-Entwicklungs-Tools

Das Asset Compute-Entwicklungs-Tool kann über den folgenden Terminal-Befehl vom Stammverzeichnis des Asset Compute-Projekts aus ausgeführt werden:

```
$ aio app run
```

Dadurch wird das Entwicklungs-Tool unter __http://localhost:9000__ gestartet und automatisch in einem Browser-Fenster geöffnet. Damit das Entwicklungs-Tool ausgeführt werden kann, [muss ein gültiges, automatisch generiertes „devToolToken“ über einen Abfrageparameter bereitgestellt werden](#troubleshooting__devtooltoken).

## Grundlegendes zur Benutzeroberfläche des Asset Compute-Entwicklungs-Tools{#interface}

![Asset Compute-Entwicklungs-Tool](./assets/development-tool/asset-compute-dev-tool.png)

1. __Quelldatei:__ Die Auswahl der Quelldatei dient folgenden Zwecken:
   + Auswählen der Asset-Binärdatei, die als `source`-Binärdatei dient, die an den Asset Compute-Sekundär übergeben wird
   + Hochladen von Quelldateien
1. __Definition des Asset Compute-Profils:__ Definiert den auszuführenden Asset Compute-Sekundär einschließlich des URL-Endpunkts des Sekundärs, des Ausgabedarstellungs-Namens und aller anderen Parameter
1. __Ausführen:__ Mit der Schaltfläche „Ausführen“ wird das Asset Compute-Profil ausgeführt, wie im Asset Compute-Konfigurationsprofil-Editor definiert
1. __Abbruch:__ Mit der Schaltfläche „Abbrechen“ wird ein Vorgang abgebrochen, der durch Tippen auf die Schaltfläche „Ausführen“ eingeleitet wurde.
1. __Anfrage/Antwort:__ Stellt die HTTP-Anfrage und -Antwort an den bzw. vom Asset Compute-Sekundär bereit, der in Adobe I/O Runtime ausgeführt wird. Dies kann beim Debugging hilfreich sein
1. __Aktivierungsprotokolle:__ Die Protokolle, in denen die Ausführung des Asset Compute-Sekundärs beschrieben wird, sowie etwaige Fehler. Diese Informationen sind auch in der Standardausgabe von `aio app run` verfügbar
1. __Ausgabedarstellungen:__ Zeigt alle Ausgabedarstellungen an, die durch die Ausführung des Asset Compute-Sekundärs generiert wurden
1. __devToolToken-Abfrageparameter:__ Das Asset Compute-Entwicklungs-Tool-Token erfordert, dass ein gültiger `devToolToken`-Abfrageparameter vorhanden ist. Dieses Token wird automatisch jedes Mal generiert, wenn ein neues Entwicklungs-Tool erzeugt wird

### Ausführen eines benutzerdefinierten Sekundärs

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clickthrough der Ausführung einer Asset Compute-Arbeit im Entwicklungs-Tool (kein Audio)_

1. Stellen Sie sicher, dass das Asset Compute-Entwicklungs-Tool von Ihrem Projektstamm aus mit dem Befehl `aio app run` gestartet wird.
1. Laden bzw. wählen Sie im Asset Compute-Entwicklungs-Tool eine [Beispielgrafikdatei](../assets/samples/sample-file.jpg)
   + Stellen Sie sicher, dass die Datei in der Dropdown-Liste der __Quelldateien__ ausgewählt ist
1. Überprüfen Sie den Textbereich der __Asset Compute-Profildefinition__
   + Der `worker`-Schlüssel definiert die URL zum bereitgestellten Asset Compute-Sekundär
   + Der `name`-Schlüssel definiert den Namen der zu generierenden Ausgabedarstellung
   + Andere Schlüssel/Werte können in diesem JSON-Objekt bereitgestellt werden und sind im Sekundär unter dem Objekt `rendition.instructions` verfügbar
      + Fügen Sie optional Werte für `size`, `contrast` und `brightness` hinzu:

        ```json
        {
            "renditions": [
                {
                    "worker": "...",
                    "name": "rendition.png",
                    "size":"800",
                    "contrast": "0.30",
                    "brightness": "-0.15"
                }
            ]
        }
        ```

1. Tippen Sie auf die Schaltfläche __Ausführen__
1. Der __Abschnitt „Ausgabedarstellung“__ wird mit einem Platzhalter für die Ausgabedarstellung aufgefüllt
1. Sobald der Sekundär abgeschlossen ist, zeigt der Darstellungs-Platzhalter die generierte Ausgabedarstellung an

Wenn Sie Code-Änderungen am Code des Sekundärs vornehmen, während das Entwicklungs-Tool ausgeführt wird, werden die Änderungen per „Hot Deploy“ bereitgestellt. Die „Hot Deploy“-Bereitstellung dauert ein paar Sekunden. Warten Sie also, bis der Bereitstellungsvorgang abgeschlossen ist, bevor Sie den Sekundär über das Entwicklungs-Tool erneut ausführen.

## Fehlerbehebung

+ [Falscher YAML-Einzug](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize-Limit ist zu niedrig eingestellt](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Entwicklungs-Tool kann nicht gestartet werden, da der private Schlüssel fehlt](../troubleshooting.md#missing-private-key)
+ [Dropdown-Liste mit den Quelldateien falsch](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Fehlender oder ungültiger devToolToken-Abfrageparameter](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Quelldateien können nicht entfernt werden](../troubleshooting.md#unable-to-remove-source-files)
+ [Ausgabedarstellung nur teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
