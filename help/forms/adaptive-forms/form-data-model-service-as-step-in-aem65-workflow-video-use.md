---
title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5-Arbeitsablauf
seo-title: Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5-Arbeitsablauf
description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung des "Formulardatenmodelldienstes aufrufen"in AEM Arbeitsablauf sehr einfach geworden. Im folgenden Video werden Sie durch die Schritte geführt, die bei der Verwendung des Dienstes "Formulardatenmodell aufrufen"im Arbeitsablauf AEM.
seo-description: AEM Forms 6.5 bietet die Möglichkeit, Variablen im AEM Workflow zu erstellen. Mit dieser neuen Funktion ist die Verwendung des "Formulardatenmodelldienstes aufrufen"in AEM Arbeitsablauf sehr einfach geworden. Im folgenden Video werden Sie durch die Schritte geführt, die bei der Verwendung des Dienstes "Formulardatenmodell aufrufen"im Arbeitsablauf AEM.
feature: Workflow
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 2%

---


# Verwenden des Formulardatenmodelldienstes als Schritt in AEM 6.5-Arbeitsablauf {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 können wir jetzt den Formulardatenmodelldienst als Teil AEM Arbeitsablaufs verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodellschritts im Arbeitsablauf AEM Arbeitsablauf erforderlich sind

>!![NOTE]Die in diesem Video demonstrierte Funktionalität erfordert AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen

* Setup tomcat mit der Datei SampleRest.war, wie beschrieben [hier](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Die in Tomcat bereitgestellte Kriegsdatei hat den Code, um die Bonität des Antragstellers zurückzugeben.Die Bonitätsbewertung ist eine Zufallszahl zwischen 200 und 800

* [ Assets mit dem Package Manager in AEM importieren](assets/aem65-loanapplication.zip)
* Das Paket enthält die folgenden:

   * Workflow-Modell, das FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular zum Trigger des Workflows beim Senden.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie ab. Beim Senden des Formulars wird der [Darlehensanwendungs-Workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/invokefdm651.PNG).
Der Arbeitsablauf nutzt die Komponente &quot;Oder teilen&quot;, um die Anwendung an den Administrator weiterzuleiten, wenn die Bonität über 500 liegt. Wenn die Bonität unter 500 liegt, wird der Antrag zur Aushöhlung weitergeleitet.
