---
title: Erstellen einer Abfrageschnittstelle
description: Ein mehrteiliges Tutorial, das Sie durch die Schritte führt, die beim Abfragen von im Azure-Portal gespeicherten Formularübermittlungen erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 100%

---

# Erstellen einer Abfrageschnittstelle

Es wurde eine einfache Abfrageschnittstelle entwickelt, über die „Admins“ Suchkriterien eingeben können, um bestimmte Formularübermittlungen abzurufen. Die Ergebnisse werden dann in einem einfachen Tabellenformat mit der Option angezeigt, eine bestimmte Formularübermittlung anzuzeigen.

![query-submissions](assets/query-submissions.png)

Die Dropdown-Listen in dieser Schnittstelle sind kaskadierende Dropdown-Listen. Die in der Dropdown-Liste verfügbaren Optionen ändern sich entsprechend der in der vorherigen Dropdown-Liste getroffenen Auswahl.

Die Dropdown-Listen werden mithilfe von RESTful-Datenquellen gefüllt.

Die Suchergebnisse werden in einer benutzerdefinierten Komponente namens „SearchResults“ angezeigt. Wenn die Benutzerin oder der Benutzer auf die Schaltfläche „Anzeigen“ klickt, wird das Formular mit den übermittelten Daten und Anhängen vorausgefüllt.

## Nächste Schritte

[Schreiben des Vorbefüllungsdienstes](./part4.md)
