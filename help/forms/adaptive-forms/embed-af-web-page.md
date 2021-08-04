---
title: Einbetten von adaptiven Forms-/HTML5-Formularen in Webseiten
description: Konfigurationsschritte, die zum Einbetten von adaptiven Forms- oder HTML5-Formularen in eine AEM Web-Seite erforderlich sind.
feature: Adaptive Formulare
feature-set: Adaptive Forms
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
kt: 8390
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 28%

---


# Einbetten des adaptiven Formulars oder HTML5-Formulars in eine Webseite

Das eingebettete adaptive Formular ist voll funktionsfähig und Benutzer können es ausfüllen und senden, ohne die Seite zu verlassen. Es hilft Benutzern, im Kontext anderer Elemente auf der Webseite zu bleiben und gleichzeitig mit dem Formular zu interagieren.

Im folgenden Video werden die Schritte erläutert, die zum Einbetten eines adaptiven oder HTML5-Formulars in eine Webseite erforderlich sind.
Informationen zu Best Practices, Best Practices usw. finden Sie in der [Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) .
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Sie können die im Video [verwendeten Beispieldateien hier](assets/embedding-af-web-page.zip) herunterladen.

Im Folgenden finden Sie den Code, mit dem das adaptive Formular abgerufen und in den Container eingebettet wird, der durch den Klassennamen **right** identifiziert wird.

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














