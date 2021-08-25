---
title: Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server
description: Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten ist mit Asset-Vorlagen bei der Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Einrichten von Asset-Vorlagen mit AEM Assets und InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Inhalte erstellen, verwalten und bereitstellen. Die Erstellung von Marketing-Broschüren, Visitenkarten, Flyern, Anzeigen und Postkarten ist mit Asset-Vorlagen bei der Integration mit dem InDesign-Server viel einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt beschrieben.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **muss** beim Hochladen der INDD-Vorlage mit einem laufenden InDesign-Server verbunden sein. Ein Teil der anfänglichen Verarbeitung auf der INDD-Datei erfordert einen InDesign-Server.

## InDesign Server-Testversion herunterladen {#download-indesign-server-trial}

Laden Sie [InDesign Server-Testdownload-Website](https://www.adobe.com/devnet/premiere/sdk/cs5/indesign-server-trial-downloads.html) herunter.

## Starten von InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
