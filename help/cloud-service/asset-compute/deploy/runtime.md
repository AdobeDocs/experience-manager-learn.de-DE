---
title: Asset Compute-Mitarbeiter für die Verwendung mit AEM als Cloud Service in Adobe I/O Runtime bereitstellen
description: 'Asset Compute-Projekte und die darin enthaltenen Mitarbeiter müssen in Adobe I/O Runtime bereitgestellt werden, damit sie von AEM als Cloud Service verwendet werden können. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 0%

---


# Auf Adobe I/O Runtime bereitstellen

Asset Compute-Projekte und die darin enthaltenen Mitarbeiter müssen über die Adobe-I/O-CLI nach Adobe I/O Runtime entsandt werden, um von AEM als Cloud Service verwendet werden zu können.

Bei der Bereitstellung auf Adobe I/O Runtime zur Verwendung durch AEM als Cloud Service Author-Dienst sind nur zwei Umgebung erforderlich:

+ `AIO_runtime_namespace` verweist auf die Adobe Project Firefly Workspace, die für
+ `AIO_runtime_auth` sind die Authentifizierungsberechtigungen des Adobe Project Firefly Workspace

Die anderen in der `.env` Datei definierten Standardvariablen werden implizit von AEM als Cloud Service bereitgestellt, wenn der Asset Compute-Mitarbeiter aufgerufen wird.

## Entwicklungsarbeitsbereich

Da dieses Projekt mit `aio app init` dem `Development` Arbeitsbereich erstellt wurde, `AIO_runtime_namespace` wird automatisch auf `81368-wkndaemassetcompute-development` die Übereinstimmung `AIO_runtime_auth` in unserer lokalen `.env` Datei eingestellt.  Wenn eine `.env` Datei im Ordner vorhanden ist, in dem der Bereitstellungsbefehl ausgegeben wird, werden deren Werte verwendet, es sei denn, sie werden durch einen Variablenexport auf Betriebssystemebene ersetzt, d. h., wie [stage- und production](#stage-and-production) -Arbeitsbereiche ausgerichtet werden.

![Bereitstellung der App mit .env-Variablen](./assets/runtime/development__aio.png)

So stellen Sie die in der `.env` Projektdatei definierten Arbeitsflächen bereit:

1. Öffnen Sie die Befehlszeile im Stammverzeichnis des Asset Compute-Anwendungsprojekts
1. Befehl ausführen `aio app deploy`
1. Führen Sie den Befehl `aio app get-url` aus, um die Worker-URL für die Verwendung im AEM als Cloud Service Processing-Profil abzurufen, um auf diesen benutzerdefinierten Asset Compute-Worker zu verweisen. Wenn das Projekt mehrere Arbeiter enthält, werden für jeden Arbeiter separate URLs aufgelistet.

Wenn lokale Entwicklungs- und AEM als Cloud Service-Entwicklungs-Umgebung separate Asset-Compute-Bereitstellungen verwenden, können Bereitstellungen für AEM als Cloud Service-Dev auf dieselbe Weise verwaltet werden wie Bereitstellungen für [Stage und Produktion](#stage-and-production).

## Stage- und Produktionsarbeitsbereiche{#stage-and-production}

Die Bereitstellung auf Stage- und Produktionsarbeitsflächen erfolgt in der Regel über Ihr CI/CD-System Ihrer Wahl. Das Asset Compute-Projekt muss diskret in jedem Arbeitsbereich (Phase und dann Produktion) bereitgestellt werden.

Wenn Sie True Umgebung-Variablen festlegen, werden Werte für die gleichnamigen Variablen in `.env`überschrieben.

![Bereitstellung der App mit Exportvariablen](./assets/runtime/stage__export-and-aio.png)

Der allgemeine Ansatz, der normalerweise von einem CI/CD-System automatisiert wird, für die Bereitstellung auf Stage- und Production-Umgebung lautet:

1. Stellen Sie sicher, dass das [Adobe-Modul für die Befehlszeilenschnittstelle und das Asset Compute-Plug-In](../set-up/development-environment.md#aio) installiert sind.
1. Überprüfen Sie die Asset-Compute-Anwendung, die von Git bereitgestellt werden soll.
1. Legen Sie die Umgebung mit den Werten fest, die der Zielgruppe Workspace (Phase oder Produktion) entsprechen
   + Die beiden erforderlichen Variablen werden pro Arbeitsbereich in der Adobe-E/A-Entwicklerkonsole über die Funktion &quot;Alle `AIO_runtime_namespace` herunterladen&quot;von Workspace abgerufen `AIO_runtime_auth` und erhalten ____ .

![Adobe Developer Console - AIO Runtime Namensraum und Auth](./assets/runtime/stage-auth-namespace.png)

Die Werte dieser Schlüssel können durch Ausgabe von Exportbefehlen über die Befehlszeile festgelegt werden:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Wenn Ihre Mitarbeiter von Asset Compute andere Variablen benötigen, z. B. in der Cloud-Datenspeicherung, sollten diese ebenfalls als Umgebung-Variablen exportiert werden.

1. Nachdem alle Variablen für die Umgebung festgelegt wurden, für die der Zielgruppe Workspace bereitgestellt werden soll, führen Sie den Befehl &quot;deploy&quot;aus:
   + `aio app deploy`
1. Die Arbeiter-URL(s), auf die der AEM als Cloud Service-VerarbeitungsProfil verweist, ist ebenfalls verfügbar über:
   + `aio app get-url`.

Wenn sich die Anwendungsversion zum Berechnen von Assets ändert, ändern sich auch die Arbeits-URL(s) entsprechend der neuen Version und die URL muss in den Profilen zum Verarbeiten aktualisiert werden.

## Bereitstellung der Workspace-API{#workspace-api-provisioning}

Beim [Einrichten des Projekts &quot;Adobe-Projekt - Firefly&quot;in Adobe I/O](../set-up/firefly.md) zur Unterstützung der lokalen Entwicklung wurde ein neuer Entwicklungsarbeitsbereich erstellt und __Asset Compute-, I/O-Ereignisse__ - und __I/O-Ereignisse-Management-APIs__ hinzugefügt.

Die APIs für __Asset Compute-, I/O-Ereignis__ - und __I/O-Ereignisse-Management-APIs__ werden nur explizit zu den für die lokale Entwicklung verwendeten Arbeitsbereichen hinzugefügt. Arbeitsflächen, die (ausschließlich) mit AEM als Cloud Service-Umgebung integriert werden, benötigen diese APIs __nicht__ explizit, da die APIs automatisch als Cloud Service AEM zur Verfügung gestellt werden.
