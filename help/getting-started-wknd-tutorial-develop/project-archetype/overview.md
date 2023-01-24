---
title: Erste Schritte mit AEM Sites - Projektarchetyp
description: Erste Schritte mit AEM Sites - Projektarchetyp. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu bei Adobe Experience Manager sind. Das Tutorial führt durch die Implementierung einer AEM Site für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 32%

---

# Erste Schritte mit AEM Sites - Projektarchetyp {#project-archetype}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager (AEM) sind. Dieses mehrteilige Tutorial führt durch die Implementierung einer AEM-Site für eine fiktive Lifestyle-Marke namens WKND.

Dieses Tutorial beginnt mit der Verwendung des [AEM Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) , um ein neues Projekt zu erstellen.

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** und ist abwärtskompatibel mit **AEM 6.5.14+**. Die Site wird mit folgenden Elementen implementiert:

* [Maven-AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de)
* [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)
* [Bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=de)
* [Stilsystem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=de)

*Schätzen Sie 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Zum Abschluss dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das in einer macOS-Umgebung mit [Visual Studio-Code](https://code.visualstudio.com/) als IDE. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

Folgendes sollte lokal installiert werden:

* [Lokale AEM **Autor** instance](https://experience.adobe.com/#/downloads) (Cloud Service SDK oder 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/en/) (LTS - Langfristige Unterstützung)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio-Code](https://code.visualstudio.com/) oder einer entsprechenden IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - In der Anleitung verwendetes Tool

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich die [Befolgen Sie die Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de).
>
> **Neu bei AEM 6.5?** Sehen Sie sich die [Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de).

## GitHub {#github}

Der Code aus diesem Tutorial finden Sie auf GitHub im AEM Guide-Repository:

**[GitHub: WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd)**

Darüber hinaus hat jeder Teil des Tutorials eine eigene Verzweigung in GitHub. Ein Benutzer kann das Lernprogramm an einem beliebigen Punkt beginnen, indem er einfach die Verzweigung ausprobiert, die dem vorherigen Teil entspricht.

## Nächste Schritte {#next-steps}

Worauf wartest du? Starten Sie das Tutorial, indem Sie zur [Projekteinrichtung](project-setup.md) Kapitel und erfahren Sie, wie Sie ein neues Adobe Experience Manager-Projekt mithilfe des AEM Projektarchetyps erstellen.
