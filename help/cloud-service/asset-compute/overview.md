---
title: asset compute microservices-Erweiterbarkeit für AEM als Cloud Service
description: Dieses Lernprogramm führt Sie durch die Erstellung eines einfachen Asset compute-Workers, der eine Asset-Darstellung erstellt, indem das ursprüngliche Asset auf einen Kreis zugeschnitten und konfigurierbare Kontrast- und Helligkeitseinstellungen angewendet werden. Obwohl der Mitarbeiter selbst ein Grundelement ist, verwendet dieses Lernprogramm es, um die Erstellung, Entwicklung und Bereitstellung eines benutzerdefinierten Asset compute-Workers für die Verwendung mit AEM als Cloud Service zu untersuchen.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 3%

---


# asset compute Microservices-Erweiterbarkeit

AEM die Asset compute Microservices von Cloud Service unterstützen die Entwicklung und Bereitstellung von benutzerdefinierten Workern, die zum Lesen und Bearbeiten von Binärdaten von in AEM gespeicherten Assets verwendet werden, um in der Regel benutzerdefinierte Asset-Darstellungen zu erstellen.

Während in AEM 6.x benutzerdefinierte AEM Workflow-Prozesse zum Lesen, Transformieren und Zurückschreiben von Asset-Darstellungen verwendet wurden, AEM als Cloud Service-Asset compute-Mitarbeiter diese Anforderung erfüllen.

## Was Sie tun werden

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Dieses Lernprogramm führt Sie durch die Erstellung eines einfachen Asset compute-Workers, der eine Asset-Darstellung erstellt, indem das ursprüngliche Asset auf einen Kreis zugeschnitten und konfigurierbare Kontrast- und Helligkeitseinstellungen angewendet werden. Obwohl der Mitarbeiter selbst ein Grundelement ist, verwendet dieses Lernprogramm es, um die Erstellung, Entwicklung und Bereitstellung eines benutzerdefinierten Asset compute-Workers für die Verwendung mit AEM als Cloud Service zu untersuchen.

### Ziele {#objective}

1. Bereitstellung und Einrichtung der erforderlichen Konten und Dienste zum Aufbau und Einsatz eines Asset compute-Arbeiters
1. Erstellen und Konfigurieren eines Asset compute-Projekts
1. Entwickeln Sie am Asset compute-Worker, der eine benutzerdefinierte Darstellung generiert
1. Testen und Debuggen des benutzerdefinierten Asset compute-Workers schreiben
1. Stellen Sie den Asset compute-Worker bereit und integrieren Sie ihn AEM als Cloud Service-Autorendienst über VerarbeitungsProfile

## Setup

Erfahren Sie, wie Sie sich ordnungsgemäß auf die Erweiterung von Asset compute-Workern vorbereiten, und lernen Sie, welche Dienste und Konten bereitgestellt und konfiguriert werden müssen und welche Software lokal für die Entwicklung installiert werden muss.

### Konto- und Dienstbereitstellung{#accounts-and-services}

Die folgenden Konten und Dienste erfordern Bereitstellung und Zugriff, um das Tutorial, AEM als Cloud Service Dev Umgebung oder Sandbox Programm, Zugriff auf Adobe Project Firefly und Microsoft Azurblase Blob Datenspeicherung abzuschließen.

+ [Erbringung von Konten und Dienstleistungen](./set-up/accounts-and-services.md)

### Umgebung der lokalen Entwicklung

Für die lokale Entwicklung von Asset compute-Projekten ist ein spezielles Entwicklerwerkzeug erforderlich, das sich von der herkömmlichen AEM unterscheidet. Dazu gehören: Microsoft Visual Studio Code, Docker Desktop, Node.js und unterstützende npm Module.

+ [Umgebung zur lokalen Entwicklung einrichten](./set-up/development-environment.md)

### Projekt Adobe

asset compute-Projekte sind speziell definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

+ [Adobe-Projekt einrichten](./set-up/firefly.md)

## Entwicklung

Erfahren Sie, wie Sie ein Asset compute-Projekt erstellen und konfigurieren und dann einen benutzerdefinierten Worker entwickeln, der eine benutzerdefinierte Asset-Darstellung generiert.

### Neues Asset compute-Projekt erstellen

asset compute-Projekte, die einen oder mehrere Asset compute-Mitarbeiter enthalten, werden mithilfe der interaktiven Adobe I/O-CLI generiert. asset compute-Projekte sind speziell strukturierte Adobe Projekt Firefly Projekte, die wiederum Node.js Projekte.

+ [Neues Asset compute-Projekt erstellen](./develop/project.md)

### Umgebungsvariablen konfigurieren

Die Variablen &quot;Umgebung&quot;werden in der Datei `.env` für die lokale Entwicklung beibehalten und dienen zur Bereitstellung von Adobe I/O-Anmeldeinformationen und Cloud-Datenspeicherung-Anmeldeinformationen, die für die lokale Entwicklung erforderlich sind.

