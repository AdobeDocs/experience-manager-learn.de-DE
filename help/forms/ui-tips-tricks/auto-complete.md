---
title: Funktion zur automatischen Vervollständigung in AEM Forms
description: Ermöglicht es Benutzern, beim Eingeben schnell aus einer vorausgefüllten Liste von Werten zu suchen und diese auszuwählen, indem sie Such- und Filterfunktionen nutzen.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Implementierung der automatischen Vervollständigung

Implementieren Sie die Funktion für die automatische Vervollständigung in AEM Formulare mithilfe der Funktion &quot;Automatische Vervollständigung&quot;von jquery.
Das in diesem Artikel enthaltene Beispiel verwendet eine Vielzahl von Datenquellen (statisches Array, dynamisches Array, das aus einer REST-API-Antwort gefüllt wird), um die Vorschläge zu füllen, wenn der Benutzer mit der Eingabe in das Textfeld beginnt.

Der Code, der zum Ausführen der Funktion für die automatische Vervollständigung verwendet wird, ist mit dem initialize-Ereignis des Felds verknüpft.


## Vorschläge für den Ländernamen angeben

![country-recommendations](assets/auto-complete1.png)

## Vorschlag für Adresse

![country-recommendations](assets/auto-complete2.png)

Im Folgenden finden Sie den Code, mit dem Straßenadressenvorschläge bereitgestellt werden

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```

## Vorschläge mit Emoji&#39;s

![country-recommendations](assets/auto-complete3.png)

Der folgende Code wurde verwendet, um Emoji in der Liste mit Vorschlägen anzuzeigen

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

Die [Beispielformular kann heruntergeladen werden](assets/auto-complete-form.zip) von hier aus. Stellen Sie sicher, dass Sie mithilfe des Code-Editors Ihren eigenen Benutzernamen/API-Schlüssel für den Code angeben, damit der REST-Aufruf erfolgreich ist.

>[!NOTE]
>
> Damit die automatische Verarbeitung funktioniert, stellen Sie sicher, dass Ihr Formular die folgende Client-Bibliothek verwendet **cq.jquery.ui**. Diese Client-Bibliothek ist mit AEM ausgestattet.
