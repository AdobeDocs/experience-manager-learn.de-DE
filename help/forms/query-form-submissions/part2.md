---
title: Abfrage zur Formularübermittlung
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
source-wordcount: '164'
ht-degree: 3%

---

# Erstellen einer benutzerdefinierten Übermittlung

Ein benutzerdefinierter Sende-Handler wurde geschrieben, um die Formularübermittlung zu handhaben. Auf hoher Ebene führt der benutzerdefinierte Submit-Handler Folgendes aus:

* Extrahiert den Namen des gesendeten Formulars.
* Extrahiert die gesendeten Daten. Die gesendeten Daten eines auf einer Kernkomponente basierenden Formulars liegen immer im JSON-Format vor.
* Extrahiert und speichert die Formularanhänge im Azure-Portal. Aktualisiert die gesendeten JSON-Daten mit der URL des Anhangs.
* Erstellt Blob-Index-Tags - Findet die Liste des durchsuchbaren Felds für das Formular und den zugehörigen Wert aus den gesendeten Daten.
* Verbindet die Blob-Index-Tags mit den gesendeten Daten und speichert sie in Azure Portal.

Der folgende Screenshot zeigt die Blob-Index-Tags in Azure Portal

![blob-index-tags](assets/blob-index-tags.png)

Der benutzerdefinierte Sende-Code befindet sich in **_StoreFormDataWithBlobIndexTagsInAzure_** und sich der Code zum Speichern und Abrufen von Daten aus Azure in der Komponente befindet **_SaveAndFetchFromAzure_**

## Nächste Schritte

[Erstellen der Abfrageschnittstelle](./part3.md)

