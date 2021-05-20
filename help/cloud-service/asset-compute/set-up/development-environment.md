---
title: Lokale Entwicklungsumgebung für die Asset compute-Erweiterbarkeit einrichten
description: Die Entwicklung von Asset compute-Workern, bei denen es sich um JavaScript-Anwendungen von Node.js handelt, erfordert spezifische Entwicklungs-Tools, die sich von der herkömmlichen AEM unterscheiden, von Node.js und verschiedenen npm-Modulen bis hin zu Docker Desktop und Microsoft Visual Studio Code.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrationen, Entwicklung
role: Developer
level: Intermediate, Experienced
source-git-commit: 53c20b9774c15b04a1c78c7c0c7b61a60996bf60
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 1%

---


# Lokale Entwicklungsumgebung einrichten

Adobe Asset compute-Projekte können nicht in die vom AEM SDK bereitgestellte lokale AEM-Laufzeit integriert werden und werden mithilfe ihrer eigenen Toolkette entwickelt, die von der für AEM Anwendungen basierend auf dem AEM Maven-Projektarchetyp erforderlichen unterscheidet.

Um Asset compute-Microservices zu erweitern, müssen die folgenden Tools auf dem lokalen Entwicklercomputer installiert sein.

## Abgekürzte Anweisungen zur Einrichtung

Im Folgenden finden Sie eine Anleitung zur Einrichtung von Abrissen. Details zu diesen Entwicklungstools werden in den folgenden Abschnitten beschrieben.

1. [Installieren Sie Docker ](https://www.docker.com/products/docker-desktop) Desktops und rufen Sie die erforderlichen Docker-Bilder ab:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installieren von Visual Studio Code](https://code.visualstudio.com/download)
1. [Installieren von Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installieren Sie die erforderlichen npm-Module und Adobe I/O CLI-Plug-ins über die Befehlszeile:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Weitere Informationen zu den verkürzten Installationsanweisungen finden Sie in den folgenden Abschnitten.

## Installieren von Visual Studio Code{#vscode}

[Microsoft Visual Studio-](https://code.visualstudio.com/download) Codes zum Entwickeln und Debuggen von Asset compute-Sekundären. Während andere [JavaScript-kompatible IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) zum Entwickeln des Workers verwendet werden können, kann nur Visual Studio Code in [debug](../test-debug/debug.md) Asset compute Worker integriert werden.

_Visual Studio Code 1.48.x+ ist erforderlich, damit  [](#wskdebug) wskdebuggen funktioniert._

In diesem Tutorial wird von der Verwendung von Visual Studio Code ausgegangen, da es das beste Entwicklererlebnis für die Erweiterung von Asset compute bietet.

## Docker Desktop installieren{#docker}

Laden Sie das neueste, stabile [Docker Desktop](https://www.docker.com/products/docker-desktop) herunter und installieren Sie es, da dies für [test](../test-debug/test.md)- und [debug](../test-debug/debug.md)-Asset compute-Projekte erforderlich ist.

Starten Sie Docker Desktop nach der Installation und installieren Sie die folgenden Docker-Bilder über die Befehlszeile:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Entwickler auf Windows-Computern sollten sicherstellen, dass sie Linux-Container für die oben genannten Bilder verwenden. Die Schritte zum Wechseln zu Linux-Containern werden in der [Docker for Windows-Dokumentation](https://docs.docker.com/docker-for-windows/) beschrieben.

## Installieren Sie Node.js (und npm){#node-js}

asset compute-Worker sind [Node.js](https://nodejs.org/)-basiert und erfordern daher die Entwicklung und Erstellung von Node.js 10+ (und npm).

+ [Installieren Sie Node.js (und npm)](../../local-development-environment/development-tools.md#node-js)  auf die gleiche Weise wie für die herkömmliche AEM.

## Installieren von Adobe I/O CLI{#aio}

[Installieren Sie die Adobe I/O-CLI](../../local-development-environment/development-tools.md#aio-cli) oder  ____ aiois ein Befehlszeilen-NPM-Modul (CLI), das die Nutzung von Adobe I/O-Technologien und deren Interaktion erleichtert und sowohl zum Generieren als auch zur lokalen Entwicklung benutzerdefinierter Asset compute-Sekundäre verwendet wird.

```
$ npm install -g @adobe/aio-cli
```

## Installieren Sie das Adobe I/O CLI-Asset compute-Plugin{#aio-asset-compute}

Das [Adobe I/O CLI-Asset compute-Plugin](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installieren von wskdebug{#wskdebug}

Laden Sie das Modul [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm herunter und installieren Sie es, um das lokale Debugging von Asset compute-Workern zu erleichtern.

_Visual Studio Code 1.48.x+ ist erforderlich, damit  [](#wskdebug) wskdebuggen funktioniert._

```
$ npm install -g @openwhisk/wskdebug
```

## ngrok installieren{#ngrok}

Laden Sie das npm-Modul [ngrok](https://www.npmjs.com/package/ngrok) herunter und installieren Sie es, das öffentlichen Zugriff auf Ihren lokalen Entwicklungscomputer bietet, um das lokale Debugging von Asset compute-Workern zu erleichtern.

```
$ npm install -g ngrok --unsafe-perm=true
```
