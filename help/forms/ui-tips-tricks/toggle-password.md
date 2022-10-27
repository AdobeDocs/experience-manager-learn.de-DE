---
title: Tipps und Tricks zur Benutzeroberfläche
description: Dokument zur Veranschaulichung nützlicher Tipps zur Benutzeroberfläche
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 8%

---

# Sichtbarkeit des Kennwortfelds ein/aus

Ein gängiger Anwendungsfall besteht darin, den Ausfüllern des Formulars die Anzeige des im Kennwortfeld eingegebenen Texts zu ermöglichen.
Um diesen Anwendungsfall zu erreichen, habe ich das Augensymbol von [Font Awesome Library](https://fontawesome.com/). Die erforderliche CSS und die eye.svg sind in der Client-Bibliothek enthalten, die für diese Demonstration erstellt wurde.



## Beispielcode

Das adaptive Formular verfügt über ein Feld vom Typ PasswordBox namens **ssnField**.

Der folgende Code wird ausgeführt, wenn das Formular geladen wird

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

Die folgende CSS wurde verwendet, um die **Auge** Symbol im Kennwortfeld

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Beispiel für Umschalter-Kennwort bereitstellen

* Laden Sie die [Client-Bibliothek](assets/simple-ui-tips.zip)
* Laden Sie die [Beispielformular](assets/simple-ui-tricks-form.zip)
* Importieren Sie die Client-Bibliothek mit der [Package Manager-Benutzeroberfläche](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie das Beispielformular mit dem [Forms und Dokument](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Nutzen Sie die Funktion zur Formularvorschau.](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
