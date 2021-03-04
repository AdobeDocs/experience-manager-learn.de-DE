---
title: Bereitstellen von Asset compute-Mitarbeitern in Adobe I/O Runtime zur Verwendung mit AEM als Cloud Service
description: 'asset compute-Projekte und die darin enthaltenen Arbeiter müssen nach Adobe I/O Runtime entsandt werden, damit AEM als Cloud Service verwendet werden kann. '
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---


# Auf Adobe I/O Runtime bereitstellen

asset compute-Projekte und die darin enthaltenen Arbeiter müssen über die Adobe I/O-CLI nach Adobe I/O Runtime entsandt werden, um von AEM als Cloud Service genutzt werden zu können.

Bei der Bereitstellung auf Adobe I/O Runtime zur Verwendung durch AEM als Cloud Service Author-Dienst sind nur zwei Umgebung erforderlich:

+ `AIO_runtime_namespace` verweist auf die Adobe Project Firefly Workspace, die für
+ `AIO_runtime_auth` sind die Authentifizierungsberechtigungen des Adobe Project Firefly Workspace

Die anderen in der Datei `.env` definierten Standardvariablen werden implizit von AEM als Cloud Service bereitgestellt, wenn der Asset compute-Worker aufgerufen wird.

## Entwicklungsarbeitsbereich

Da dieses Projekt mithilfe von `aio app init` mithilfe des Arbeitsbereichs `Development` generiert wurde, wird `AIO_runtime_namespace` automatisch auf `81368-wkndaemassetcompute-development` und mit der entsprechenden `AIO_runtime_auth` in unserer lokalen `.env`-Datei eingestellt.  Wenn eine `.env`-Datei im Ordner vorhanden ist, in dem der Bereitstellungsbefehl ausgegeben wird, werden die zugehörigen Werte verwendet, es sei denn, sie werden durch einen Variablenexport auf Betriebssystemebene ersetzt, d. h., die [stage- und production](#stage-and-production)-Arbeitsbereiche werden als Ziel ausgewählt.

![Bereitstellung der App mit .env-Variablen](./assets/runtime/development__aio.png)

So stellen Sie die Bereitstellung im Arbeitsbereich bereit, der in der Datei `.env` definiert ist:

1. Öffnen Sie die Befehlszeile im Stammverzeichnis des Asset compute-Projekts
1. Befehl `aio app deploy` ausführen
1. Führen Sie den Befehl `aio app get-url` aus, um die Worker-URL abzurufen, die im AEM als Cloud Service Processing-Profil verwendet werden soll, um auf diesen benutzerdefinierten Asset compute-Worker zu verweisen. Wenn das Projekt mehrere Arbeiter enthält, werden für jeden Arbeiter separate URLs aufgelistet.

Wenn lokale Entwicklungs- und AEM als Cloud Service-Entwicklungs-Umgebung separate Asset compute-Bereitstellungen verwenden, können Bereitstellungen AEM als Cloud Service-Dev auf dieselbe Weise verwaltet werden wie [Stage- und Produktions-Bereitstellungen](#stage-and-production).

## Arbeitsbereiche für Phase und Produktion{#stage-and-production}

Die Bereitstellung auf Stage- und Produktionsarbeitsflächen erfolgt in der Regel über Ihr CI/CD-System Ihrer Wahl. Das Asset compute-Projekt muss diskret in jedem Arbeitsbereich (Phase und dann Produktion) bereitgestellt werden.

Wenn Sie True Umgebung-Variablen festlegen, werden Werte für die gleichnamigen Variablen in `.env` außer Kraft gesetzt.

![Bereitstellung der App mit Exportvariablen](./assets/runtime/stage__export-and-aio.png)

Der allgemeine Ansatz, der normalerweise von einem CI/CD-System automatisiert wird, für die Bereitstellung auf Stage- und Production-Umgebung lautet:

1. Stellen Sie sicher, dass das CLI-Modul und das Asset compute-Plug-in [Adobe I/O](../set-up/development-environment.md#aio) installiert sind.
1. Sehen Sie sich das von Git bereitzustellende Asset compute-Projekt an.
1. Legen Sie die Umgebung mit den Werten fest, die der Zielgruppe Workspace (Phase oder Produktion) entsprechen
   + Die beiden erforderlichen Variablen sind `AIO_runtime_namespace` und `AIO_runtime_auth` und werden per Workspace in der Adobe I/O Developer Console über die Funktion __Alle herunterladen__ abgerufen.

![Adobe Developer Console - AIO Runtime Namensraum und Auth](./assets/runtime/stage-auth-namespace.png)

Die Werte dieser Schlüssel können durch Ausgabe von Exportbefehlen über die Befehlszeile festgelegt werden:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Wenn Ihre Asset compute-Mitarbeiter andere Variablen benötigen, z. B. in der Cloud-Datenspeicherung, sollten diese ebenfalls als Umgebung-Variablen exportiert werden.

1. Nachdem alle Variablen für die Umgebung festgelegt wurden, für die der Zielgruppe Workspace bereitgestellt werden soll, führen Sie den Befehl &quot;deploy&quot;aus:
   + `aio app deploy`
1. Die Arbeiter-URL(s), auf die der AEM als Cloud Service-VerarbeitungsProfil verweist, ist ebenfalls verfügbar über:
   + `aio app get-url`.

Wenn sich die Asset compute-Projektversion ändert, ändern sich auch die Arbeits-URL(s) entsprechend der neuen Version und die URL muss in den Profilen für die Verarbeitung aktualisiert werden.

## Workspace API-Bereitstellung{#workspace-api-provisioning}

Beim Einrichten des Projekts &quot;Adobe - Projekt - Firefly&quot;in Adobe I/O](../set-up/firefly.md) zur Unterstützung der lokalen Entwicklung wurde ein neuer Arbeitsbereich für Entwicklung erstellt und __Asset compute, I/O-Ereignis__ und __I/O-Ereignisse-Management-APIs__ hinzugefügt.[

Die APIs __Asset compute, I/O-Ereignis__ und __I/O-Ereignisse-Management-APIs__ werden nur explizit zu den für die lokale Entwicklung verwendeten Arbeitsbereichen hinzugefügt. Arbeitsflächen, die (ausschließlich) mit AEM als Cloud Service-Umgebung integrieren, benötigen __nicht__ diese APIs explizit, da die APIs natürlich als Cloud Service AEM zur Verfügung gestellt werden.
