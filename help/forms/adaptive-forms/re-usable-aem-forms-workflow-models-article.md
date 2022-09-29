---
title: Wiederverwendbare AEM Forms-Workflow-Modelle
description: Erfahren Sie, wie Sie unabhängig von Adaptive Forms Workflow-Modelle erstellen.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 1%

---

# Erstellen wiederverwendbarer AEM Forms-Workflow-Modelle{#create-re-usable-aem-forms-workflow-models}

Ab AEM Forms-Version 6.5 können wir jetzt Workflow-Modelle erstellen, die nicht an ein bestimmtes adaptives Formular gebunden sind. Mit dieser Funktion können Sie jetzt ein Workflow-Modell erstellen, das bei verschiedenen Übermittlungen adaptiver Formulare aufgerufen werden kann. Mit dieser Funktion können Sie über einen generischen Workflow verfügen, mit dem alle Übermittlungen adaptiver Formulare zur Überprüfung und Genehmigung verarbeitet werden.

Führen Sie die folgenden Schritte aus, um einen solchen Workflow zu erstellen

1. Melden Sie sich bei AEM an.
1. Zeigen Sie Ihren Browser auf [Workflow-Modell](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Klicken __Erstellen > Modell erstellen__ Hinzufügen eines Workflow-Modells
1. Geben Sie dem Workflow-Modell den entsprechenden Namen und Titel ein und klicken Sie auf Fertig .
1. Öffnen Sie das neu erstellte Modell im Bearbeitungsmodus
1. Ziehen Sie die Komponente &quot;Aufgabe zuweisen&quot;per Drag-and-Drop in Ihr Workflow-Modell
1. Öffnen Sie die Konfigurationseigenschaften der Komponente &quot;Assign Task&quot;
1. Registerkarte &quot;Forms und Dokumente&quot;
1. Wählen Sie den Typ: Adaptives Formular oder Schreibgeschütztes adaptives Formular aus.

Es gibt drei Möglichkeiten, den Formularpfad anzugeben

1. Verfügbar unter einem absoluten Pfad - Dies bedeutet, dass der Workflow eng mit dem adaptiven Formular verbunden ist. Das wollen wir hier nicht
1. **An den Workflow gesendet** - Wenn das adaptive Formular übermittelt wird, extrahiert die Workflow-Engine den Namen des Formulars aus den gesendeten Daten. Dies ist die auszuwählende Option
1. Verfügbar unter einem Pfad in einer Variablen - Das bedeutet, dass das adaptive Formular aus der Workflow-Variablen abgerufen wird Der folgende Screenshot zeigt die richtige Option, die Sie für die Entkopplung des Workflows vom adaptiven Formular benötigen

![Wiederverwendbare AEM Forms-Workflow-Modelle](assets/workflomodel.PNG)
