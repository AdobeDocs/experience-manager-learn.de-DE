---
title: Smart-Tags für Bilder mit AEM Assets
description: Smart-Tags für Bilder erweitern AEM Suchfunktionen, indem Metadaten-Tags automatisch und intelligent basierend auf dem Bildinhalt zu Bild-Assets hinzugefügt werden.
topic: Content Management
feature: Smart Tags
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 645
thumbnail: 17019.jpg
last-substantial-update: 2022-06-09T00:00:00Z
exl-id: c72dc489-70e6-48ca-99a8-663d4c0652ba
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 11%

---

# Smart-Tags für Bilder

Die Smart-Tags für Bilder von AEM Assets erweitern die Suche von AEM Assets, indem sie automatisch abgeleitete Metadaten-Tags zu Bild-Assets hinzufügen. Dies verbessert das Authoring-Erlebnis, da es einfacher und schneller ist, das richtige Bild zu finden.

>[!VIDEO](https://video.tv.adobe.com/v/17019?quality=12&learn=on)

## Einrichten für AEM 6.x{#set-up}

>[!NOTE]
> Smart-Tags für Bilder werden automatisch für AEM as a Cloud Service bereitgestellt.

>[!VIDEO](https://video.tv.adobe.com/v/17023?quality=12&learn=on)

Bevor Sie den Smart Content Service verwenden können, stellen Sie Folgendes sicher, um eine Integration in Adobe I/O zu erstellen:

* Es ist ein Adobe ID-Konto mit Administratorrechten für die Organisation vorhanden
* Der Smart Content Service ist für Ihre Organisation aktiviert

Im Video werden die folgenden Aufgaben beschrieben, die zum Konfigurieren des Adobe I/O Smart Content Service erforderlich sind, der für Smart-Tag-Bilder verwendet wird.

* Erstellen Sie in AEM eine Konfiguration für den Smart Content Service , um einen öffentlichen Schlüssel zu generieren. Erhalten Sie ein öffentliches Zertifikat für die OAuth-Integration.
* Erstellen Sie eine Integration in Adobe I/O und laden Sie den generierten öffentlichen Schlüssel hoch.
* Konfigurieren Sie Ihre AEM-Instanz mithilfe des API-Schlüssels und anderer Anmeldedaten aus Adobe I/O.
* Aktivieren Sie optional das automatische Tagging beim Hochladen eines Assets.

## Zusätzliche Ressourcen

* [Dokumentation zu AEM Assets Smart Tags](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
