---
title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# Herunterladen teilweise ausgefüllter mobiler Formulare und Senden an AEM Workflow

Ein gängiges Nutzungsszenario besteht darin, die XDP für Datenerfassungsaktivitäten als HTML wiederzugeben. Dies funktioniert gut, wenn die Formulare einfach sind und online ausgefüllt und gesendet werden können. Wenn das Formular jedoch komplex ist, können Benutzer das Formular möglicherweise nicht online ausfüllen, müssen wir die Möglichkeit bieten, den Ausfüllern zu ermöglichen, die interaktive Version des Formulars herunterzuladen, die offline mit Acrobat/Reader ausgefüllt werden kann. Sobald das Formular ausgefüllt ist, kann der Benutzer online sein, um das Formular zu senden.
Um dieses Anwendungsbeispiel zu erstellen, müssen wir die folgenden Schritte ausführen:

* Möglichkeit, interaktive/ausfüllbare PDF mit den im Mobile-Formular eingegebenen Daten zu generieren
* Verarbeiten der PDF-Übermittlung von Acrobat/Reader
* Trigger Adobe Experience Manager (AEM)-Workflow zum Überprüfen der gesendeten PDF

In diesem Tutorial werden die Schritte erläutert, die zum Ausführen des oben genannten Anwendungsbeispiels erforderlich sind. Beispielcode und Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](part-four.md)

Im folgenden Video erhalten Sie einen Überblick über den Anwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

