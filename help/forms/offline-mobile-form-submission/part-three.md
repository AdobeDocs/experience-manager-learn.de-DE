---
title: AEM Workflow bei der Übermittlung des HTML5-Formulars auslösen
seo-title: AEM Workflow bei der Übermittlung von HTML5-Formularen auslösen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie ein mobiles Formular, um AEM Workflow auszulösen
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie ein mobiles Formular, um AEM Workflow auszulösen
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# Arbeitsablauf zum Überprüfen und Genehmigen der gesendeten PDF

Der letzte und letzte Schritt besteht darin, AEM Arbeitsablauf zu erstellen, der eine statische oder nicht interaktive PDF-Datei zur Überprüfung und Genehmigung generiert. Der Workflow wird über einen auf dem Knoten konfigurierten AEM-Starter ausgelöst `/content/pdfsubmissions`.

Der folgende Screenshot zeigt die Schritte im Workflow.

![ Workflow](assets/workflow.PNG)

## Arbeitsablaufschritt &quot;Nicht interaktive PDF erstellen&quot;

Die XDP-Vorlage und die mit der Vorlage zusammenzuführenden Daten werden hier angegeben. Die zusammenzuführenden Daten sind die gesendeten Daten aus der PDF-Datei. Diese gesendeten Daten werden unter dem Knoten gespeichert `/content/pdfsubmissions`.

![ Workflow](assets/generate-pdf1.PNG)

Die generierte PDF wird der Workflow-Variablen namens `submittedPDF`.

![ Workflow](assets/generate-pdf2.PNG)

### Zuweisen der generierten PDF zur Überprüfung und Genehmigung

Die Workflow-Komponente &quot;Aufgabe zuweisen&quot;wird hier verwendet, um die generierte PDF-Datei zur Überprüfung und Genehmigung zuzuweisen. Die Variable `submittedPDF` wird auf der Registerkarte &quot;Forms&quot;und &quot;Dokumente&quot;der Workflow-Komponente &quot;Aufgabe zuweisen&quot;verwendet.

![ Workflow](assets/assign-task.PNG)
