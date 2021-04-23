---
title: Erste Schritte mit AEM Sites - Projektarchiv
description: Erste Schritte mit AEM Sites - Projektarchiv. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager sind. Das Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke, die WKND. Das Lernprogramm behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Core-Komponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
index: y
feature: Hauptkomponenten, Seiten-Editor, bearbeitbare Vorlagen, AEM Projektarchiv
topic: Content-Management, Entwicklung
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 31%

---


# Erste Schritte mit AEM Sites - Projektarchiv {#project-archetype}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler entwickelt wurde, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke der WKND.

Dieses Lernprogramm wird mit dem AEM [Projektarchiv](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) durchgeführt, um ein neues Beginn zu erstellen.

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und ist rückwärtskompatibel mit **AEM 6.5.5.0+** und **AEM 6.4.8.1+**. Die Site wird wie folgt implementiert:

* [Maven-AEM-Projektarchetyp](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling-Modelle
* [Bearbeitbare Vorlagen](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stilsystem](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schätzen Sie sich 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Eine Umgebung zur lokalen Entwicklung ist erforderlich, um dieses Lernprogramm abzuschließen. Screenshots und Videos werden mit dem AEM als Cloud Service-SDK erfasst, das auf einer Mac OS-Umgebung mit [Visual Studio-Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

Folgendes sollte lokal installiert werden:

* Lokale AEM **Autoreninstanz** (Cloud Service SDK, 6.5.5+ oder 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/en/) (LTS - Langfristige Unterstützung)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio-](https://code.visualstudio.com/) Codeoder eine entsprechende IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Tool wird während des Lernprogramms verwendet

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung mit dem AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) einzurichten.
>
> **Neu zu AEM 6.5?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) einzurichten.

## GitHub {#github}

Der gesamte Code für das Projekt finden Sie auf Github im AEM Guide Repo:

**[GitHub: WKND-Sites-Projekt](https://github.com/adobe/aem-guides-wknd)**

Darüber hinaus hat jeder Teil des Tutorials seine eigene Filiale in GitHub. Ein Benutzer kann das Lernprogramm an einem beliebigen Punkt beginnen, indem er einfach die Verzweigung ausprobiert, die dem vorherigen Teil entspricht.

## Nächste Schritte {#next-steps}

Worauf wartest du? Beginn Sie das Lernprogramm, indem Sie zum Kapitel [Projektualisierung](project-setup.md) navigieren und lernen, wie ein neues Adobe Experience Manager-Projekt mit dem AEM-Projektarchiv erstellt wird.
