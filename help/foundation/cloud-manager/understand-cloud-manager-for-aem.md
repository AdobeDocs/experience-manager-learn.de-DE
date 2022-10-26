---
title: Grundlegendes zu Adobe Cloud Manager
description: Adobe Cloud Manager bietet eine einfache, aber robuste Lösung, die eine einfache Verwaltung, Einführung und Self-Service von AEM Umgebungen ermöglicht.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# Grundlegendes zu Adobe Cloud Manager

Adobe Cloud Manager bietet eine einfache, aber robuste Lösung, die eine einfache Verwaltung, Einführung und Self-Service von AEM Umgebungen ermöglicht.

## Übersicht über Cloud Manager

In dieser Videoreihe werden die wichtigsten Funktionen von Cloud Manager für AEM untersucht, darunter:

* [Programme](#programs)
* [Umgebungen](#environments)
* [Berichte](#reports)
* [CI/CD-Produktions-Pipeline](#cicd-production-pipeline)
* [CI/CD Nicht-Produktions-Pipelines](#cicd-non-production-pipeline)
* [Aktivität](#activity)

Eine vollständige Übersicht finden Sie im [Cloud Manager-Benutzerhandbuch](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## Programme {#programs}

[Cloud Manager-Programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) stellt eine Reihe von AEM Umgebungen dar, die logische Gruppen von Geschäftsinitiativen unterstützen, die normalerweise einem erworbenen Service Level Agreement (SLA) entsprechen. Beispielsweise kann ein Programm die AEM-Ressourcen zur Unterstützung der globalen öffentlichen Websites darstellen, während ein anderes Programm ein internes zentrales DAM darstellt.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Umgebungen {#environments}

[Cloud Manager-Umgebungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) bestehen aus AEM-Autoren-, AEM-Veröffentlichungs- und Dispatcher-Instanzen. Verschiedene Umgebungen unterstützen Rollen und können mit verschiedenen CI/CD-Pipelines interagiert werden (siehe unten). Cloud Manager-Umgebungen verfügen in der Regel über eine Produktions- und eine Staging-Umgebung.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Berichte {#reports}

[Cloud Manager-Berichte](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) bieten einen Überblick über die Umgebungen und AEM Instanzen des Programms in Diagrammen, die verschiedene Metriken für jede AEM melden und verfolgen.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD-Produktions-Pipeline {#cicd-production-pipeline}

*[Verwenden der CI/CD-Pipeline in Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Videoreihen bieten einen tiefen Einblick in die Ausführung der Produktions-Pipeline, einschließlich der Untersuchung fehlgeschlagener und erfolgreicher Bereitstellungen.*

>[!NOTE]
>
> In diesen Videos wurden die Erstellungs-, Test- und Bereitstellungszeiten verkürzt, um die Zeit des Videos zu verkürzen. Eine vollständige Pipelineausführung dauert in der Regel 45 Minuten oder länger (einschließlich der obligatorischen 30-minütigen Leistungstests), je nach Projektgröße, Anzahl AEM Instanzen und UAT-Prozessen.

### Konfiguration

Die [CI/CD-Produktions-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) -Konfiguration definiert den Trigger, der die Pipeline initiiert, sowie die Parameter, die die Produktionsbereitstellung und Leistungstestparameter steuern.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Pipeline-Ausführung

Die [CI/CD-Produktions-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) wird zum Erstellen und Bereitstellen von Code über die Staging-Umgebung in der Produktionsumgebung verwendet, wodurch die Zeit bis zum Wert reduziert wird.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD Nicht-Produktions-Pipelines {#cicd-non-production-pipeline}

[CI/CD-Produktionsfremde Pipelines sind in zwei Kategorien unterteilt: Codequalitätspipelines und Bereitstellungs-Pipelines. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) Codequalitätspipelines leiten den gesamten Code aus einer Git-Verzweigung, der erstellt und anhand der Code-Qualitätsprüfung von Cloud Manager geprüft werden soll. Bereitstellungs-Pipelines unterstützen die automatisierte Bereitstellung von Code aus dem Git-Repository in einer Nicht-Produktionsumgebung, d. h. in jeder bereitgestellten AEM Umgebung, die nicht in der Staging- oder Produktionsumgebung enthalten ist.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Aktivität {#activity}

Cloud Manager bietet eine konsolidierte Ansicht der Programmaktivität, in der alle CI/CD-Pipeline-Ausführungen aufgelistet werden, sowohl Produktions- als auch Nicht-Produktions-Ausführungen. So können Sie Einblicke in die vergangene und aktuelle Aktivität erhalten. Außerdem können die Details jeder Aktivität überprüft werden.

Cloud Manager lässt sich auch auf Benutzerebene in [Adobe Experience Cloud-Benachrichtigungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), die einen allgegenwärtigen Überblick über Ereignisse und Aktionen von Interesse bietet.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
