---
title: Asset Computing Development Tool
description: Das Asset Compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset-Computer-Arbeiter lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK gegen die Asset Compute-Ressourcen in Adobe I/O Runtime.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Asset Computing Development Tool

Das Asset Compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset-Computer-Arbeiter lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK gegen die Asset Compute-Ressourcen in Adobe I/O Runtime.

## Asset Compute Development Tool ausführen

Das Asset Compute Development Tool kann über den Befehl &quot;Terminal&quot;aus dem Stammordner des Asset Compute-Projekts ausgeführt werden:

```
$ aio app run
```

Dadurch wird das Entwicklungstool unter __http://localhost:9000__ Beginn und automatisch in einem Browserfenster geöffnet. Damit das Entwicklungstool ausgeführt werden kann, muss [ein gültiges, automatisch generiertes devToolToken über einen Abfrage-Parameter](#troubleshooting__devtooltoken)bereitgestellt werden.

## Benutzeroberfläche der Asset Compute Development Tools{#interface}

![Asset Computing Development Tool](./assets/development-tool/asset-compute-dev-tool.png)

1. __Quelldatei:__ Die Auswahl der Quelldatei dient zum:
   + Die Asset-Binärdatei wurde ausgewählt, bei der es sich um die `source` Binärdatei handelt, die an den Asset Compute-Mitarbeiter übergeben wird.
   + Hochladen von Quelldateien
1. __Definition der Profil für die Asset-Berechnung:__ Definiert den auszuführenden Asset Compute-Worker einschließlich der folgenden Parameter: einschließlich des URL-Endpunkts des Workers, des Ausgabenamens und aller Parameter
1. __Ausführen:__ Über die Schaltfläche &quot;Ausführen&quot;wird das Profil &quot;Asset Compute&quot;ausgeführt, wie im Editor für das Asset Compute-Konfigurationseditor definiert.
1. __Abbrechen:__ Mit der Schaltfläche Abbrechen wird eine Ausführung abgebrochen, die durch Tippen auf die Schaltfläche Ausführen eingeleitet wurde
1. __Anforderung/Antwort:__ Stellt die HTTP-Anforderung und -Antwort an den/vom Asset Compute-Mitarbeiter bereit, der in Adobe I/O Runtime ausgeführt wird. Dies kann beim Debugging hilfreich sein
1. __Aktivierungen-Protokolle:__ Die Protokolle, in denen die Ausführung des Assets Compute-Workers sowie etwaige Fehler beschrieben werden. Diese Informationen sind auch im `aio app run` Standard-Layout verfügbar.
1. __Darstellungen:__ Zeigt alle Darstellungen an, die durch die Ausführung des Workers &quot;Asset Compute&quot;generiert wurden
1. __abfrage-Parameter devToolToken:__ Für das Asset Compute Development Tool-Token muss ein gültiger Parameter für die `devToolToken` Abfrage vorhanden sein. Dieses Token wird automatisch jedes Mal generiert, wenn ein neues Entwicklungstool erzeugt wird

### Ausführen eines benutzerdefinierten Arbeitnehmers

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clickthrough zum Ausführen einer Asset-Compute-Arbeit im Entwicklungstool (kein Audio)_

1. Stellen Sie sicher, dass das Asset Compute Development Tool mit dem `aio app run` Befehl vom Projektstamm aus gestartet wurde.
1. Laden Sie im Asset Compute Development Tool eine [Beispielbilddatei hoch oder wählen Sie sie aus](../assets/samples/sample-file.jpg)
   + Vergewissern Sie sich, dass die Datei im Dropdown-Menü __Quelldatei__ ausgewählt ist.
1. Überprüfen Sie den Textbereich für die Definition __des__ Asset Compute-Profils.
   + Der `worker` Schlüssel definiert die URL zum bereitgestellten Asset Compiler-Mitarbeiter
   + Der `name` Schlüssel definiert den Namen der zu generierenden Darstellung
   + Andere Schlüssel/Werte können in diesem JSON-Objekt bereitgestellt werden und stehen im Worker unter dem `rendition.instructions` Objekt zur Verfügung
      + Fügen Sie optional Werte für `size`, `contrast` und `brightness`:

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

1. Tap the __Run__ button
1. Der Abschnitt __Ausgabeformate__ wird mit einem Platzhalter für die Darstellung gefüllt
1. Sobald der Worker abgeschlossen ist, zeigt der Darstellungs-Platzhalter die generierte Darstellung an

Wenn Sie Codeänderungen am Arbeitscode vornehmen, während das Entwicklungstool ausgeführt wird, werden die Änderungen &quot;hot deploy&quot;durchgeführt. Die &quot;Hot-Bereitstellung&quot;dauert mehrere Sekunden, damit die Bereitstellung abgeschlossen werden kann, bevor der Worker über das Development-Tool erneut ausgeführt wird.

## Fehlerbehebung

+ [Falscher YAML-Einzug](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit ist zu niedrig eingestellt](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Entwicklungstool kann aufgrund fehlender privater.key-Elemente nicht Beginn werden](../troubleshooting.md#missing-private-key)
+ [Dropdown-Liste der Quelldateien falsch](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Fehlender oder ungültiger Parameter für die devToolToken-Abfrage](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Quelldateien können nicht entfernt werden](../troubleshooting.md#unable-to-remove-source-files)
+ [Ausgabe teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
