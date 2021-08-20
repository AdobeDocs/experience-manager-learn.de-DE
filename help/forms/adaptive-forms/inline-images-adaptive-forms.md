---
title: Anzeigen von Inline-Bildern in Adaptive Forms
description: Inline-Anzeige hochgeladener Bilder in Adaptive Forms
feature: Adaptive Formulare
topics: development
version: 6.3,6.4,6.5
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 1%

---


# Inline-Bilder in Adaptive Forms

Ein gängiger Anwendungsfall besteht darin, das hochgeladene Bild als Inline-Bild im adaptiven Formular anzuzeigen. Standardmäßig wird das hochgeladene Bild als Link angezeigt und dieses Erlebnis kann durch die Anzeige des Bildes im adaptiven Formular verbessert werden. Dieser Artikel führt Sie durch die Schritte, die für die Anzeige von Inline-Bildern erforderlich sind.

[Live-Beispiel dieser Funktion](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1)

## Platzhalterbild hinzufügen

Der erste Schritt besteht darin, der Dateianlagenkomponente ein Platzhalter-div vorzuhängen. Im folgenden Code wird die Dateianlagenkomponente durch den CSS-Klassennamen des Foto-Uploads identifiziert. Die JavaScript-Funktion ist Teil der Client-Bibliothek, die mit den adaptiven Formularen verknüpft ist. Diese Funktion wird im initialize-Ereignis der Dateianlagenkomponente aufgerufen.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Inline-Bild anzeigen

Nachdem der Benutzer das Bild hochgeladen hat, wird die unten aufgeführte Funktion im commit-Ereignis der Dateianlagenkomponente aufgerufen. Die Funktion empfängt das hochgeladene Dateiobjekt als Argument.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Auf Ihrem Server bereitstellen

* Laden Sie die [Client-Bibliothek](assets/inline-image-client-library.zip) mit AEM Paketmanager auf Ihre AEM-Instanz herunter und installieren Sie sie.
* Laden Sie das [Beispielformular](assets/inline-image-af.zip) mit AEM Paketmanager auf Ihre AEM-Instanz herunter und installieren Sie es.
* Zeigen Sie Ihren Browser auf [Inline-Bild hinzufügen](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Klicken Sie auf die Schaltfläche &quot;Foto anhängen&quot;, um ein Bild hinzuzufügen.