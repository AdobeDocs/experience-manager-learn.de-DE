---
title: Anzeigen von Inline-Bildern in Adaptive Forms
description: Inline-Anzeige hochgeladener Bilder in Adaptive Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 64
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 100%

---

# Inline-Bilder in Adaptive Forms

Ein gängiger Anwendungsfall besteht darin, das hochgeladene Bild als Inline-Bild im adaptiven Formular anzuzeigen. Standardmäßig wird das hochgeladene Bild als Link angezeigt und dieses Erlebnis kann durch die Anzeige des Bildes im adaptiven Formular verbessert werden. Dieser Artikel führt Sie durch die Schritte, die für die Anzeige von Inline-Bildern erforderlich sind.

## Hinzufügen eines Platzhalterbildes

Der erste Schritt besteht darin, der Dateianlagenkomponente ein Platzhalter-Div vorzuhängen. Im folgenden Code wird die Dateianlagenkomponente durch den CSS-Klassennamen des Foto-Uploads identifiziert. Die JavaScript-Funktion ist Teil der Client-Bibliothek, die mit den adaptiven Formularen verknüpft ist. Diese Funktion wird im Ereignis „initialize“ der Dateianlagenkomponente aufgerufen.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Anzeigen eines Inline-Bildes

Nachdem die Benutzenden das Bild hochgeladen haben, wird die unten aufgeführte Funktion im Commit-Ereignis der Dateianlagenkomponente aufgerufen. Die Funktion empfängt das hochgeladene Dateiobjekt als Argument.

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

### Bereitstellen auf Ihrem Server

* Laden Sie die [Client-Bibliothek](assets/inline-image-client-library.zip) herunter und installieren Sie sie auf Ihrer AEM-Instanz mithilfe von AEM Package Manager.
* Laden Sie das [Beispielformular](assets/inline-image-af.zip) herunter und installieren Sie es auf Ihrer AEM-Instanz mithilfe von AEM Package Manager.
* Lassen Sie Ihren Browser auf [Inline-Bild hinzufügen](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled) verweisen
* Klicken Sie auf die Schaltfläche „Foto anhängen“, um ein Bild hinzuzufügen.
