---
title: Trigger AEM Workflow für die Übermittlung von HTML5-Formularen - PDF überprüfen und genehmigen
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 2%

---

# Workflow zur Überprüfung und Genehmigung der übermittelten PDF

Der letzte und letzte Schritt besteht darin, AEM Workflow zu erstellen, der eine statische oder nicht interaktive PDF zur Überprüfung und Genehmigung generiert. Der Workflow wird über einen auf dem Knoten konfigurierten AEM-Starter ausgelöst `/content/pdfsubmissions`.

Der folgende Screenshot zeigt die im Workflow erforderlichen Schritte.

![ Workflow](assets/workflow.PNG)

## Workflow-Schritt &quot;Nicht-interaktive PDF generieren&quot;

Die XDP-Vorlage und die mit der Vorlage zusammenzuführenden Daten werden hier angegeben. Die zusammenzuführenden Daten sind die von der PDF übermittelten Daten. Diese übermittelten Daten werden unter dem Knoten gespeichert. `/content/pdfsubmissions`.

![ Workflow](assets/generate-pdf1.PNG)

Die generierte PDF wird der Workflow-Variablen mit dem Namen `submittedPDF`.

![ Workflow](assets/generate-pdf2.PNG)

### Zuweisen des generierten PDF-Dokuments zur Überprüfung und Genehmigung

Die Workflow-Komponente Aufgabe zuweisen wird hier verwendet, um die generierte PDF zur Überprüfung und Genehmigung zuzuweisen. Die Variable `submittedPDF` wird auf der Registerkarte &quot;Forms&quot;und &quot;Dokumente&quot;der Workflow-Komponente &quot;Aufgabe zuweisen&quot;verwendet.

![ Workflow](assets/assign-task.PNG)
