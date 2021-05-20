---
title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# Workflow zum Überprüfen und Genehmigen der gesendeten PDF

Der letzte und letzte Schritt besteht darin, AEM Workflow zu erstellen, der eine statische oder nicht interaktive PDF-Datei zur Überprüfung und Genehmigung generiert. Der Workflow wird über einen AEM-Starter ausgelöst, der auf dem Knoten `/content/pdfsubmissions` konfiguriert ist.

Der folgende Screenshot zeigt die im Workflow erforderlichen Schritte.

![ Workflow](assets/workflow.PNG)

## Workflow-Schritt &quot;Nicht-interaktive PDF generieren&quot;

Die XDP-Vorlage und die mit der Vorlage zusammenzuführenden Daten werden hier angegeben. Die zusammenzuführenden Daten sind die gesendeten Daten aus der PDF-Datei. Diese übermittelten Daten werden unter dem Knoten `/content/pdfsubmissions` gespeichert.

![ Workflow](assets/generate-pdf1.PNG)

Die generierte PDF-Datei wird der Workflow-Variablen `submittedPDF` zugewiesen.

![ Workflow](assets/generate-pdf2.PNG)

### Zuweisen des generierten PDF-Dokuments zur Überprüfung und Genehmigung

Die Workflow-Komponente &quot;Aufgabe zuweisen&quot;wird hier verwendet, um die generierte PDF-Datei zur Überprüfung und Genehmigung zuzuweisen. Die Variable `submittedPDF` wird auf der Registerkarte &quot;Forms und Dokumente&quot;der Workflow-Komponente &quot;Aufgabe zuweisen&quot;verwendet.

![ Workflow](assets/assign-task.PNG)
