---
title: Bereitstellen von Asset Compute-Sekundären in Adobe I/O Runtime zur Verwendung mit AEM as a Cloud Service
description: Asset Compute-Projekte und die darin enthaltenen Sekundäre müssen in Adobe I/O Runtime bereitgestellt werden, damit sie mit AEM as a Cloud Service verwendet werden können.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 165
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 100%

---

# Bereitstellen für Adobe I/O Runtime

Asset Compute-Projekte und die darin enthaltenen Sekundäre müssen über die Adobe I/O-CLI in Adobe I/O Runtime bereitgestellt werden, damit sie von AEM as a Cloud Service verwendet werden können.

Bei der Bereitstellung in Adobe I/O Runtime zur Verwendung von Author-Services in AEM as a Cloud Service sind nur zwei Umgebungsvariablen erforderlich:

+ `AIO_runtime_namespace` verweist auf den für die Bereitstellung vorgesehenen App-Entwicklungs-Arbeitsbereich.
+ `AIO_runtime_auth` entspricht den Authentifizierungsberechtigungen des App-Entwicklungs-Arbeitsbereich.

Die anderen Standardvariablen in der `.env`-Datei werden beim Aufruf des Asset Compute-Sekundärs implizit von AEM as a Cloud Service bereitgestellt.

## Entwicklungsarbeitsbereich

Da dieses Projekt mit `aio app init` über den Arbeitsbereich `Development` generiert wurde, ist `AIO_runtime_namespace` in unserer lokalen `.env`-Datei automatisch auf `81368-wkndaemassetcompute-development` mit passendem Eintrag für `AIO_runtime_auth` festgelegt. Wenn eine `.env`-Datei in dem Verzeichnis vorhanden ist, in dem der Bereitstellungsbefehl ausgegeben wird, werden die zugehörigen Werte verwendet, es sei denn, sie werden durch einen Variablenexport auf Betriebssystemebene ersetzt. Auf diese Weise werden die [Stagings- und Produktionsarbeitsbereiche](#stage-and-production) als Ziel ausgewählt.

![„aio app deploy“ mit .env-Variablen](./assets/runtime/development__aio.png)

Für eine Bereitstellung im Arbeitsbereich definieren Sie Folgendes in der `.env`-Projektdatei:

1. Öffnen Sie eine Befehlszeile im Stammverzeichnis des Asset Compute-Projekts.
1. Führen Sie den Befehl `aio app deploy` aus.
1. Führen Sie den Befehl `aio app get-url` aus, um die Sekundär-URL zur Verwendung im AEM as a Cloud Service-Verarbeitungsprofil abzurufen und so auf diesen benutzerdefinierten Asset Compute-Sekundär zu verweisen. Wenn das Projekt mehrere Sekundäre umfasst, werden separate URLs für jeden Sekundär aufgelistet.

Wenn lokale Entwicklungs- und AEM as a Cloud Service-Entwicklungsumgebungen separate Asset Compute-Bereitstellungen verwenden, können Bereitstellungen für die AEM as a Cloud Service-Entwicklung auf dieselbe Weise verwaltet werden wie [Staging- und Produktionsbereitstellungen](#stage-and-production).

## Staging- und Produktionsarbeitsbereiche{#stage-and-production}

Die Bereitstellung in Staging- und Produktionsarbeitsbereichen erfolgt normalerweise durch ein CI/CD-System Ihrer Wahl. Das Asset Compute-Projekt muss für jeden Arbeitsbereich (Staging und dann Produktion) einzeln bereitgestellt werden.

Durch Festlegen der Umgebungsvariablen auf „true“ werden Werte für die gleichnamigen Variablen in `.env` außer Kraft gesetzt.

![„aio app deploy“ mit Exportvariablen](./assets/runtime/stage__export-and-aio.png)

Der allgemeine und normalerweise von einem CI/CD-System automatisierte Ansatz zur Bereitstellung in Staging- und Produktionsumgebungen lautet wie folgt:

1. Sicherstellen, dass das [Adobe I/O-CLI-npm-Modul und das Asset Compute-Plug-in](../set-up/development-environment.md#aio) installiert sind
1. Auschecken des Asset Compute-Projekts zur Bereitstellung über Git
1. Festlegen der Umgebungsvariablen auf die Werte, die dem Zielarbeitsbereich (Staging oder Produktion) entsprechen
   + Die beiden erforderlichen Variablen `AIO_runtime_namespace` und `AIO_runtime_auth` werden pro Arbeitsbereich in Adobe I/O Developer Console über die Arbeitsbereichsfunktion __Alle herunterladen__ abgerufen.

![Adobe Developer Console – AIO Runtime-Namespace und -Authentifizierung](./assets/runtime/stage-auth-namespace.png)

Die Werte dieser Schlüssel können durch Ausgabe von Exportbefehlen über die Befehlszeile festgelegt werden:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Wenn Ihre Asset Compute-Sekundäre andere Variablen benötigen, z. B. im Cloud-Speicher, sollten diese ebenfalls als Umgebungsvariablen exportiert werden.

1. Nachdem alle Umgebungsvariablen für den Zielarbeitsbereich der Bereitstellung festgelegt wurden, führen Sie den Bereitstellungsbefehl aus:
   + `aio app deploy`
1. Die Sekundär-URL, auf die vom AEM as a Cloud Service-Verarbeitungsprofil verwiesen wird, ist ebenfalls verfügbar, und zwar über:
   + `aio app get-url`.

Wenn sich die Asset Compute-Projektversion ändert, ändert sich auch die URL des Sekundärs, um die neue Version widerzuspiegeln. Die URL muss dann in den Verarbeitungsprofilen aktualisiert werden.

## Bereitstellung der Arbeitsbereichs-API{#workspace-api-provisioning}

Beim [Einrichten des App-Entwicklungsprojekts in Adobe I/O](../set-up/app-builder.md) zur Unterstützung der lokalen Entwicklung wurde ein neuer Entwicklungsarbeitsbereich erstellt und __Asset Compute-, I/O-Ereignis-__ und __I/O-Ereignis-Management-APIs__ wurden hinzugefügt.

Die __Asset Compute-, I/O-Ereignis-__ und __I/O-Ereignis-Management-APIs__ werden explizit nur zu den für die lokale Entwicklung verwendeten Arbeitsbereichen hinzugefügt. Für Arbeitsbereiche, die (ausschließlich) in AEM as a Cloud Service-Umgebungen integriert werden, müssen diese APIs __nicht__ explizit hinzugefügt werden, da die APIs von Natur aus für AEM as a Cloud Service verfügbar gemacht werden.
