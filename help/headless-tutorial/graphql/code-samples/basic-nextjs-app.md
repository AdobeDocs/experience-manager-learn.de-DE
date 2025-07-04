---
title: Grundlegende Next.js-App
description: Eine einfache Next.js-App, die eine Liste der WKND-Adventures und deren Details anzeigt
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: ht
source-wordcount: '98'
ht-degree: 100%

---

# Grundlegende Next.js-App

Die [Next.js](https://nextjs.org/)-App zeigt, wie Inhalte mit den AEM GraphQL-APIs mit persistierten Abfragen abgefragt werden können. Diese App ermöglicht die Filterung von WKND-Adventures und zeigt bei der Auswahl eines Adventures alle Details dieses Adventures an.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> Codesandbox.io unterstützt die Bearbeitung der Next.js-App in der eingebetteten IDE nicht. So bearbeiten Sie dieses Codebeispiel: [Öffnen Sie die Next.js-App direkt auf codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
