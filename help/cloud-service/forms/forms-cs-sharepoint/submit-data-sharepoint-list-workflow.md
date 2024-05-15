---
title: Senden von Daten an die SharePoint-Liste mithilfe des Workflow-Schritts
description: Einfügen von Daten in die Sharepoint-Liste mithilfe des Workflow-Schritts „Formulardatenmodelldienst aufrufen“
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# Einfügen von Daten in die SharePoint-Liste mithilfe des Workflow-Schritts „Formulardatenmodelldienst aufrufen“


In diesem Artikel werden die Schritte erläutert, die zum Einfügen von Daten in die SharePoint-Liste mithilfe des AEM-Workflow-Schritts „Formulardatenmodelldienst aufrufen“ erforderlich sind.

In diesem Artikel wird davon ausgegangen, dass Sie [das adaptive Formular erfolgreich konfiguriert haben, um Daten an die SharePoint-Liste zu senden](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=de#connect-af-sharepoint-list).


## Erstellen eines Formulardatenmodells basierend auf der SharePoint-Listen-Datenquelle

* Erstellen Sie ein Formulardatenmodell basierend auf der SharePoint-Listen-Datenquelle.
* Fügen Sie das entsprechende Modell und den Dienst „get“ des Formulardatenmodells hinzu.
* Konfigurieren Sie den Dienst „insert“, um das Modellobjekt der obersten Ebene einzufügen.
* Testen Sie den Dienst „insert“.


## Erstellen eines Workflows

* Erstellen Sie einen einfachen Workflow mit dem Schritt „Formulardatenmodelldienst aufrufen“.
* Konfigurieren Sie den Schritt „Formulardatenmodelldienst aufrufen“, um das im vorherigen Schritt erstellte Formulardatenmodell zu verwenden.
* ![Verknüpfen des Formulardatenmodells](assets/fdm-insert-1.png)

* ![Zuordnen von Eingabeparametern](assets/fdm-insert-2.png)
* Wie Sie sehen, wird die JSON-Punktnotation verwendet. Die übermittelten Daten haben das unten stehende Format. Daraus extrahieren wir das ContactUS-Objekt.

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



## Konfigurieren adaptiver Formulare zum Auslösen eines AEM-Workflows

* Erstellen Sie ein adaptives Formular basierend auf dem im vorherigen Schritt erstellten Formulardatenmodell.
* Ziehen Sie einige Felder aus der Datenquelle in das Formular.
* Konfigurieren Sie die Übermittlungsaktion des Formulars, wie nachfolgend dargestellt:
* ![submit-action](assets/configure-af.png)



## Testen des Formulars

Zeigen Sie das im vorherigen Schritt erstellte Formular in einer Vorschau an.  Füllen Sie das Formular aus und senden Sie es ab. Die Daten aus dem Formular sollten in die SharePoint-Liste eingefügt werden.
