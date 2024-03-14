---
title: Daten mithilfe des Workflow-Schritts an die SharePoint-Liste senden
description: Fügen Sie Daten mithilfe des FDM-Workflow-Schritts in die Sharepoint-Liste ein.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 3%

---

# Daten mithilfe des Workflow-Schritts &quot;FDM aufrufen&quot;in die SharePoint-Liste einfügen


In diesem Artikel werden die Schritte erläutert, die zum Einfügen von Daten in die SharePoint-Liste mithilfe des FDM-Aufrufschritts im Workflow-AEM erforderlich sind.

In diesem Artikel wird davon ausgegangen, dass [das adaptive Formular erfolgreich konfiguriert hat, um Daten an die SharePoint-Liste zu senden.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=de#connect-af-sharepoint-list)


## Formulardatenmodell basierend auf der SharePoint-Listendatenquelle erstellen

* Erstellen Sie ein neues Formulardatenmodell basierend auf der SharePoint-Listendatenquelle.
* Fügen Sie das entsprechende Modell und den get-Dienst des Formulardatenmodells hinzu.
* Konfigurieren Sie den insert-Dienst, um das Modellobjekt der obersten Ebene einzufügen.
* Testen Sie den insert-Dienst.


## Erstellen eines Workflows

* Erstellen Sie einen einfachen Workflow mit dem Schritt FDM aufrufen .
* Konfigurieren Sie den Schritt &quot;FDM aufrufen&quot;, um das im vorherigen Schritt erstellte Formulardatenmodell zu verwenden.
* ![Associate-FDM](assets/fdm-insert-1.png)

* ![map-input-parameters](assets/fdm-insert-2.png)
* Beachten Sie die Verwendung der JSON-Punktnotation. Die übermittelten Daten haben das unten stehende Format und wir extrahieren das ContactUS-Objekt aus den gesendeten Daten.

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## Workflow &quot;Adaptives Formular für Trigger konfigurieren AEM&quot;

* Erstellen Sie ein adaptives Formular mithilfe des Formulardatenmodells, das im vorherigen Schritt erstellt wurde.
* Ziehen Sie einige Felder aus der Datenquelle in das Formular.
* Konfigurieren Sie die Sendeaktion des Formulars wie unten dargestellt
* ![submit-action](assets/configure-af.png)



## Testen des Formulars

Vorschau des im vorherigen Schritt erstellten Formulars Füllen Sie das Formular aus und senden Sie es. Die Daten aus dem Formular sollten in die SharePoint-Liste eingefügt werden.

