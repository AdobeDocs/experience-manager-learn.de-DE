---
title: Automatisch startende Workflows
description: Automatisch startende Workflows erweitern die Asset-Verarbeitung, indem beim Hochladen oder erneuten Verarbeiten automatisch ein benutzerdefinierter Workflow aufgerufen wird.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
kt: 4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
source-git-commit: 929fd045b81652463034b54c557de04df3d4e64a
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Automatisch startende Workflows

Automatisch startende Workflows erweitern die Asset-Verarbeitung in AEM as a Cloud Service, indem beim Hochladen oder erneuten Verarbeiten automatisch ein benutzerdefinierter Workflow aufgerufen wird, sobald die Asset-Verarbeitung abgeschlossen ist.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>Verwenden Sie Workflows zum automatischen Starten, um die Nachbearbeitung von Assets anzupassen, anstatt Workflow-Starter zu verwenden. Automatisch startende Workflows sind _only_ Wird aufgerufen, sobald die Verarbeitung eines Assets abgeschlossen ist, anstatt von Startern, die während der Asset-Verarbeitung mehrmals aufgerufen werden können.

## Anpassen des Nachbearbeitungs-Workflows

Um den Nachbearbeitungs-Workflow anzupassen, kopieren Sie die standardmäßige Asset Cloud-Nachbearbeitung [Workflow-Modell](../../foundation/workflow/use-the-workflow-editor.md).

1. Starten Sie im Bildschirm Workflow-Modelle , indem Sie zu _Instrumente_ > _Workflow_ > _Modelle_
2. Suchen und Auswählen _Nachbearbeitung von Assets Cloud_ Workflow-Modell<br/>
   ![Auswählen des Assets Cloud-Nachbearbeitungs-Workflow-Modells](assets/auto-start-workflow-select-workflow.png)
3. Wählen Sie die _Kopieren_ Schaltfläche zum Erstellen eines benutzerdefinierten Workflows
4. Wählen Sie Ihr jetzt verwendetes Workflow-Modell aus (namens _Assets Cloud-Nachbearbeitung1_) und klicken Sie auf die _Bearbeiten_ Schaltfläche zum Bearbeiten des Workflows
5. Geben Sie in den Workflow-Eigenschaften Ihrem benutzerdefinierten Nachbearbeitungs-Workflow einen aussagekräftigen Namen.<br/>
   ![Ändern des Namens](assets/auto-start-workflow-change-name.png)
6. Fügen Sie die Schritte hinzu, um Ihre Geschäftsanforderungen zu erfüllen. Fügen Sie in diesem Fall eine Aufgabe hinzu, wenn die Verarbeitung der Assets abgeschlossen ist. Stellen Sie sicher, dass der letzte Schritt des Workflows immer der _Workflow abgeschlossen_ Schritt<br/>
   ![Hinzufügen von Workflow-Schritten](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >Automatisch startende Workflows werden bei jedem Hochladen oder erneuten Verarbeiten von Assets ausgeführt. Überlegen Sie daher sorgfältig die Auswirkungen der Skalierung von Workflow-Schritten, insbesondere bei Massenvorgängen wie [Massenimporte](../../cloud-service/migration/bulk-import.md) oder Migrationen.

7. Wählen Sie die _Synchronisieren_ Schaltfläche zum Speichern Ihrer Änderungen und Synchronisieren des Workflow-Modells

## Verwenden eines benutzerdefinierten Nachbearbeitungs-Workflows

Die benutzerdefinierte Nachbearbeitung wird für Ordner konfiguriert. So konfigurieren Sie einen benutzerdefinierten Nachbearbeitungs-Workflow für einen Ordner:

1. Wählen Sie den Ordner aus, für den Sie den Workflow konfigurieren möchten, und bearbeiten Sie die Ordnereigenschaften
2. Wechseln Sie zu _Asset-Verarbeitung_ tab
3. Wählen Sie Ihren benutzerdefinierten Nachbearbeitungs-Workflow im _Automatischer Start-Workflow_ Auswahlfeld<br/>
   ![Einrichten des Nachbearbeitungs-Workflows](assets/auto-start-workflow-set-workflow.png)
4. Speichern Sie Ihre Änderungen

Jetzt wird Ihr benutzerdefinierter Nachbearbeitungs-Workflow für alle Assets ausgeführt, die unter diesem Ordner hochgeladen oder erneut verarbeitet wurden.
