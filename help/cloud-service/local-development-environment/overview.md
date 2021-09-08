---
title: Lokale Entwicklungsumgebung für AEM als Cloud Service
description: Übersicht über die lokale Entwicklungsumgebung in Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 2%

---


# Lokale Entwicklungsumgebung einrichten

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Übersicht"
>abstract="Das Einrichten einer lokalen Entwicklungsumgebung für AEM as a Cloud Service umfasst Entwicklungs-Tools, die zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich sind, sowie lokale Laufzeitumgebungen, mit denen Entwickler neue Funktionen schnell lokal validieren können, bevor sie über Adobe Cloud Manager in AEM als Cloud Service bereitgestellt werden."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=de" text="Entwicklungsrichtlinien"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Entwicklungsgrundlagen"

Dieses Tutorial führt Sie durch die Einrichtung einer lokalen Entwicklungsumgebung für Adobe Experience Manager (AEM), die das AEM as a Cloud Service SDK verwendet. Dazu gehören das Entwicklungs-Tool, das zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich ist, sowie die lokalen Laufzeitumgebungen, mit denen Entwickler neue Funktionen schnell lokal validieren können, bevor sie über Adobe Cloud Manager als Cloud Service in AEM bereitgestellt werden.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM als Cloud Service Lokale Entwicklungsumgebung - Technologiestapel](./assets/overview/aem-sdk-technology-stack.png)

Die lokale Entwicklungsumgebung für AEM kann in drei logische Gruppen unterteilt werden:

+ Das __AEM Projekt__ enthält den benutzerdefinierten Code, die Konfiguration und den Inhalt, der das benutzerdefinierte AEM ist.
+ Die __Local AEM Runtime__ , die eine lokale Version der AEM Author- und Publish-Dienste lokal ausführt.
+ Die __Local Dispatcher Runtime__ , die eine lokale Version von Apache HTTP Web Server und Dispatcher ausführt.

In diesem Tutorial wird erläutert, wie die im obigen Diagramm hervorgehobenen Elemente installiert und eingerichtet werden und wie eine stabile lokale Entwicklungsumgebung für AEM Entwicklung bereitgestellt wird.

## Dateisystemorganisation

In diesem Tutorial wurde der Speicherort des AEM als Cloud Service-SDK-Artefakte und AEM Projektcode wie folgt festgelegt:

+ `~/aem-sdk` ist ein Organisationsordner mit den verschiedenen Tools, die vom AEM als Cloud Service-SDK bereitgestellt werden.
+ `~/aem-sdk/author` enthält den AEM-Autorendienst
+ `~/aem-sdk/publish` enthält den AEM-Veröffentlichungsdienst
+ `~/aem-sdk/dispatcher` enthält die Dispatcher Tools
+ `~/code/<project name>` enthält den benutzerdefinierten AEM-Quellcode des Projekts

Beachten Sie, dass `~` in Kurzform für das Benutzerverzeichnis steht. In Windows entspricht dies `%HOMEPATH%`;

## Entwicklungstools für AEM Projekte

Das AEM-Projekt ist die benutzerdefinierte Codebasis, die den Code, die Konfiguration und den Inhalt enthält, der über Cloud Manager für AEM als Cloud Service bereitgestellt wird. Die Grundlinien-Projektstruktur wird über den AEM [Projektarchetyp](https://github.com/adobe/aem-project-archetype) erstellt.

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ Installieren [!DNL Java]
+ Installieren Sie [!DNL Node.js] (und npm)
+ Installieren [!DNL Maven]
+ Installieren [!DNL Git]

[Einrichten von Entwicklungstools für AEM Projekte](./development-tools.md)

## Lokale AEM Runtime

Das AEM as a Cloud Service SDK stellt eine [!DNL QuickStart Jar] bereit, die eine lokale Version von AEM ausführt. Der [!DNL QuickStart Jar] kann verwendet werden, um entweder den AEM-Autorendienst oder den AEM-Veröffentlichungsdienst lokal auszuführen. Beachten Sie, dass [!DNL QuickStart Jar] zwar ein lokales Entwicklungs-Erlebnis bietet, jedoch nicht alle in AEM als Cloud Service verfügbaren Funktionen in [!DNL QuickStart Jar] enthalten sind.

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ Installieren [!DNL Java]
+ AEM SDK herunterladen
+ Führen Sie [!DNL AEM Author Service] aus.
+ Führen Sie [!DNL AEM Publish Service] aus.

[Einrichten der lokalen AEM-Laufzeit](./aem-runtime.md)

## Lokale [!DNL Dispatcher] Laufzeit

AEM Dispatcher Tools des Cloud Service-SDK bieten alles, was zum Einrichten der lokalen [!DNL Dispatcher]-Laufzeitumgebung erforderlich ist. [!DNL Dispatcher] Tools sind  [!DNL Docker]basiert und bieten Befehlszeilenwerkzeuge, um  [!DNL Apache HTTP] Webserver- und  [!DNL Dispatcher] Konfigurationsdateien in kompatible Formate zu übertragen und sie für die  [!DNL Dispatcher] Ausführung im  [!DNL Docker] Container bereitzustellen.

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ AEM SDK herunterladen
+ Installieren Sie [!DNL Dispatcher] Tools
+ Führen Sie die lokale Laufzeit [!DNL Dispatcher] aus.

[Einrichten der Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
