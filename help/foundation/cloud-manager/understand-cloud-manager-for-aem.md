---
title: Überblick darüber, was Adobe Cloud Manager ist
description: Adobe Cloud Manager bietet eine einfache, aber robuste Lösung, die eine einfache Verwaltung, Einblicke und Selbstverwaltung von AEM-Umgebungen ermöglicht.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1026
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 100%

---

# Überblick darüber, was Adobe Cloud Manager ist

Adobe Cloud Manager bietet eine einfache, aber robuste Lösung, die eine einfache Verwaltung, Einblicke und Selbstverwaltung von AEM-Umgebungen ermöglicht.

## Übersicht über Cloud Manager

In dieser Videoreihe werden die wichtigsten Funktionen von Cloud Manager für AEM untersucht, darunter:

* [Programme](#programs)
* [Umgebungen](#environments)
* [Berichte](#reports)
* [CI/CD Produktions-Pipeline](#cicd-production-pipeline)
* [CI/CD produktionsfremde Pipelines](#cicd-non-production-pipeline)
* [Aktivität](#activity)

Eine vollständige Übersicht finden Sie im [Cloud Manager-Benutzerhandbuch](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=de).

## Programme {#programs}

[Cloud Manager-Programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=de) stellen eine Reihe von AEM-Umgebungen dar, die logische Gruppen von Geschäftsinitiativen unterstützen. Diese entsprechen in der Regel einem erworbenen Service Level Agreement (SLA). Beispielsweise kann ein Programm die AEM-Ressourcen zur Unterstützung der globalen öffentlichen Websites darstellen, während ein anderes Programm ein internes, zentrales DAM darstellt.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Umgebungen {#environments}

[Cloud Manager-Umgebungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=de) bestehen aus AEM Author-, AEM Publish- und Dispatcher-Instanzen. Verschiedene Umgebungen unterstützen Rollen und können mit verschiedenen CI/CD-Pipelines (nachfolgend beschrieben) interagieren. Cloud Manager-Umgebungen verfügen in der Regel über eine Produktions- und eine Staging-Umgebung.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Berichte {#reports}

[Cloud Manager-Berichte](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=de) bieten einen Einblick in die Programmumgebungen und AEM-Instanzen durch eine Reihe von Diagrammen, die über verschiedene Metriken für jede AEM-Instanz berichten und diese verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD Produktions-Pipeline {#cicd-production-pipeline}

*[Die Videoreihe zur Verwendung der CI/CD-Pipeline in Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) bietet einen tiefen Einblick in die Ausführung der Produktions-Pipeline, einschließlich der Untersuchung von fehlgeschlagenen und erfolgreichen Bereitstellungen.*

>[!NOTE]
>
> In diesen Videos werden die Build-, Test- und Bereitstellungszeiten verkürzt dargestellt, um die Videodauer zu verringern. Eine vollständige Pipeline-Ausführung dauert in der Regel mindestens 45 Minuten (einschließlich der obligatorischen 30-minütigen Leistungstests), je nach Projektgröße, Anzahl der AEM-Instanzen und UAT-Prozessen.

### Konfiguration

Die Konfiguration der [CI/CD-Produktions-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=de) definiert den Auslöser, der die Pipeline initiiert, sowie Parameter, die die Produktionsbereitstellung und die Leistungstestparameter steuern.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Pipeline-Ausführung

Die [CI/CD-Produktions-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=de) wird zum Erstellen und Bereitstellen von Code über die Staging-Umgebung in der Produktionsumgebung verwendet, wodurch die Zeit bis zum Wert reduziert wird.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD produktionsfremde Pipelines {#cicd-non-production-pipeline}

[CI/CD-Produktionsfremde Pipelines sind in zwei Kategorien unterteilt: Codequalitätspipelines und Bereitstellungs-Pipelines. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=de) Code-Qualitäts-Pipelines leiten den gesamten Code aus einer Git-Verzweigung, der erstellt und anhand der Code-Qualitätsprüfung von Cloud Manager geprüft werden soll. Bereitstellungs-Pipelines unterstützen die automatisierte Bereitstellung von Code aus dem Git-Repository in einer Nicht-Produktionsumgebung, d. h. in einer AEM-Umgebung, die weder Staging- noch Produktionsumgebung ist.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Aktivität {#activity}

Cloud Manager bietet eine Übersicht über die Aktivitäten eines Programms, in der alle CI/CD-Pipeline-Ausführungen aufgelistet werden (egal ob Produktion oder produktionsfremd), sodass Sie die vergangene und aktuelle Aktivität einsehen können. Zu jeder Aktivität lassen sich Details anzeigen.

Cloud Manager ist außerdem auf Benutzerebene mit [Adobe Experience Cloud-Benachrichtigungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=de) integriert und bietet einen allgegenwärtigen Überblick über Ereignisse und Aktionen von Interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
