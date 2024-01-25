---
title: Erste Schritte mit AEM Sites – Projektarchetyp
description: Erste Schritte mit AEM Sites – Projektarchetyp. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwicklerinnen und Entwickler konzipiert ist, für die Adobe Experience Manager neu ist. Das Tutorial führt durch die Implementierung einer AEM-Site für eine fiktive Lifestyle-Marke namens WKND. Das Tutorial behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Kernkomponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
version: 6.5, Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 84
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 100%

---

# Erste Schritte mit AEM Sites – Projektarchetyp {#project-archetype}

{{edge-delivery-services-and-page-editor}}

Willkommen bei einem mehrteiligen Tutorial, das für Entwicklerinnen und Entwickler konzipiert ist, für die Adobe Experience Manager (AEM) neu ist. Dieses mehrteilige Tutorial führt durch die Implementierung einer AEM-Site für eine fiktive Lifestyle-Marke namens WKND.

Dieses Tutorial beginnt mit der Verwendung des [AEM Projektarchetyps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de), um ein neues Projekt zu erstellen.

Das Tutorial wurde entworfen für die Arbeit mit **AEM as a Cloud Service** und ist abwärtskompatibel mit **AEM 6.5.14+**. Die Site wird implementiert unter Verwendung von:

* [Maven-AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de)
* [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=de)
* [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)
* [Bearbeitbare Vorlagen](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=de)
* [Stilsystem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=de)

*Rechnen Sie mit 1–2 Stunden für die Bearbeitung aller einzelnen Teile des Tutorials.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Für die Durchführung dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos wurden mit dem AEM as a Cloud Service SDK erfasst, betrieben in einer macOS-Umgebung mit [Visual Studio Code](https://code.visualstudio.com/) als IDE. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

Folgendes sollte lokal installiert werden:

* [Lokale AEM-**Autoreninstanz**](https://experience.adobe.com/#/downloads) (Cloud Service SDK oder 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/de/) (LTS – Langfristige Unterstützung)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual-Studio Code](https://code.visualstudio.com/) oder vergleichbare IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) – Im Tutorial verwendetes Tool

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich eine [detaillierte Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) an.
>
> **Neu bei AEM 6.5?** Sehen Sie sich die [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de) an.

## GitHub {#github}

Den Code aus diesem Tutorial finden Sie auf GitHub im AEM Guide-Repository:

**[GitHub: WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd)**

Darüber hinaus hat jedes Kapitel des Tutorial seine eigene Verzweigung in GitHub. Eine Benutzerin bzw. ein Benutzer kann das Lernprogramm an einem beliebigen Punkt beginnen, indem sie bzw. er einfach die Verzweigung auscheckt, die dem vorherigen Teil entspricht.

## Nächste Schritte {#next-steps}

Worauf warten Sie noch? Starten Sie das Tutorial, indem Sie zum Kapitel [Projekteinrichtung](project-setup.md) navigieren und lernen, wie Sie ein neues Adobe Experience Manager-Projekt mithilfe des AEM-Projektarchetyps erstellen.
