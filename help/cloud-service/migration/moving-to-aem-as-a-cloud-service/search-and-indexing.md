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
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 3%

---

# Suchen und Indizieren

Erfahren Sie mehr über AEM Suchindizes von as a Cloud Service, wie Sie AEM 6 Indexdefinitionen konvertieren, um sie AEM as a Cloud Service kompatibel zu sein, und wie Sie Indizes für AEM as a Cloud Service bereitstellen.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Indexkonverter-Tool

![Indexkonverter-Tool](./assets/index-converter.png)

Verwenden Sie als Teil der Refaktorierung Ihrer Codebasis die [Indexkonverter-Tool](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) , um benutzerdefinierte Oak-Indexdefinitionen in AEM as a Cloud Service kompatible Indexdefinitionen zu konvertieren.

### Wichtigste Aktivitäten

* Verwenden Sie die [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) Tool zum Migrieren von Asset-Verarbeitungs-Workflows zur Verwendung der Asset compute-Microservices.
* Richten Sie eine [lokale Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) und stellen die angepassten Indizes bereit. Stellen Sie sicher, dass die aktualisierten Indizes auf dem neuesten Stand sind.
* Stellen Sie die aktualisierte Codebasis in einer AEM as a Cloud Service Entwicklungsumgebung bereit und fahren Sie mit der Validierung fort.
* Wenn Sie einen vordefinierten Index ändern **IMMER** Kopieren Sie die neueste Indexdefinition aus einer AEM as a Cloud Service Umgebung, die mit der neuesten Version ausgeführt wird. Ändern Sie die kopierte Indexdefinition entsprechend Ihren Anforderungen.