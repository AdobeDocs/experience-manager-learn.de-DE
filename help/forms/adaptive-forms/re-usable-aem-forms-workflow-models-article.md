---
title: Erstellen Sie wiederverwendbare AEM Forms-Workflow-Modelle.
description: Workflow-Modelle, die unabhängig von Adaptive Forms sind.
feature: Workflow
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Erstellen wiederverwendbarer AEM Forms-Workflow-Modelle{#create-re-usable-aem-forms-workflow-models}

Ab AEM Forms-Version 6.5 können wir jetzt Workflow-Modelle erstellen, die nicht an ein bestimmtes adaptives Formular gebunden sind. Mit dieser Funktion können Sie jetzt ein Workflow-Modell erstellen, das bei verschiedenen Übermittlungen adaptiver Formulare aufgerufen werden kann. Mit dieser Funktion können Sie über einen generischen Workflow verfügen, mit dem alle Übermittlungen adaptiver Formulare zur Überprüfung und Genehmigung verarbeitet werden.

Führen Sie die folgenden Schritte aus, um einen solchen Workflow zu erstellen

1. Bei AEM anmelden
1. Verweisen Sie Ihren Browser auf [Workflow-Modell](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Klicken Sie auf Erstellen | Modell zum Hinzufügen eines Workflow-Modells erstellen
1. Geben Sie dem Workflow-Modell den entsprechenden Namen und Titel ein und klicken Sie auf Fertig .
1. Öffnen Sie das neu erstellte Modell im Bearbeitungsmodus
1. Ziehen Sie die Komponente &quot;Aufgabe zuweisen&quot;per Drag-and-Drop in Ihr Workflow-Modell
1. Öffnen Sie die Konfigurationseigenschaften der Komponente &quot;Assign Task&quot;
1. Registerkarte &quot;Forms und Dokumente&quot;
1. Wählen Sie den Typ: Adaptives Formular oder Schreibgeschütztes adaptives Formular aus.

Es gibt drei Möglichkeiten, den Formularpfad anzugeben

1. Verfügbar unter einem absoluten Pfad - Dies bedeutet, dass der Workflow eng mit dem adaptiven Formular gekoppelt wird. Das wollen wir hier nicht
1. **Gesendet an den Workflow**  - Das bedeutet, dass die Workflow-Engine beim Senden des adaptiven Formulars den Namen des Formulars aus den gesendeten Daten extrahiert. Dies ist die auszuwählende Option
1. Verfügbar unter einem Pfad in einer Variablen - Das bedeutet, dass das adaptive Formular aus der Workflow-Variablen abgerufen wird.
Der folgende Screenshot zeigt die richtige Option, die Sie für die Entkopplung des Workflows vom adaptiven Formular benötigen

![workflowmodel](assets/workflomodel.PNG)