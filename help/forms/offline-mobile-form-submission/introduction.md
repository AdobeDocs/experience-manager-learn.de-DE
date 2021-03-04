---
title: Arbeitsablauf für Trigger AEM HTML5-Formularübermittlung
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Herunterladen teilweise ausgefüllter Formulare für Mobilgeräte und Senden an AEM Arbeitsablauf

Ein gängiger Anwendungsfall besteht darin, die XDP für Datenerfassungs-Aktivitäten als HTML wiederzugeben. Dies funktioniert gut, wenn die Formulare einfach sind und online ausgefüllt und gesendet werden können. Ist das Formular jedoch komplex, können die Benutzer das Formular möglicherweise nicht online ausfüllen, müssen wir den Ausfüllern die Möglichkeit geben, eine interaktive Version des Formulars herunterzuladen, die offline mit Acrobat/Reader ausgefüllt werden kann. Sobald das Formular ausgefüllt ist, kann der Benutzer online sein, um das Formular zu senden.
Um diesen Verwendungsfall zu erreichen, müssen wir die folgenden Schritte ausführen:

* Möglichkeit, interaktive/ausfüllbare PDF-Dateien mit den Daten zu generieren, die in das mobile Formular eingegeben wurden
* Verarbeiten der PDF-Übermittlung von Acrobat/Reader
* Arbeitsablauf von Trigger Adobe Experience Manager (AEM) zum Überprüfen der gesendeten PDF

Dieses Lernprogramm führt Sie durch die Schritte, die zur Durchführung des oben genannten Anwendungsfalls erforderlich sind. Beispielcode und Assets zu diesem Tutorial sind [hier verfügbar.](part-four.md)

Das folgende Video gibt Ihnen einen Überblick über den Verwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

