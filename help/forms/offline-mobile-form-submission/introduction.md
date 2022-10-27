---
title: Einführung in den Trigger AEM Workflow für die Übermittlung von HTML5-Formularen
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Herunterladen teilweise ausgefüllter mobiler Formulare und Senden an AEM Workflow

Ein gängiges Nutzungsszenario besteht darin, die XDP als HTML für Datenerfassungsaktivitäten wiederzugeben. Dies funktioniert gut, wenn die Formulare einfach sind und online ausgefüllt und gesendet werden können. Wenn das Formular jedoch komplex ist, können Benutzer das Formular möglicherweise nicht online ausfüllen, müssen wir die Möglichkeit bieten, den Ausfüllern zu ermöglichen, die interaktive Version des Formulars herunterzuladen, die offline mit Acrobat/Reader ausgefüllt werden kann. Sobald das Formular ausgefüllt ist, kann der Benutzer online sein, um das Formular zu senden.
Um dieses Anwendungsbeispiel zu erstellen, müssen wir die folgenden Schritte ausführen:

* Möglichkeit, interaktive/ausfüllbare PDF mit den im Mobile-Formular eingegebenen Daten zu generieren
* Verarbeiten der PDF-Übermittlung von Acrobat/Reader
* Trigger Adobe Experience Manager (AEM)-Workflow zum Überprüfen der gesendeten PDF

In diesem Tutorial werden die Schritte erläutert, die zum Ausführen des oben genannten Anwendungsbeispiels erforderlich sind. Beispielcode und Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](part-four.md)

Im folgenden Video erhalten Sie einen Überblick über den Anwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
