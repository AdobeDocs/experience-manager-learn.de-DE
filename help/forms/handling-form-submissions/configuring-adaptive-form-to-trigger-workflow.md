---
title: Konfigurieren des adaptiven Formulars für Trigger AEM Workflow - Übersicht
description: Konfigurieren von Payload-Optionen beim Auslösen AEM Workflows bei der Formularübermittlung
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 7%

---

# Konfigurieren des Workflows &quot;Adaptives Formular für Trigger AEM&quot;

## Voraussetzungen

Das in diesem Workflow verwendete Beispielformular basiert auf einer benutzerdefinierten adaptiven Formularvorlage, die in Ihren AEM importiert werden muss. Das bereitgestellte Beispielformular muss nach dem Import der Vorlage importiert werden.

### Adaptive Formularvorlagen abrufen

* Download [Adaptive Formularvorlage](assets/af-form-template.zip)
* [Importieren Sie die Vorlage mit Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Hochladen und Installieren der adaptiven Formularvorlage

### Abrufen des adaptiven Beispielformulars

* Download [Adaptives Formular](assets/peak-application-form.zip)
* Navigieren Sie zu [Formular und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen > Datei-Upload .
* Das adaptive Beispielformular wird in einem Ordner namens [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

Im folgenden Video wird erläutert, wie Sie ein adaptives Formular so konfigurieren, dass es in einen AEM Workflow Trigger wird
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

Das folgende Video zeigt die Workflow-Payload und andere Details im CRX-Repository

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)
