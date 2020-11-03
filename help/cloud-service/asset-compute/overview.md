---
title: Erweiterbarkeit von Asset Computing-Mikrodiensten für AEM als Cloud Service
description: Dieses Lernprogramm führt Sie durch die Erstellung eines einfachen Workers für Asset Compute, der eine Asset-Darstellung erstellt, indem das ursprüngliche Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet wird. Obwohl der Mitarbeiter selbst ein grundlegendes Element ist, werden in diesem Lernprogramm die Erstellung, Entwicklung und Bereitstellung eines benutzerdefinierten Asset Compute-Workers für die Verwendung mit AEM als Cloud Service erforscht.
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
ht-degree: 1%

---


# Erweiterbarkeit von Asset Computing-Mikrodiensten

AEM die Asset Compute-Mikrodienste von Cloud Service unterstützen die Entwicklung und Bereitstellung von benutzerdefinierten Workern, die zum Lesen und Manipulieren von Binärdaten von in AEM gespeicherten Assets verwendet werden, um in der Regel benutzerdefinierte Asset-Darstellungen zu erstellen.

Während in AEM 6.x benutzerdefinierte AEM Workflow-Prozesse zum Lesen, Transformieren und Zurückschreiben von Asset-Darstellungen verwendet wurden, AEM als Cloud Service Asset Compute-Mitarbeiter diese Anforderung erfüllen.

## Was Sie tun werden

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Dieses Lernprogramm führt Sie durch die Erstellung eines einfachen Workers für Asset Compute, der eine Asset-Darstellung erstellt, indem das ursprüngliche Asset auf einen Kreis zugeschnitten und konfigurierbarer Kontrast und Helligkeit angewendet wird. Obwohl der Mitarbeiter selbst ein grundlegendes Element ist, werden in diesem Lernprogramm die Erstellung, Entwicklung und Bereitstellung eines benutzerdefinierten Asset Compute-Workers für die Verwendung mit AEM als Cloud Service erforscht.

### Ziele {#objective}

1. Bereitstellung und Einrichtung der erforderlichen Konten und Dienste zum Erstellen und Bereitstellen eines Asset Compute-Mitarbeiters
1. Erstellen und Konfigurieren eines Asset Computing-Projekts
1. Entwickler am Asset Compute-Mitarbeiter, der eine benutzerdefinierte Darstellung generiert
1. Testen und Debuggen des benutzerdefinierten Asset Compute-Workers schreiben
1. Stellen Sie den Asset Compute-Mitarbeiter bereit und integrieren Sie ihn AEM als Cloud Service-Autorendienst über Verarbeitungswerkzeuge.

## Setup

Erfahren Sie, wie Sie sich ordnungsgemäß auf die Erweiterung von Asset Compute-Mitarbeitern vorbereiten und verstehen, welche Dienste und Konten bereitgestellt und konfiguriert werden müssen und welche Software lokal für die Entwicklung installiert werden muss.

### Konto- und Dienstbereitstellung{#accounts-and-services}

Die folgenden Konten und Dienste erfordern Bereitstellung und Zugriff, um das Tutorial, AEM als Cloud Service Dev Umgebung oder Sandbox Programm, Zugriff auf Adobe Project Firefly und Microsoft Azurblase Blob Datenspeicherung abzuschließen.

+ [Erbringung von Konten und Dienstleistungen](./set-up/accounts-and-services.md)

### Umgebung der lokalen Entwicklung

Für die lokale Entwicklung von Asset Compute-Projekten ist ein spezieller Entwicklerwerkzeug erforderlich, das sich von der herkömmlichen AEM unterscheidet. Dazu gehören: Microsoft Visual Studio Code, Docker Desktop, Node.js und unterstützende npm Module.

+ [Umgebung zur lokalen Entwicklung einrichten](./set-up/development-environment.md)

### Projekt Adobe

Asset Compute-Projekte sind eigens definierte Adobe Project Firefly-Projekte und erfordern daher Zugriff auf Adobe Project Firefly in der Adobe Developer Console, um sie einzurichten und bereitzustellen.

+ [Adobe-Projekt einrichten](./set-up/firefly.md)

## Entwicklung

Erfahren Sie, wie Sie ein Asset Compute-Projekt erstellen und konfigurieren und dann einen benutzerdefinierten Worker entwickeln, der eine benutzerdefinierte Asset-Darstellung generiert.

### Neues Asset-Computingprojekt erstellen

Asset Compute-Projekte, die einen oder mehrere Asset Compute-Mitarbeiter enthalten, werden mithilfe der interaktiven Adobe-I/O-CLI generiert. Asset Compute Projekte sind speziell strukturierte Adobe Project Firefly Projekte, die wiederum Node.js Projekte.

+ [Neues Asset-Computingprojekt erstellen](./develop/project.md)

### Umgebungsvariablen konfigurieren

Umgebung-Variablen werden in der `.env` Datei für die lokale Entwicklung beibehalten und zur Bereitstellung von Adobe-E/A-Anmeldeinformationen und Cloud-Datenspeicherung-Anmeldeinformationen verwendet, die für die lokale Entwicklung erforderlich sind.

+ [Umgebung konfigurieren](./develop/environment-variables.md)

