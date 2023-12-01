---
title: Senden von E-Mails mit SendGrid
description: Auslösen einer E-Mail mit einem Link zum gespeicherten Formular
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 100%

---

# Senden einer E-Mail

Nachdem die Formulardaten in Azure Blob Storage gespeichert wurden, wird eine E-Mail mit einem Link zum gespeicherten Formular an die Benutzenden gesendet. Diese E-Mail wird mit der SendGrid-REST-API gesendet.

Die Swagger-Datei, das Formulardatenmodell und die Cloud Services-Konfiguration, die zum Senden von E-Mails erforderlich sind, werden Ihnen als Teil der Artikel-Assets bereitgestellt.

Sie müssen eine dynamische Vorlage für ein SendGrid-Konto erstellen, die im adaptiven Formular erfasste Daten erfassen kann.


Im Folgenden finden Sie das HTML-Code-Snippet, das in der dynamischen Vorlage verwendet wird. Der Wert des blobID-Parameters wird mithilfe des Dienstes zum Aufrufen von Formulardatenmodellen übergeben.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


