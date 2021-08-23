---
title: Erste Schritte mit AEM Sites - Projektarchetyp
description: Erste Schritte mit AEM Sites - Projektarchetyp. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu bei Adobe Experience Manager sind. Das Tutorial führt durch die Implementierung einer AEM Site für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Kernkomponenten, Seiten-Editor, bearbeitbare Vorlagen, AEM Projektarchetyp
topic: Content Management, Entwicklung
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 23%

---


# Erste Schritte mit AEM Sites - Projektarchetyp {#project-archetype}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial führt Sie durch die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke, die WKND.

Dieses Tutorial beginnt mit der Verwendung des [AEM Projektarchetyps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) zum Generieren eines neuen Projekts.

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und ist rückwärtskompatibel mit **AEM 6.5.5.0+** und **AEM 6.4.8.1+**. Die Site wird mit folgenden Elementen implementiert:

* [Maven-AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Sling-Modelle
* [Bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stilsystem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schätzen Sie 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Zum Abschluss dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das in einer Mac OS-Umgebung mit [Visual Studio Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

Folgendes sollte lokal installiert werden:

* Lokale AEM **Autoreninstanz** (Cloud Service SDK, 6.5.5+ oder 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/en/)  (LTS - Langfristige Unterstützung)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Codeor-Entsprechung IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Tool, das während des Tutorials verwendet wird

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) an.
>
> **Neu bei AEM 6.5?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de) an.

## GitHub {#github}

Den gesamten Code für das Projekt finden Sie auf Github im AEM Guide Repo:

**[GitHub: WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd)**

Darüber hinaus hat jeder Teil des Tutorials eine eigene Verzweigung in GitHub. Ein Benutzer kann das Lernprogramm an einem beliebigen Punkt beginnen, indem er einfach die Verzweigung ausprobiert, die dem vorherigen Teil entspricht.

## Nächste Schritte {#next-steps}

Worauf wartest du?! Starten Sie das Tutorial, indem Sie zum Kapitel [Projekteinrichtung](project-setup.md) navigieren und erfahren Sie, wie Sie ein neues Adobe Experience Manager-Projekt mithilfe des AEM Projektarchetyps erstellen.
