---
title: Anzeige von DAM-Bilder-Inline in adaptiven Formularen
description: Anzeigen von DAM-Bilder-Inline in adaptiven Formularen
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 92
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# Anzeigen von DAM-Bildern in adaptiven Formularen

Ein gängiger Anwendungsfall besteht darin, die Bilder im CRX-Repository-Inline in einem adaptiven Formular anzuzeigen.

## Hinzufügen eines Platzhalterbildes

Der erste Schritt besteht darin, der Bedienfeldkomponente ein Platzhalter-Div vorzuhängen. Im folgenden Code wird die Bedienfeldkomponente durch ihren CSS-Klassennamen „photo-upload“ identifiziert. Die JavaScript-Funktion ist Teil der Client-Bibliothek, die mit den adaptiven Formularen verknüpft ist. Diese Funktion wird im Ereignis „initialize“ der Dateianlagenkomponente aufgerufen.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Anzeigen eines Inline-Bildes

Nachdem die Benutzerin bzw. der Benutzer das Bild ausgewählt hat, wird das ausgeblendete Feld „ImageName“ mit dem ausgewählten Bildnamen ausgefüllt. Dieser Bildname wird dann an die Funktion „damURLToFile“ übergeben, die die Funktion „createFile“ aufruft, um eine URL in einen Blob für „FileReader.readAsDataURL()“ zu konvertieren.

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Bereitstellen auf Ihrem Server

* Laden Sie die [Client-Bibliothek und die Beispielbilder](assets/InlineDAMImage.zip) herunter und installieren Sie sie mit dem AEM Package Manager auf Ihrer AEM-Instanz.
* Laden Sie das [Beispielformular](assets/FieldInspectionForm.zip) herunter und installieren Sie es mit dem AEM Package Manager in Ihrer AEM-Instanz.
* Richten Sie Ihren Browser auf [FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Wählen Sie eine der Vorrichtungen
* Das Bild sollte im Formular angezeigt werden
