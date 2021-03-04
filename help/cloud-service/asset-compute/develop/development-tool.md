---
title: asset compute Development Tool
description: Das Asset compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset Computer-Arbeiter lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK gegen die Asset compute-Ressourcen in Adobe I/O Runtime.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# asset compute Development Tool

Das Asset compute Development Tool ist eine lokale Webanwendung, mit der Entwickler Asset Computer-Arbeiter lokal konfigurieren und ausführen können, außerhalb des Kontexts des AEM SDK gegen die Asset compute-Ressourcen in Adobe I/O Runtime.

## Asset compute Development Tool ausführen

Das Asset compute Development Tool kann über den Befehl terminal aus dem Stammordner des Asset compute-Projekts ausgeführt werden:

```
$ aio app run
```

Dadurch wird das Entwicklungstool unter __http://localhost:9000__ Beginn und automatisch in einem Browserfenster geöffnet. Damit das Entwicklungstool ausgeführt werden kann, muss [ein gültiges, automatisch generiertes devToolToken über einen Abfrage-Parameter](#troubleshooting__devtooltoken) bereitgestellt werden.

## Die Benutzeroberfläche der Asset compute Development Tools{#interface}

![asset compute Development Tool](./assets/development-tool/asset-compute-dev-tool.png)

1. __Quelldatei:__ Die Auswahl der Quelldatei dient folgenden Zwecken:
   + Die Asset-Binärdatei wurde ausgewählt, bei der es sich um die an den Asset compute-Worker übergebene Binärdatei (`source`) handelt
   + Hochladen von Quelldateien
1. __asset compute-Profil(s)-Definition:__ Definiert den auszuführenden Asset compute-Worker einschließlich der folgenden Parameter: einschließlich des URL-Endpunkts des Workers, des Ausgabenamens und aller Parameter
1. __Ausführen:__ Die Schaltfläche &quot;Ausführen&quot;führt das Asset compute-Profil aus, wie im Editor für Asset compute Configuration Profil definiert.
1. __Abbruch:__ Die Schaltfläche Abbrechen bricht eine Ausführung ab, die durch Tippen auf die Schaltfläche Ausführen eingeleitet wurde
1. __Anforderung/Antwort:__ Stellt die HTTP-Anforderung und -Antwort an/von dem in Adobe I/O Runtime ausgeführten Asset compute-Worker bereit. Dies kann beim Debugging hilfreich sein
1. __Aktivierung Logs:__ Die Protokolle, die die Ausführung des Asset compute-Workers zusammen mit etwaigen Fehlern beschreiben. Diese Informationen finden Sie auch im Standard-Out von `aio app run`
1. __Darstellungen:__ Zeigt alle Darstellungen an, die durch die Ausführung des Asset compute Worker generiert wurden
1. __devToolToken-Abfrage:__ Das Asset compute Development Tool-Token erfordert einen gültigen  `devToolToken` Abfrage-Parameter. Dieses Token wird automatisch jedes Mal generiert, wenn ein neues Entwicklungstool erzeugt wird

### Ausführen eines benutzerdefinierten Arbeitnehmers

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Clickthrough zum Ausführen eines Asset compute-Werkes im Entwicklungstool (kein Ton)_

1. Stellen Sie sicher, dass das Asset compute Development Tool mit dem Befehl `aio app run` vom Projektstamm aus gestartet wird.
1. Laden Sie im Asset compute Development Tool eine [Beispielbilddatei](../assets/samples/sample-file.jpg) hoch oder wählen Sie sie aus.
   + Vergewissern Sie sich, dass die Datei im Dropdown-Menü __Quelldatei__ ausgewählt ist.
1. Überprüfen Sie den Textbereich __Asset compute Profil definition__
   + Der `worker`-Schlüssel definiert die URL zum bereitgestellten Asset compute Worker
   + Der `name`-Schlüssel definiert den Namen der zu generierenden Darstellung
   + Andere Schlüssel/Werte können in diesem JSON-Objekt bereitgestellt werden und stehen im Worker unter dem `rendition.instructions`-Objekt zur Verfügung
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
