---
title: asset compute-Entwicklungstool
description: Das Asset compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset-Computer-Sekundäre lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK, um sie gegen die Asset compute-Ressourcen in Adobe I/O Runtime zu schützen.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# asset compute-Entwicklungstool

Das Asset compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset-Computer-Sekundäre lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK, um sie gegen die Asset compute-Ressourcen in Adobe I/O Runtime zu schützen.

## Asset compute Development Tool ausführen

Das Asset compute Development Tool kann über den Terminal-Befehl vom Stamm des Asset compute-Projekts aus ausgeführt werden:

```
$ aio app run
```

Dadurch wird das Entwicklungstool unter __http://localhost:9000__ gestartet und automatisch in einem Browserfenster geöffnet. Damit das Entwicklungstool ausgeführt werden kann, muss [ein gültiges, automatisch generiertes devToolToken über einen Abfrageparameter](#troubleshooting__devtooltoken) bereitgestellt werden.

## Grundlegendes zur Benutzeroberfläche der Asset compute Development Tools{#interface}

![asset compute-Entwicklungstool](./assets/development-tool/asset-compute-dev-tool.png)

1. __Quelldatei:__  Die Quelldatei wird verwendet, um:
   + Die Asset-Binärdatei wurde ausgewählt, die die an den Asset compute Worker übergebene `source`-Binärdatei ist.
   + Hochladen von Quelldateien
1. __Definition des/der asset compute-Profils:__  Definiert den auszuführenden Asset compute Worker einschließlich der folgenden Parameter: einschließlich des URL-Endpunkts des Sekundärs, des Ausgabedarstellungsnamens und aller Parameter
1. __Ausführen:__  Die Schaltfläche Ausführen führt das Asset compute-Profil aus, wie im Asset compute-Konfigurationsprofil-Editor definiert
1. __Abbruch:__ Mit der Schaltfläche Abbruch wird eine Ausführung abgebrochen, die durch Tippen auf die Schaltfläche Ausführen initiiert wurde.
1. __Anfrage/Antwort:__ Stellt die HTTP-Anfrage und -Antwort an/vom Asset compute Worker bereit, der in Adobe I/O Runtime ausgeführt wird. Dies kann beim Debugging hilfreich sein
1. __Aktivierungsprotokolle:__ Die Protokolle, in denen die Ausführung des Asset compute Worker sowie etwaige Fehler beschrieben werden. Diese Informationen sind auch im `aio app run` Standard-Out verfügbar.
1. __Ausgabedarstellungen:__ Zeigt alle Ausgabedarstellungen an, die durch die Ausführung des Asset compute Worker generiert wurden
1. __devToolToken-Abfrageparameter:__ Für das Asset compute Development Tool-Token muss ein gültiger  `devToolToken` Abfrageparameter vorhanden sein. Dieses Token wird automatisch jedes Mal generiert, wenn ein neues Entwicklungstool erzeugt wird

### Ausführen eines benutzerdefinierten Sekundärs

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clickthrough der Ausführung einer Asset compute-Arbeit im Entwicklungstool (kein Audio)_

1. Stellen Sie sicher, dass das Asset compute Development Tool über den Befehl `aio app run` vom Projektstamm aus gestartet wird.
1. Laden Sie im Asset compute Development Tool eine [Beispielbilddatei](../assets/samples/sample-file.jpg) hoch oder wählen Sie sie aus.
   + Stellen Sie sicher, dass die Datei im Dropdown-Menü __Quelldatei__ ausgewählt ist.
1. Überprüfen Sie den Textbereich __Asset compute-Profildefinition__ .
   + Der Schlüssel `worker` definiert die URL zum bereitgestellten Asset compute Worker
   + Der Schlüssel `name` definiert den Namen der Ausgabedarstellung, die generiert werden soll
   + Andere Schlüssel/Werte können in diesem JSON-Objekt angegeben werden und sind im Worker unter dem `rendition.instructions`-Objekt verfügbar.
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

1. Tippen Sie auf die Schaltfläche __Ausführen__ .
1. Der Abschnitt __Ausgabeformate__ wird mit einem Platzhalter für Ausgabeformate gefüllt
1. Sobald der Worker abgeschlossen ist, zeigt der Platzhalter für die Ausgabedarstellung die generierte Ausgabedarstellung an

Wenn Sie Code-Änderungen am Worker-Code vornehmen, während das Entwicklungstool ausgeführt wird, werden die Änderungen &quot;Hot Deploy&quot;ausgeführt. Die &quot;Hot Deploy&quot;-Bereitstellung dauert mehrere Sekunden, sodass die Bereitstellung abgeschlossen werden kann, bevor der Worker über das Entwicklungstool erneut ausgeführt wird.

## Fehlerbehebung

+ [Falscher YAML-Einzug](../troubleshooting.md#incorrect-yaml-indentation)
+ [Speichergrößenlimit ist zu niedrig eingestellt](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Das Entwicklungstool kann nicht gestartet werden, da private.key fehlt.](../troubleshooting.md#missing-private-key)
+ [Dropdown-Liste der Quelldateien falsch](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Fehlender oder ungültiger devToolToken-Abfrageparameter](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Quelldateien können nicht entfernt werden](../troubleshooting.md#unable-to-remove-source-files)
+ [Wiedergabe teilweise gezeichnet/beschädigt zurückgegeben](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
