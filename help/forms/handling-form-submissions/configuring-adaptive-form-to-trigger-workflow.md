---
title: Konfigurieren des Workflows "Adaptives Formular für Trigger AEM"
description: Konfigurieren von Payload-Optionen beim Auslösen AEM Workflows bei der Formularübermittlung
sub-product: Formulare
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 9%

---


# Konfigurieren des Workflows &quot;Adaptives Formular für Trigger AEM&quot;

## Voraussetzungen

Das in diesem Workflow verwendete Beispielformular basiert auf einer benutzerdefinierten adaptiven Formularvorlage, die in Ihren AEM importiert werden muss. Das bereitgestellte Beispielformular muss nach dem Import der Vorlage importiert werden.

### Adaptive Formularvorlagen abrufen

* [Adaptive Formularvorlage](assets/af-form-template.zip) herunterladen
* [Importieren Sie die Vorlage mit Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Hochladen und Installieren der adaptiven Formularvorlage

### Abrufen des adaptiven Beispielformulars

* [Adaptives Formular](assets/peak-application-form.zip) herunterladen
* Navigieren Sie zu [Formular und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen > Datei-Upload .
* Das adaptive Beispielformular wird in einem Ordner namens [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms) platziert.

Im folgenden Video wird erläutert, wie Sie ein adaptives Formular so konfigurieren, dass es in einen AEM Workflow Trigger wird
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

Das folgende Video zeigt die Workflow-Payload und andere Details im CRX-Repository

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


