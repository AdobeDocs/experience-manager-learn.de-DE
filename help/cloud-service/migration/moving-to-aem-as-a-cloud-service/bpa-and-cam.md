---
title: Einrichten des BPA- und CAM-Projekts
description: Erfahren Sie, wie Best Practice Analyzer und Cloud Acceleration Manager eine benutzerdefinierte Anleitung für die Migration auf AEM as a Cloud Service bereitstellen.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 3%

---

# Best Practice Analyzer und Cloud Acceleration Manager

Erfahren Sie, wie Best Practice Analyzer (BPA) und Cloud Acceleration Manager (CAM) eine benutzerdefinierte Anleitung für die Migration auf AEM as a Cloud Service bieten. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Bereitschaft bewerten

![Hochrangige Darstellung von BPA und CAM](assets/bpa-cam-diagram.png)

Das BPA-Paket sollte auf einem Klon der Produktionsumgebung AEM 6.x installiert sein. Der BPA wird einen Bericht erstellen, der dann in die CAM hochgeladen werden kann, der eine Anleitung für die wichtigsten Aktivitäten bietet, die für die Umstellung auf AEM as a Cloud Service erforderlich sind.

### Wichtigste Aktivitäten

* Erstellen Sie einen Klon Ihrer 6.x-Produktionsumgebung. Wenn Sie Inhalte migrieren und Code umgestalten, ist ein Klon einer Produktionsumgebung nützlich, um verschiedene Tools und Änderungen zu testen.
* Laden Sie das neueste BPA-Tool aus dem [Software Distribution-Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) und installieren Sie sie in Ihrer AEM 6.x geklonten Umgebung.
* Verwenden Sie das BPA-Tool, um einen Bericht zu erstellen, der in den Cloud Acceleration Manager (CAM) hochgeladen werden kann. Der Zugriff auf die CAM erfolgt über [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* Verwenden Sie CAM, um zu erfahren, welche Aktualisierungen an der aktuellen Codebasis und Umgebung vorgenommen werden müssen, um zu AEM as a Cloud Service wechseln zu können.
