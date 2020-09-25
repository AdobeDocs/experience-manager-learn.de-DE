---
title: Anzeigen von Inline-Bildern im adaptiven Forms
seo-title: Anzeigen von Inline-Bildern im adaptiven Forms
description: Hochgeladene Bilder inline im adaptiven Forms anzeigen
seo-description: Hochgeladene Bilder inline im adaptiven Forms anzeigen
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Inline-Bilder im adaptiven Forms

In der Regel wird das hochgeladene Bild als Inline-Bild im adaptiven Formular angezeigt. Standardmäßig wird das hochgeladene Bild als Link angezeigt. Dieses Erlebnis kann durch die Anzeige des Bildes im adaptiven Formular verbessert werden. In diesem Artikel werden Sie durch die Schritte zur Anzeige des Inline-Bildes geführt.

## hinzufügen Platzhalterbild

Der erste Schritt besteht darin, der Dateianlagenkomponente ein Platzhalterdiv vorzustellen. Im Code unter der Dateianlagenkomponente wird der CSS-Klassenname des Foto-Uploads angegeben. Die JavaScript-Funktion ist Teil der Client-Bibliothek, die mit den adaptiven Formularen verknüpft ist. Diese Funktion wird im initialize-Ereignis der Dateianlagenkomponente aufgerufen.

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

### Auf dem Server bereitstellen

* Laden Sie die [Clientbibliothek](assets/inline-image-client-library.zip) mit AEM Package Manager auf Ihre AEM-Instanz herunter und installieren Sie sie.
* Laden Sie das [Musterformular](assets/inline-image-af.zip) mit AEM Package Manager auf Ihre AEM-Instanz herunter und installieren Sie es.
* Verweisen Sie Ihren Browser auf [Hinzufügen Inline-Bild](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Klicken Sie auf die Schaltfläche &quot;Foto anhängen&quot;, um ein Bild hinzuzufügen.