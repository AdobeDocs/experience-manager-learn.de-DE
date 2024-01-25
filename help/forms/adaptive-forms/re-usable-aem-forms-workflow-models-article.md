---
title: Wiederverwendbare AEM Forms-Workflow-Modelle
description: Erfahren Sie, wie Sie Workflow-Modelle erstellen können, die unabhängig von adaptiven Formularen sind.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 63
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 100%

---

# Erstellen wiederverwendbarer AEM Forms-Workflow-Modelle{#create-re-usable-aem-forms-workflow-models}

Ab AEM Forms-Version 6.5 können jetzt Workflow-Modelle erstellt werden, die nicht an ein bestimmtes adaptives Formular gebunden sind. Mit dieser Funktion können Sie jetzt ein Workflow-Modell erstellen, das bei verschiedenen Übermittlungen adaptiver Formulare aufgerufen werden kann. Mit dieser Funktion können Sie über einen generischen Workflow verfügen, mit dem alle Übermittlungen adaptiver Formulare zur Überprüfung und Genehmigung verarbeitet werden können.

Führen Sie die folgenden Schritte aus, um einen solchen Workflow zu erstellen

1. Melden Sie sich bei AEM an
1. Verweisen Sie Ihren Browser auf [Workflow-Modell](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Klicken Sie auf __Erstellen > Modell erstellen__, um ein Workflow-Modell hinzuzufügen
1. Geben Sie dem Workflow-Modell den entsprechenden Namen und Titel und klicken Sie auf „Fertig“
1. Öffnen Sie das neu erstellte Modell im Bearbeitungsmodus
1. Ziehen Sie die Komponente „Aufgabe zuweisen“ per Drag-and-Drop in Ihr Workflow-Modell
1. Öffnen Sie die Konfigurationseigenschaften der Komponente „Task zuweisen“
1. Wechseln Sie zur Registerkarte „Formulare und Dokumente“
1. Wählen Sie den Typ aus: „Adaptives Formular“ oder „Schreibgeschütztes adaptives Formular“.

Es gibt drei Möglichkeiten, den Formularpfad anzugeben

1. Verfügbar unter einem absoluten Pfad: Dies bedeutet, dass der Workflow eng an das adaptiven Formular gekoppelt ist. Dies ist hier nicht angemessen
1. **An den Workflow gesendet**: Wenn das adaptive Formular übermittelt wird, extrahiert die Workflow-Engine den Namen des Formulars aus den gesendeten Daten. Dies ist die auszuwählende Option
1. Verfügbar unter einem Pfad in einer Variablen: Das bedeutet, dass das adaptive Formular aus der Workflow-Variablen abgerufen wird.
Der folgende Screenshot zeigt die Option, die Sie für die Entkopplung des Workflows vom adaptiven Formular benötigen

![Wiederverwendbare AEM Forms-Workflow-Modelle](assets/workflomodel.PNG)
