---
title: Erfassen von Workflow-Kommentaren im Workflow für adaptive Formulare
description: Erfassen von Workflow-Kommentaren im AEM-Workflow
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '375'
ht-degree: 100%

---

# Erfassen von Workflow-Kommentaren im Workflow für adaptive Formulare{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Gilt nur für AEM Forms 6.4. In AEM Forms 6.5 verwenden Sie bitte die Variablenfunktion, um diesen Anwendungsfall zu erreichen]

Ein häufiger Wunsch ist die Möglichkeit, die vom Aufgabenprüfenden eingegebenen Kommentare in eine E-Mail aufzunehmen. In AEM Forms 6.4 gibt es keinen standardmäßigen Mechanismus zum Erfassen der von Benutzenden eingegebenen Kommentare und zum Einfügen dieser Kommentare in E-Mails.

Um diese Anforderung zu erfüllen, wird ein OSGi-Beispielpaket bereitgestellt, das zum Erfassen von Kommentaren und zum Speichern dieser Kommentare als Workflow-Metadateneigenschaft verwendet werden kann.

Der folgende Screenshot zeigt Ihnen, wie Sie den Prozessschritt im [AEM-Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) verwenden, um Kommentare zu erfassen und als Metadateneigenschaft zu speichern. „Capture Workflow Comments“ ist der Name der Java-Klasse, die im Prozessschritt verwendet werden muss. Sie müssen den Metadaten-Eigenschaftsnamen übergeben, der die Kommentare enthält. Im folgenden Screenshot ist „managerComments“ die Metadateneigenschaft, in der die Kommentare gespeichert werden.

![workflowcomments1](assets/workflowcomments1.gif)

Um diese Funktion auf Ihrem System zu testen, führen Sie die folgenden Schritte aus:
* [Stellen Sie sicher, dass der Prozessschritt im Workflow so konfiguriert ist, dass er „Capture Workflow Comments“ verwendet](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Stellen Sie das Bundle „DevelopingWithServiceUser“ bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Stellen Sie das Bundle „SetValue“ bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dieses Bundle enthält den Beispiel-Code zum Erfassen der Kommentare und zum Speichern als Metadateneigenschaft

* [Laden Sie die Assets zu diesem Artikel herunter und entpacken Sie sie in Ihr Dateisystem](assets/capturecomments.zip) Die Assets enthalten ein Workflow-Modell und ein Beispiel für ein adaptives Formular.

* Importieren Sie die zwei ZIP-Dateien in AEM mithilfe des Package Manager.

* [Sehen Sie sich das Formular an, indem Sie diese URL aufrufen](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Füllen Sie die Formularfelder aus und senden Sie das Formular ab.

* [Überprüfen Sie Ihren AEM-Posteingang](http://localhost:4502/aem/inbox)

* Öffnen Sie die Aufgabe aus dem Posteingang und senden Sie das Formular ab. Geben Sie bei Aufforderung einige Kommentare ein.

Die Kommentare werden in der Metadateneigenschaft `managerComments` im AEM-Repository gespeichert. Um nach den Kommentaren zu suchen, melden Sie sich bei CRX als Admin an. Die Workflow-Instanzen werden im folgenden Pfad gespeichert:

`/var/workflow/instances/server0`

Wählen Sie die entsprechende Workflow-Instanz aus und suchen Sie im Metadatenknoten nach der Eigenschaft „managerComments“.
