---
title: Auslösen des AEM-Workflows bei der Übermittlung von HTML5-Formularen – Überprüfen und Genehmigen von PDF-Dateien
description: Workflow zur Überprüfung übermittelter PDF-Dateien
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: ht
source-wordcount: '172'
ht-degree: 100%

---

# Workflow zur Überprüfung und Genehmigung übermittelter PDF-Dateien

Der letzte und abschließende Schritt besteht darin, einen AEM-Workflow zu erstellen, bei dem eine statische (oder nicht interaktive) PDF-Datei zur Überprüfung und Genehmigung generiert wird. Der Workflow wird über einen auf dem Knoten `/content/formsubmissions` konfigurierten AEM-Starter ausgelöst.

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