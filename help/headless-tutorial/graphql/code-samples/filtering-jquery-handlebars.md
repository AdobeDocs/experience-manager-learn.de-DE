---
title: Filtern mit jQuery und Handlebars
description: Eine JavaScript-Implementierung mit jQuery und Handlebars , die die Anzeige von WKND-Abenteuern filtert. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 2%

---


# Filtern mit jQuery und Handlebars

Erkunden Sie AEM Headless-GraphQL-APIs, um Daten mithilfe einer JavaScript-App zu filtern, die [jQuery](https://jquery.com/) und [Handlebars](https://handlebarsjs.com/). Diese App erstellt eine Liste der WKND-Abenteuer, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung von Adobe [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) zum Aufrufen persistenter GraphQL-Abfragen. Diese App verwendet die `wknd-shared/adventures-all` persistente Abfrage zur Sammlung aller Abenteuer und Ableitung einer Liste der verfügbaren Aktivitätstypen. Wenn ein Benutzer einen Aktivitätstyp auswählt, wird der ausgewählte Typ an die `wknd-shared/adventures-by-activity` persistente Abfrage und ruft nur die Abenteuerdetails für die Abenteuer des angegebenen Aktivitätstyps ab.

Dieser Code:

+ Ist mit einem AEM-Veröffentlichungsdienst verbunden und erfordert keine Authentifizierung
+ Verwendet die persistenten Abfragen des WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
