---
title: Abfragen einer Formularübermittlung
description: Ein mehrteiliges Tutorial, das Sie durch die Schritte führt, die beim Abfragen von im Azure-Portal gespeicherten Formularübermittlungen erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '164'
ht-degree: 100%

---

# Erstellen einer benutzerdefinierten Übermittlung

Es wurde ein benutzerdefinierter Übermittlungs-Handler geschrieben, um die Formularübermittlung zu verarbeiten.  Der benutzerdefinierte Übermittlungs-Handler führt im Wesentlichen Folgendes aus:

* Extrahiert den Namen des gesendeten Formulars.
* Extrahiert die gesendeten Daten. Die gesendeten Daten eines auf einer Kernkomponente basierenden Formulars liegen immer im JSON-Format vor.
* Extrahiert und speichert die Formularanhänge im Azure-Portal. Aktualisiert die gesendeten JSON-Daten mit der URL des Anhangs.
* Erstellt Blob-Index-Tags – Sucht nach der Liste des durchsuchbaren Felds für das Formular und dem zugehörigen Wert aus den gesendeten Daten.
* Verknüpft die Blob-Index-Tags mit den gesendeten Daten und speichert sie im Azure-Portal.

Der folgende Screenshot zeigt die Blob-Index-Tags im Azure-Portal

![blob-index-tags](assets/blob-index-tags.png)

Der benutzerdefinierte Übermittlungs-Code befindet sich in **_StoreFormDataWithBlobIndexTagsInAzure_** und der Code zum Speichern und Abrufen von Daten aus Azure befindet sich in der Komponente **_SaveAndFetchFromAzure_**

## Nächste Schritte

[Erstellen einer Abfrageschnittstelle](./part3.md)
