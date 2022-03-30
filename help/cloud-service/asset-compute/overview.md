---
title: asset compute-Microservices-Erweiterbarkeit für AEM as a Cloud Service
description: Dieses Tutorial führt Sie durch die Erstellung eines einfachen Asset compute-Sekundärs, der eine Asset-Ausgabedarstellung erstellt, indem das Original-Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet wird. Obwohl der Worker selbst grundlegend ist, nutzt dieses Tutorial es, um das Erstellen, Entwickeln und Bereitstellen eines benutzerdefinierten Asset compute-Sekundärs für die Verwendung mit AEM as a Cloud Service zu untersuchen.
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
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 3%

---

# Erweiterbarkeit von asset compute-Microservices

AEM Asset compute-Microservices von Cloud Service unterstützen die Entwicklung und Bereitstellung von benutzerdefinierten Sekundären, die zum Lesen und Bearbeiten von Binärdaten von in AEM gespeicherten Assets verwendet werden, um meist benutzerdefinierte Asset-Ausgabedarstellungen zu erstellen.

Während in AEM 6.x benutzerdefinierte AEM Workflow-Prozesse zum Lesen, Transformieren und Zurückschreiben von Asset-Ausgabedarstellungen verwendet wurden, erfüllen AEM as a Cloud Service Asset compute-Sekundäre diese Anforderung.

## Was Sie tun werden

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Dieses Tutorial führt Sie durch die Erstellung eines einfachen Asset compute-Sekundärs, der eine Asset-Ausgabedarstellung erstellt, indem das Original-Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet wird. Obwohl der Worker selbst grundlegend ist, nutzt dieses Tutorial es, um das Erstellen, Entwickeln und Bereitstellen eines benutzerdefinierten Asset compute-Sekundärs für die Verwendung mit AEM as a Cloud Service zu untersuchen.

### Ziele {#objective}

1. Bereitstellung und Einrichtung der erforderlichen Konten und Dienste zum Erstellen und Bereitstellen eines Asset compute Worker
1. Erstellen und Konfigurieren eines Asset compute-Projekts
1. Entwickeln eines Asset compute Sekundärs, der eine benutzerdefinierte Ausgabedarstellung generiert
1. Schreiben von Tests für und Erfahren Sie, wie Sie den benutzerdefinierten Asset compute Worker debuggen
1. Stellen Sie den Asset compute Worker bereit und integrieren Sie ihn AEM as a Cloud Service Autorendienst über Verarbeitungsprofile

## Setup

Erfahren Sie, wie Sie sich ordnungsgemäß auf die Erweiterung von Asset compute-Workern vorbereiten und verstehen, welche Dienste und Konten bereitgestellt und konfiguriert werden müssen, sowie Software, die für die Entwicklung lokal installiert wird.

### Konto- und Dienstbereitstellung{#accounts-and-services}

Für die folgenden Konten und Dienste ist eine Bereitstellung und ein Zugriff auf erforderlich, um das Tutorial, AEM as a Cloud Service Entwicklungsumgebung oder Sandbox-Programm, den Zugriff auf App Builder und Microsoft Azure Blob Storage abzuschließen.

+ [Erbringung von Konten und Dienstleistungen](./set-up/accounts-and-services.md)

### Lokale Entwicklungsumgebung

Die lokale Entwicklung von Asset compute-Projekten erfordert ein spezifisches Entwickler-Tool-Set, das sich von der herkömmlichen AEM-Entwicklung unterscheidet, darunter: Microsoft Visual Studio Code, Docker Desktop, Node.js und unterstützende npm-Module.

+ [Lokale Entwicklungsumgebung einrichten](./set-up/development-environment.md)

### App Builder

asset compute-Projekte sind speziell definierte App Builder-Projekte und erfordern daher Zugriff auf App Builder in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

+ [App Builder einrichten](./set-up/app-builder.md)

## Entwickeln

Erfahren Sie, wie Sie ein Asset compute-Projekt erstellen und konfigurieren und dann einen benutzerdefinierten Worker entwickeln, der eine maßgeschneiderte Asset-Ausgabedarstellung generiert.

### Neues Asset compute-Projekt erstellen

asset compute-Projekte, die einen oder mehrere Asset compute-Sekundäre enthalten, werden mithilfe der interaktiven Adobe I/O-CLI generiert. asset compute-Projekte sind speziell strukturierte App Builder-Projekte, bei denen es sich um Node.js-Projekte handelt.

+ [Neues Asset compute-Projekt erstellen](./develop/project.md)

### Umgebungsvariablen konfigurieren

Umgebungsvariablen werden im `.env` für die lokale Entwicklung und werden verwendet, um Anmeldeinformationen für die Adobe I/O und Cloud-Speicher bereitzustellen, die für die lokale Entwicklung erforderlich sind.

+ [Umgebungsvariablen konfigurieren](./develop/environment-variables.md)

