---
title: Einrichten einer lokalen Entwicklungsumgebung für die Asset Compute-Erweiterbarkeit
description: Die Entwicklung von Asset Compute-Sekundären, bei denen es sich um JavaScript-Anwendungen von Node.js handelt, erfordert spezifische Entwicklungs-Tools, die sich von der herkömmlichen AEM-Entwicklung unterscheiden, die von Node.js und verschiedenen npm-Modulen bis hin zu Docker Desktop und Microsoft Visual Studio Code reichen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 111
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 100%

---

# Einrichten einer lokalen Entwicklungsumgebung

Adobe Asset Compute-Projekte können nicht in die vom AEM SDK bereitgestellte lokale AEM-Laufzeit integriert werden und werden mithilfe ihrer eigenen Kette von Tools entwickelt, die sich von der für AEM-Anwendungen basierend auf dem AEM Maven-Projektarchetyp erforderlichen unterscheidet.

Um Asset Compute-Microservices zu erweitern, müssen die folgenden Tools auf dem lokalen Computer der Entwicklungsperson installiert sein.

## Abgekürzte Anweisungen zur Einrichtung

Im Folgenden finden Sie eine verkürzte Anleitung zur Einrichtung. Details zu diesen Entwicklungs-Tools werden in den folgenden Abschnitten beschrieben.

1. [Installieren Sie Docker Desktop](https://www.docker.com/products/docker-desktop) und rufen Sie die erforderlichen Docker-Bilder ab:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installieren Sie Visual Studio Code](https://code.visualstudio.com/download)
1. [Installieren Sie Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installieren Sie die erforderlichen npm-Module und Adobe I/O-CLI-Plug-ins über die Befehlszeile:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Weitere Informationen zu den verkürzten Installationsanweisungen finden Sie in den folgenden Abschnitten.

## Installieren von Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) wird zum Entwickeln und Debuggen von Asset Compute-Sekundären verwendet. Es kann zwar auch eine andere [JavaScript-kompatible IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) zum Entwickeln des Sekundärs verwendet werden, jedoch kann nur Visual Studio Code integriert werden, um den Asset Compute-Sekundär zu [debuggen](../test-debug/debug.md).

In diesem Tutorial wird von der Verwendung von Visual Studio Code ausgegangen, da es das beste Entwicklererlebnis für die Erweiterung von Asset Compute bietet.

## Installieren von Docker Desktop{#docker}

Laden Sie die neueste, stabile Version von [Docker Desktop](https://www.docker.com/products/docker-desktop) herunter und installieren Sie sie, da dies erforderlich ist, um Asset Compute-Projekte lokal zu [testen](../test-debug/test.md) und zu [debuggen](../test-debug/debug.md).

Starten Sie Docker Desktop nach der Installation und installieren Sie die folgenden Docker-Bilder über die Befehlszeile:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Entwicklerinnen und Entwickler auf Windows-Computern sollten sicherstellen, dass sie Linux-Container für die oben genannten Bilder verwenden. Die Schritte zum Wechseln zu Linux-Containern werden im Abschnitt [Dokumentation zu Docker für Windows](https://docs.docker.com/docker-for-windows/) beschrieben.

## Installieren von Node.js (und npm){#node-js}

Asset Compute-Sekundäre sind [Node.js](https://nodejs.org/)-basiert und benötigen daher Node.js 10+ (und npm) zur Entwicklung und Erstellung.

+ [Installieren Sie Node.js (und npm)](../../local-development-environment/development-tools.md#node-js) auf die gleiche Weise wie bei der traditionellen AEM-Entwicklung.

## Installieren von Adobe I/O CLI{#aio}

[Installieren Sie Adobe I/O-CLI](../../local-development-environment/development-tools.md#aio-cli) oder __aio__. Dabei handelt es sich um ein Befehlszeilen(CLI)-NPM-Modul, das die Nutzung von und die Interaktion mit Adobe I/O-Technologien erleichtert und sowohl zum Generieren als auch zur lokalen Entwicklung benutzerdefinierter Asset Compute-Sekundäre verwendet wird.

```
$ npm install -g @adobe/aio-cli
```

## Installieren des Asset Compute-Plug-ins für Adobe I/O-CLI{#aio-asset-compute}

Das [Asset Compute-Plug-in von Adobe I/O-CLI](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installieren von wskdebug{#wskdebug}

Laden Sie das npm-Modul [Apache OpenWhisk-debug](https://www.npmjs.com/package/@openwhisk/wskdebug) herunter und installieren Sie es, um das lokale Debugging von Asset Compute-Sekundären zu erleichtern.

_Visual Studio Code 1.48.x+ ist erforderlich, damit [wskdebug](#wskdebug) funktioniert._

```
$ npm install -g @openwhisk/wskdebug
```

## Installieren von ngrok{#ngrok}

Laden Sie das npm-Modul [ngrok](https://www.npmjs.com/package/ngrok) herunter und installieren Sie es. Es bietet öffentlichen Zugriff auf Ihren lokalen Entwicklungs-Computer, um das lokale Debugging von Asset Compute-Sekundären zu erleichtern.

```
$ npm install -g ngrok --unsafe-perm=true
```
