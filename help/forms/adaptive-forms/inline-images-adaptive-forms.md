---
title: Anzeigen von Inline-Bildern in Adaptive Forms
description: Inline-Anzeige hochgeladener Bilder in Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Inline-Bilder in Adaptive Forms

Ein gängiger Anwendungsfall besteht darin, das hochgeladene Bild als Inline-Bild im adaptiven Formular anzuzeigen. Standardmäßig wird das hochgeladene Bild als Link angezeigt und dieses Erlebnis kann durch die Anzeige des Bildes im adaptiven Formular verbessert werden. Dieser Artikel führt Sie durch die Schritte, die für die Anzeige von Inline-Bildern erforderlich sind.

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

* Laden Sie die [Client-Bibliothek](assets/inline-image-client-library.zip) auf Ihrer AEM-Instanz mithilfe AEM Package Manager.
* Laden Sie die [Beispielformular](assets/inline-image-af.zip) auf Ihrer AEM Instanz mithilfe AEM Package Manager.
* Zeigen Sie Ihren Browser auf [Inline-Bild hinzufügen](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Klicken Sie auf die Schaltfläche &quot;Foto anhängen&quot;, um ein Bild hinzuzufügen.
