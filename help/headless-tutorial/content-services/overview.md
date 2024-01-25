---
title: Erste Schritte mit AEM Headless – Content Services
description: Ein Tutorial, in dem von Anfang bis Ende erläutert wird, wie Inhalte mithilfe von AEM Headless aufgebaut und bereitgestellt werden können.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 322
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# Erste Schritte mit AEM Headless – Content Services

AEM Content Services nutzt herkömmliche AEM-Seiten, um Headless-REST-API-Endpunkte zu erstellen. AEM-Komponenten definieren oder referenzieren den Inhalt, der für diese Endpunkte bereitgestellt werden soll.

AEM Content Services ermöglicht dieselbe Inhaltsabstraktionen wie bei der Erstellung von Web-Seiten in AEM Sites, um den Inhalt und die Schemata dieser HTTP-APIs zu definieren. Durch die Verwendung von AEM-Seiten und AEM-Komponenten können Marketer flexible JSON-APIs, die für jede Anwendung geeignet sind, schnell erstellen und aktualisieren.

## Content Services-Tutorial

In diesem Tutorial wird beschrieben, wie Inhalte mithilfe von AEM erstellt und bereitgestellt und von einer nativen App in einem Headless-CMS-Anwendungsszenario genutzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

In diesem Tutorial wird gezeigt, wie mithilfe von AEM Content Services die Nutzung einer Mobile App optimiert werden kann, über die Veranstaltungsinformationen (Musik, Performance, Kunst usw.) vom WKND-Team bereitgestellt werden.

In diesem Tutorial werden die folgenden Themen behandelt:

* Erstellen von Inhalten für eine Veranstaltung mithilfe von Inhaltsfragmenten
* Definieren von AEM Content Services-Endpunkten mithilfe der Vorlagen und Seiten von AEM Sites, die Ereignisdaten als JSON bereitstellen
* Verwendung von AEM-WCM-Kernkomponenten zur Erstellung von JSON-Endpunkten durch Marketer
* Verwendung von AEM Content Services-JSON über eine Mobile App
   * Android wird eingesetzt, da dieses Betriebssystem über einen plattformübergreifenden Emulator verfügt, den alle Benutzenden (Windows, macOS und Linux) dieses Tutorials verwenden können, um die native App auszuführen.

## GitHub-Projekt

Der Quell-Code und die Inhaltspakete sind in den [AEM-Handbüchern für das WKND-Mobile-GitHub-Projekt](https://github.com/adobe/aem-guides-wknd-mobile) verfügbar.

Wenn Sie ein Problem mit dem Tutorial oder Code haben, eröffnen Sie bitte einen [GitHub-Artikel](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL und AEM Content Services im Vergleich

|                                | AEM-GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM-Komponenten |
| Inhalt | Inhaltsfragmente | AEM-Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM-Seite |
| Bereitstellungsformat | GraphQL-JSON | AEM ComponentExporter-JSON |
