---
title: AEM Workflow beim Senden des Formulars auslösen
description: Konfigurieren Sie das adaptive Formular, um AEM Workflow auszulösen.
sub-product: Formulare
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: kt-5407.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 6%

---


# Konfigurieren des adaptiven Formulars zum Auslösen AEM Workflows

* Herunterladen [des adaptiven Formulars](assets/time-off-application.zip)
* Nach [Formular und Dokumenten suchen](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen -> Datei-Upload. Wählen Sie das in Schritt 1 heruntergeladene Formular aus
* Open [Adaptive Form in edit mode](http://localhost:4502/editor.html/content/forms/af/timeofapplication.html).
* Öffnen des Inhaltsexplorers
   ![Content Explorer](assets/af-workflow-submission.PNG)
* Wählen Sie den Knoten &quot;Form Container&quot;und öffnen Sie seine Konfigurationseigenschaften
   ![Übermittlung](assets/af-workflow-submission1.PNG)
* Bedienfeld &quot;Übermittlung&quot;erweitern
* Legen Sie die Übermittlungsaktion des Formulars wie im Screenshot oben angegeben fest.
   _Notieren Sie sich unbedingt den Wert, der im Feld Datendateipfad angegeben ist. Dieser Wert muss mit dem Wert übereinstimmen, den Sie im Abschnitt &quot;Vorauffüllen&quot;der Komponente &quot;Aufgabe zuweisen&quot;Ihres Workflows angeben._

Wenn Sie das adaptive Formular nun ausfüllen und senden, wird der mit der Übermittlungsaktion des Formulars verbundene Arbeitsablauf ausgelöst.
