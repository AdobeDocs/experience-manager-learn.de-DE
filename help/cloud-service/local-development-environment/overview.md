---
title: Lokale Entwicklungsumgebung für AEM as a Cloud Service
description: Übersicht über die lokale Entwicklungsumgebung in Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 6%

---

# Lokale Entwicklungsumgebung einrichten {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Übersicht"
>abstract="Das Einrichten einer lokalen Entwicklungsumgebung für AEM as a Cloud Service umfasst Entwicklungs-Tools, die zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich sind, sowie lokale Laufzeitumgebungen, mit denen Entwickler schnell neue Funktionen lokal validieren können, bevor sie über Adobe Cloud Manager für die Bereitstellung auf as a Cloud Service Geräten bereitgestellt AEM."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=de" text="Entwicklungsrichtlinien"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=de" text="Entwicklungsgrundlagen"

Dieses Tutorial führt Sie durch das Einrichten einer lokalen Entwicklungsumgebung für Adobe Experience Manager (AEM) mithilfe des AEM as a Cloud Service SDK. Dazu gehören das Entwicklungs-Tool, das zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich ist, sowie die lokalen Laufzeitumgebungen, mit denen Entwickler neue Funktionen schnell lokal validieren können, bevor sie über Adobe Cloud Manager auf AEM as a Cloud Service bereitgestellt werden.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM as a Cloud Service Technologiestapel für lokale Entwicklungsumgebungen](./assets/overview/aem-sdk-technology-stack.png)

Die lokale Entwicklungsumgebung für AEM kann in drei logische Gruppen unterteilt werden:

+ Die __AEM__ enthält den benutzerdefinierten Code, die Konfiguration und den Inhalt, der das benutzerdefinierte AEM-Programm ist.
+ Die __Lokale AEM Runtime__ , die eine lokale Version der AEM-Autoren- und Veröffentlichungsdienste lokal ausführt.
+ Die __Local Dispatcher Runtime__ , die eine lokale Version von Apache HTTP Web Server und Dispatcher ausführt.

In diesem Tutorial wird erläutert, wie die im obigen Diagramm hervorgehobenen Elemente installiert und eingerichtet werden und wie eine stabile lokale Entwicklungsumgebung für AEM Entwicklung bereitgestellt wird.

## Dateisystemorganisation

In diesem Tutorial wurde der Speicherort der AEM as a Cloud Service SDK-Artefakte und AEM Projektcode wie folgt festgelegt:

+ `~/aem-sdk` ist ein Organisationsordner mit den verschiedenen Tools, die vom AEM as a Cloud Service SDK bereitgestellt werden.
+ `~/aem-sdk/author` enthält den AEM-Autorendienst
+ `~/aem-sdk/publish` enthält den AEM-Veröffentlichungsdienst
+ `~/aem-sdk/dispatcher` enthält die Dispatcher Tools
+ `~/code/<project name>` enthält den benutzerdefinierten AEM-Quellcode des Projekts

Beachten Sie Folgendes: `~` steht für das Benutzerverzeichnis. Unter Windows entspricht dies dem `%HOMEPATH%`;

## Entwicklungstools für AEM Projekte

Das AEM-Projekt ist die benutzerdefinierte Codebasis, die den Code, die Konfiguration und den Inhalt enthält, der über Cloud Manager für AEM as a Cloud Service Bereitstellung bereitgestellt wird. Die Grundlinien-Projektstruktur wird über die [AEM Projektarchetyp Maven](https://github.com/adobe/aem-project-archetype).

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ Installieren des [!DNL Java]
+ Installieren [!DNL Node.js] (und npm)
+ Installieren des [!DNL Maven]
+ Installieren des [!DNL Git]

[Einrichten von Entwicklungstools für AEM Projekte](./development-tools.md)

## Lokale AEM Runtime

Das AEM as a Cloud Service SDK bietet eine [!DNL QuickStart Jar] , die eine lokale Version von AEM ausführt. Die [!DNL QuickStart Jar] kann verwendet werden, um entweder den AEM-Autorendienst oder den AEM-Veröffentlichungsdienst lokal auszuführen. Beachten Sie, dass während der [!DNL QuickStart Jar] bietet ein lokales Entwicklungs-Erlebnis, nicht alle in AEM as a Cloud Service verfügbaren Funktionen sind in der [!DNL QuickStart Jar].

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ Installieren des [!DNL Java]
+ AEM SDK herunterladen
+ Führen Sie die [!DNL AEM Author Service]
+ Führen Sie die [!DNL AEM Publish Service]

[Einrichten der lokalen AEM-Laufzeit](./aem-runtime.md)

## Lokal [!DNL Dispatcher] Laufzeit

AEM Dispatcher Tools des as a Cloud Service SDK bietet alles, was zum Einrichten der lokalen [!DNL Dispatcher] Laufzeit. [!DNL Dispatcher] Instrumente sind [!DNL Docker]-basiert und bietet Befehlszeilenwerkzeuge zum Übersetzen [!DNL Apache HTTP] Webserver und [!DNL Dispatcher] Konfigurationsdateien in kompatiblen Formaten und stellen Sie sie für [!DNL Dispatcher] im [!DNL Docker] Container.

In diesem Abschnitt des Tutorials erfahren Sie, wie Sie:

+ AEM SDK herunterladen
+ Installieren [!DNL Dispatcher] Instrumente
+ Lokale Ausführung [!DNL Dispatcher] runtime

[Lokale Einrichtung [!DNL Dispatcher] Laufzeit](./dispatcher-tools.md)
