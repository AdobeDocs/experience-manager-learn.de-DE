---
title: Einführung in das Auslösen des AEM-Workflows für die Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des Mobile-Formulars im Offline-Modus fort und übermitteln Sie das Mobile-Formular, um den AEM-Workflow auszulösen.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 345
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 100%

---

# Herunterladen teilweise ausgefüllter Mobile-Formulare und Senden an AEM-Workflow

Ein gängiges Nutzungsszenario besteht darin, die XDP als HTML für Datenerfassungsaktivitäten wiederzugeben. Dies funktioniert gut, wenn die Formulare einfach sind und online ausgefüllt und gesendet werden können. Wenn das Formular jedoch komplex ist, können Benutzende das Formular möglicherweise nicht online ausfüllen. Dann müssen wir denen, die es ausfüllen wollen, die Möglichkeit bieten, eine interaktive Version des Formulars herunterzuladen, die offline mit Acrobat/Reader ausgefüllt werden kann. Sobald das Formular ausgefüllt ist, können die Benutzenden online gehen, um das Formular zu senden.
Um diesen Anwendungsfall zu erfüllen, müssen wir die folgenden Schritte ausführen:

* Möglichkeit, eine interaktive/ausfüllbare PDF mit den im Mobile-Formular eingegebenen Daten zu generieren
* Verarbeiten der PDF-Übermittlung von Acrobat/Reader
* Auslösen des Adobe Experience Manager(AEM)-Workflows zum Überprüfen der gesendeten PDF

In diesem Tutorial werden die Schritte erläutert, die zum Erreichen des oben genannten Anwendungsbeispiels erforderlich sind. Beispiel-Code und -Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](part-four.md)

Im folgenden Video erhalten Sie einen Überblick über den Anwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
