---
title: Einfügen von Formularanlagen in eine Datenbank
description: Fügen Sie mithilfe des AEM-Workflows eine Formularanlage in eine Datenbank ein.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 114
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 100%

---

# Einfügen von Formularanlagen in eine Datenbank

In diesem Artikel wird der Anwendungsfall zum Speichern von Formularanlagen in der MySQL-Datenbank erläutert.

Eine gängige Anforderung der Kundschaft besteht darin, erfasste Formulardaten und die Formularanlage in einer Datenbanktabelle zu speichern.
Um diesen Anwendungsfall zu realisieren, wurden die folgenden Schritte durchgeführt:

## Erstellen einer Datenbanktabelle für die Formulardaten und -anlagen

Eine Tabelle mit dem Namen „newhire“ wurde erstellt, um die Formulardaten zu speichern. Beachten Sie den Spaltennamen Bild des Typs **LONGBLOB** zur Speicherung der Formularanlage
![table-schema](assets/insert-picture-table.png)

## Erstellen von Formulardatenmodellen

Es wurde ein Formulardatenmodell zur Kommunikation mit der MySQL-Datenbank erstellt. Sie müssen Folgendes erstellen:

* [JDBC-Datenquelle in AEM](./data-integration-technical-video-setup.md)
* [Formulardatenmodell basierend auf der JDBC-Datenquelle](./jdbc-data-model-technical-video-use.md)

## Erstellen eines Workflows

Wenn Sie Ihr adaptives Formular für die Übermittlung an einen AEM-Workflow konfigurieren möchten, können Sie die Formularanhänge in einer Workflow-Variablen speichern oder die Anlagen in einem angegebenen Ordner unter dem Payload speichern. Für diesen Anwendungsfall müssen wir die Anlagen in einer Workflow-Variablen des Typs „ArrayList of Document“ speichern. Aus dieser ArrayList müssen wir das erste Element extrahieren und eine Dokumentvariable initialisieren. Die Workflow-Variablen namens **listOfDocuments** und **employeePhoto** wurden erstellt.
Wenn das adaptive Formular zur Auslösung des Workflows übermittelt wird, initialisiert ein Schritt im Workflow die Variable „employeePhoto“ mithilfe des ECMA-Skripts. Im Folgenden finden Sie den ECMA-Skript-Code:

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

Der nächste Schritt im Workflow besteht darin, Daten und den Formularanhang mithilfe von „Formulardatenmodell-Dienstkomponente aufrufen“ in die Tabelle einzufügen.
![insert-pic](assets/fdm-insert-pic.png)
[Der vollständige Workflow mit dem Beispiel-ECMA-Skript kann hier heruntergeladen werden.](assets/add-new-employee.zip).

>[!NOTE]
> Sie müssen ein neues JDBC-basiertes Formulardatenmodell erstellen und dieses Formulardatenmodell im Workflow verwenden.

## Erstellen eines adaptiven Formulars

Erstellen Sie Ihr adaptives Formular basierend auf dem Formulardatenmodell, das Sie im vorherigen Schritt erstellt haben. Ziehen Sie die Elemente des Formulardatenmodells per Drag-and-Drop in das Formular. Konfigurieren Sie die Übermittlung des Formulars, um den Workflow auszulösen, und geben Sie die folgenden Eigenschaften an, wie im folgenden Screenshot dargestellt:
![form-attachments](assets/form-attachments.png)
