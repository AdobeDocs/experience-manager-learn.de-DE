---
title: Einfache React-App
description: Eine einfache React-App, die eine Liste der WKND-Adventures und deren Details anzeigt
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 17
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 100%

---

# Einfache React-App

Diese [React](https://reactjs.org/)-App zeigt, wie Sie mithilfe von AEM GraphQL-APIs unter Verwendung von persistierten Abfragen Inhalte abfragen können. Diese App ermöglicht die Filterung von WKND-Adventures und zeigt bei der Auswahl eines Adventures alle Details dieses Adventures an.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`

Eine genauere Übersicht über den Aufbau dieser Next.js-App finden Sie im Abschnitt [Dokumentation für eine Beispiel-React-App](../example-apps/react-app.md).
