---
title: Auslösen eines AEM-Workflows bei der Übermittlung eines PDF-Formulars
description: Fahren Sie mit dem Ausfüllen des Mobile-Formulars im Offline-Modus fort und übermitteln Sie das Mobile-Formular, um den AEM-Workflow auszulösen.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '214'
ht-degree: 100%

---

# Herunterladen eines teilweise ausgefüllten Formulars für Mobilgeräte und Übermittlung, um einen AEM-Workflow auszulösen

Ein gängiges Nutzungsszenario besteht darin, die XDP als HTML für Datenerfassungsaktivitäten wiederzugeben. Dies funktioniert gut, wenn die Formulare einfach sind und online ausgefüllt und gesendet werden können. Wenn das Formular jedoch komplex ist, können Benutzende das Formular möglicherweise nicht online ausfüllen. Dann müssen wir denen, die es ausfüllen wollen, die Möglichkeit bieten, eine interaktive Version des Formulars herunterzuladen, die offline mit Acrobat/Reader ausgefüllt werden kann. Sobald das Formular ausgefüllt ist, können die Benutzenden online gehen, um das Formular zu senden.
Um diesen Anwendungsfall zu erfüllen, müssen wir die folgenden Schritte ausführen:

* Möglichkeit, eine interaktive/ausfüllbare PDF mit den im Mobile-Formular eingegebenen Daten zu generieren
* Verarbeiten der PDF-Übermittlung von Acrobat/Reader
* Auslösen des Adobe Experience Manager(AEM)-Workflows zum Überprüfen der gesendeten PDF

In diesem Tutorial werden die Schritte erläutert, die zum Erreichen des oben genannten Anwendungsbeispiels erforderlich sind. Beispiel-Code und -Assets im Zusammenhang mit diesem Tutorial sind [hier verfügbar.](./deploy-assets.md)

Im folgenden Video erhalten Sie einen Überblick über den Anwendungsfall

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Nächste Schritte

[Erstellen eines benutzerdefinierten Profils](./custom-profile.md)