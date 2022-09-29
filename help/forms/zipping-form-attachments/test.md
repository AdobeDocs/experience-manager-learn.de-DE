---
title: Testen der Lösung - Schritte, die zum Testen der beiden Ansätze erforderlich sind
description: Testen Sie die Lösung, indem Sie dem Formular Anhänge hinzufügen und den Workflow zum Senden der E-Mail an den Trigger übergeben.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 7%

---

# Schritte zum Testen der beiden Ansätze

* Konfigurieren [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) zum Senden von E-Mails vom AEM Forms-Server
* Stellen Sie die [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) Bundle mit [Felix-Webkonsole](http://localhost:4502/system/console/bundles)

## Senden einer ZIP-Datei als E-Mail-Anhang



* Stellen Sie die [Workflow SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Dieser Workflow verwendet die Komponente &quot;E-Mail senden&quot;, um die Datei &quot;zipped_attachments.zip&quot;zu senden, die vom benutzerdefinierten Prozessschritt im Payload-Ordner gespeichert wird. Konfigurieren Sie die E-Mail-Adressen der Absender und Empfänger entsprechend Ihren Anforderungen.
* Importieren Sie die [Beispielformular](assets/zip-form-attachments-form.zip) von [Benutzeroberfläche von Forms und Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) und fügen Sie einige Anlagen hinzu und senden Sie das Formular.
* Der Workflow sollte ausgelöst und eine E-Mail-Benachrichtigung mit der ZIP-Datei gesendet werden.

## Senden von Formularanlagen als einzelne Dateien

* Stellen Sie die [Workflow SendForm .](assets/send-form-attachments-model.zip) Dieser Workflow verwendet die Komponente E-Mail senden , um die Formularanhänge als einzelne Dateien zu senden. Konfigurieren Sie die E-Mail-Adresse der Absender und Empfänger entsprechend Ihren Anforderungen.
* Importieren Sie die [Beispielformular](assets/send-list-attachments-form.zip) von [Benutzeroberfläche von Forms und Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Formularvorschau](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) und fügen Sie einige Anlagen hinzu und senden Sie das Formular.
* Der Workflow sollte ausgelöst und eine E-Mail-Benachrichtigung mit den Formularanlagen gesendet werden.
