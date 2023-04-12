---
title: Grundlegende Next.js-App
description: Eine einfache Next.js-App, die eine Liste der WKND-Adventures und deren Details anzeigt
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 772acab316ba2ff463fa5cacff02013bea920579
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 95%

---


# Grundlegende Next.js-App

Die [Next.js](https://nextjs.org/)-App zeigt, wie Inhalte mit den AEM GraphQL-APIs mit persistierten Abfragen abgefragt werden können. Diese App ermöglicht die Filterung von WKND-Adventures und zeigt bei der Auswahl eines Adventures alle Details dieses Adventures an.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`

Eine genauere Übersicht über die Erstellung dieser Next.js-App erhalten Sie im Abschnitt [Beispiel-Dokumentation zur Next.js-App](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io unterstützt die Bearbeitung der Next.js-App in der eingebetteten IDE nicht. So bearbeiten Sie dieses Codebeispiel: [Öffnen Sie die Next.js-App direkt auf codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
