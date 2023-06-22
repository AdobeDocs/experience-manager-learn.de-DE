---
title: Senden des Headless-Formulars an einen benutzerdefinierten Submit-Dienst
description: Antwort auf Basis gesendeter Daten anpassen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 2%

---


# Antwort basierend auf gesendeten Daten anpassen

Nachdem das Formular übermittelt wurde, ist es wichtig, dem Benutzer Feedback zum Ergebnis der Übermittlung zu geben. Die Übermittlungsantwort kann eine Transaktions-ID oder einfach eine personalisierte Antwort enthalten. Um diesen Anwendungsfall zu vervollständigen, wird ein benutzerdefinierter Sendedienst in AEM Forms geschrieben und das Headless-Formular wird an diesen benutzerdefinierten Sendedienst gesendet.

## Voraussetzungen

Um diese Funktion erfolgreich zu implementieren, sollten Sie mit Folgendem vertraut sein

* Erlebnis mit Git
* Erlebnis mit AEM Cloud Manager
* Maven (dieser Artikel wurde mit 3.8.6 getestet)
* Lokale AEM Forms Cloud-bereite Autoreninstanz
* Zugriff auf die AEM Forms as Cloud Service-Umgebung
* IntelliJ oder eine andere IDE


## Nächste Schritte

[Schreiben Sie den benutzerdefinierten Sendedienst](./custom-submit-service.md)