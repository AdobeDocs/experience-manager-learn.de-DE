---
title: Asset-Vorlagen mit AEM Assets und InDesign Server einrichten
description: Mit Asset-Vorlagen können Marketingexperten digitale Assets für digitale und gedruckte Dokumente erstellen, verwalten und bereitstellen. Die Erstellung von Marketingbroschüren, Visitenkarten, Flyer, Anzeigen und Postkarten ist bei der Integration mit dem InDesign-Server wesentlich einfacher. Die Konfiguration des InDesign-Servers mit AEM wird in diesem Abschnitt behandelt.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

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
