---
title: Verwenden des Formulardatenmodelldienstes als Schritt im Workflow
description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts "Formulardatenmodell"in AEM Workflow erforderlich sind.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Verwenden des Formulardatenmodelldienstes als Schritt im Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil AEM Arbeitsablaufs zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Schritts &quot;Formulardatenmodell&quot;in AEM Workflow erforderlich sind


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen
* [Herunterladen und Bereitstellen des setvalue-Bundles](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das Metadateneigenschaften festlegt.
>!![NOTE]In AEM Forms 6.5 und höher ist diese Funktion standardmäßig als [hier beschreiben](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Einrichten von Tomcat mit der Datei SampleRest.war wie beschrieben [here](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Die in Tomcat bereitgestellte WAR-Datei hat den Code, um die Kreditwürdigkeit des Antragstellers zurückzugeben. Die Bonität ist eine Zufallszahl zwischen 200 und 800

* [Importieren von Assets in AEM mit Package Manager](assets/invoke-fdm-as-service-step.zip).Das Paket enthält Folgendes:

   * Workflow-Modell, das den FDM-Schritt verwendet.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung Trigger.
* Öffnen Sie die [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie sie. Bei der Übermittlung des Formulars [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst wird.

![ Workflow ](assets/fdm-as-service-step-workflow.PNG).
Der Workflow nutzt die Komponente ODER-Teilung , um die Anwendung an den Administrator weiterzuleiten, wenn die Gewichtung über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag zur Aufnahme weitergeleitet.
