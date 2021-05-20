---
title: Verwenden von Funktionen und Code-Editor
seo-title: Verwenden von Funktionen und Code-Editor
description: Verwenden von Funktionen und Code-Editor zum Erstellen von Geschäftsregeln
seo-description: Verwenden von Funktionen und Code-Editor zum Erstellen von Geschäftsregeln
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: Adaptive Formulare
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---


# Verwenden benutzerdefinierter Funktionen und des Code-Editors {#using-functions-and-code-editor}

In diesem Teil verwenden wir benutzerdefinierte Funktionen und den Code-Editor, um Geschäftsregeln zu erstellen.

haben Sie die [ClientLib mit der benutzerdefinierten Funktion](assets/client-libs-and-logo.zip) bereits zuvor in diesem Tutorial installiert.

Normalerweise besteht eine Client-Bibliothek aus CSS- und JavaScript-Dateien. Diese Client-Bibliothek enthält eine JavaScript-Datei, die eine Funktion zum Ausfüllen von Dropdown-Listenwerten verfügbar macht.


## Funktion zum Ausfüllen der Dropdown-Liste {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Zusammenfassungstitel des Bedienfelds festlegen {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Validierungsfenster {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

Im Folgenden finden Sie den Code, der zum Überprüfen von Bereichsfeldern verwendet wird

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

Sie können die Auskommentierung von Zeile 1 aufheben, um den Code im Browserfenster zu debuggen.

Zeile 4 - Aktuelles Bedienfeld abrufen

Zeile 5 - Überprüfen des aktuellen Bedienfelds.

Zeile 9: Wenn keine Fehler zum nächsten Bereich wechseln

Zeigen Sie eine Vorschau des Formulars an und testen Sie die neu aktivierte Funktion.
