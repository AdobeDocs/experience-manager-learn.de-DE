---
title: Konfigurieren eines adaptiven Formulars zum Auslösen von AEM-Workflows – Übersicht
description: Konfigurieren von Payload-Optionen beim Auslösen von AEM-Workflows bei der Formularübermittlung
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 583
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 100%

---

# Konfigurieren adaptiver Formulare zum Auslösen eines AEM-Workflows 

## Voraussetzungen

Das in diesem Workflow verwendete Beispielformular basiert auf einer benutzerdefinierten adaptiven Formularvorlage, die in Ihren AEM-Server importiert werden muss. Das bereitgestellte Beispielformular muss nach dem Import der Vorlage importiert werden.

### Abrufen der adaptiven Formularvorlagen

* Laden Sie die [adaptive Formularvorlage](assets/af-form-template.zip) herunter.
* [Importieren Sie die Vorlage mit Package Manager.](http://localhost:4502/crx/packmgr/index.jsp)
* Laden Sie die adaptive Formularvorlage hoch und installieren Sie sie.

### Abrufen des adaptiven Beispielformulars

* Laden Sie das [adaptive Formular](assets/peak-application-form.zip) herunter.
* Navigieren Sie zu [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Klicken Sie auf „Erstellen“ > „Datei hochladen“.
* Das adaptive Beispielformular wird in einem Ordner namens [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms) hinterlegt.

Im folgenden Video wird erläutert, wie Sie ein adaptives Formular so konfigurieren, dass es einen AEM-Workflow auslöst.
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

Das folgende Video zeigt die Payload des Workflows und andere Details im CRX-Repository.

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
