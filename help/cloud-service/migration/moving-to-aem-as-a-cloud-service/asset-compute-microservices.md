---
title: AEM Assets Microservices und Umstellung auf AEM as a Cloud Service
description: Erfahren Sie, wie Sie mit den asset compute-Microservices von AEM Assets as a Cloud Service automatisch und effizient jede Ausgabedarstellung für Ihre Assets generieren können. Dadurch wird diese Rolle des herkömmlichen AEM-Workflows ersetzt.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 5%

---

# AEM Assets Microservices - Umstellung auf AEM as a Cloud Service

Erfahren Sie, wie Sie mit den asset compute-Microservices von AEM Assets as a Cloud Service automatisch und effizient jede Ausgabedarstellung für Ihre Assets generieren können. Dadurch wird diese Rolle des herkömmlichen AEM-Workflows ersetzt.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Workflow-Migrations-Tool

![Asset-Workflow-Migrations-Tool](./assets/asset-workflow-migration.png)

Verwenden Sie als Teil der Refaktorierung Ihrer Codebasis die [Asset-Workflow-Migrations-Tool](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=de) , um bestehende Workflows zu migrieren und die Asset compute-Microservices in AEM as a Cloud Service zu verwenden.

## Wichtigste Aktivitäten

+ Verwenden Sie die [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) Tool zum Migrieren von Asset-Verarbeitungs-Workflows zur Verwendung der Asset compute-Microservices.
+ Richten Sie eine [lokale Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) und stellen Sie die aktualisierten Workflows bereit. Für komplexe Workflows kann eine manuelle Anpassung erforderlich sein.
+ Führen Sie die Iteration in einer lokalen Entwicklungsumgebung mit dem AEM SDK fort, bis der aktualisierte Workflow mit der Funktionsparität übereinstimmt.
+ Stellen Sie die aktualisierte Codebasis in einer AEM as a Cloud Service Entwicklungsumgebung bereit und fahren Sie mit der Validierung fort.

## Übungen

Wenden Sie Ihr Wissen an, indem Sie ausprobieren, was Sie mit dieser praktischen Übung gelernt haben.

Bevor Sie die praktische Übung testen, müssen Sie sich das Video und die folgenden Materialien angesehen und verstanden haben:

+ [Über AEM as a Cloud Service anders nachdenken](./introduction.md)
+ [Einstieg ](./onboarding.md)

Stellen Sie außerdem sicher, dass Sie die vorherige praktische Übung abgeschlossen haben:

+ [praktische Übung zum Suchen und Indizieren](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="GitHub-Repository für praktische Übungen" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Vorgehensweise beim Hochladen von Assets</div>
            <p style="margin:1rem 0">
                Erfahren Sie, wie Sie AEM Assets-Verarbeitungsprofile definieren und Ordnern zuweisen und Assets mit dem CLI-Modul "aem-upload"in AEM hochladen.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testen der Asset-Verwaltung</span>
            </a>
        </td>
    </tr>
</table>
