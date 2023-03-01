---
title: Grundlegende Next.js-App
description: Eine einfache Next.js-App, die eine Liste der WKND-Abenteuer und deren Details anzeigt
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
ht-degree: 0%

---


# Grundlegende Next.js-App

Diese [Next.js](https://nextjs.org/) Das Programm zeigt, wie Sie mithilfe AEM GraphQL-APIs mithilfe persistenter Abfragen Inhalte abfragen können. Diese Anwendung ermöglicht die Filterung von WKND-Abenteuern und zeigt bei der Auswahl eines Abenteuers die Abenteuer mit allen Details an.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`

Eine genauere Übersicht über die Erstellung dieser Next.js-App erhalten Sie im Abschnitt [Beispieldokumentation zur App &quot;Next.js&quot;](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io unterstützt die Bearbeitung der Next.js-Anwendung in der eingebetteten IDE nicht. So bearbeiten Sie dieses Codebeispiel: [Öffnen Sie die App &quot;Next.js&quot;direkt auf &quot;codesandbox.io&quot;.](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
