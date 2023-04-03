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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 6%

---


# Dispatcher

Erfahren Sie mehr über AEM Dispatcher für AEM as a Cloud Service, wobei Sie sich auf wichtige Änderungen vom Dispatcher für AEM 6, das Dispatcher-Konvertierungstool und die Verwendung des Dispatcher Tools SDK konzentrieren.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Verwenden Sie als Teil der Refaktorierung Ihrer Codebasis die [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) , um bestehende On-Premise- oder Adobe Managed Services-Dispatcher-Konfigurationen so umzustrukturieren, dass eine as a Cloud Service kompatible Dispatcher-Konfiguration AEM.

## Wichtigste Aktivitäten

+ Verwenden Sie die [Adobe I/O Dispatcher Converter-Tool](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) , um eine vorhandene Dispatcher-Konfiguration zu migrieren.
+ Referenzieren Sie das Dispatcher-Modul aus dem [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) als Best Practice.
+ [Einrichten lokaler Dispatcher Tools](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=de) , um den Dispatcher zu validieren, bevor er in einer Cloud Service-Umgebung getestet wird.

## Übungen

Wenden Sie Ihr Wissen an, indem Sie ausprobieren, was Sie mit dieser praktischen Übung gelernt haben.

Bevor Sie die praktische Übung testen, müssen Sie sich das Video und die folgenden Materialien angesehen und verstanden haben:

+ [AEM-Modernisierungs-Tools](./aem-modernization-tools.md)
+ [Onboarding](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Stellen Sie außerdem sicher, dass Sie die vorherige praktische Übung abgeschlossen haben:

+ [Cloud Manager - praktische Übung](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="GitHub-Repository für praktische Übungen" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on mit den Dispatcher Tools</div>
            <p style="margin:1rem 0">
                Erfahren Sie, wie Sie mit den Dispatcher Tools des AEM SDK Dispatcher-Konfigurationen überprüfen und AEM Dispatcher lokal mit Docker ausführen können.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dispatcher Tools ausprobieren</span>
            </a>
        </td>
    </tr>
</table>
