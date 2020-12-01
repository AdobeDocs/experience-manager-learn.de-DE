---
title: Lokale Entwicklungs-Umgebung für Asset compute-Erweiterbarkeit einrichten
description: Die Entwicklung von Asset compute-Workern, bei denen es sich um JavaScript-Anwendungen von Node.js handelt, erfordert spezielle Entwicklungs-Tools, die sich von der herkömmlichen AEM unterscheiden, von Node.js und verschiedenen NPM-Modulen bis hin zu Docker Desktop und Microsoft Visual Studio-Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 1%

---


# Umgebung zur lokalen Entwicklung einrichten

Adobe Asset compute-Projekte können nicht in die vom AEM SDK bereitgestellte lokale AEM-Laufzeit integriert werden und werden mithilfe ihrer eigenen Toolkette entwickelt, die von der für AEM Anwendungen auf Basis des AEM Maven-Projektarchivs benötigt wird.

Um Asset compute microservices zu erweitern, müssen die folgenden Tools auf dem lokalen Entwicklercomputer installiert sein.

## Abgekürzte Einrichtungsanweisungen

Im Folgenden finden Sie eine Anleitung zur Einrichtung des Abkürzungssystems. Details zu diesen Entwicklungstools sind in den einzelnen Abschnitten unten beschrieben.

1. [Installieren Sie Docker ](https://www.docker.com/products/docker-desktop) Desktop und ziehen Sie die erforderlichen Docker-Bilder:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Installieren von Visual Studio-Code](https://code.visualstudio.com/download)
1. [Node.js 10+ installieren](../../local-development-environment/development-tools.md#node-js)
1. Installieren Sie die erforderlichen NPM-Module und Adobe I/O CLI-Plug-Ins über die Befehlszeile:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Weitere Informationen zu den verkürzten Installationsanweisungen finden Sie in den folgenden Abschnitten.

## Installieren von Visual Studio-Code{#vscode}

[Microsoft Visual Studio-](https://code.visualstudio.com/download) Code zum Entwickeln und Debuggen von Asset compute-Workern. Während andere [JavaScript-kompatible IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) zum Entwickeln des Workers verwendet werden können, kann nur Visual Studio-Code in [debug](../test-debug/debug.md) Asset compute-Worker integriert werden.

_Visual Studio-Code 1.48.x+ ist erforderlich, damit  [](#wskdebug) wskdebuggen funktioniert._

In diesem Lernprogramm wird von der Verwendung von Visual Studio-Code ausgegangen, da es das beste Entwicklererlebnis zum Erweitern von Asset compute bietet.

## Docker Desktop{#docker} installieren

Laden Sie die neueste, stabile [Docker Desktop](https://www.docker.com/products/docker-desktop) herunter und installieren Sie sie, da dies für die lokale Asset compute von [test](../test-debug/test.md)- und [debug](../test-debug/debug.md)-Projekten erforderlich ist.

Nachdem Sie Docker Desktop installiert haben, führen Sie den Beginn aus und installieren Sie die folgenden Docker-Bilder über die Befehlszeile:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Entwickler auf Windows-Computern sollten sicherstellen, dass sie Linux-Container für die oben genannten Bilder verwenden. Die Schritte zum Wechseln zu Linux-Containern werden in der [Docker for Windows-Dokumentation](https://docs.docker.com/docker-for-windows/) beschrieben.

## Node.js (und npm){#node-js} installieren

asset compute-Worker sind [Node.js](https://nodejs.org/)-basiert und benötigen daher Node.js 10+ (und npm), um zu entwickeln und zu erstellen.

+ [Installieren Sie Node.js (und npm) ](../../local-development-environment/development-tools.md#node-js) auf die gleiche Weise wie bei der herkömmlichen AEM.

## Adobe I/O CLI{#aio} installieren

[Installieren Sie das Adobe I/O CLI](../../local-development-environment/development-tools.md#aio-cli) oder  ____ aiois ein Befehlszeilenmodul (CLI), das die Verwendung von Adobe I/O-Technologien und deren Interaktion mit diesen ermöglicht. Es wird sowohl für Generierungs- als auch für die lokale Entwicklung von benutzerdefinierten Asset compute-Workern verwendet.

```
$ npm install -g @adobe/aio-cli
```

## Installieren Sie das Adobe I/O CLI Asset compute-Plugin{#aio-asset-compute}

Das [Adobe I/O CLI Asset compute-Plugin](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installation von wskdebug{#wskdebug}

Laden Sie das Modul [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm herunter und installieren Sie es, um das lokale Debugging von Asset compute-Workern zu erleichtern.

_Visual Studio-Code 1.48.x+ ist erforderlich, damit  [](#wskdebug) wskdebuggen funktioniert._

```
$ npm install -g @openwhisk/wskdebug
```

## Installieren Sie ngrok{#ngrok}

Laden Sie das Modul [ngrok](https://www.npmjs.com/package/ngrok) npm herunter und installieren Sie es, das öffentlichen Zugriff auf Ihren lokalen Entwicklungscomputer bietet, um das lokale Debugging von Asset compute-Workern zu erleichtern.

```
$ npm install -g ngrok --unsafe-perm=true
```
