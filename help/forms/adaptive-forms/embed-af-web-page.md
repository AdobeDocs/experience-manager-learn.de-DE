---
title: Einbetten von adaptiven Forms-/HTML5-Formularen in Webseiten
description: Konfigurationsschritte, die zum Einbetten von adaptiven Forms- oder HTML5-Formularen in eine AEM Web-Seite erforderlich sind.
feature: Adaptive Formulare
type: Tutorial
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: 8390
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
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














