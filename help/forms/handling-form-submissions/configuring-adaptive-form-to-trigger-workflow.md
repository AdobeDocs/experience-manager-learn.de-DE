---
title: Konfigurieren des adaptiven Formulars zum Auslösen AEM Workflows
description: Payload-Optionen beim Auslösen AEM Workflows bei Formularübermittlung konfigurieren
sub-product: Formulare
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 8%

---


# Konfigurieren des adaptiven Formulars zum Auslösen AEM Workflows

## Voraussetzungen

Das in diesem Workflow verwendete Musterformular basiert auf einer benutzerdefinierten Vorlage für adaptive Formulare, die in Ihren AEM importiert werden muss. Das bereitgestellte Musterformular muss nach dem Importieren der Vorlage importiert werden.

### Adaptive Formularvorlagen abrufen

* [Vorlage für adaptive Formulare herunterladen](assets/af-form-template.zip)
* [Importieren der Vorlage mit dem Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Hochladen und Installieren der Vorlage für das adaptive Formular

### Beispiel für ein adaptives Formular abrufen

* [Adaptives Formular](assets/peak-application-form.zip) herunterladen
* Gehen Sie zu [Formular und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Klicken Sie auf Erstellen -> Datei-Upload
* Das adaptive Beispielformular wird in einem Ordner namens [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

Im folgenden Video wird erläutert, wie ein adaptives Formular so konfiguriert wird, dass ein AEM Workflow ausgelöst wird
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

Das folgende Video zeigt die Workflow-Nutzlast und andere Details im CRX-Repository

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


