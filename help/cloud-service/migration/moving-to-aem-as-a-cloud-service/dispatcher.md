---
title: Konfigurieren des Dispatchers beim Wechsel zu AEM as a Cloud Service
description: Erfahren Sie mehr über wichtige Änderungen an AEM Dispatcher für AEM as a Cloud Service, das Dispatcher-Konvertierungstool und die Verwendung des Dispatcher Tools SDK.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Erfahren Sie mehr über AEM Dispatcher für AEM as a Cloud Service, wobei Sie sich auf wichtige Änderungen vom Dispatcher für AEM 6, das Dispatcher-Konvertierungstool und die Verwendung des Dispatcher Tools SDK konzentrieren.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Verwenden Sie als Teil der Refaktorierung Ihrer Codebasis die [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) , um bestehende On-Premise- oder Adobe Managed Services-Dispatcher-Konfigurationen so umzustrukturieren, dass eine as a Cloud Service kompatible Dispatcher-Konfiguration AEM.

### Wichtigste Aktivitäten

* Verwenden Sie die [Adobe I/O Dispatcher Converter-Tool](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) , um eine vorhandene Dispatcher-Konfiguration zu migrieren.
* Referenzieren Sie das Dispatcher-Modul aus dem [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) als Best Practice.
* [Einrichten lokaler Dispatcher Tools](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) , um den Dispatcher zu validieren, bevor er in einer Cloud Service-Umgebung getestet wird.


