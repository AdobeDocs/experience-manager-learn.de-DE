---
title: Lokale Entwicklungsumgebung für AEM as a Cloud Service
description: Übersicht über die lokale Entwicklungsumgebung in Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 869
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# Einrichten einer lokalen Entwicklungsumgebung {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Übersicht"
>abstract="Das Einrichten einer lokalen Entwicklungsumgebung für AEM as a Cloud Service umfasst Entwicklungs-Tools, die zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich sind, sowie lokale Laufzeiten, die Entwicklerinnen und Entwicklern ermöglichen, neue Funktionen schnell vor der Bereitstellung in AEM as a Cloud Service über Adobe Cloud Manager lokal zu validieren."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=de" text="Entwicklungsrichtlinien"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=de" text="Entwicklungsgrundlagen"

In diesem Tutorial wird erläutert, wie Sie mit dem AEM as a Cloud Service SDK eine lokale Entwicklungsumgebung für Adobe Experience Manager (AEM) einrichten. Dazu gehören die Entwicklungs-Tools, die zum Entwickeln, Erstellen und Kompilieren von AEM-Projekten erforderlich sind, sowie lokale Laufzeiten, die Entwicklerinnen und Entwicklern ermöglichen, neue Funktionen schnell vor der Bereitstellung in AEM as a Cloud Service über Adobe Cloud Manager lokal zu validieren.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service – Technologieplattform für lokale Entwicklungsumgebungen](./assets/overview/aem-sdk-technology-stack.png)

Die lokale Entwicklungsumgebung für AEM kann in drei logische Gruppen unterteilt werden:

+ Das __AEM-Projekt__ enthält den benutzerdefinierten Code, die Konfiguration und den Inhalt, also die benutzerdefinierte AEM-Anwendung.
+ Die __lokale AEM-Laufzeit__ führt eine lokale Version der AEM-Autoren- und -Veröffentlichungs-Services lokal aus.
+ Die __lokale Dispatcher-Laufzeit__ führt eine lokale Version von Apache HTTP Web Server und Dispatcher aus.

In diesem Tutorial wird erläutert, wie die in der obigen Abbildung hervorgehobenen Elemente installiert und eingerichtet werden und wie eine stabile lokale Entwicklungsumgebung zur AEM-Entwicklung bereitgestellt wird.

## Dateisystemorganisation

Für dieses Tutorial gelten die folgenden Speicherorte für AEM as a Cloud Service SDK-Artefakte und den AEM Projekt-Code:

+ `~/aem-sdk` ist ein organisationsbezogener Ordner mit den verschiedenen Tools, die vom AEM as a Cloud Service SDK bereitgestellt werden.
+ `~/aem-sdk/author` enthält den AEM-Autoren-Service
+ `~/aem-sdk/publish` enthält den AEM-Veröffentlichungs-Service
+ `~/aem-sdk/dispatcher` enthält die Dispatcher-Tools
+ `~/code/<project name>` enthält den benutzerdefinierten Quell-Code des AEM-Projekts

Hinweis: `~` steht für das Benutzerverzeichnis. Unter Windows entspricht dies `%HOMEPATH%`.

## Entwicklungs-Tools für AEM-Projekte

Das AEM-Projekt ist die benutzerdefinierte Code-Basis mit dem Code, der Konfiguration und dem Inhalt, der über Cloud Manager für AEM as a Cloud Service bereitgestellt wird. Die grundlegende Projektstruktur wird über den [AEM-Projekt-Maven-Archetyp](https://github.com/adobe/aem-project-archetype) generiert.

In diesem Abschnitt des Tutorials werden folgende Vorgänge beschrieben:

+ Installieren von [!DNL Java]
+ Installieren von [!DNL Node.js] (und npm)
+ Installieren von [!DNL Maven]
+ Installieren von [!DNL Git]

[Einrichten von Entwicklungs-Tools für AEM-Projekte](./development-tools.md)

## Lokale AEM-Runtime

Das AEM as a Cloud Service SDK bietet eine [!DNL QuickStart Jar]-Datei, die eine lokale Version von AEM ausführt. Die Datei [!DNL QuickStart Jar] kann verwendet werden, um entweder den AEM-Autoren-Service oder den AEM-Veröffentlichungs-Service lokal auszuführen. Beachten Sie, dass die Datei [!DNL QuickStart Jar] zwar ein lokales Entwicklungserlebnis bietet, aber nicht alle in AEM as a Cloud Service verfügbaren Funktionen in [!DNL QuickStart Jar] enthalten sind.

In diesem Abschnitt des Tutorials werden folgende Vorgänge beschrieben:

+ Installieren von [!DNL Java]
+ Herunterladen des AEM-SDKs
+ Ausführen des [!DNL AEM Author Service]
+ Ausführen des [!DNL AEM Publish Service]

[Einrichten der lokalen AEM-Laufzeit](./aem-runtime.md)

## Lokale [!DNL Dispatcher]-Laufzeit

Die Dispatcher-Tools des AEM Cloud Service SDK bieten alles, was zum Einrichten der lokalen [!DNL Dispatcher]-Laufzeit erforderlich ist. [!DNL Dispatcher]-Tools sind [!DNL Docker]-basiert und bieten Befehlszeilen-Tools, um [!DNL Apache HTTP]-Webserver- und [!DNL Dispatcher]-Konfigurationsdateien in kompatible Formate zu transpilieren und für den im [!DNL Docker]-Container ausgeführten [!DNL Dispatcher] bereitzustellen.

In diesem Abschnitt des Tutorials werden folgende Vorgänge beschrieben:

+ Herunterladen des AEM-SDKs
+ Installieren von [!DNL Dispatcher]-Tools
+ Ausführen der lokalen [!DNL Dispatcher]-Runtime

[Einrichten der lokalen [!DNL Dispatcher] -Runtime](./dispatcher-tools.md)
