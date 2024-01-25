---
title: Senden des Headless-Formulars an einen benutzerdefinierten Sendedienst
description: Anpassen Ihrer Antwort auf Basis gesendeter Daten
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 29
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 100%

---

# Anpassen der Antwort auf Basis gesendeter Daten

Nachdem das Formular übermittelt wurde, ist es wichtig, den Benutzenden Feedback zum Ergebnis der Übermittlung zu geben. Die Übermittlungsantwort kann eine Transaktions-ID enthalten oder einfach eine personalisierte Antwort sein. Um diesem Anwendungsfall zu entsprechen, wird ein benutzerdefinierter Sendedienst in AEM Forms geschrieben und das Headless-Formular an diesen benutzerdefinierten Sendedienst gesendet.

## Voraussetzungen

Um diese Funktion erfolgreich zu implementieren, sollten Sie mit Folgendem vertraut sein:

* Erfahrung mit Git
* Erfahrung mit AEM Cloud Manager
* Maven (dieser Artikel wurde mit Version 3.8.6 getestet)
* Cloud-bereite, lokale Autoreninstanz für AEM Forms 
* Zugriff auf die AEM Forms as a Cloud Service-Umgebung
* IntelliJ oder eine andere IDE


## Nächste Schritte

[Schreiben des benutzerdefinierten Sendediensts](./custom-submit-service.md)
