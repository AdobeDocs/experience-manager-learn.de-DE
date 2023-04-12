---
title: Filtern mit jQuery und Handlebars
description: Eine JavaScript-Implementierung mit jQuery und Handlebars, die die Anzeige von WKND-Adventures filtert.
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
workflow-type: ht
source-wordcount: '146'
ht-degree: 100%

---


# Filtern mit jQuery und Handlebars

Erkunden Sie die Fähigkeit von AEM Headless-GraphQL-APIs, Daten mithilfe einer JavaScript-App zu filtern, die [jQuery](https://jquery.com/) und [Handlebars](https://handlebarsjs.com/) nutzt. Diese App erstellt eine Liste der WKND-Adventures, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht, wie mit dem [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) von Adobe persistierte GraphQL-Abfragen aufgerufen werden können. Diese App verwendet die persistierte Abfrage `wknd-shared/adventures-all` zur Erfassung aller Adventures und zur Ableitung einer Liste der verfügbaren Aktivitätstypen. Bei Auswahl eines Aktivitätstyps durch Benutzende wird der ausgewählte Typ an die persistierte Abfrage `wknd-shared/adventures-by-activity` übergeben und es werden nur die Adventure-Details für die Adventures des angegebenen Aktivitätstyps abgerufen.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`.
