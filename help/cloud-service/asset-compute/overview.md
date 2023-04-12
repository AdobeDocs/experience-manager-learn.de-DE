---
title: Erweiterbarkeit der Asset Compute-Microservices für AEM as a Cloud Service
description: In diesem Tutorial wird erläutert, wie ein einfacher Asset Compute-Sekundär erstellt wird, der eine Asset-Ausgabedarstellung bereitstellt, indem das Original-Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet werden. Während der Sekundär selbst recht einfach ist, greift dieses Tutorial darauf zurück, um das Erstellen, Entwickeln und Bereitstellen eines benutzerdefinierten Asset Compute-Sekundärs für die Verwendung mit AEM as a Cloud Service zu untersuchen.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: ht
source-wordcount: '1020'
ht-degree: 100%

---

# Erweiterbarkeit von Asset Compute-Microservices

Die Asset Compute-Microservices von AEM as a Cloud Service unterstützen die Entwicklung und Bereitstellung von benutzerdefinierten Sekundären, die zum Lesen und Bearbeiten von Binärdaten von in AEM gespeicherten Assets verwendet werden, am häufigsten mit dem Ziel, benutzerdefinierte Asset-Ausgabedarstellungen zu erstellen.

Während in AEM 6.x benutzerdefinierte AEM-Workflow-Prozesse zum Lesen, Transformieren und Zurückschreiben von Asset-Ausgabedarstellungen verwendet wurden, werden diese Anforderungen in AEM as a Cloud Service von Asset Compute-Sekundären erfüllt.

## Vorgehensweise

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

In diesem Tutorial wird erläutert, wie ein einfacher Asset Compute-Sekundär erstellt wird, der eine Asset-Ausgabedarstellung bereitstellt, indem das Original-Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet werden. Während der Sekundär selbst recht einfach ist, greift dieses Tutorial darauf zurück, um das Erstellen, Entwickeln und Bereitstellen eines benutzerdefinierten Asset Compute-Sekundärs für die Verwendung mit AEM as a Cloud Service zu untersuchen.

### Ziele {#objective}

1. Bereitstellen und Einrichten der erforderlichen Konten und Services zum Erstellen und Bereitstellen eines Asset Compute-Sekundärs
1. Erstellen und Konfigurieren eines Asset Compute-Projekts
1. Entwickeln eines Asset Compute-Sekundärs, der eine benutzerdefinierte Ausgabedarstellung generiert
1. Schreiben von Tests für benutzerdefinierte Asset Compute-Sekundäre und lernen, wie Sie diese debuggen
1. Bereitstellen des Asset Compute-Sekundärs und Integrieren dieses Sekundärs in den Autoren-Service von AEM as a Cloud Service über Verarbeitungsprofile

## Setup

Lernen Sie, wie Sie sich ordnungsgemäß auf die Erweiterung von Asset Compute-Sekundären vorbereiten, und erfahren Sie, welche Services und Konten bereitgestellt und konfiguriert sowie welche Software-Programme lokal für Entwicklungszwecke installiert werden müssen.

### Konto- und Service-Bereitstellung{#accounts-and-services}

Für die folgenden Konten und Services ist eine Bereitstellung von und ein Zugriff auf App Builder und Microsoft Azure Blob Storage erforderlich, um das Tutorial für die AEM as a Cloud Service-Entwicklungsumgebung oder das Sandbox-Programm abzuschließen.

+ [Bereistellen von Konten und Services](./set-up/accounts-and-services.md)

### Lokale Entwicklungsumgebung

Für die lokale Entwicklung von Asset Compute-Projekten ist ein spezifischer Satz von Entwickler-Tools erforderlich, der sich von der herkömmlichen AEM-Entwicklung unterscheidet. Dazu gehören Microsoft Visual Studio Code, Docker Desktop, Node.js und unterstützende npm-Module.

+ [Einrichten einer lokalen Entwicklungsumgebung](./set-up/development-environment.md)

### App Builder

Asset Compute-Projekte sind speziell definierte App Builder-Projekte und erfordern daher Zugriff auf App Builder in Adobe Developer Console, um sie einzurichten und bereitzustellen.

+ [Einrichten von App Builder](./set-up/app-builder.md)

## Entwickeln

Erfahren Sie, wie Sie ein Asset Compute-Projekt erstellen und konfigurieren und dann einen benutzerdefinierten Sekundär entwickeln können, der eine maßgeschneiderte Asset-Ausgabedarstellung generiert.

### Erstellen eines neuen Asset Compute-Projekts

Asset Compute-Projekte, die einen oder mehrere Asset Compute-Sekundäre enthalten, werden mithilfe der interaktiven Adobe I/O-Befehlszeilenschnittstelle generiert. Bei Asset Compute-Projekten handelt es sich um speziell strukturierte App Builder-Projekte, die wiederum Node.js-Projekte sind.

+ [Erstellen eines neuen Asset Compute-Projekts](./develop/project.md)

### Konfigurieren von Umgebungsvariablen

Umgebungsvariablen werden in der Datei `.env` für die lokale Entwicklung verwaltet und verwendet, um die für die lokale Entwicklung erforderlichen Adobe I/O- und Cloud-Speicher-Anmeldeinformationen bereitzustellen.

