---
title: Filtern mit jQuery und Handlebars
description: Eine JavaScript-Implementierung mit jQuery und Handlebars, die die Anzeige von WKND-Adventures filtert.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 43
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Filtern mit jQuery und Handlebars

Erkunden Sie die Fähigkeit von AEM Headless-GraphQL-APIs, Daten mithilfe einer JavaScript-App zu filtern, die [jQuery](https://jquery.com/) und [Handlebars](https://handlebarsjs.com/) nutzt. Diese App erstellt eine Liste der WKND-Adventures, die nach Aktivitätstyp gefiltert werden können.

Dieser Code veranschaulicht, wie mit dem [AEM Headless-Client für JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) von Adobe persistierte GraphQL-Abfragen aufgerufen werden können. Diese App verwendet die persistierte Abfrage `wknd-shared/adventures-all` zur Erfassung aller Adventures und zur Ableitung einer Liste der verfügbaren Aktivitätstypen. Bei Auswahl eines Aktivitätstyps durch Benutzende wird der ausgewählte Typ an die persistierte Abfrage `wknd-shared/adventures-by-activity` übergeben und es werden nur die Adventure-Details für die Adventures des angegebenen Aktivitätstyps abgerufen.

Dieser Code:

+ ist mit einem AEM-Veröffentlichungs-Service verbunden und erfordert keine Authentifizierung,
+ Verwendet die persistierten Abfragen von WKND: `wknd-shared/adventures-all` und `wknd-shared/adventures-by-activity`
