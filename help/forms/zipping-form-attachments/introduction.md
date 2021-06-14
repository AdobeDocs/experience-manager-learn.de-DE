---
title: Senden adaptiver Formularanhänge
description: Senden adaptiver Formularanhänge mit der Komponente E-Mail senden
feature: adaptive Formulare
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 1%

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



