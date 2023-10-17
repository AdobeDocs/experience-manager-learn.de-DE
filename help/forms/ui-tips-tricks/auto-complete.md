---
title: Funktion zur automatischen Vervollständigung in AEM Forms
description: Ermöglicht es Benutzenden, beim Eingeben schnell aus einer vorausgefüllten Liste von Werten zu suchen und diese auszuwählen, indem sie Such- und Filterfunktionen nutzen.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '177'
ht-degree: 100%

---

# Implementierung der automatischen Vervollständigung

Implementieren Sie die Funktion für die automatische Vervollständigung in AEM-Formularen mithilfe der Funktion zur automatischen Vervollständigung von jQuery.
Das in diesem Artikel enthaltene Beispiel verwendet eine Vielzahl von Datenquellen (statisches Array, dynamisches Array, das aus einer REST-API-Antwort gefüllt wird), um die Vorschläge zu füllen, wenn die Benutzerin bzw. der Benutzer mit der Eingabe in das Textfeld beginnt.

Der Code, der zum Ausführen der Funktion für die automatische Vervollständigung verwendet wird, ist mit dem Ereignis „initialize“ des Feldes verknüpft.

## Vorschlag für die Adresse

![country-suggestions](assets/auto-complete2.png)



Der folgende Code wird verwendet, um Vorschläge für Straßenadressen bereitzustellen

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

![country-suggestions](assets/auto-complete3.png)

Der folgende Code wurde verwendet, um Emoji in der Liste mit Vorschlägen anzuzeigen:

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

Das [Beispielformular kann hier heruntergeladen werden](assets/auto-complete-form.zip). Stellen Sie sicher, dass Sie mithilfe des Code-Editors Ihren eigenen Benutzernamen/API-Schlüssel für den Code angeben, damit der REST-Aufruf erfolgreich ist.

>[!NOTE]
>
> Damit die automatische Verarbeitung funktioniert, stellen Sie sicher, dass Ihr Formular die folgende Client-Bibliothek verwendet **cq.jquery.ui**. Diese Client-Bibliothek ist mit AEM ausgestattet.
