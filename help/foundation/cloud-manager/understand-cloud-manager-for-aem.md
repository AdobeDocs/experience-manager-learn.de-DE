---
title: Adobe Cloud Manager
description: Adobe Cloud Manager bietet eine einfache und dennoch robuste Lösung, die eine einfache Verwaltung, Selbstprüfung und Selbstbedienung von AEM Umgebung ermöglicht.
sub-product: cloud-manager, Stiftung
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 20%

---


# Adobe Cloud Manager

Adobe Cloud Manager bietet eine einfache und dennoch robuste Lösung, die eine einfache Verwaltung, Selbstprüfung und Selbstbedienung von AEM Umgebung ermöglicht.

## Übersicht über Cloud Manager

In dieser Videoserie werden die wichtigsten Funktionen von Cloud Manager AEM erläutert, darunter:

* [Programme](#programs)
* [Umgebungen](#environments)
* [Berichte](#reports)
* [CI/CD-Produktionsleitung](#cicd-production-pipeline)
* [CI/CD Nicht-Produktionsrohre](#cicd-non-production-pipeline)
* [Aktivität](#activity)

Eine vollständige Übersicht finden Sie im [Cloud Manager-Benutzerhandbuch](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programme {#programs}

[Cloud Manager-Programm](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) stellen AEM Umgebung dar, die logische Geschäftsinitiativen unterstützen, die in der Regel einem erworbenen Service Level Agreement (SLA) entsprechen. Beispielsweise kann ein Programm die AEM zur Unterstützung der globalen öffentlichen Websites darstellen, während ein anderes Programm einen internen DAM darstellt.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Umgebungen {#environments}

[Cloud Manager-Umgebung](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) bestehen aus AEM Author-, AEM Publish- und Dispatcher-Instanzen. Verschiedene Umgebung unterstützen Rollen und können mit verschiedenen CI/CD-Pipelines (siehe unten) eingestellt werden. Cloud Manager-Umgebung verfügen in der Regel über eine Produktions- und eine Stage-Umgebung.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Berichte {#reports}

[Cloud Manager-Berichte bieten einen Überblick über die Umgebungen und AEM-Instanzen des Programms in Form von Diagrammen, die eine Vielzahl von Metriken für jede AEM-Instanz melden und verfolgen.](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html)

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[Die Verwendung der CI/CD-Pipeline in der Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)-Videoreihe bietet einen tiefen Einstieg in die Produktionspipeline-Ausführung, einschließlich der Untersuchung fehlerhafter und erfolgreicher Bereitstellungen.*

>[!NOTE]
>
> Während dieser Videos wurden die Build-, Test- und Bereitstellungszeiten verkürzt, um die Zeit des Videos zu verkürzen. Die Ausführung einer vollständigen Pipeline dauert in der Regel 45 Minuten oder mehr (einschließlich des obligatorischen 30-minütigen Leistungstests), je nach Projektgröße, Anzahl AEM Instanzen und UAT-Prozessen.

### Konfiguration

The [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) configuration defines the trigger that will initiate the pipeline, parameters controlling the production deployment and performance test parameters.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Pipeline-Ausführung

Die [CI/CD-Produktionsleitung](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) wird verwendet, um Code über Stage zu erstellen und in der Produktions-Umgebung bereitzustellen, wodurch die Wertschöpfungszeit verkürzt wird.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD Nicht-Produktionsrohre {#cicd-non-production-pipeline}

[CI/CD-Nicht-Produktions-Pipelines sind in zwei Kategorien unterteilt: Codequalität-Pipelines und Bereitstellungs-Pipelines. ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) Codequalität-Pipelines leiten den gesamten Code aus einer Git-Verzweigung, der erstellt und anhand der Code-Qualitätsprüfung von Cloud Manager geprüft werden soll. Bereitstellungs-Pipelines unterstützen die automatisierte Bereitstellung von Code aus dem Git-Repository in einer Nicht-Produktions-Umgebung, d. h. jeder bereitgestellten AEM Umgebung, die nicht Stage oder Produktion ist.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Aktivität {#activity}

Cloud Manager bietet eine konsolidierte Ansicht in die Aktivität eines Programms, in der alle CI/CD-Pipeline-Ausführungen aufgelistet werden, sowohl Produktions- als auch Nicht-Produktionsausführung. So können Sie einen Einblick in die vergangene und aktuelle Aktivität erhalten und alle Details einer Aktivität überprüfen.

Cloud Manager kann auch auf Benutzerebene in [Adobe Experience Cloud-Benachrichtigungen](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)integriert werden, was eine allgegenwärtige Ansicht in Ereignis und Aktionen von Interesse bietet.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
