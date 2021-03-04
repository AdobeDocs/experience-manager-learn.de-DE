---
title: Arbeitsablauf für Trigger AEM HTML5-Formularübermittlung
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# Arbeitsablauf zum Überprüfen und Genehmigen der gesendeten PDF

Der letzte und letzte Schritt besteht darin, AEM Arbeitsablauf zu erstellen, der eine statische oder nicht interaktive PDF-Datei zur Überprüfung und Genehmigung generiert. Der Workflow wird über einen AEM Launcher ausgelöst, der auf dem Knoten `/content/pdfsubmissions` konfiguriert ist.

Der folgende Screenshot zeigt die Schritte im Workflow.

![ Workflow](assets/workflow.PNG)

## Arbeitsablaufschritt &quot;Nicht interaktive PDF erstellen&quot;

Die XDP-Vorlage und die mit der Vorlage zusammenzuführenden Daten werden hier angegeben. Die zusammenzuführenden Daten sind die gesendeten Daten aus der PDF-Datei. Diese gesendeten Daten werden unter dem Knoten `/content/pdfsubmissions` gespeichert.

![ Workflow](assets/generate-pdf1.PNG)

Die generierte PDF wird der Workflow-Variablen `submittedPDF` zugewiesen.

![ Workflow](assets/generate-pdf2.PNG)

### Zuweisen der generierten PDF zur Überprüfung und Genehmigung

Die Workflow-Komponente &quot;Aufgabe zuweisen&quot;wird hier verwendet, um die generierte PDF-Datei zur Überprüfung und Genehmigung zuzuweisen. Die Variable `submittedPDF` wird auf der Registerkarte &quot;Forms und Dokumente&quot;der Komponente &quot;Aufgabe zuweisen&quot;verwendet.

![ Workflow](assets/assign-task.PNG)
