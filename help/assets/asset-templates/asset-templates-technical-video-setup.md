---
title: Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server
description: Mit Asset-Vorlagen können Marketing-Fachleute digitale Assets für digitale sowie gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten mit Asset-Vorlagen ist bei Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '153'
ht-degree: 100%

---

# Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Mit Asset-Vorlagen können Marketing-Fachleute digitale Assets für digitale sowie gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten mit Asset-Vorlagen ist bei Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>AEM **muss** mit einem laufenden InDesign-Server verbunden sein, wenn die INDD-Vorlage hochgeladen wird. Ein Teil der anfänglichen Verarbeitung auf der INDD-Datei erfordert einen InDesign-Server.

## Herunterladen der InDesign Server-Testversion {#download-indesign-server-trial}

Herunterladen über die [Website für den InDesign Server-Testdownload](https://www.adobeprerelease.com/)

## Starten von InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
