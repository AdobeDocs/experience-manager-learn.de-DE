---
title: Umgebung zur lokalen Entwicklung für AEM Cloud Service
description: Übersicht über die Umgebung der lokalen Entwicklung in Adobe Experience Manager (AEM).
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# Umgebung für lokale Entwicklung einrichten

Dieses Lernprogramm führt Sie durch die Einrichtung einer lokalen Entwicklungs-Umgebung für Adobe Experience Manager (AEM) mithilfe des AEM als Cloud Service-SDK. Dazu gehören die für die Entwicklung, Erstellung und Kompilierung AEM Projekte erforderlichen Entwicklungs-Tools sowie die Möglichkeit zur schnellen Überprüfung neuer Funktionen auf lokaler Ebene, bevor sie über Adobe Cloud Manager als Cloud Service AEM werden.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM als Cloud Service Local Development Umgebung Technology Stack](./assets/overview/aem-sdk-technology-stack.png)

Die örtliche Umgebung für AEM kann in drei logische Gruppen unterteilt werden:

+ Das __AEM Projekt__ enthält den benutzerdefinierten Code, die Konfiguration und den Inhalt, der die benutzerdefinierte AEM ist.
+ Die __lokale AEM Laufzeit__ , die eine lokale Version der AEM Author- und Publish-Dienste lokal ausführt.
+ Die __lokale Dispatcher-Laufzeit__ , die eine lokale Version von Apache HTTP Web Server und Dispatcher ausführt.

In diesem Lernprogramm wird erläutert, wie die im obigen Diagramm hervorgehobenen Elemente installiert und eingerichtet werden, um eine stabile lokale Entwicklungs-Umgebung für AEM Entwicklung zu gewährleisten.

## Dateisystemorganisation

In diesem Lernprogramm wurde der Speicherort des AEM als Cloud Service-SDK-Artefakte und AEM Projektcode wie folgt festgelegt:

+ `~/aem-sdk` ist ein Unternehmensordner mit den verschiedenen Tools, die vom AEM als Cloud Service-SDK bereitgestellt werden.
+ `~/aem-sdk/author` enthält den AEM Author-Dienst
+ `~/aem-sdk/publish` enthält den AEM Publish-Dienst
+ `~/aem-sdk/dispatcher` enthält die Dispatcher Tools
+ `~/code/<project name>` enthält den benutzerdefinierten AEM Project-Quellcode

Beachten Sie, dass dies für das Benutzerverzeichnis `~` Kurzform ist. Unter Windows entspricht dies `%HOMEPATH%`;

## Entwicklungstools für AEM Projekte

Das AEM-Projekt ist die benutzerspezifische Codebasis mit dem Code, der Konfiguration und dem Inhalt, der über Cloud Manager bereitgestellt wird, um als Cloud Service AEM zu werden. Die Projektstruktur wird über das [AEM Projekt Maven Archetype](https://github.com/adobe/aem-project-archetype)erstellt.

Dieser Abschnitt des Tutorials zeigt, wie:

+ Installieren [!DNL Java]
+ Installation [!DNL Node.js] (und npm)
+ Installieren [!DNL Maven]
+ Installieren [!DNL Git]

[Einrichten von Entwicklungstools für AEM Projekte](./development-tools.md)

## Lokale AEM Laufzeit

Das AEM als Cloud Service-SDK bietet eine [!DNL QuickStart Jar] , die eine lokale Version von AEM ausführt. Mit der [!DNL QuickStart Jar] kann entweder der AEM Author-Dienst oder der AEM Publish-Dienst lokal ausgeführt werden. Beachten Sie, dass die Funktion zwar eine lokale Entwicklungsumgebung [!DNL QuickStart Jar] bietet, jedoch nicht alle in AEM als Cloud Service verfügbaren Funktionen in der [!DNL QuickStart Jar]enthalten sind.

Dieser Abschnitt des Tutorials zeigt, wie:

+ Installieren [!DNL Java]
+ AEM SDK herunterladen
+ Führen Sie die [!DNL AEM Author Service]
+ Führen Sie die [!DNL AEM Publish Service]

[Richten Sie die lokale AEM ein](./aem-runtime.md)

## Lokale [!DNL Dispatcher] Laufzeit

AEM als Cloud Service SDKs Dispatcher Tools bieten alles, was zum Einrichten der lokalen [!DNL Dispatcher] Laufzeit erforderlich ist. [!DNL Dispatcher] Die Tools sind [!DNL Docker]auf Befehlszeilenwerkzeugen basieren, um [!DNL Apache HTTP] Webserver- und [!DNL Dispatcher] -Konfigurationsdateien in kompatible Formate zu übertragen und für die [!DNL Dispatcher] Ausführung im [!DNL Docker] Container bereitzustellen.

Dieser Abschnitt des Tutorials zeigt, wie:

+ AEM SDK herunterladen
+ Installieren [!DNL Dispatcher] -Tools
+ Lokale [!DNL Dispatcher] Laufzeit ausführen

[Einrichten der [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)
