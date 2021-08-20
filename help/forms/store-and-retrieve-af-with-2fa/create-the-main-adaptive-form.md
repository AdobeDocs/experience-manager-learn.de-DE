---
title: Erstellen des wichtigsten adaptiven Formulars
description: Erstellen Sie die adaptiven Formulare, um die Bewerberinformationen und das adaptive Formular zu erfassen und das gespeicherte adaptive Formular abzurufen
feature: Adaptive Formulare
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Entwicklung
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# Erstellen des wichtigsten adaptiven Formulars

Das Formular **StoreAFWithAttachments** ist das wichtigste adaptive Formular. Dieses adaptive Formular ist der Einstiegspunkt zum Anwendungsfall. In diesem Formular werden Benutzerdetails einschließlich der Mobiltelefonnummer erfasst. Dieses Formular kann auch einige Anhänge hinzufügen. Wenn auf die Schaltfläche Speichern und Beenden geklickt wird, wird der serverseitige Code ausgeführt, um die Formulardaten in der Datenbank zu speichern. Außerdem wird eine eindeutige Anwendungs-ID generiert und dem Benutzer zur sicheren Aufbewahrung angezeigt. Mit dieser Anwendungs-ID wird die mit der Anwendung verknüpfte Mobiltelefonnummer abgerufen.

![Hauptanwendungsformular](assets/6552.JPG)

Dieses Formular ist mit den zuvor im Kurs erstellten **bootboxjs540,storeAFWithAttachments** Client-Bibliotheken und einem AEM Workflow verknüpft, der beim Senden des Formulars ausgelöst wird.


* Die Beispielformulare basieren auf [benutzerdefinierten adaptiven Formularvorlagen](assets/custom-template-with-page-component.zip), die in AEM importiert werden müssen, damit die Beispielformulare korrekt wiedergegeben werden.

* Das ausgefüllte [StoreAfWithAttachments-Formular](assets/store-af-with-attachments-form.zip) kann heruntergeladen und in Ihre AEM-Instanz importiert werden.

* Der mit diesem Formular verknüpfte [AEM Workflow](assets/workflow-model-store-af-with-attachments.zip) muss in Ihre AEM-Instanz importiert werden, damit das Formular funktioniert.



