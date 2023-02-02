---
title: Filtern der Express-App
description: Eine einfache Express-App, die WKND-Abenteuer filtert, die mit Inhaltsfragmenten modelliert wurden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Filtern der Express-App

Erkunden Sie AEM Headless-GraphQL-APIs, mit denen Daten mithilfe eines [Express](https://expressjs.com/) und [Pug](https://pugjs.org/) App. Diese Express-App erstellt eine Liste der WKND-Abenteuer, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung von Adobe [AEM Headless-Client für NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) zum Aufrufen persistenter GraphQL-Abfragen mithilfe von Node.js-basiertem JavaScript. Diese App verwendet die `wknd-shared/adventures-all` persistente Abfrage zur Sammlung aller Abenteuer und Ableitung einer Liste der verfügbaren Aktivitätstypen. Wenn ein Benutzer einen Aktivitätstyp auswählt, wird der ausgewählte Typ an die `wknd-shared/adventures-by-activity` persistente Abfrage und ruft nur die Abenteuerdetails für die Abenteuer des angegebenen Aktivitätstyps ab. Abenteuerdetails werden von AEM über die `wknd-shared/adventures-by-slug` persistente Abfrage.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`und `wknd-shared/adventures-by-slug`
