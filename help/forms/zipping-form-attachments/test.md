---
title: Testen der Lösung – Erforderliche Schritte zum Testen der beiden Ansätze
description: Testen Sie die Lösung, indem Sie dem Formular Anhänge hinzufügen und den Workflow zum E-Mai-Versand auslösen.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 53
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 100%

---

# Erforderliche Schritte zum Testen der beiden Ansätze

* Konfigurieren des [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=de#configuring-the-mail-service) zum Senden von E-Mails vom AEM Forms-Server
* Bereitstellen des [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)-Bundles mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles)

## Senden einer ZIP-Datei als E-Mail-Anhang



* Stellen Sie den [SendFormAttachmentsViaEmail-Workflow](assets/zipped-form-attachments-model.zip) bereit. Dieser Workflow verwendet die Komponente „E-Mail senden“, um die Datei „zipped_attachments.zip“ zu senden, die im Rahmen des benutzerdefinierten Prozessschritts im Payload-Ordner gespeichert wird. Konfigurieren Sie die Absender- und Empfänger-E-Mail-Adressen entsprechend Ihren Anforderungen.
* Importieren Sie das [Beispielformular](assets/zip-form-attachments-form.zip) über die Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled), fügen Sie einige Anhänge hinzu und übermitteln Sie das Formular.
* Der Workflow sollte ausgelöst und eine E-Mail-Benachrichtigung mit der ZIP-Datei gesendet werden.

## Senden von Formularanhängen als einzelne Dateien

* Stellen Sie den [SendForm-Workflow](assets/send-form-attachments-model.zip) bereit. Dieser Workflow verwendet die Komponente „E-Mail senden“, um die Formularanhänge als einzelne Dateien zu senden. Konfigurieren Sie die Absender- und Empfänger-E-Mail-Adressen entsprechend Ihren Anforderungen.
* Importieren Sie das [Beispielformular](assets/send-list-attachments-form.zip) über die Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled), fügen Sie einige Anlagen hinzu und übermitteln Sie das Formular.
* Der Workflow sollte ausgelöst und eine E-Mail-Benachrichtigung mit den Formularanlagen gesendet werden.
