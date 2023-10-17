---
title: Senden adaptiver Formularanhänge
description: Senden adaptiver Formularanhänge mit der Komponente „E-Mail senden“
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: ht
source-wordcount: '137'
ht-degree: 100%

---

# Einführung



Ein häufiger Anwendungsfall ist das Senden der adaptiven Formularanhänge mithilfe der Komponente „E-Mail senden“ in einem AEM-Workflow.
Die Kundschaft komprimiert normalerweise die Formularanhänge oder sendet die Anhänge als einzelne Dateien, indem sie die Komponente „E-Mail senden“ verwendet.

## Senden der Formularanhänge in einer ZIP-Datei

Um den Anwendungsfall auszuführen, wurde ein benutzerdefinierter Workflow-Prozessschritt geschrieben. In diesem benutzerdefinierten Prozessschritt wird eine Zip-Datei mit den Formularanhängen erstellt und unter dem Payload-Ordner in einer Datei namens *zipped_attachments.zip* gespeichert.

![send-form-attachments](assets/send-form-attachments.JPG)

## Senden der Formularanhänge einzeln

Um dies zu erreichen, wurde ein benutzerdefinierter Workflow-Prozessschritt geschrieben. In diesem benutzerdefinierten Prozessschritt füllen wir Workflow-Variablen des Typs „ArrayList of Documents“ und „ArrayList of Strings“.

![send-list-of-documents](assets/send-list-of-documents.JPG)

## Nächste Schritte

[ZIP-Formularanhänge](./custom-process-step.md)