### manifest.yml konfigurieren

Asset Compute-Projekte enthalten Manifeste, die alle im Projekt enthaltenen Asset Compute-Mitarbeiter sowie die verfügbaren Ressourcen definieren, wenn sie zur Ausführung in Adobe I/O Runtime bereitgestellt werden.

+ [manifest.yml konfigurieren](./develop/manifest.md)

### Entwickeln eines Arbeitnehmers

Die Entwicklung eines Asset Compute-Workers ist der Kern der Erweiterung von Asset Compute-Mikrodiensten, da der Worker den benutzerdefinierten Code enthält, der die Generierung der resultierenden Asset-Darstellung generiert oder orchestriert.

+ [Entwickeln eines Asset Computing-Mitarbeiters](./develop/worker.md)

### Verwenden Sie das Asset Compute Development Tool

Das Asset Compute Development Tool bietet eine lokale Weboberfläche zum Bereitstellen, Ausführen und Anzeigen einer Vorschau von vom Benutzer erstellten Darstellungen, die eine schnelle und iterative Entwicklung von Asset Compute-Mitarbeitern unterstützt.

+ [Verwenden Sie das Asset Compute Development Tool](./develop/development-tool.md)

## Testen und Debuggen

Hier erfahren Sie, wie Sie benutzerdefinierte Mitarbeiter von Asset Compute testen, um sich mit ihrem Betrieb vertraut zu machen, und wie Sie die Mitarbeiter von Asset Compute debuggen, um zu verstehen und Fehler bei der Ausführung des benutzerspezifischen Codes zu beheben.

### Arbeiter testen

Asset Compute bietet ein Testframework zum Erstellen von Test-Suites für Arbeiter, wodurch Tests definiert werden, die sicherstellen, dass das richtige Verhalten einfach ist.

+ [Arbeiter testen](./test-debug/test.md)

### Debuggen eines Arbeitnehmers

Asset Compute-Mitarbeiter bieten verschiedene Debugging-Ebenen, von der herkömmlichen `console.log(..)` Ausgabe über Integrationen mit __VS-Code__ bis hin zum __wskdebug__, sodass Entwickler während der Ausführung in Echtzeit den Arbeitsablaufcode durchlaufen können.

+ [Debuggen eines Arbeitnehmers](./test-debug/debug.md)

## Bereitstellen

Erfahren Sie, wie Sie benutzerdefinierte Asset-Compute-Mitarbeiter mit AEM als Cloud Service integrieren, indem Sie sie zunächst in Adobe I/O Runtime bereitstellen und dann über die Verarbeitungselemente von AEM Assets von AEM als Cloud Service-Autor aufrufen.

### Auf Adobe I/O Runtime bereitstellen

Mitarbeiter in Asset Compute müssen nach Adobe I/O Runtime entsandt werden, um AEM als Cloud Service zu verwenden.

+ [Verwenden von Profilen zur Verarbeitung](./deploy/runtime.md)

### Integrieren von Mitarbeitern über AEM VerarbeitungsProfil

Nach der Bereitstellung in Adobe I/O Runtime können Mitarbeiter von Asset Compute über [Assets Processing Profils](../../assets/configuring/processing-profiles.md)in AEM als Cloud Service registriert werden. Verarbeitungsordner werden wiederum auf Asset-Profil angewendet, die auf die darin enthaltenen Assets angewendet werden.

+ [Integration mit AEM-Profilen](./deploy/processing-profiles.md)

## Erweitert

Diese gekürzten Lernprogramme behandeln fortgeschrittenere Nutzungsszenarien, die auf den in den vorherigen Kapiteln gewonnenen Erkenntnissen aufbauen.

+ [Entwickeln Sie einen Metadaten-Arbeiter](./advanced/metadata.md) zum Berechnen von Assets, der Metadaten in die

## Codebase auf Github

Die Codebase des Tutorials ist auf Github verfügbar unter:

+ [adobe/aem-guides-work-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ Übergeordnet branch

Der Quellcode enthält nicht die erforderlichen `.env` oder `config.json` Dateien. Diese müssen mit Ihren [Konto- und Dienstinformationen](#accounts-and-services) hinzugefügt und konfiguriert werden.

## Zusätzliche Ressourcen

Im Folgenden finden Sie eine Reihe von Adoben, die weitere Informationen und nützliche APIs und SDKs für die Entwicklung von Asset Compute-Mitarbeitern bereitstellen.

### Dokumentation

+ [Dokumentation zum Asset Computing-Dienst](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Asset Computing Development Tool - Bitte lesen](https://github.com/adobe/asset-compute-devtool)
+ [Beispielarbeiter von Asset Computing](https://github.com/adobe/asset-compute-example-workers)

### APIs und SDKs

+ [Asset Computing SDK](https://github.com/adobe/asset-compute-sdk)
   + [Befehle zum Berechnen von Assets](https://github.com/adobe/asset-compute-commons)
   + [XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe Cloud Blobstore Wrapper-Bibliothek](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adoben-Node - Wiederholungsbibliothek abrufen](https://github.com/adobe/node-fetch-retry)
+ [Beispielarbeiter von Asset Computing](https://github.com/adobe/asset-compute-example-workers)
