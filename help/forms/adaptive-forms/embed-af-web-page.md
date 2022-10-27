---
title: Einbetten von adaptiven Forms/HTML 5-Formularen in eine Webseite
description: Konfigurationsschritte, die zum Einbetten von adaptiven Forms- oder HTML5-Formularen in eine AEM Web-Seite erforderlich sind.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 27%

---

# Einbetten des adaptiven Formulars oder HTML5-Formulars in eine Webseite

Das eingebettete adaptive Formular ist voll funktionsfähig und Benutzer können es ausfüllen und senden, ohne die Seite zu verlassen. Es hilft Benutzern, im Kontext anderer Elemente auf der Webseite zu bleiben und gleichzeitig mit dem Formular zu interagieren.

Im folgenden Video werden die Schritte erläutert, die zum Einbetten eines adaptiven oder HTML5-Formulars in eine Webseite erforderlich sind.
Weitere Informationen finden Sie unter [Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) für Best Practices, Best Practices usw.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Sie können die im Video verwendeten Beispieldateien herunterladen [von hier](assets/embedding-af-web-page.zip)

Im Folgenden finden Sie den Code, mit dem das adaptive Formular abgerufen und in den Container eingebettet wird, der durch den Klassennamen identifiziert wird. **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
