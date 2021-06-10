---
title: Senden adaptiver Formularanhänge
description: Komprimieren Sie adaptive Formularanhänge und senden Sie sie mit der Komponente E-Mail senden .
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
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 2%

---


# Einführung



Üblicher Anwendungsfall besteht darin, die adaptiven Formularanhänge zu komprimieren und mit der Komponente E-Mail senden in einem AEM-Workflow zu senden. Um den Anwendungsfall auszuführen, wurde ein benutzerdefinierter Workflow-Prozessschritt geschrieben. In diesem benutzerdefinierten Prozessschritt eine ZIP-Datei mit den Formularanlagen, die im Payload-Ordner erstellt und gespeichert sind, in einer Datei mit dem Namen *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)


