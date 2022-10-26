---
title: Designarbeitsablauf | AEM SchnellSite-Erstellung
description: Erfahren Sie, wie Sie die Themenquellen einer Adobe Experience Manager-Site aktualisieren, um markenspezifische Stile anzuwenden. Erfahren Sie, wie Sie mit einem Proxy-Server eine Live-Vorschau von CSS- und JavaScript-Aktualisierungen anzeigen können. In diesem Tutorial wird auch beschrieben, wie Sie Designaktualisierungen mithilfe der Frontend-Pipeline von Adobe Cloud Manager auf einer AEM Site bereitstellen.
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# Designarbeitsablauf {#theming}

In diesem Kapitel aktualisieren wir die Themenquellen einer Adobe Experience Manager-Site, um markenspezifische Stile anzuwenden. Wir erfahren, wie Sie einen Proxy-Server verwenden, um eine Vorschau von CSS- und JavaScript-Aktualisierungen anzuzeigen, während wir Code für die Live-Site verwenden. In diesem Tutorial wird auch beschrieben, wie Sie Designaktualisierungen mithilfe der Frontend-Pipeline von Adobe Cloud Manager auf einer AEM Site bereitstellen.

Am Ende wird unsere Site aktualisiert und enthält Stile, die mit der WKND-Marke übereinstimmen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Seitenvorlagen](./page-templates.md) -Kapitel abgeschlossen.

## Ziele

1. Erfahren Sie, wie die Themenquellen für eine Site heruntergeladen und geändert werden können.
1. Erfahren Sie, wie Sie Code mit der Live-Site vergleichen, um eine Echtzeitvorschau zu erhalten.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf für die Bereitstellung kompilierter CSS- und JavaScript-Aktualisierungen als Teil eines Designs mithilfe der Frontend-Pipeline von Adobe Cloud Manager vertraut.

## Aktualisieren eines Designs {#theme-update}

Nehmen Sie als Nächstes Änderungen an den Themenquellen vor, damit die Site dem Erscheinungsbild der WKND-Marke entspricht.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Erstellen Sie einen lokalen Benutzer in AEM zur Verwendung mit einem Proxy-Entwicklungsserver.
1. Laden Sie die Designquellen von AEM herunter und öffnen Sie sie mit einer lokalen IDE, wie VSCode.
1. Ändern Sie die Designquellen und verwenden Sie einen Proxy-Dev-Server, um CSS- und JavaScript-Änderungen in Echtzeit in der Vorschau anzuzeigen.
1. Aktualisieren Sie die Themenquellen so, dass der Zeitschriftenartikel mit den Markenstilen und -mockups der WKND übereinstimmt.

### Lösungsdateien

Herunterladen der fertigen Stile für die [WKND-Beispieldesign](assets/theming/WKND-THEME-src-1.1.zip)

## Bereitstellen eines Designs mithilfe einer Frontend-Pipeline {#deploy-theme}

Stellen Sie mithilfe der Frontend-Pipeline von Cloud Manager Updates für ein Design in einer AEM Umgebung bereit.

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Neues Git erstellen [Repository in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Fügen Sie Ihr Design-Quellen-Projekt zum Cloud Manager-Git-Repository hinzu:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Konfigurieren Sie eine [Front-End-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) in Cloud Manager , um den Frontend-Code bereitzustellen.
1. Führen Sie die Front-End-Pipeline aus, um Aktualisierungen für die Ziel-AEM-Umgebung bereitzustellen.

### Beispielberichte

Es gibt einige Beispiele für GitHub-Repos, die als Referenz verwendet werden können:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Wird als Beispiel für &quot;reale&quot; Projekte verwendet.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade ein Thema aktualisiert und AEM bereitgestellt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit der AEM-Entwicklung vertraut und verstehen Sie mehr über die zugrunde liegende Technologie, indem Sie mithilfe der [AEM Projektarchetyp](../project-archetype/overview.md).
