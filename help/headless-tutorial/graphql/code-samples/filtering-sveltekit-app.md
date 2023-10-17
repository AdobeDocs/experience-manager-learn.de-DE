---
title: Einfache SvelteKit-App
description: Eine einfache SvelteKit-App, die mithilfe von Inhaltsfragmenten erstellte WKND-Adventures darstellt.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '120'
ht-degree: 100%

---

# SvelteKit-App zum Filtern

Erkunden Sie die Möglichkeit von AEM Headless-GraphQL-APIs, mithilfe einer [SvelteKit](https://kit.svelte.dev/)-App Daten aufzulisten. Diese SvelteKit-App erstellt eine Liste von WKND-Adventures, die ausgewählt werden können, um die Details des Adventures anzuzeigen.

Dieser Code veranschaulicht die Verwendung von [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) von Adobe, um über SvelteKit persistierte GraphQL-Abfragen aufzurufen. Diese App verwendet die persistierte Abfrage von `wknd-shared/adventures-all`, um alle Adventures zu sammeln und daraus eine Liste der verfügbaren Aktivitätstypen abzuleiten. Informationen zu Adventures erhalten Sie über die persistierte Abfrage `wknd-shared/adventures-by-slug`.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`
