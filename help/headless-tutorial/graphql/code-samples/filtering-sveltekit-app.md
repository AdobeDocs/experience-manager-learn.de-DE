---
title: Einfache SvelteKit-App
description: Eine einfache SvelteKit-App, die WKND-Abenteuer anzeigt, die mit Inhaltsfragmenten modelliert wurden.
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
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# Filtern der SvelteKit-App

Erkunden Sie AEM Headless-GraphQL-APIs, um Daten mithilfe eines [SvelteKit](https://kit.svelte.dev/) App. Diese SvelteKit-App erstellt eine Liste von WKND-Abenteuern, die ausgewählt werden können, um die Details des Abenteuers anzuzeigen.

Dieser Code veranschaulicht die Verwendung von Adobe [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) , um persistente GraphQL-Abfragen von SvelteKit aufzurufen. Diese App verwendet die `wknd-shared/adventures-all` persistente Abfrage zur Sammlung aller Abenteuer und Ableitung einer Liste der verfügbaren Aktivitätstypen. Informationen zu Abenteuern erhalten Sie über die `wknd-shared/adventures-by-slug` persistente Abfrage.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-slug`
