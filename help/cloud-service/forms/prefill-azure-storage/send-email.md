---
title: E-Mail mit SendGrid senden
description: Trigger einer E-Mail mit einem Link zum gespeicherten Formular
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 8474
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# E-Mail senden

Nachdem die Formulardaten im Azure Blob Storage gespeichert wurden, wird eine E-Mail mit einem Link zum gespeicherten Formular an den Benutzer gesendet. Diese E-Mail wird mit der SendGrid REST API gesendet.

Die Swagger-Datei, das Formulardatenmodell und die Cloud Services-Konfiguration, die zum Senden von E-Mails erforderlich sind, werden Ihnen als Teil der Artikel-Assets bereitgestellt.

Sie müssen ein SendGrid-Konto mit der dynamischen Vorlage erstellen, mit der im adaptiven Formular erfasste Daten erfasst werden können.


Im Folgenden finden Sie den Codeausschnitt von HTML-Code, der in der dynamischen Vorlage verwendet wird. Der Wert des Parameters blobID wird mithilfe des Aufrufdienstes für das Formulardatenmodell übergeben.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


