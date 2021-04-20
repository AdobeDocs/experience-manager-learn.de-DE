---
title: Formulardatenmodelldienst als Schritt im Workflow verwenden
seo-title: Formulardatenmodelldienst als Schritt im Workflow verwenden
description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodells im Arbeitsablauf AEM erforderlich sind.
seo-description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodells im Arbeitsablauf AEM erforderlich sind.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# Verwenden des Formulardatenmodelldienstes als Schritt im Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodellschritts im Arbeitsablauf AEM Arbeitsablauf erforderlich sind


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen
* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das Metadateneigenschaften festlegt.
>!![NOTE]In AEM Forms 6.5 und höher ist diese Funktion standardmäßig verfügbar, wie hier  [beschrieben.](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Setup tomcat mit der Datei SampleRest.war, wie [hier](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html) beschrieben. Die in Tomcat bereitgestellte Kriegsdatei hat den Code, um die Bonität des Antragstellers zurückzugeben. Die Bonitätsbewertung ist eine Zufallszahl zwischen 200 und 800

* [Importieren Sie die Assets mit Package Manager](assets/invoke-fdm-as-service-step.zip) in AEM. Das Paket enthält Folgendes:

   * Workflow-Modell, das FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular zum Trigger des Workflows beim Senden.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie ab. Beim Senden des Formulars wird der [Darlehensanwendungs-Workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/fdm-as-service-step-workflow.PNG).
Der Arbeitsablauf nutzt die Komponente &quot;Oder teilen&quot;, um die Anwendung an den Administrator weiterzuleiten, wenn die Bonität über 500 liegt. Wenn die Bonität unter 500 liegt, wird der Antrag zur Auszahlung weitergeleitet
