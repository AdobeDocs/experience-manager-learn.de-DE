---
title: 'Formularanlage in Datenbank einfügen '
description: Fügen Sie mithilfe AEM Workflows Formularanhänge in die Datenbank ein.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Formularanhang in Datenbank einfügen

In diesem Artikel wird der Anwendungsfall zum Speichern von Formularanlagen in der MySQL-Datenbank erläutert.

Eine gängige Anforderung von Kunden besteht darin, erfasste Formulardaten und den Formularanhang in einer Datenbanktabelle zu speichern.
Gehen Sie wie folgt vor, um dieses Anwendungsbeispiel zu erstellen:

## Erstellen Sie eine Datenbanktabelle für die Formulardaten und den Anhang

Eine Tabelle mit dem Namen newhire wurde erstellt, um die Formulardaten zu speichern. Beachten Sie das Spaltennamenbild des Typs **LONGBLOB** zum Speichern des Formularanhangs
![table-schema](assets/insert-picture-table.png)

## Erstellen von Formulardatenmodellen

Es wurde ein Formulardatenmodell zur Kommunikation mit der MySQL-Datenbank erstellt. Sie müssen Folgendes erstellen:

* [JDBC-Datenquelle in AEM](./data-integration-technical-video-setup.md)
* [Formulardatenmodell basierend auf der JDBC-Datenquelle](./jdbc-data-model-technical-video-use.md)

## Workflow erstellen

Wenn Sie Ihr adaptives Formular für die Übermittlung an einen AEM-Workflow konfigurieren möchten, können Sie die Formularanhänge in einer Workflow-Variablen speichern oder die Anlagen in einem angegebenen Ordner unter der Payload speichern. Für diesen Anwendungsfall müssen wir die Anlagen in einer Workflow-Variablen des Typs ArrayList of Document speichern. Aus dieser ArrayList müssen wir das erste Element extrahieren und eine document -Variable initialisieren. Die Workflow-Variablen namens **listOfDocuments** und **employeeFoto** erstellt wurden.
Wenn das adaptive Formular an den Trigger des Workflows gesendet wird, initialisiert ein Schritt im Workflow die employeeFoto -Variable mithilfe des ECMA-Skripts. Im Folgenden finden Sie den ECMA-Skriptcode

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

Der nächste Schritt im Workflow besteht darin, Daten und den Formularanhang mithilfe der Dienstkomponente Formulardatenmodell aufrufen in die Tabelle einzufügen.
![insert-pic](assets/fdm-insert-pic.png)
[Der vollständige Workflow mit dem Beispielecma-Skript kann hier heruntergeladen werden](assets/add-new-employee.zip).

>[!NOTE]
> Sie müssen ein neues JDBC-basiertes Formulardatenmodell erstellen und dieses Formulardatenmodell im Workflow verwenden

## Adaptives Formular erstellen

Erstellen Sie Ihr adaptives Formular basierend auf dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben. Ziehen Sie die Formulardatenmodellelemente in das Formular. Konfigurieren Sie die Formularübermittlung für den Trigger des Workflows und geben Sie die folgenden Eigenschaften an, wie im Screenshot unten dargestellt
![form-attachments](assets/form-attachments.png)