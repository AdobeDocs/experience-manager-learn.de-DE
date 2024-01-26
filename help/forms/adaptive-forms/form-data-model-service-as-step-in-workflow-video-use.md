---
title: Verwenden des Formulardatenmodell-Dienstes als Schritt im Workflow
description: Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil des AEM-Workflows zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodell-Schritts im AEM-Workflow erforderlich sind.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 220
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 100%

---

# Verwenden des Formulardatenmodell-Dienstes als Schritt im Workflow {#using-form-data-model-service-as-step-in-workflow}

Ab AEM Forms 6.4 haben wir jetzt die Möglichkeit, das Formulardatenmodell als Teil des AEM-Workflows zu verwenden. Im folgenden Video werden die Schritte erläutert, die zum Konfigurieren des Formulardatenmodell-Schritts im AEM-Workflow erforderlich sind


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Um dies auf Ihrem Server zu testen, folgen Sie den folgenden Anweisungen
* [Laden Sie das setvalue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Paket, das Metadaten-Eigenschaften festlegt.
>In AEM Forms 6.5 und höher ist diese Funktion [wie hier beschrieben](form-data-model-service-as-step-in-aem65-workflow-video-use.md) vorkonfiguriert

* Einrichten von Tomcat mit der Datei „SampleRest.war“, wie [hier](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=de) beschrieben. Die in Tomcat bereitgestellte WAR-Datei hat den Code, um die Kreditwürdigkeit des Antragstellers zurückzugeben. Die Kreditwürdigkeit ist eine Zufallszahl zwischen 200 und 800

* [Importieren Sie die Assets mithilfe von Package Manager in AEM](assets/invoke-fdm-as-service-step.zip). Das Paket enthält Folgendes:

   * Workflow-Modell, das den FDM-Schritt nutzt.
   * Formulardatenmodell, das im FDM-Schritt verwendet wird.
   * Adaptives Formular, um den Workflow bei der Übermittlung auszulösen.
* Öffnen Sie [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Füllen Sie die Details aus und senden Sie es. Bei der Übermittlung des Formulars wird [loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) ausgelöst.

![ Workflow ](assets/fdm-as-service-step-workflow.PNG).
Der Workflow nutzt die Komponente „ODER-Teilung“, um die Anwendung an Admins weiterzuleiten, wenn die Kreditwürdigkeit über 500 liegt. Wenn der Bonitätswert unter 500 liegt, wird der Antrag an Cavery weitergeleitet
