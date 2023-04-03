---
title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow
description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung des "Formulardatenmodelldienstes aufrufen"in AEM Workflow sehr einfach geworden. Das folgende Video führt Sie durch die Schritte, die bei der Verwendung des Formulardatenmodelldienstes aufrufen in AEM Workflow erforderlich sind.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 2%

---

# Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Teil AEM Arbeitsablaufs verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts &quot;Formulardatenmodell&quot;in AEM Workflow erforderlich sind

>!![NOTE]Die in diesem Video demonstrierte Funktion erfordert AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen

* Einrichten von Tomcat mit der Datei SampleRest.war wie beschrieben [here](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Die in Tomcat bereitgestellte WAR-Datei hat den Code, um die Kreditwürdigkeit des Antragstellers zurückzugeben. Die Kreditwürdigkeit ist eine zufällige Zahl zwischen 200 und 800.

* [ Importieren von Assets in AEM mit Package Manager](assets/aem65-loanapplication.zip)
* Das Paket enthält die folgenden:

   * Workflow-Modell, das den FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung Trigger.
* Öffnen Sie die [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie. Bei der Übermittlung des Formulars [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst wird.

![ Workflow ](assets/invokefdm651.PNG).
Der Workflow nutzt die Komponente ODER-Teilung , um die Anwendung an den Administrator weiterzuleiten, wenn die Gewichtung über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag zur Aufnahme weitergeleitet.
