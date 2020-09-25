---
title: Funktionen und Code-Editor
seo-title: Funktionen und Code-Editor
description: Verwenden von Funktionen und Code-Editor zum Erstellen von Geschäftsregeln
seo-description: Verwenden von Funktionen und Code-Editor zum Erstellen von Geschäftsregeln
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Verwenden von benutzerdefinierten Funktionen und Code-Editor {#using-functions-and-code-editor}

In diesem Teil verwenden wir benutzerdefinierte Funktionen und den Code-Editor, um Geschäftsregeln zu erstellen.

Sie haben die [ClientLib bereits mit der benutzerdefinierten Funktion](assets/client-libs-and-logo.zip) installiert.

Normalerweise besteht eine Client-Bibliothek aus CSS- und JavaScript-Dateien. Diese Client-Bibliothek enthält die JavaScript-Datei, die eine Funktion zum Ausfüllen von Dropdown-Listen bereitstellt.


## Funktion zum Ausfüllen der Dropdown-Liste {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Übersichtstitel des Bedienfelds festlegen {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Überprüfungsbedienfeld {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

Im Folgenden finden Sie den Code, der zur Überprüfung von Bereichsfeldern verwendet wird

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

Sie können den Kommentar für Zeile 1 aufheben, um den Code im Browserfenster zu debuggen.

Zeile 4: Aktuelles Bedienfeld abrufen

Zeile 5: Bestätigen des aktuellen Bereichs.

Zeile 9: Wenn keine Fehler zum nächsten Bedienfeld verschoben werden

Vorschau des Formulars und Testen der neu aktivierten Funktionalität.
