---
title: Senden adaptiver Formularanhänge
description: Senden adaptiver Formularanhänge mit der Komponente E-Mail senden
feature: Adaptive Formulare
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---


# Einführung



Häufige Anwendungsfälle sind das Senden der adaptiven Formularanhänge mithilfe der Komponente E-Mail senden in einem AEM Workflow.
Kunden komprimieren normalerweise die Formularanhänge oder senden die Anhänge als einzelne Dateien, indem sie die Komponente E-Mail senden verwenden.

## Senden der Formularanhänge in einer ZIP-Datei

Um den Anwendungsfall auszuführen, wurde ein benutzerdefinierter Workflow-Prozessschritt geschrieben. In diesem benutzerdefinierten Prozessschritt eine ZIP-Datei mit den Formularanlagen, die im Payload-Ordner erstellt und gespeichert sind, in einer Datei mit dem Namen *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## Senden der Formularanhänge einzeln

Um dies zu erreichen, wurde ein benutzerdefinierter Workflow-Prozessschritt geschrieben. In diesem benutzerdefinierten Prozessschritt füllen wir Workflow-Variablen des Typs ArrayList of Documents und ArrayList of Strings.

![send-list-of-documents](assets/send-list-of-documents.JPG)



