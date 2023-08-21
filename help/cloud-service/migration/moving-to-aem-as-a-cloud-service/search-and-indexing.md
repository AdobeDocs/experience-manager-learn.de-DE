---
title: Suche und Indizierung in AEM as a Cloud Service
description: Erfahren Sie mehr über AEM Suchindizes von as a Cloud Service, wie Sie AEM 6 Indexdefinitionen konvertieren und wie Sie Indizes bereitstellen.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 77b960315c07ba194642a412a0cc6049edcf7bd2
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Suchen und Indizieren

Erfahren Sie mehr über AEM Suchindizes von as a Cloud Service, wie Sie AEM 6 Indexdefinitionen konvertieren, um sie AEM as a Cloud Service kompatibel zu sein, und wie Sie Indizes für AEM as a Cloud Service bereitstellen.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Indexkonverter-Tool

![Indexkonverter-Tool](./assets/index-converter.png)

Verwenden Sie als Teil der Refaktorierung Ihrer Codebasis die Variable [Indexkonverter-Tool](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) , um benutzerdefinierte Oak-Indexdefinitionen in AEM as a Cloud Service kompatible Indexdefinitionen zu konvertieren.

Überprüfen Sie die [Indexdokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) für den vollständigen und aktuellen Satz von Index Converter-Funktionen.

## Wichtigste Aktivitäten

+ Verwenden Sie die [Adobe I/O Workflow-Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) Tool zum Migrieren von Asset-Verarbeitungs-Workflows zur Verwendung der Asset compute-Microservices.
+ Richten Sie eine [lokale Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) und stellen die angepassten Indizes bereit. Stellen Sie sicher, dass die aktualisierten Indizes auf dem neuesten Stand sind.
+ Stellen Sie die aktualisierte Codebasis in einer AEM as a Cloud Service Entwicklungsumgebung bereit und fahren Sie mit der Validierung fort.
+ Wenn Sie einen vordefinierten Index ändern **IMMER** Kopieren Sie die neueste Indexdefinition aus einer AEM as a Cloud Service Umgebung, die mit der neuesten Version ausgeführt wird. Ändern Sie die kopierte Indexdefinition entsprechend Ihren Anforderungen.

## Übungen

Wenden Sie Ihr Wissen an, indem Sie ausprobieren, was Sie mit dieser praktischen Übung gelernt haben.

Bevor Sie die praktische Übung testen, müssen Sie sich das Video und die folgenden Materialien angesehen und verstanden haben:

+ [Über AEM as a Cloud Service anders nachdenken](./introduction.md)
+ [Repository-Modernisierung](./repository-modernization.md)

Stellen Sie außerdem sicher, dass Sie die vorherige praktische Übung abgeschlossen haben:

+ [Übung mit dem Content Transfer Tool](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="GitHub-Repository für praktische Übungen" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on mit Indizes</div>
            <p style="margin:1rem 0">
                Erfahren Sie mehr über das Definieren und Bereitstellen von Oak-Indizes für AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testen der Indizierung</span>
            </a>
        </td>
    </tr>
</table>
