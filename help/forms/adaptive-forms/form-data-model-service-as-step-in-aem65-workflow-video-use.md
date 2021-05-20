---
title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow
seo-title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow
description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung des "Formulardatenmodelldienstes aufrufen"in AEM Workflow sehr einfach geworden. Das folgende Video führt Sie durch die Schritte, die bei der Verwendung des Formulardatenmodelldienstes aufrufen in AEM Workflow erforderlich sind.
seo-description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung des "Formulardatenmodelldienstes aufrufen"in AEM Workflow sehr einfach geworden. Das folgende Video führt Sie durch die Schritte, die bei der Verwendung des Formulardatenmodelldienstes aufrufen in AEM Workflow erforderlich sind.
feature: Workflow
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---


# Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Teil AEM Arbeitsablaufs verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts &quot;Formulardatenmodell&quot;in AEM Workflow erforderlich sind

>!![NOTE]Die in diesem Video demonstrierte Funktion erfordert AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen

* Richten Sie tomcat mit der Datei SampleRest.war ein, wie [hier](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html) beschrieben. Die in Tomcat bereitgestellte WAR-Datei hat den Code, um die Kreditwürdigkeit des Antragstellers zurückzugeben. Die Kreditwürdigkeit ist eine zufällige Zahl zwischen 200 und 800.

* [ Importieren von Assets in AEM mit Package Manager](assets/aem65-loanapplication.zip)
* Das Paket enthält die folgenden:

   * Workflow-Modell, das den FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung Trigger.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie. Bei der Formularübermittlung wird der [loanapplication-Workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/invokefdm651.PNG).
Der Workflow nutzt die Komponente ODER-Teilung , um die Anwendung an den Administrator weiterzuleiten, wenn die Gewichtung über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag zur Aufnahme weitergeleitet.
