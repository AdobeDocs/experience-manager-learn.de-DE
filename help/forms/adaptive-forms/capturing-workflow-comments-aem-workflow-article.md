---
title: Erfassen von Workflow-Kommentaren im adaptiven Forms Workflow
description: Erfassen von Workflow-Kommentaren in AEM Workflow
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---

# Erfassen von Workflow-Kommentaren im adaptiven Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Gilt nur für AEM Forms 6.4. Verwenden Sie in AEM Forms 6.5 die Variablenfunktion, um dieses Anwendungsbeispiel zu erreichen.]

Eine gängige Anfrage besteht darin, die vom Aufgabenvalidierer eingegebenen Kommentare in eine E-Mail einzuschließen. In AEM Forms 6.4 gibt es keinen nativen Mechanismus, um die vom Benutzer eingegebenen Kommentare zu erfassen und diese Kommentare in die E-Mail einzuschließen.

Um diese Anforderung zu erfüllen, wird ein OSGi-Beispielpaket bereitgestellt, das zum Erfassen von Kommentaren und zum Speichern dieser Kommentare als Workflow-Metadateneigenschaft verwendet werden kann.

Der folgende Screenshot zeigt Ihnen, wie Sie den Prozessschritt in [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) , um Kommentare zu erfassen und sie als Metadateneigenschaft zu speichern. &quot;Capture Workflow Comments&quot;ist der Name der Java-Klasse, die im Prozessschritt verwendet werden muss. Sie müssen den Metadaten-Eigenschaftsnamen übergeben, der die Kommentare enthält. Im folgenden Screenshot ist managerComments die Metadateneigenschaft, in der die Kommentare gespeichert werden.

![workflowcomments1](assets/workflowcomments1.gif)

Um diese Funktion auf Ihrem System zu testen, führen Sie die folgenden Schritte aus:
* [Stellen Sie sicher, dass der Prozessschritt im Workflow so konfiguriert ist, dass er die &quot;Capture Workflow Comments&quot;verwendet](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Bereitstellen des Entwicklungs-mit-Service-Benutzer-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue-Bundle bereitstellen](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dieses Bundle enthält den Beispielcode zum Erfassen der Kommentare und zum Speichern als Metadateneigenschaft

* [Laden Sie die Assets herunter und entpacken Sie sie, die mit diesem Artikel in in Verbindung stehen, in Ihr Dateisystem.](assets/capturecomments.zip) Die Assets enthalten Workflow-Modell und Beispiel-Adaptives Formular.

* Importieren Sie die zwei ZIP-Dateien in AEM mit Package Manager.

* [Zeigen Sie eine Vorschau des Formulars an, indem Sie zu dieser URL navigieren.](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Füllen Sie die Formularfelder aus und senden Sie das Formular.

* [Überprüfen des AEM-Posteingangs](http://localhost:4502/aem/inbox)

* Öffnen Sie die Aufgabe aus dem Posteingang und senden Sie das Formular. Geben Sie bei Aufforderung einige Kommentare ein.

Die Kommentare werden in der Metadateneigenschaft mit dem Namen `managerComments` in AEM Repository. Um nach den Kommentaren zu suchen, melden Sie sich bei crx als Administrator an. Die Workflow-Instanzen werden im folgenden Pfad gespeichert:

`/var/workflow/instances/server0`

Wählen Sie die entsprechende Workflow-Instanz aus und suchen Sie im Metadatenknoten nach der Eigenschaft managerComments .
