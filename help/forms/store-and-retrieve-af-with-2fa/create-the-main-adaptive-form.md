---
title: Erstellen des wichtigsten adaptiven Formulars
description: Erstellen Sie die adaptiven Formulare, um die Bewerberinformationen und das adaptive Formular zu erfassen und das gespeicherte adaptive Formular abzurufen
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Erstellen des wichtigsten adaptiven Formulars

Das Formular **StoreAFWithAttachments** ist das wichtigste adaptive Formular. Dieses adaptive Formular ist der Einstiegspunkt zum Anwendungsfall. In diesem Formular werden Benutzerdetails einschließlich der Mobiltelefonnummer erfasst. Dieses Formular kann auch einige Anhänge hinzufügen. Wenn auf die Schaltfläche Speichern und Beenden geklickt wird, wird der serverseitige Code ausgeführt, um die Formulardaten in der Datenbank zu speichern. Außerdem wird eine eindeutige Anwendungs-ID generiert und dem Benutzer zur sicheren Aufbewahrung angezeigt. Mit dieser Anwendungs-ID wird die mit der Anwendung verknüpfte Mobiltelefonnummer abgerufen.

![Hauptanwendungsformular](assets/6552.JPG)

Dieses Formular ist mit **bootboxjs540,storeAFWithAttachments** Client-Bibliotheken, die zuvor im Kurs erstellt wurden, und ein AEM Workflow, der beim Senden des Formulars ausgelöst wird.


* Die Musterformulare basieren auf [benutzerdefinierte adaptive Formularvorlage](assets/custom-template-with-page-component.zip) müssen in AEM importiert werden, damit die Musterformulare korrekt wiedergegeben werden.

* Die [StoreAfWithAttachments-Formular](assets/store-af-with-attachments-form.zip) können heruntergeladen und in Ihre AEM-Instanz importiert werden.

* Die [AEM Workflow für dieses Formular](assets/workflow-model-store-af-with-attachments.zip) in Ihre AEM-Instanz importiert werden, damit das Formular funktioniert.


## Nächste Schritte

[Erstellen des Formulars, das das gespeicherte Formular abruft](./retrieve-saved-form.md)