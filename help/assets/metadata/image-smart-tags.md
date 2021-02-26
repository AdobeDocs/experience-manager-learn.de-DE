---
title: Smart-Tags für Bilder mit AEM Assets
description: Smart-Tags für Bilder erweitern AEM Suchfunktionen durch das automatische und intelligente Hinzufügen von Metadaten-Tags zu Bild-Assets, basierend auf dem Bildinhalt.
topic: Content Management
feature: Smart-Tags
role: Geschäftspraktiker
level: Zwischenschaltung
version: 6.3, 6.4, 6.5, cloud-service
kt: 645
thumbnail: 17019.jpg
translation-type: tm+mt
source-git-commit: a5fb96275194ddc46169832c0fb79a2587833564
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 44%

---


# Smart-Tags für Bilder

Die intelligenten Tags für Bilder in AEM Assets erweitern die Suche von AEM Assets, indem sie automatisch abgeleitete Metadaten-Tags zu Bild-Assets hinzufügen. Auf diese Weise wird die Authoring-Erfahrung verbessert, da das richtige Bild einfacher und schneller gefunden werden kann.

>[!VIDEO](https://video.tv.adobe.com/v/17019/?quality=12&learn=on)

## Einrichten für AEM 6.x{#set-up}

>[!NOTE]
> Intelligente Tags für Bilder werden automatisch als Cloud Service für AEM bereitgestellt.

>[!VIDEO](https://video.tv.adobe.com/v/17023/?quality=12&learn=on)

Stellen Sie vor der Verwendung des Smart Content Service Folgendes sicher, um eine Integration in Adobe I/O zu erstellen:

* Es ist ein Adobe ID-Konto mit Administratorrechten für die Organisation vorhanden
* Der Smart Content Service ist für Ihre Organisation aktiviert

Im Video werden die folgenden Aufgaben erläutert, die zum Konfigurieren des Adobe I/O Smart Content-Dienstes erforderlich sind, der für Smart-Tag-Bilder verwendet wird.

* Erstellen Sie in AEM eine Konfiguration für den Smart Content Service, um einen öffentlichen Schlüssel zu erstellen. Erhalten Sie ein öffentliches Zertifikat für die OAuth-Integration.
* Erstellen Sie eine Integration in Adobe I/O und laden Sie den generierten öffentlichen Schlüssel hoch.
* Konfigurieren Sie die AEM-Instanz mithilfe des API-Schlüssels und anderen Anmeldeinformationen aus Adobe I/O.
* Aktivieren Sie optional das automatische Tagging beim Hochladen eines Assets.

## Zusätzliche Ressourcen

* [Dokumentation zu AEM Assets Smart Tags](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/manage/smart-tags.html)
