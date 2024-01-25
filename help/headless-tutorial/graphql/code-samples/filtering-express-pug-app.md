---
title: Express-App zum Filtern
description: Eine einfache Express-App, die WKND-Adventures filtert, die mit Inhaltsfragmenten modelliert wurden.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 100%

---

# Express-App zum Filtern

Erkunden Sie die Fähigkeit von AEM Headless-GraphQL-APIs, Daten mithilfe einer [Express](https://expressjs.com/)- und [Pug](https://pugjs.org/)-App zu filtern. Diese Express-App erstellt eine Liste der WKND-Adventures, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht die Verwendung des Adobe [AEM Headless-Clients für NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) zum Aufrufen persistierter GraphQL-Abfragen mithilfe von Node.js-basiertem JavaScript. Diese App verwendet die persistierte Abfrage `wknd-shared/adventures-all` zur Erfassung aller Adventures und zur Ableitung einer Liste der verfügbaren Aktivitätstypen. Bei Auswahl eines Aktivitätstyps durch Benutzende wird der ausgewählte Typ an die persistierte Abfrage `wknd-shared/adventures-by-activity` übergeben und es werden nur die Adventure-Details für die Adventures des angegebenen Aktivitätstyps abgerufen. Adventure-Details werden von AEM über die persistierte Abfrage `wknd-shared/adventures-by-slug` abgerufen.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity` und `wknd-shared/adventures-by-slug`
