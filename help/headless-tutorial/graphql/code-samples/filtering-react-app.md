---
title: Filternde React-App
description: Eine einfache React-App, die WKND-Adventures filtert, die mit Inhaltsfragmenten modelliert wurden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---


# Filternde React-App

Erkunden Sie die Fähigkeit von AEM Headless-GraphQL-APIs, Daten mit einer [React](https://reactjs.org/)-App zu filtern. Diese React-App erstellt eine Liste von WKND-Adventures, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung des Adobe [AEM Headless-Clients für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md), um persistierte GraphQL-Abfragen aus React aufzurufen. Diese App verwendet die persistierte Abfrage von `wknd-shared/adventures-all`, um alle Adventures zu sammeln und daraus eine Liste der verfügbaren Aktivitätstypen abzuleiten. Bei Auswahl eines Aktivitätstyps durch Benutzende wird der ausgewählte Typ an die persistierte Abfrage `wknd-shared/adventures-by-activity` übergeben und es werden nur die Adventure-Details für die Adventures des angegebenen Aktivitätstyps abgerufen.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
