---
title: Preact-App filtern
description: Eine einfache Preact-App, die WKND-Abenteuer filtert, die mit Inhaltsfragmenten modelliert wurden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Preact-App filtern

Erkunden AEM Headless GraphQL-APIs, mit denen Daten mithilfe einer [Preact](https://preactjs.com/) App. Diese Preact-App erstellt eine Liste von WKND-Abenteuern, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung von Adobe [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) , um persistente GraphQL-Abfragen von React aufzurufen. Diese App verwendet die `wknd-shared/adventures-all` persistente Abfrage zur Sammlung aller Abenteuer und Ableitung einer Liste der verfügbaren Aktivitätstypen. Wenn ein Benutzer einen Aktivitätstyp auswählt, wird der ausgewählte Typ an die `wknd-shared/adventures-by-activity` persistente Abfrage und ruft nur die Abenteuerdetails für die Abenteuer des angegebenen Aktivitätstyps ab.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
