---
title: Erstellen der Abfrageschnittstelle
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die für die Abfrage von Formularübermittlungen im Azure Portal erforderlich sind
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# Erstellen der Abfrageschnittstelle

Es wurde eine einfache Abfrageschnittstelle entwickelt, über die der &quot;Administrator&quot;Suchkriterien eingeben kann, um bestimmte Formularübermittlungen abzurufen. Die Ergebnisse werden dann in einem einfachen Tabellenformat mit der Option angezeigt, eine bestimmte Formularübermittlung anzuzeigen.

![query-submit](assets/query-submissions.png)

Die Dropdown-Listen in dieser Benutzeroberfläche sind kaskadierende Dropdown-Listen. Die im Dropdown-Menü verfügbaren Optionen ändern sich entsprechend der im vorherigen Dropdown-Menü getroffenen Auswahl.

Die Dropdown-Listen werden mit RESTful-Datenquellen gefüllt.

Die Suchergebnisse werden in einer benutzerdefinierten Komponente namens &quot;SearchResults&quot;angezeigt. Wenn der Benutzer auf die Ansichtsschaltfläche klickt, wird das Formular mit den gesendeten Daten und Anlagen vorausgefüllt.

## Nächste Schritte

[Erstellen des Vorbefüllungs-Dienstes](./part4.md)
