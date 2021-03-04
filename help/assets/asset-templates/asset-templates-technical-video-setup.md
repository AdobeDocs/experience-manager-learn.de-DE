---
title: Asset-Vorlagen mit AEM Assets und InDesign Server einrichten
description: Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Dokumente erstellen, verwalten und bereitstellen. Die Erstellung von Marketingbroschüren, Visitenkarten, Flyer, Anzeigen und Postkarten ist bei der Integration mit dem InDesign-Server wesentlich einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt behandelt.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---


# Asset-Vorlagen mit AEM Assets und InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server} einrichten

Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Dokumente erstellen, verwalten und bereitstellen. Die Erstellung von Marketingbroschüren, Visitenkarten, Flyer, Anzeigen und Postkarten ist bei der Integration mit dem InDesign-Server wesentlich einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt behandelt.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **muss** beim Hochladen der INDD-Vorlage mit einem laufenden InDesign-Server verbunden sein. Ein Teil der anfänglichen Verarbeitung in der INDD-Datei erfordert einen InDesign-Server.

## InDesign Server-Testversion herunterladen {#download-indesign-server-trial}

[Website zum Herunterladen der InDesign Server-Testversion](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html) herunterladen

## Starten von InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
