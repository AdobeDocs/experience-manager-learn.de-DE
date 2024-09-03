---
title: Trigger AEM Workflow auf HTML 5 Formularübermittlung - PDF überprüfen und genehmigen
description: Workflow zum Überprüfen der gesendeten PDF
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 81%

---

# Workflow zur Überprüfung und Genehmigung übermittelter PDF-Dateien

Der letzte und abschließende Schritt besteht darin, einen AEM-Workflow zu erstellen, bei dem eine statische (oder nicht-interaktive) PDF-Datei zur Überprüfung und Genehmigung generiert wird. Der Workflow wird über einen auf dem Knoten `/content/formsubmissions` konfigurierten AEM-Starter ausgelöst.

Der folgende Screenshot zeigt die für den Workflow erforderlichen Schritte.

![Workflow](assets/workflow.PNG)

## Workflow-Schritt „Nicht-interaktive PDF-Datei generieren“

Die XDP-Vorlage und die mit der Vorlage zusammenzuführenden Daten werden hier angegeben. Bei den zusammenzuführenden Daten handelt es sich um die von der PDF-Datei übermittelten Daten. Diese übermittelten Daten werden unter dem Knoten ```/content/formsubmissions``` gespeichert.

![Workflow](assets/generate-pdf1.PNG)

Die generierte PDF-Datei wird der Workflow-Variablen `submittedPDF` zugewiesen.

![Workflow](assets/generate-pdf2.PNG)

### Zuweisen der generierten PDF-Datei zur Überprüfung und Genehmigung

Die Workflow-Komponente „Aufgabe zuweisen“ wird hier verwendet, um die generierte PDF-Datei zur Überprüfung und Genehmigung zuzuweisen. Die Variable `submittedPDF` wird auf der Registerkarte „Formulare/Dokumente“ der Workflow-Komponente „Aufgabe zuweisen“ verwendet.

![Workflow](assets/assign-task.PNG)


## Nächste Schritte

[Bereitstellen der Assets in Ihrer Umgebung](./deploy-assets.md)