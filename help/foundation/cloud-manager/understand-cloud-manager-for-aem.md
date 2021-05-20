---
title: Grundlegendes zu Adobe Cloud Manager
description: Adobe Cloud Manager bietet eine einfache, aber dennoch robuste Lösung, die eine einfache Verwaltung, Überprüfung und Self-Service von AEM Umgebungen ermöglicht.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architektur
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 22%

---


# Grundlegendes zu Adobe Cloud Manager

Adobe Cloud Manager bietet eine einfache, aber dennoch robuste Lösung, die eine einfache Verwaltung, Überprüfung und Self-Service von AEM Umgebungen ermöglicht.

## Übersicht über Cloud Manager

In dieser Videoreihe werden die wichtigsten Funktionen von Cloud Manager für AEM untersucht, darunter:

* [Programme](#programs)
* [Umgebungen](#environments)
* [Berichte](#reports)
* [CI/CD-Produktions-Pipeline](#cicd-production-pipeline)
* [CI/CD Nicht-Produktions-Pipelines](#cicd-non-production-pipeline)
* [Aktivität](#activity)

Eine vollständige Übersicht finden Sie im [Cloud Manager-Benutzerhandbuch](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programme {#programs}

[Cloud Manager-](https://docs.adobe.com/content/help/de-DE/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) Programme stellen AEM Umgebungen dar, die logische Gruppen von Geschäftsinitiativen unterstützen. Diese entsprechen normalerweise einem erworbenen Service Level Agreement (SLA). Beispielsweise kann ein Programm die AEM Ressourcen zur Unterstützung der globalen öffentlichen Websites darstellen, während ein anderes Programm einen internen zentralen DAM darstellt.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Umgebungen {#environments}

[Cloud Manager-](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) Umgebungen bestehen aus AEM-Autoren-, AEM-Veröffentlichungs- und Dispatcher-Instanzen. Verschiedene Umgebungen unterstützen Rollen und können mit verschiedenen CI/CD-Pipelines interagiert werden (siehe unten). Cloud Manager-Umgebungen verfügen in der Regel über eine Produktions- und eine Staging-Umgebung.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Berichte {#reports}

[Cloud Manager-Berichte bieten einen Überblick über die Umgebungen und AEM-Instanzen des Programms in Form von Diagrammen, die eine Vielzahl von Metriken für jede AEM-Instanz melden und verfolgen.](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html)

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD-Produktions-Pipeline {#cicd-production-pipeline}

*[Die Verwendung der CI/CD-Pipeline in der Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager-Videoreihe bietet einen tiefen Einblick in die Ausführung der Produktions-Pipeline, einschließlich der Untersuchung fehlgeschlagener und erfolgreicher Bereitstellungen.*

>[!NOTE]
>
> In diesen Videos wurden die Erstellungs-, Test- und Bereitstellungszeiten verkürzt, um die Zeit des Videos zu verkürzen. Eine vollständige Pipelineausführung dauert in der Regel 45 Minuten oder länger (einschließlich der obligatorischen 30-minütigen Leistungstests), je nach Projektgröße, Anzahl AEM Instanzen und UAT-Prozessen.

### Konfiguration

Die Konfiguration [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) definiert den Trigger, der die Pipeline initiiert, Parameter, die die Produktionsbereitstellung steuern, und Leistungstestparameter.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Pipeline-Ausführung

Die [CI/CD-Produktions-Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) wird verwendet, um Code über die Staging-Umgebung zu erstellen und bereitzustellen, wodurch die Zeit bis zum Wert reduziert wird.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD Nicht-Produktions-Pipelines {#cicd-non-production-pipeline}

[CI/CD-Nicht-Produktions-Pipelines sind in zwei Kategorien unterteilt: Codequalität-Pipelines und Bereitstellungs-Pipelines. ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) Codequalität-Pipelines leiten den gesamten Code aus einer Git-Verzweigung, der erstellt und anhand der Code-Qualitätsprüfung von Cloud Manager geprüft werden soll. Bereitstellungs-Pipelines unterstützen die automatisierte Bereitstellung von Code aus dem Git-Repository in einer Nicht-Produktionsumgebung, d. h. in jeder bereitgestellten AEM Umgebung, die nicht in der Staging- oder Produktionsumgebung enthalten ist.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Aktivität {#activity}

Cloud Manager bietet eine konsolidierte Ansicht der Programmaktivität, in der alle CI/CD-Pipeline-Ausführungen aufgelistet werden, sowohl Produktions- als auch Nicht-Produktions-Ausführungen. So können Sie Einblicke in die vergangene und aktuelle Aktivität erhalten. Außerdem können die Details jeder Aktivität überprüft werden.

Cloud Manager lässt sich auch auf Benutzerebene mit [Adobe Experience Cloud-Benachrichtigungen](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html) integrieren und bietet so eine allgegenwärtige Sicht auf Ereignisse und Aktionen von Interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