+ [Umgebung konfigurieren](./develop/environment-variables.md)

### manifest.yml konfigurieren

asset compute-Projekte enthalten Manifeste, die alle im Projekt enthaltenen Asset compute-Mitarbeiter definieren, sowie Angaben zu den Ressourcen, die ihnen zur Verfügung stehen, wenn sie zur Ausführung in Adobe I/O Runtime bereitgestellt werden.

+ [manifest.yml konfigurieren](./develop/manifest.md)

### Entwickeln eines Arbeitnehmers

Die Entwicklung eines Asset compute-Workers ist der Kern der Erweiterung von Asset compute-Mikrodiensten, da der Worker den benutzerdefinierten Code enthält, der die Generierung der resultierenden Asset-Darstellung generiert oder orchestriert.

+ [Entwickeln eines Asset compute-Workers](./develop/worker.md)

### Verwenden des Asset compute Development Tool

Das Asset compute Development Tool bietet eine lokale Weboberfläche zum Bereitstellen, Ausführen und Anzeigen einer Vorschau von vom Benutzer erstellten Darstellungen, um eine schnelle und iterative Asset compute-Worker-Entwicklung zu unterstützen.

+ [Verwenden des Asset compute Development Tool](./develop/development-tool.md)

## Testen und Debuggen

Erfahren Sie, wie Sie benutzerdefinierte Asset compute-Mitarbeiter testen, um sich mit ihrem Betrieb vertraut zu machen, und Asset compute-Workern debuggen, um zu verstehen und Fehler bei der Ausführung des benutzerspezifischen Codes zu beheben.

### Arbeiter testen

asset compute bietet ein Testframework zum Erstellen von Test-Suites für Arbeiter, sodass Tests, die sicherstellen, dass das richtige Verhalten einfach ist, definiert werden können.

+ [Arbeiter testen](./test-debug/test.md)

### Debuggen eines Arbeitnehmers

asset compute-Workers bieten verschiedene Debugging-Ebenen, von der herkömmlichen `console.log(..)`-Ausgabe bis hin zu Integrationen mit __VS-Code__ und __wskdebug__, sodass Entwickler während der Ausführung in Echtzeit den Arbeitscode durchlaufen können.

+ [Debuggen eines Arbeitnehmers](./test-debug/debug.md)

## Bereitstellen

Erfahren Sie, wie Sie benutzerdefinierte Asset compute-Mitarbeiter mit AEM als Cloud Service integrieren, indem Sie sie zunächst in Adobe I/O Runtime bereitstellen und dann über die VerarbeitungsProfil von AEM Assets von AEM als Cloud Service-Autor aufrufen.

### Auf Adobe I/O Runtime bereitstellen

asset compute-Mitarbeiter müssen nach Adobe I/O Runtime entsandt werden, um AEM als Cloud Service zu verwenden.

+ [Verwenden von Profilen zur Verarbeitung](./deploy/runtime.md)

### Integrieren von Mitarbeitern über AEM VerarbeitungsProfil

Nach der Bereitstellung in Adobe I/O Runtime können Asset compute-Mitarbeiter über [Assets Processing Profils](../../assets/configuring/processing-profiles.md) in AEM als Cloud Service registriert werden. Verarbeitungsordner werden wiederum auf Asset-Profil angewendet, die auf die darin enthaltenen Assets angewendet werden.

+ [Integration mit AEM-Profilen](./deploy/processing-profiles.md)

## Erweitert

Diese gekürzten Lernprogramme behandeln fortgeschrittenere Nutzungsszenarien, die auf den in den vorherigen Kapiteln gewonnenen Erkenntnissen aufbauen.

+ [Entwickeln eines Asset compute-Metadaten-](./advanced/metadata.md) Workers, der Metadaten in die

## Codebase auf Github

Die Codebase des Tutorials ist auf Github verfügbar unter:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ Übergeordnet-branch

Der Quellcode enthält nicht die erforderlichen `.env`- oder `config.json`-Dateien. Diese müssen mit den [Konten und Dienste](#accounts-and-services)-Informationen hinzugefügt und konfiguriert werden.

## Zusätzliche Ressourcen

Im Folgenden finden Sie verschiedene Adoben-Ressourcen, die weitere Informationen und nützliche APIs und SDKs für die Entwicklung von Asset compute-Workern bereitstellen.

### Dokumentation

+ [Dokumentation zum asset compute-Dienst](https://docs.adobe.com/content/help/de/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute Development Tool - Bitte lesen](https://github.com/adobe/asset-compute-devtool)
+ [asset compute-Beispielarbeiter](https://github.com/adobe/asset-compute-example-workers)

### APIs und SDKs

+ [asset compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper-Bibliothek](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adoben-Node - Wiederholungsbibliothek abrufen](https://github.com/adobe/node-fetch-retry)
+ [asset compute-Beispielarbeiter](https://github.com/adobe/asset-compute-example-workers)
