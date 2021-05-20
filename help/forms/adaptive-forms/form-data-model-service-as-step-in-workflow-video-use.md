---
title: Verwenden des Formulardatenmodelldienstes als Schritt im Workflow
seo-title: Verwenden des Formulardatenmodelldienstes als Schritt im Workflow
description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts "Formulardatenmodell"in AEM Workflow erforderlich sind.
seo-description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts "Formulardatenmodell"in AEM Workflow erforderlich sind.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '328'
ht-degree: 0%

---


# Verwenden des Formulardatenmodelldienstes als Schritt in Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts &quot;Formulardatenmodell&quot;in AEM Workflow erforderlich sind


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen
* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das Metadateneigenschaften festlegt.
>!![NOTE]In AEM Forms 6.5 und höher ist diese Funktion standardmäßig verfügbar, wie hier  [beschrieben.](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Richten Sie tomcat mit der Datei SampleRest.war ein, wie [hier](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html) beschrieben. Die in Tomcat bereitgestellte WAR-Datei hat den Code, um die Kreditwürdigkeit des Antragstellers zurückzugeben. Die Bonität ist eine Zufallszahl zwischen 200 und 800

* [Importieren Sie die Assets mit Package Manager](assets/invoke-fdm-as-service-step.zip) in AEM. Das Paket enthält Folgendes:

   * Workflow-Modell, das den FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung Trigger.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie. Bei der Formularübermittlung wird der [loanapplication-Workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/fdm-as-service-step-workflow.PNG).
Der Workflow nutzt die Komponente ODER-Teilung , um die Anwendung an den Administrator weiterzuleiten, wenn die Gewichtung über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag zur Aufnahme weitergeleitet.
