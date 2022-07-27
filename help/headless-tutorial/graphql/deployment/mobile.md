---
title: AEM Headless-Mobile-Implementierungen
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# AEM Headless-Mobile-Implementierungen

AEM Headless-Mobile-Implementierungen sind native mobile Apps für iOS, Android usw. die Inhalte in AEM Headless verbrauchen und damit interagieren.

Für mobile Bereitstellungen ist eine minimale Konfiguration erforderlich, da HTTP-Verbindungen zu AEM Headless-APIs nicht im Kontext eines Browsers initiiert werden.

## Bereitstellungskonfigurationen

| Mobile App stellt eine Verbindung zu | AEM Author | AEM Publish | AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./dispatcher-fitlers.md) | ✘ | ms | ms |
| [CORS-Konfiguration](./cors.md) | ✘ | ✘ | ✘ |
| Bild-URL-Hostname | ms | ms | ms |

## Beispiel für mobile Apps

Adobe bietet Beispiele für mobile iOS- und Android-Apps.


