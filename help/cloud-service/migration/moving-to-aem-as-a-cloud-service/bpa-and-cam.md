---
title: Einrichten des BPA- und CAM-Projekts
description: Erfahren Sie, wie Best Practices Analyzer und Cloud Acceleration Manager eine angepasste Anleitung für die Migration auf AEM as a Cloud Service bereitstellen.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---

# Best Practices für Analyzer und Cloud Acceleration Manager

Erfahren Sie, wie Best Practices Analyzer (BPA) und Cloud Acceleration Manager (CAM) eine benutzerdefinierte Anleitung für die Migration auf AEM as a Cloud Service bieten. 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## Verwendung von BPA und CAM

![Hochrangige Darstellung von BPA und CAM](assets/bpa-cam-diagram.png)

Das BPA-Paket sollte auf einem Klon der Produktionsumgebung AEM 6.x installiert sein. Der BPA wird einen Bericht erstellen, der dann in die CAM hochgeladen werden kann, der eine Anleitung für die wichtigsten Aktivitäten bietet, die für die Umstellung auf AEM as a Cloud Service erforderlich sind.

## Wichtigste Aktivitäten

+ Erstellen Sie einen Klon Ihrer 6.x-Produktionsumgebung. Wenn Sie Inhalte migrieren und Code umgestalten, ist es nützlich, einen Klon einer Produktionsumgebung zu haben, um verschiedene Tools und Änderungen zu testen.
+ Laden Sie das neueste BPA-Tool aus dem [Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) und installieren Sie sie in Ihrer AEM 6.x geklonten Umgebung.
+ Verwenden Sie das BPA-Tool, um einen Bericht zu erstellen, der in den Cloud Acceleration Manager (CAM) hochgeladen werden kann. Der Zugriff auf die CAM erfolgt über [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ Verwenden Sie CAM, um zu erfahren, welche Aktualisierungen an der aktuellen Codebasis und Umgebung vorgenommen werden müssen, um zu AEM as a Cloud Service wechseln zu können.

## Übungen

Wenden Sie Ihr Wissen an, indem Sie ausprobieren, was Sie mit dieser praktischen Übung gelernt haben.

Bevor Sie die praktische Übung testen, müssen Sie sich das Video und die folgenden Materialien angesehen und verstanden haben:

+ [Über AEM as a Cloud Service anders nachdenken](./introduction.md)
+ [Was ist AEM as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Architektur von AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Veränderlicher und unveränderlicher Inhalt](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Unterschiede bei der Entwicklung für AEM as a Cloud Service und AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="GitHub-Repository für praktische Übungen" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on mit Best Practices Analyzer</div>
            <p style="margin:1rem 0">
                Durchsuchen Sie den Best Practices Analyzer (BPA) und überprüfen Sie die Ergebnisse, indem Sie ihn für eine ältere WKND-Codebasis ausführen, die Beispielverletzungen enthält.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Best Practices Analyzer ausprobieren</span>
            </a>
        </td>
    </tr>
</table>


## Sonstige Ressourcen

+ [Herunterladen des Best Practices Analyzer](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)