### Konfigurieren von manifest.yml

asset compute-Projekte enthalten Manifeste, die alle im Projekt enthaltenen Asset compute-Sekundäre definieren, sowie Informationen darüber, welche Ressourcen ihnen zur Ausführung zur Verfügung stehen, wenn sie in Adobe I/O Runtime bereitgestellt werden.

+ [Konfigurieren von manifest.yml](./develop/manifest.md)

### Entwickeln eines Sekundärs

Die Entwicklung eines Asset compute Worker bildet den Kern der Erweiterung von Asset compute-Microservices, da der Worker den benutzerdefinierten Code enthält, der die Generierung der resultierenden Asset-Ausgabedarstellung generiert oder orchestriert.

+ [Entwickeln eines Asset compute-Sekundärs](./develop/worker.md)

### Verwenden des Asset compute-Entwicklungstools

Das Asset compute Development Tool bietet eine lokale Web-Bibliothek für die Bereitstellung, Ausführung und Vorschau von Worker-generierten Ausgabedarstellungen, die eine schnelle und iterative Asset compute Worker-Entwicklung unterstützen.

+ [Verwenden des Asset compute-Entwicklungstools](./develop/development-tool.md)

## Testen und Debuggen

Erfahren Sie, wie Sie benutzerdefinierte Asset compute-Sekundäre testen, um sich auf ihren Betrieb zu verlassen, und Asset compute-Sekundäre debuggen können, um zu verstehen und Fehler bei der Ausführung des benutzerspezifischen Codes zu beheben.

### Worker testen

asset compute bietet ein Test-Framework für die Erstellung von Test-Suites für Mitarbeiter, mit dem Tests definiert werden, die sicherstellen, dass das richtige Verhalten einfach ist.

+ [Worker testen](./test-debug/test.md)

### Debuggen eines Sekundärs

asset compute-Sekundäre bieten verschiedene Debugging-Ebenen von herkömmlichem `console.log(..)` Ausgabe in Integrationen mit __VS-Code__ und  __wskdebug__, sodass Entwickler während der Ausführung in Echtzeit den Worker-Code durchlaufen können.

+ [Debuggen eines Sekundärs](./test-debug/debug.md)

## Implementieren von

Erfahren Sie, wie Sie benutzerdefinierte Asset compute-Sekundäre mit AEM as a Cloud Service integrieren können, indem Sie sie zuerst in Adobe I/O Runtime bereitstellen und dann über die Verarbeitungsprofile von AEM Assets von AEM as a Cloud Service Autoreninstanz aufrufen.

### Bereitstellen in Adobe I/O Runtime

asset compute-Sekundäre müssen in Adobe I/O Runtime bereitgestellt werden, damit sie mit AEM as a Cloud Service verwendet werden können.

+ [Verwenden von Verarbeitungsprofilen](./deploy/runtime.md)

### Integration von Workern über AEM Verarbeitungsprofile

Nach der Bereitstellung in Adobe I/O Runtime können Asset compute-Sekundäre in AEM as a Cloud Service über registriert werden. [Asset-Verarbeitungsprofile](../../assets/configuring/processing-profiles.md). Verarbeitungsprofile werden wiederum auf Asset-Ordner angewendet, die auf die darin enthaltenen Assets angewendet werden.

+ [Integration mit AEM Verarbeitungsprofilen](./deploy/processing-profiles.md)

## Erweitert

In diesen gekürzten Tutorials werden fortgeschrittene Anwendungsfälle behandelt, die auf den in den vorherigen Kapiteln gewonnenen Erkenntnissen aufbauen.

+ [Entwickeln eines Asset compute-Metadaten-Sekundärs](./advanced/metadata.md) , die Metadaten zurück in die

## Codebase auf Github

Die Codebase des Tutorials ist auf Github unter folgender Adresse verfügbar:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ Übergeordneter Zweig

Der Quellcode enthält nicht die erforderlichen `.env` oder `config.json` Dateien. Diese müssen hinzugefügt und mit Ihrer [Konten und Dienstleistungen](#accounts-and-services) Informationen.

## Zusätzliche Ressourcen

Im Folgenden finden Sie verschiedene Adoben-Ressourcen, die weitere Informationen und nützliche APIs und SDKs für die Entwicklung von Asset compute-Arbeitern bereitstellen.

### Dokumentation

+ [asset compute-Service-Dokumentation](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=de)
+ [asset compute Development Tool - Bitte lesen](https://github.com/adobe/asset-compute-devtool)
+ [asset compute-Beispiel-Sekundäre](https://github.com/adobe/asset-compute-example-workers)

### APIs und SDKs

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud-Blobstore-Wrapper-Bibliothek](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe-Knotenabruf-Wiederholungsbibliothek](https://github.com/adobe/node-fetch-retry)
+ [asset compute-Beispiel-Sekundäre](https://github.com/adobe/asset-compute-example-workers)
