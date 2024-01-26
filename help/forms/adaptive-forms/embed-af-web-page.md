---
title: Einbetten von adaptiven Formularen/HTML 5-Formularen in einer Web-Seite
description: Konfigurationsschritte, die zum Einbetten von adaptiven Formularen oder HTML5-Formularen in eine Nicht-AEM-Web-Seite erforderlich sind.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 404
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Einbetten des adaptiven Formulars oder HTML5-Formulars in eine Web-Seite

Das eingebettete adaptive Formular ist voll funktionsfähig, und Benutzende können es ausfüllen und senden, ohne die Seite zu verlassen. Es hilft Benutzenden, im Kontext anderer Elemente auf der Web-Seite zu bleiben und gleichzeitig mit dem Formular zu interagieren.

Im folgenden Video werden die Schritte erläutert, die zum Einbetten eines adaptiven oder HTML5-Formulars in eine Web-Seite erforderlich sind.
Bitte beachten Sie die [Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=de) für die besten Voraussetzungen, Best Practices usw.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Sie können die im Video verwendeten Beispieldateien [hier herunterladen](assets/embedding-af-web-page.zip)

Der folgende Code wird verwendet, um das adaptive Formular abzurufen und das Formular in den Container einzubetten, der durch den Klassennamen **right** gekennzeichnet ist:

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
