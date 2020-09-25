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
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 0%

---


# Asset Computing Development Tool

Das Asset Compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset-Computer-Arbeiter lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK gegen die Asset Compute-Ressourcen in Adobe I/O Runtime.

## Asset Compute Development Tool ausführen

Das Asset Compute Development Tool kann über den Befehl &quot;Terminal&quot;aus dem Stammordner des Asset Compute-Anwendungsprojekts ausgeführt werden:

```
$ aio app run
```

Dadurch wird das Entwicklungstool unter __http://localhost:9000__ Beginn und automatisch in einem Browserfenster geöffnet. Damit das Entwicklungstool ausgeführt werden kann, muss [ein gültiges, automatisch generiertes devToolToken über einen Abfrage-Parameter](#troubleshooting__devtooltoken)bereitgestellt werden.

## Benutzeroberfläche der Asset Compute Development Tools{#interface}

![Asset Computing Development Tool](./assets/development-tool/asset-compute-dev-tool.png)

1. __Quelldatei:__ Die Auswahl der Quelldatei dient zum:
   + Die Asset-Binärdatei wurde ausgewählt, bei der es sich um die `source` Binärdatei handelt, die an den Asset Compute-Mitarbeiter übergeben wird.
   + Hochladen von Quelldateien
1. __Definition des Profils &quot;Asset Computing&quot;:__ Definiert den auszuführenden Asset Compute-Worker einschließlich der folgenden Parameter: einschließlich des URL-Endpunkts des Workers, des Ausgabenamens und aller Parameter
1. __Ausführen:__ Über die Schaltfläche &quot;Ausführen&quot;wird das Profil &quot;Asset Compute&quot;ausgeführt, wie im Editor für das Asset Compute-Konfigurationseditor definiert.
1. __Abbrechen:__ Mit der Schaltfläche Abbrechen wird eine Ausführung abgebrochen, die durch Tippen auf die Schaltfläche Ausführen eingeleitet wurde
1. __Anforderung/Antwort:__ Stellt die HTTP-Anforderung und -Antwort auf/von der Asset Compute-Anwendung bereit, die in Adobe Runtime ausgeführt wird. Dies kann beim Debugging hilfreich sein
1. __Aktivierungen-Protokolle:__ Die Protokolle, in denen die Ausführung der Asset-Compute-Anwendung beschrieben wird, sowie etwaige Fehler. Diese Informationen sind auch im `aio app run` Standard-Layout verfügbar.
1. __Darstellungen:__ Zeigt alle Darstellungen an, die durch die Ausführung der Anwendung &quot;Asset Berechnen&quot;generiert wurden.
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

### Dropdown-Liste der Quelldateien falsch{#troubleshooting__dev-tool-application-cache}

Das Asset Compute Development Tool kann einen Status eingeben, in dem statische Daten abgerufen werden, und ist im Dropdown-Menü __Quelldatei__ mit falschen Elementen am auffälligsten.

+ __Fehler:__ Das Dropdown-Menü Quelldatei enthält falsche Elemente.
+ __Ursache:__ Der Status &quot;Nicht im Cache gespeicherter Browser&quot;verursacht
+ __Lösung:__ Löschen Sie in Ihrem Browser den Anwendungsstatus der Browser-Registerkarte, den Browser-Cache, die lokale Datenspeicherung und den Service-Mitarbeiter vollständig.

### Fehlender Parameter für die devToolToken-Abfrage{#troubleshooting__devtooltoken}

+ __Fehler:__ Benachrichtigung &quot;Nicht autorisiert&quot;im Asset Computing Development Tool
+ __Ursache:__ `devToolToken` fehlt oder ist ungültig
+ __Lösung:__ Schließen Sie das Browserfenster des Tools für die Asset-Berechnung, beenden Sie alle laufenden Entwicklungstool-Prozesse, die über den `aio app run` Befehl initiiert wurden, und starten Sie das Beginn-Entwicklungstool (unter Verwendung `aio app run`).

### Quelldateien können nicht entfernt werden{#troubleshooting__remove-source-files}

+ __Fehler:__ Es gibt keine Möglichkeit, hinzugefügte Quelldateien aus der Benutzeroberfläche der Entwicklungstools zu entfernen
+ __Ursache:__ Diese Funktion wurde nicht implementiert.
+ __Lösung:__ Melden Sie sich bei Ihrem Cloud-Anbieter für Datenspeicherung mit den unter `.env`&quot;Anmeldeinformationen&quot;definierten Anmeldeinformationen an. Suchen Sie den Container, der von den Entwicklungstools verwendet wird (siehe auch `.env`), navigieren Sie zum __Quellordner__ und löschen Sie alle Quellbilder. Möglicherweise müssen Sie die in der Dropdown-Liste &quot; [Quelldateien&quot;beschriebenen Schritte falsch](#troubleshooting__dev-tool-application-cache) ausführen, wenn die gelöschten Quelldateien weiterhin im Dropdown-Menü angezeigt werden, da sie im Anwendungszustand &quot;Entwicklungstools&quot;lokal zwischengespeichert werden können.

   ![Microsoft Azure Blob Storage](./assets/development-tool/troubleshooting__remove-source-files.png)
