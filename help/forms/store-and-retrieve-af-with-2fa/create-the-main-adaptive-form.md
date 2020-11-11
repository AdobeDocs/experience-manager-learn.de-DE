---
title: Erstellen des wichtigsten adaptiven Formulars
description: Erstellen Sie die adaptiven Formulare, um die Informationen zum Antragsteller und das adaptive Formular zum Abrufen des gespeicherten adaptiven Formulars zu erfassen.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Erstellen des wichtigsten adaptiven Formulars

Das Formular **StoreAFWithAttachments** ist das wichtigste adaptive Formular. Dieses adaptive Formular ist der Einstiegspunkt zum Anwendungsfall. In diesem Formular werden Benutzerdetails einschließlich der Mobiltelefonnummer erfasst. Dieses Formular kann auch Anlagen hinzufügen. Wenn auf die Schaltfläche Speichern und Beenden geklickt wird, wird der serverseitige Code ausgeführt, um die Formulardaten in der Datenbank zu speichern. Es wird eine eindeutige Anwendungs-ID generiert und dem Benutzer zur sicheren Aufbewahrung angezeigt. Mit dieser Anwendungs-ID wird die mit der Anwendung verknüpfte Mobiltelefonnummer abgerufen.

![Hauptantragsformular](assets/6552.JPG)

Dieses Formular ist mit **bootboxjs540,storeAFWithAttachments** -Client-Bibliotheken verknüpft, die zuvor im Verlauf des Kurses erstellt wurden, und einem AEM Arbeitsablauf, der beim Senden des Formulars ausgelöst wird.


* Die Musterformulare basieren auf einer [benutzerdefinierten Vorlage](assets/custom-template-with-page-component.zip) für adaptive Formulare, die in AEM importiert werden muss, damit die Musterformulare korrekt wiedergegeben werden können.

* Das ausgefüllte [StoreAfWithAttachments-Formular](assets/store-af-with-attachments-form.zip) kann heruntergeladen und in Ihre AEM-Instanz importiert werden.

* Der mit diesem Formular [verknüpfte](assets/workflow-model-store-af-with-attachments.zip) AEM Workflow muss in Ihre AEM-Instanz importiert werden, damit das Formular funktioniert.



