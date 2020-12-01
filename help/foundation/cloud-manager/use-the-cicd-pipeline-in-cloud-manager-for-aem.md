---
title: Verwenden der CI/CD-Pipeline in Adobe Cloud Manager
description: Adobe Cloud Manager bietet eine einfache, aber flexible Selbstbedienungs-CI/CD-Pipeline, mit der AEM Projektteams schnell, sicher und konsistent Code für alle in AMS gehosteten AEM Umgebung bereitstellen können. In dieser Videoreihe wird die Einrichtung und Ausführung der CI/CD-Pipeline von Cloud Manager sowohl in Fehlern- als auch in Erfolgsszenarien untersucht.
sub-product: cloud-manager, Stiftung
feature: pipelines, quality-gates
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---


# Verwenden der CI/CD-Pipeline in Adobe Cloud Manager

Adobe Cloud Manager bietet eine einfache, aber flexible Selbstbedienungs-CI/CD-Pipeline, mit der AEM Projektteams schnell, sicher und konsistent Code für alle in AMS gehosteten AEM Umgebung bereitstellen können. In dieser Videoreihe wird die Einrichtung und Ausführung der CI/CD-Pipeline von Cloud Manager sowohl in Fehlern- als auch in Erfolgsszenarien untersucht.

## Einführung

Eine Kurzzusammenstellung zu Cloud Manager- und Cloud Manager-Programmen.

>[!NOTE]
>
>Während dieser Videos wurden die Build-, Test- und Bereitstellungszeiten verkürzt, um die Zeit des Videos zu verkürzen. Die Ausführung einer vollständigen Pipeline dauert in der Regel 45 Minuten oder mehr (einschließlich des obligatorischen 30-minütigen Leistungstests), je nach Projektgröße, Anzahl AEM Instanzen und UAT-Prozessen.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## Einrichten der CI/CD-Pipeline

In diesem Video wird das Einrichten der Pipeline für das Programm in Cloud Manager untersucht.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## Fehlerhafte Pipeline-Ausführung

In diesem Video wird die Ausführung der CI/CD-Pipeline mithilfe von Code untersucht, der die erforderlichen Qualitätsprüfungen von Cloud Manager fehlschlägt, und zwar mithilfe der Repository-Verzweigung **[!DNL yellow]**.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## Erfolgreiche Pipeline-Ausführung

In diesem Video wird die erfolgreiche Ausführung der CI/CD-Pipeline mithilfe von Code untersucht, der die erforderlichen Qualitätsprüfungen von Cloud Manager mithilfe der Repository-Verzweigung **[!DNL master]** durchläuft.

Dieses Video berührt auch die Konsole [!UICONTROL Aktivität] in Cloud Manager, die den erneuten Zugriff auf aktive Ausführungen oder die Überprüfung abgeschlossener oder fehlgeschlagener Ausführung ermöglicht.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## Begleitmaterialien

* [Cloud Manager-Benutzerhandbuch](https://helpx.adobe.com/experience-manager/cloud-manager/user-guide.html)
* [Herunterladen von Code- [!DNL SonarQube] Scannerregeln](https://helpx.adobe.com/experience-manager/cloud-manager/using/understand-your-test-results.html#CodeQualityTesting)
   * *XLSX verfügbar am unteren Rand des verknüpften Abschnitts*
* [[!DNL SonarQube] Java-Regelindex](https://rules.sonarsource.com/java/)
