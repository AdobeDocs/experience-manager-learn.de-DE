---
title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow
description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM-Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung von „Formulardatenmodelldienst aufrufen“ im AEM-Workflow sehr einfach geworden. Das folgende Video führt Sie durch die Schritte, die bei der Verwendung von „Formulardatenmodelldienst aufrufen“ im AEM-Workflow erforderlich sind.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 253
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '257'
ht-degree: 100%

---

# Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5 Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Teil des AEM-Workflows verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodell-Schritts im AEM-Workflow erforderlich sind

>Die in diesem Video demonstrierte Funktion erfordert AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Um dies auf Ihrem Server zu testen, folgen Sie den folgenden Anweisungen

* Richten Sie Tomcat mit der Datei SampleRest.war ein, wie [hier](https://helpx.adobe.com/de/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html) beschrieben. Die WAR-Datei, die in Tomcat eingesetzt wird, enthält den Code für die Rückgabe der Kreditwürdigkeit der Person, die den Antrag gestellt hat. Die Kreditwürdigkeit ist eine Zufallszahl zwischen 200 und 800.

* [Importieren Sie die Assets mithilfe von Package Manager in AEM.](assets/aem65-loanapplication.zip)
* Das Paket enthält Folgendes:

   * Workflow-Modell, das den FDM-Schritt nutzt.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung auszulösen.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie es. Bei der Übermittlung des Formulars wird [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/invokefdm651.PNG).
Der Workflow nutzt die Komponente „ODER-Teilung“, um die Anwendung an Admins weiterzuleiten, wenn die Kreditwürdigkeit über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag an Cavery weitergeleitet.
