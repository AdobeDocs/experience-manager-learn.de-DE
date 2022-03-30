---
title: Bereitstellen von Asset compute-Arbeitern in Adobe I/O Runtime zur Verwendung mit AEM as a Cloud Service
description: asset compute-Projekte und die darin enthaltenen Arbeiter müssen in Adobe I/O Runtime bereitgestellt werden, damit sie von AEM as a Cloud Service verwendet werden können.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Bereitstellen in Adobe I/O Runtime

asset compute-Projekte und die darin enthaltenen Arbeiter müssen über die Adobe I/O-CLI in Adobe I/O Runtime bereitgestellt werden, damit sie von AEM as a Cloud Service verwendet werden können.

Bei der Bereitstellung in Adobe I/O Runtime zur Verwendung durch AEM as a Cloud Service Author-Dienste sind nur zwei Umgebungsvariablen erforderlich:

+ `AIO_runtime_namespace` verweist auf den App Builder Workspace, der bereitgestellt werden soll, um
+ `AIO_runtime_auth` sind die Authentifizierungsberechtigungen des App Builder-Arbeitsbereichs

Die anderen in der Variablen `.env` -Datei werden implizit von AEM as a Cloud Service bereitgestellt, wenn sie den Asset compute Worker aufruft.

## Entwicklungsarbeitsbereich

Da dieses Projekt mit `aio app init` mithilfe der `Development` Arbeitsbereich, `AIO_runtime_namespace` automatisch auf `81368-wkndaemassetcompute-development` mit dem Abgleich `AIO_runtime_auth` in unserem lokalen `.env` -Datei.  Wenn eine `.env` -Datei in dem Verzeichnis vorhanden ist, in dem der Bereitstellungsbefehl ausgegeben wird, werden die zugehörigen Werte verwendet, es sei denn, sie werden durch einen Variablenexport auf Betriebssystemebene ersetzt. So wird [Staging und Produktion](#stage-and-production) Arbeitsbereiche werden als Ziel ausgewählt.

![aio app deploy using .env variables](./assets/runtime/development__aio.png)

Bereitstellen im Arbeitsbereich, der in den Projekten definiert wird `.env` Datei:

1. Öffnen Sie die Befehlszeile im Stammverzeichnis des Asset compute-Projekts.
1. Ausführen des Befehls `aio app deploy`
1. Ausführen des Befehls `aio app get-url` , um die Worker-URL zur Verwendung im AEM as a Cloud Service Verarbeitungsprofil abzurufen, um auf diesen benutzerdefinierten Asset compute Worker zu verweisen. Wenn das Projekt mehrere Sekundäre enthält, werden separate URLs für jeden Worker aufgelistet.

Wenn lokale Entwicklungs- und AEM as a Cloud Service Entwicklungsumgebungen separate Asset compute-Bereitstellungen verwenden, können Implementierungen für AEM as a Cloud Service Entwicklung auf dieselbe Weise verwaltet werden wie [Staging- und Produktionsimplementierungen](#stage-and-production).

## Arbeitsbereiche für Staging- und Produktionsumgebungen{#stage-and-production}

Die Bereitstellung in Staging- und Produktionsarbeitsbereichen erfolgt normalerweise durch Ihr CI/CD-System Ihrer Wahl. Das Asset compute-Projekt muss diskret in jedem Arbeitsbereich (Staging und dann Produktion) bereitgestellt werden.

Durch das Festlegen von true -Umgebungsvariablen werden Werte für die gleichnamigen Variablen in `.env`.

![aio app deploy using export variables](./assets/runtime/stage__export-and-aio.png)

Der allgemeine Ansatz, der normalerweise von einem CI/CD-System für die Bereitstellung in Staging- und Produktionsumgebungen automatisiert wird, lautet:

1. Stellen Sie sicher, dass [Adobe I/O CLI npm-Modul und Asset compute-Plug-in](../set-up/development-environment.md#aio) installiert sind
1. Sehen Sie sich das Asset compute-Projekt an, das von Git bereitgestellt werden soll.
1. Legen Sie die Umgebungsvariablen mit den Werten fest, die dem Zielarbeitsbereich entsprechen (Staging oder Produktion)
   + Die beiden erforderlichen Variablen sind `AIO_runtime_namespace` und `AIO_runtime_auth` und werden pro Arbeitsbereich in der Adobe I/O Developer Console über die Workspace- __Alle herunterladen__ Funktion.

![Adobe Developer Console - AIO Runtime-Namespace und Auth](./assets/runtime/stage-auth-namespace.png)

Die Werte dieser Schlüssel können durch Ausgabe von Exportbefehlen über die Befehlszeile festgelegt werden:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Wenn Ihre Asset compute-Sekundäre andere Variablen benötigen, z. B. im Cloud-Speicher, sollten diese ebenfalls als Umgebungsvariablen exportiert werden.

1. Nachdem alle Umgebungsvariablen für den Zielarbeitsbereich festgelegt wurden, für den sie bereitgestellt werden sollen, führen Sie den Bereitstellungsbefehl aus:
   + `aio app deploy`
1. Die Worker-URL(s), auf die vom AEM as a Cloud Service Verarbeitungsprofil verwiesen wird, ist ebenfalls verfügbar über:
   + `aio app get-url`.

Wenn sich die Asset compute-Projektversion ändert, ändern sich auch die Worker-URLs, um die neue Version widerzuspiegeln. Die URL muss dann in den Verarbeitungsprofilen aktualisiert werden.

## Workspace-API-Bereitstellung{#workspace-api-provisioning}

Wann [Einrichten des App Builder-Projekts in Adobe I/O](../set-up/app-builder.md) zur Unterstützung der lokalen Entwicklung wurde ein neuer Entwicklungsarbeitsbereich erstellt und __asset compute-, I/O-Ereignisse__ und __I/O Events Management-APIs__ hinzugefügt.

Die __asset compute-, I/O-Ereignisse__ und __I/O Events Management-APIs__ APIS werden nur explizit zu den für die lokale Entwicklung verwendeten Arbeitsbereichen hinzugefügt. Arbeitsbereiche, die (ausschließlich) in AEM as a Cloud Service Umgebungen integriert werden, tun dies __not__ diese APIs müssen explizit hinzugefügt werden, da die APIs AEM as a Cloud Service verfügbar gemacht werden.
