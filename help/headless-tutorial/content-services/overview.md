---
title: Erste Schritte mit AEM Headless - Content Services
description: Ein Tutorial, in dem von Anfang bis Ende erläutert wird, wie Inhalte mithilfe von AEM Headless aufgebaut und bereitgestellt werden können.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 5aa32791-861a-48e3-913c-36028373b788
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 8%

---

# Erste Schritte mit AEM Headless - Content Services

AEM Content Services nutzt herkömmliche AEM Seiten, um Headless-REST-API-Endpunkte zu erstellen, und AEM Komponenten definieren oder referenzieren den Inhalt, der für diese Endpunkte verfügbar gemacht werden soll.

AEM Content Services ermöglicht die Verwendung der gleichen Inhaltsabstraktionen, die für die Erstellung von Web-Seiten in AEM Sites verwendet werden, um den Inhalt und die Schemas dieser HTTP-APIs zu definieren. Durch die Verwendung von AEM Seiten und AEM-Komponenten können Marketingexperten flexible JSON-APIs, die für jede Anwendung geeignet sind, schnell erstellen und aktualisieren.

## Content Services-Tutorial

Ein durchgängiges Tutorial, in dem erläutert wird, wie Inhalte mithilfe von AEM erstellt und bereitgestellt werden, die von einer nativen mobilen App in einem Headless-CMS-Szenario genutzt werden.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

In diesem Tutorial wird untersucht, wie AEM Content Services genutzt werden kann, um das Erlebnis einer Mobile App zu optimieren, die Ereignisinformationen (Musik, Leistung, Kunst usw.) anzeigt. wird vom WKND-Team kuratiert.

In diesem Tutorial werden die folgenden Themen behandelt:

* Erstellen von Inhalten, die ein Ereignis mit Inhaltsfragmenten darstellen
* Definieren Sie AEM Content Services-Endpunkte mithilfe der Vorlagen und Seiten von AEM Sites, die die Ereignisdaten als JSON verfügbar machen
* Erfahren Sie, wie AEM WCM-Kernkomponenten verwendet werden können, damit Marketing-Experten JSON-Endpunkte erstellen können.
* Content Services-JSON aus einer Mobile App AEM verwenden
   * Die Verwendung von Android liegt daran, dass es über einen plattformübergreifenden Emulator verfügt, mit dem alle Benutzer (Windows, macOS und Linux) dieses Tutorials die native App ausführen können.

## GitHub-Projekt

Der Quell-Code und die Inhaltspakete sind im [AEM Handbücher - WKND Mobile GitHub-Projekt](https://github.com/adobe/aem-guides-wknd-mobile).

Wenn Sie ein Problem mit dem Tutorial oder dem Code finden, hinterlassen Sie bitte eine [GitHub-Problem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL vs. AEM Content Services

|  | AEM GraphQL-APIs | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schemadefinition | Strukturierte Inhaltsfragmentmodelle | AEM-Komponenten |
| Inhalt | Inhaltsfragmente | AEM-Komponenten |
| Inhaltssuche | Nach GraphQL-Abfrage | Nach AEM Seite |
| Versandformat | GraphQL JSON | AEM ComponentExporter JSON |
