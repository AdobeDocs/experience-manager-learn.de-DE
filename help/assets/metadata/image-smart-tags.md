---
title: Smart-Tags für Bilder mit AEM Assets
description: Smart-Tags für Bilder erweitern AEM Suchfunktionen, indem Metadaten-Tags automatisch und intelligent basierend auf dem Bildinhalt zu Bild-Assets hinzugefügt werden.
topic: Content Management
feature: Smart-Tags
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 645
thumbnail: 17019.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 45%

---


# Smart-Tags für Bilder

Die Smart-Tags für Bilder von AEM Assets erweitern die Suche von AEM Assets, indem sie automatisch abgeleitete Metadaten-Tags zu Bild-Assets hinzufügen. Dies verbessert das Authoring-Erlebnis, da es einfacher und schneller ist, das richtige Bild zu finden.

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## Einrichten für AEM 6.x{#set-up}

>[!NOTE]
> Smart-Tags für Bilder werden automatisch für AEM als Cloud Service bereitgestellt.

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

Stellen Sie vor der Verwendung des Smart Content Service Folgendes sicher, um eine Integration in Adobe I/O zu erstellen:

* Es ist ein Adobe ID-Konto mit Administratorrechten für die Organisation vorhanden
* Der Smart Content Service ist für Ihre Organisation aktiviert

Im Video werden die folgenden Aufgaben beschrieben, die zum Konfigurieren des Adobe I/O Smart Content Service erforderlich sind, der für Smart-Tag-Bilder verwendet wird.

* Erstellen Sie in AEM eine Konfiguration für den Smart Content Service, um einen öffentlichen Schlüssel zu erstellen. Erhalten Sie ein öffentliches Zertifikat für die OAuth-Integration.
* Erstellen Sie eine Integration in Adobe I/O und laden Sie den generierten öffentlichen Schlüssel hoch.
* Konfigurieren Sie die AEM-Instanz mithilfe des API-Schlüssels und anderen Anmeldeinformationen aus Adobe I/O.
* Aktivieren Sie optional das automatische Tagging beim Hochladen eines Assets.

## Zusätzliche Ressourcen

* [Dokumentation zu AEM Assets Smart Tags](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