+ [Konfigurieren der Umgebungsvariablen](./develop/environment-variables.md)

### Konfigurieren von manifest.yml

Asset Compute-Projekte enthalten Manifeste, die alle im Projekt enthaltenen Asset Compute-Sekundäre definieren, sowie Informationen darüber, welche Ressourcen ihnen bei Adobe I/O Runtime-Bereitstellng zur Ausführung zur Verfügung stehen.

+ [Konfigurieren von manifest.yml](./develop/manifest.md)

### Entwickeln eines Sekundärs

Die Entwicklung eines Asset Compute-Sekundärs bildet den Kern der Erweiterung von Asset Compute-Microservices, da der Sekundär den benutzerdefinierten Code enthält, der die Generierung der resultierenden Asset-Ausgabedarstellung durchführt oder orchestriert.

+ [Entwickeln eines Asset Compute-Sekundärs](./develop/worker.md)

### Verwenden des Asset Compute-Entwicklungs-Tools

Das Asset Compute-Entwicklungs-Tool bietet eine lokale Web-Umgebung für die Bereitstellung, Ausführung und Vorschau von Sekundär-generierten Ausgabedarstellungen. Auf diese Weise wird eine schnelle und iterative Entwicklung von Asset Compute-Sekundären unterstützt.

+ [Verwenden des Asset Compute-Entwicklungs-Tools](./develop/development-tool.md)

## Testen und Debuggen

Erfahren Sie, wie Sie benutzerdefinierte Asset Compute-Sekundäre für einen zuverlässigen Betrieb testen und Asset Compute-Sekundäre debuggen können, um die Ausführung des benutzerspezifische Codes zu verstehen und dabei auftretende Fehler zu beheben.

### Testen eines Sekundärs

Asset Compute bietet ein Test-Framework zur Erstellung von Test-Suites für Sekundäre, sodass sich Tests zum Sicherstellen einwandfreien Verhaltens problemlos definieren lassen.

+ [Testen eines Sekundärs](./test-debug/test.md)

### Debugging eines Sekundärs

Asset Compute-Sekundäre stellen verschiedene Debugging-Ebenen bereit – von herkömmlicher Ausgabe über `console.log(..)` bis zu Integrationen mit __VS-Code__ und __wskdebug__, sodass Entwicklerinnen und Entwickler während der Ausführung in Echtzeit schrittweise durch den Code des Sekundärs gehen können.

+ [Debugging eines Sekundärs](./test-debug/debug.md)

## Implementieren

Erfahren Sie, wie Sie benutzerdefinierte Asset Compute-Sekundäre mit AEM as a Cloud Service integrieren können, indem Sie sie zuerst in Adobe I/O Runtime bereitstellen und dann über die AEM Assets-Verarbeitungsprofile von der Autoreninstanz in AEM as a Cloud Service aufrufen.

### Bereitstellen für Adobe I/O Runtime

Asset Compute-Sekundäre müssen in Adobe I/O Runtime bereitgestellt werden, damit sie mit AEM as a Cloud Service verwendet werden können.

+ [Verwenden von Verarbeitungsprofilen](./deploy/runtime.md)

### Integration von Sekundären über AEM Verarbeitungsprofile

Nach der Bereitstellung in Adobe I/O Runtime können Asset Compute-Sekundäre in AEM as a Cloud Service über [Asset-Verarbeitungsprofile](../../assets/configuring/processing-profiles.md) angemeldet werden. Verarbeitungsprofile werden wiederum auf Asset-Ordner angewendet, die sie auf die darin enthaltenen Assets anwenden.

+ [Integration mit AEM-Verarbeitungsprofilen](./deploy/processing-profiles.md)

## Erweitert

Diese gekürzten Tutorials behandeln fortgeschrittene Anwendungsfälle, die auf den in den vorherigen Kapiteln gewonnenen Erkenntnissen aufbauen.

+ [Entwickeln eines Asset Compute-Metadaten-Sekundärs](./advanced/metadata.md), der Metadaten zurück in die

## Codebase auf Github schreiben kann

Die Codebase des Tutorials ist auf Github verfügbar unter:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ master branch

Der Quell-Code enthält nicht die erforderlichen `.env`- oder `config.json`-Dateien. Diese müssen unter Verwendung Ihrer Informationen zu [Konten und Services](#accounts-and-services) hinzugefügt werden.

## Zusätzliche Ressourcen

Im Folgenden finden Sie verschiedene Adobe-Ressourcen, die weitere Informationen und nützliche APIs und SDKs für die Entwicklung von Asset Compute-Sekundären bereitstellen.

### Dokumentation

+ [Dokumentation zum Asset Compute-Service](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=de)
+ [Asset Compute-Entwicklungs-Tool – Bitte lesen](https://github.com/adobe/asset-compute-devtool)
+ [Beispiel-Sekundäre in Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs und SDKs

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute-Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute-XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud-Blobstore-Wrapper-Bibliothek](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe-Knotenabrufwiederholungs-Bibliothek](https://github.com/adobe/node-fetch-retry)
+ [Beispiel-Sekundäre in Asset Compute](https://github.com/adobe/asset-compute-example-workers)
