---
title: Einige nützliche Tipps und Tricks zur Benutzeroberfläche
description: Dokument zur Veranschaulichung einiger nützlicher Tipps zur Benutzeroberfläche
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '147'
ht-degree: 100%

---

# Kennwortfeldsichtbarkeit umschalten

Ein häufiger Anwendungsfall ist die Möglichkeit, die Sichtbarkeit des in das Kennwortfeld eingegebenen Textes umzuschalten.
Um diesen Anwendungsfall zu erreichen, habe ich das Augensymbol aus der [Font Awesome Library](https://fontawesome.com/) verwendet. Die erforderliche CSS und die eye.svg sind in der Client-Bibliothek enthalten, die für diese Demonstration erstellt wurde.


## Beispiel-Code

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

Die folgende CSS wurde verwendet, um das **Augensymbol** im Kennwortfeld zu positionieren

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

## Beispiel für die Implementierung der Kennwortumschaltung

* Laden Sie die [Client-Bibliothek](assets/simple-ui-tips.zip) herunter
* Laden Sie das [Beispielformular](assets/simple-ui-tricks-form.zip) herunter
* Importieren Sie die Client-Bibliothek mit der [Package Manager-Benutzeroberfläche](http://localhost:4502/crx/packmgr/index.jsp)
* Importieren Sie das Beispielformular mithilfe der [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Zeigen Sie das Formular in der Vorschau an](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


