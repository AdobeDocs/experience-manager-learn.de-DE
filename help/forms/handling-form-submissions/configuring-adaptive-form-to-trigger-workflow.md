---
title: Konfigurieren des adaptiven Formulars für Trigger AEM Arbeitsablauf
description: Payload-Optionen beim Auslösen AEM Workflows bei Formularübermittlung konfigurieren
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
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 10%

---


# Konfigurieren des adaptiven Formulars für Trigger AEM Arbeitsablauf

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

Im folgenden Video wird erläutert, wie ein adaptives Formular für den Trigger eines AEM Arbeitsablaufs konfiguriert wird
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

Das folgende Video zeigt die Workflow-Nutzlast und andere Details im CRX-Repository

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


