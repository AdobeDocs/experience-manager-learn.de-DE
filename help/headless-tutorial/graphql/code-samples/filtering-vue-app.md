---
title: Filtern einer wertvollen App
description: Eine einfache Vue-App, die WKND-Abenteuer filtert, die mit Inhaltsfragmenten modelliert wurden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Filtern einer wertvollen App

Erkunden AEM Headless GraphQL-APIs, mit denen Daten mithilfe einer [Wert](https://vuejs.org/) App. Diese React-App erstellt eine Liste von WKND-Abenteuern, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung von Adobe [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) zum Aufrufen von persistenten GraphQL-Abfragen aus dem Wert. Diese App verwendet die `wknd-shared/adventures-all` persistente Abfrage zur Sammlung aller Abenteuer und Ableitung einer Liste der verfügbaren Aktivitätstypen. Wenn ein Benutzer einen Aktivitätstyp auswählt, wird der ausgewählte Typ an die `wknd-shared/adventures-by-activity` persistente Abfrage und ruft nur die Abenteuerdetails für die Abenteuer des angegebenen Aktivitätstyps ab.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
