---
title: Preact-App zum Filtern
description: Eine einfache Preact-App, die WKND-Adventures filtert, die mit Inhaltsfragmenten modelliert wurden.
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
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---


# Preact-App zum Filtern

Erkunden Sie die Funktionen der AEM Headless-GraphQL-APIs, um Daten mithilfe einer [Preact](https://preactjs.com/)-App zu filtern. Diese Preact-App erstellt eine Liste von WKND-Adventure-n, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung des Adobe [AEM Headless-Clients für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md), um persistierte GraphQL-Abfragen aus React aufzurufen. Diese App verwendet die persistierte Abfrage von `wknd-shared/adventures-all`, um alle Adventures zu sammeln und daraus eine Liste der verfügbaren Aktivitätstypen abzuleiten. Bei Auswahl eines Aktivitätstyps durch Benutzende wird der ausgewählte Typ an die persistierte Abfrage `wknd-shared/adventures-by-activity` übergeben und es werden nur die Adventure-Details für die Adventures des angegebenen Aktivitätstyps abgerufen.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
