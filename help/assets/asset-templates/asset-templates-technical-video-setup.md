---
title: Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server
description: Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten ist mit Asset-Vorlagen bei der Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten ist mit Asset-Vorlagen bei der Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **must** mit einem laufenden InDesign-Server verbunden sein, wenn die INDD-Vorlage hochgeladen wird. Ein Teil der anfänglichen Verarbeitung auf der INDD-Datei erfordert einen InDesign-Server.

## InDesign Server-Testversion herunterladen {#download-indesign-server-trial}

Download [Website zum InDesign Server-Testdownload](https://www.adobeprerelease.com/)

## Starten von InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
