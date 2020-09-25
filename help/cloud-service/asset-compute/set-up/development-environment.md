---
title: Lokale Entwicklungs-Umgebung für Asset Computing-Erweiterbarkeit einrichten
description: Die Entwicklung von Asset Compute-Workern, bei denen es sich um JavaScript-Anwendungen von Node.js handelt, erfordert spezielle Entwicklungs-Tools, die sich von der herkömmlichen AEM unterscheiden, von Node.js und verschiedenen npm-Modulen bis hin zu Docker Desktop und Microsoft Visual Studio-Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 53e4235c55d890765e9f13ffeb37a2c805fb307b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Umgebung zur lokalen Entwicklung einrichten

Adobe Asset Compute-Anwendungen können nicht in die vom AEM SDK bereitgestellte lokale AEM-Laufzeit integriert werden und werden mithilfe ihrer eigenen Toolkette entwickelt, die von der für AEM auf Basis des AEM Maven-Projektarchivtyps erforderlichen Applikation getrennt ist.

Um Asset Compute-Mikrodienste zu erweitern, müssen die folgenden Werkzeuge auf dem lokalen Entwicklercomputer installiert sein.

## Abgekürzte Einrichtungsanweisungen

Im Folgenden finden Sie eine Anleitung zur Einrichtung des Abkürzungssystems. Details zu diesen Entwicklungstools sind in den einzelnen Abschnitten unten beschrieben.

1. [Installieren Sie Docker Desktop](https://www.docker.com/products/docker-desktop) und ziehen Sie die erforderlichen Docker-Bilder:

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installieren von Visual Studio-Code](https://code.visualstudio.com/download)
1. [Node.js 10+ installieren](../../local-development-environment/development-tools.md#node-js)
1. Installieren Sie die erforderlichen NPM-Module und Adobe-I/O-CLI-Plug-Ins über die Befehlszeile:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Installieren von Visual Studio-Code{#vscode}

[Der Microsoft Visual Studio-Code](https://code.visualstudio.com/download) wird zum Entwickeln und Debuggen von Asset Compute-Anwendungen verwendet. Während andere [JavaScript-kompatible IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) zur Entwicklung der Anwendung verwendet werden können, kann nur Visual Studio-Code zum [Debugging](../test-debug/debug.md) von Asset Compute-Anwendungen integriert werden.

_Visual Studio-Code 1.48.x+ ist erforderlich, damit[wskdebug](#wskdebug)funktioniert._

In diesem Lernprogramm wird von der Verwendung von Visual Studio-Code ausgegangen, da es das beste Entwicklererlebnis zum Erweitern von Asset Compute bietet.

## Docker Desktop installieren{#docker}

Laden Sie den neuesten stabilen [Docker Desktop](https://www.docker.com/products/docker-desktop)herunter und installieren Sie ihn, da dies zum [Testen](../test-debug/test.md) und Debuggen [](../test-debug/debug.md) von Asset Compute-Projekten auf lokaler Ebene erforderlich ist.

Nachdem Sie Docker Desktop installiert haben, führen Sie den Beginn aus und installieren Sie die folgenden Docker-Bilder über die Befehlszeile:

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Entwickler auf Windows-Computern sollten sicherstellen, dass sie Linux-Container für die oben genannten Bilder verwenden. Die Schritte zum Wechseln zu Linux-Containern werden in der Dokumentation [zum](https://docs.docker.com/docker-for-windows/)Docker für Windows beschrieben.

## Node.js (und npm) installieren{#node-js}

Asset Compute-Mitarbeiter sind [Node.js](https://nodejs.org/) -Anwendungen und erfordern daher die Entwicklung und Erstellung von Node.js 10+ (und npm).

+ [Installieren Sie Node.js (und npm)](../../local-development-environment/development-tools.md#node-js) auf dieselbe Weise wie bei der herkömmlichen AEM.

## Install Adobe I/O CLI{#aio}

[Installieren Sie die Adobe-I/O-CLI](../../local-development-environment/development-tools.md#aio-cli)oder __aio__ ist ein Befehlszeilenmodul (CLI), das die Verwendung und Interaktion mit Adobe-I/O-Technologien erleichtert und sowohl für die Generierung als auch die lokale Entwicklung benutzerdefinierter Asset Compute-Mitarbeiter verwendet wird.

```
$ npm install -g @adobe/aio-cli
```

## Installieren des Plugins für die Adobe-E/A-CLI-Elementberechnung{#aio-asset-compute}

Das Plugin für [Adobe-I/O-CLI-Asset-Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installieren von wskdebug{#wskdebug}

Laden Sie das [Apache OpenWhisk Debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm-Modul herunter und installieren Sie es, um das lokale Debugging von Asset Compute-Anwendungen zu erleichtern.

_Visual Studio-Code 1.48.x+ ist erforderlich, damit[wskdebug](#wskdebug)funktioniert._

```
$ npm install -g @openwhisk/wskdebug
```

## ngrok installieren{#ngrok}

Laden Sie das [ngrok](https://www.npmjs.com/package/ngrok) npm-Modul herunter und installieren Sie es, das öffentlichen Zugriff auf Ihren lokalen Entwicklungscomputer bietet, um das lokale Debugging von Asset Compute-Anwendungen zu erleichtern.

```
$ npm install -g ngrok --unsafe-perm=true
```
