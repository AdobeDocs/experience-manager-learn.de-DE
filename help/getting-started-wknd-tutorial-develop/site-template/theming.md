---
title: Arbeitsablauf beim Erstellen von Themen | Schnelle Site-Erstellung mit AEM
description: Erfahren Sie, wie Sie die Themenquellen einer Adobe Experience Manager Site aktualisieren, um markenspezifische Stile anzuwenden. Erfahren Sie, wie Sie mit einem Proxy-Server eine Live-Vorschau von CSS- und JavaScript-Aktualisierungen anzeigen können. In diesem Tutorial wird auch beschrieben, wie Sie Aktualisierungen von Themen mithilfe der Frontend-Pipeline von Adobe Cloud Manager auf einer AEM-Site bereitstellen.
version: Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1298
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 100%

---

# Arbeitsablauf beim Erstellen von Themen {#theming}

In diesem Kapitel aktualisieren wir die Themenquellen einer Adobe Experience Manager-Site, um markenspezifische Stile anzuwenden. Sie erfahren, wie Sie einen Proxy-Server verwenden, um eine Vorschau von CSS- und JavaScript-Aktualisierungen anzuzeigen, während Sie Code für die Live-Site verwenden. In diesem Tutorial wird auch beschrieben, wie Sie Aktualisierungen von Themen mithilfe der Frontend-Pipeline von Adobe Cloud Manager auf einer AEM-Site bereitstellen.

Am Ende wird unsere Site aktualisiert und enthält nun Stile, die mit der WKND-Marke übereinstimmen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im Kapitel [Seitenvorlagen](./page-templates.md) beschriebenen Schritte abgeschlossen sind.

## Ziele

1. Erfahren Sie, wie die Themenquellen für eine Seite heruntergeladen und geändert werden können.
1. Erfahren Sie, wie Sie Code mit der Live-Site vergleichen, um eine Echtzeitvorschau zu erhalten.
1. Machen Sie sich mit dem durchgängigen Arbeitsablauf für die Bereitstellung kompilierter CSS- und JavaScript-Aktualisierungen als Teil eines Themas mithilfe der Frontend-Pipeline von Adobe Cloud Manager vertraut.

## Aktualisieren eines Themas {#theme-update}

Nehmen Sie als Nächstes Änderungen an den Themenquellen vor, damit die Seite dem Erscheinungsbild der WKND-Marke entspricht.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Erstellen Sie einen lokalen Benutzer in AEM zur Verwendung mit einem Proxy-Entwicklungs-Server.
1. Laden Sie die Themenquellen von AEM herunter und öffnen Sie sie mit einer lokalen IDE, wie VSCode.
1. Ändern Sie die Themenquellen und verwenden Sie einen Proxy-Dev-Server, um CSS- und JavaScript-Änderungen in Echtzeit in der Vorschau anzuzeigen.
1. Aktualisieren Sie die Themenquellen so, dass der Zeitschriftenartikel mit den Markenstilen von WKND und den Mockups übereinstimmt.

### Lösungsdateien

Herunterladen der fertigen Stile für das [WKND-Beispieldesign](assets/theming/WKND-THEME-src-1.1.zip)

## Bereitstellen eines Themas mithilfe einer Frontend-Pipeline {#deploy-theme}

Stellen Sie mithilfe der Frontend-Pipeline von Cloud Manager Updates für ein Thema in einer AEM-Umgebung bereit.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Allgemeine Schritte für das Video:

1. Erstellen Sie ein neues Git-[Repository in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html?lang=de)
1. Fügen Sie Ihr Themenquellen-Projekt zum Cloud Manager-Git-Repository hinzu:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Konfigurieren Sie eine [Frontend-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=de) in Cloud Manager, um den Frontend-Code bereitzustellen.
1. Führen Sie die Frontend-Pipeline aus, um Aktualisierungen für die AEM-Zielumgebung bereitzustellen.

### Beispiel-Repos

Es gibt einige Beispiele für GitHub-Repos, die als Referenz verwendet werden können:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e): Wird als Beispiel für „reale“ Projekte verwendet.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade ein Thema aktualisiert und für AEM bereitgestellt!

### Nächste Schritte {#next-steps}

Machen Sie sich noch besser mit der AEM-Entwicklung vertraut und verstehen Sie mehr über die zugrunde liegende Technologie, indem Sie mithilfe des [AEM-Projektarchetyps](../project-archetype/overview.md) eine Website erstellen.
