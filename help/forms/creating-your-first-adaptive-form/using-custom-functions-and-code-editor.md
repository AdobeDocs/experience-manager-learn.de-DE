---
title: Verwenden von Funktionen und Code-Editor
description: Verwenden von Funktionen und Code-Editor zum Erstellen von Geschäftsregeln
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 100%

---

# Verwenden benutzerdefinierter Funktionen und des Code-Editors {#using-functions-and-code-editor}

In diesem Teil verwenden wir benutzerdefinierte Funktionen und den Code-Editor, um Geschäftsregeln zu erstellen.

Sie haben die [ClientLib mit benutzerdefinierter Funktion](assets/client-libs-and-logo.zip) bereits früher in diesem Tutorial installiert.

Normalerweise besteht eine Client-Bibliothek aus CSS- und JavaScript-Dateien. Diese Client-Bibliothek enthält eine JavaScript-Datei, die eine Funktion zum Ausfüllen von Dropdown-Listenwerten verfügbar macht.


## Funktion zum Ausfüllen von Dropdown-Listen {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Festlegen des Zusammenfassungstitels des Bedienfelds {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Überprüfen des Bedienfelds {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

Im Folgenden finden Sie den Code, der zum Überprüfen von Feldern im Bedienfeld verwendet wird:

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

Sie können die Auskommentierung von Zeile 1 aufheben, um den Code im Browser-Fenster zu debuggen.

Zeile 4 – Abrufen des aktuellen Bedienfelds

Zeile 5 – Überprüfen des aktuellen Bedienfelds

Zeile 9 – Wechseln zum nächsten Bedienfeld, wenn keine Fehler vorhanden

Zeigen Sie eine Vorschau des Formulars an und testen Sie die neu aktivierte Funktion.
