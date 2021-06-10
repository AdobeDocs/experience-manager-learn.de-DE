---
title: Testen der Lösung
description: Testen Sie die Lösung, indem Sie dem Formular Anhänge hinzufügen und den Workflow zum Senden der E-Mail an den Trigger übergeben.
sub-product: Formulare
feature: Workflow
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 9%

---


# Testen der Lösung


* Konfigurieren Sie [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service), um E-Mails vom AEM Forms-Server zu senden.
* Stellen Sie das Bundle [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) mithilfe der [felix-Webkonsole](http://localhost:4502/system/console/bundles) bereit.
* Stellen Sie den Workflow [SendFormAttachmentsViaEmail bereit.](assets/zipped-form-attachments-model.zip) Dieser Workflow verwendet die Komponente &quot;E-Mail senden&quot;, um die Datei &quot;zipped_attachments.zip&quot;zu senden, die vom benutzerdefinierten Prozessschritt im Payload-Ordner gespeichert wird. Konfigurieren Sie die E-Mail-Adresse der Absender und Empfänger entsprechend Ihren Anforderungen.
* Importieren Sie [Beispielformular](assets/zip-form-attachments-form.zip) aus [Forms und Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Zeigen Sie eine Vorschau des ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) Formulars an, fügen Sie einige Anhänge hinzu und senden Sie das Formular.
* Der Workflow sollte ausgelöst und eine E-Mail-Benachrichtigung mit der ZIP-Datei gesendet werden.

