---
title: Erstellen des adaptiven Hauptformulars
description: Erstellen Sie die adaptiven Formulare, um die Informationen der Menschen, die sich bewerben, und das adaptive Formular zu erfassen und das gespeicherte adaptive Formular abzurufen
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
workflow-type: ht
source-wordcount: '209'
ht-degree: 100%

---

# Erstellen des adaptiven Hauptformulars

Das Formular **StoreAFWithAttachments** ist das wichtigste adaptive Formular. Dieses adaptive Formular ist der Einstiegspunkt zum Anwendungsfall. In diesem Formular werden Benutzerdetails einschließlich der Mobiltelefonnummer erfasst. Mit diesem Formular können auch einige Anhänge hinzugefügt werden. Wenn auf die Schaltfläche „Speichern und Beenden“ geklickt wird, wird der Server-seitige Code ausgeführt, um die Formulardaten in der Datenbank zu speichern. Außerdem wird eine eindeutige Anwendungs-ID generiert und den Benutzenden zur sicheren Aufbewahrung angezeigt. Mit dieser Anwendungs-ID wird die mit der Anwendung verknüpfte Mobiltelefonnummer abgerufen.

![Hauptanwendungsformular](assets/6552.JPG)

Dieses Formular ist mit **bootboxjs540,storeAFWithAttachments**-Client-Bibliotheken verknüpft, die zuvor im Kurs erstellt wurden, sowie mit einem AEM-Workflow, der bei der Formularübermittlung ausgelöst wird.


* Die Beispielformulare basieren auf einer [benutzerdefinierten adaptiven Formularvorlage](assets/custom-template-with-page-component.zip), die in AEM importiert werden muss, damit die Beispielformulare korrekt dargestellt werden.

* Das ausgefüllte [StoreAfWithAttachments-Formular](assets/store-af-with-attachments-form.zip) kann heruntergeladen und in Ihre AEM-Instanz importiert werden.

* Der [AEM-Workflow, der mit diesem Formular verknüpft ist](assets/workflow-model-store-af-with-attachments.zip), muss in Ihre AEM-Instanz importiert werden, damit das Formular funktioniert.


## Nächste Schritte

[Erstellen des Formulars, das das gespeicherte Formular abruft](./retrieve-saved-form.md)