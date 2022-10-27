---
title: Anzeigen von Inline-Bildern in Adaptive Forms
description: Inline-Anzeige hochgeladener Bilder in Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# DAM-Bild in Adaptive Forms anzeigen

Ein gängiger Anwendungsfall besteht darin, die Bilder im CRX-Repository inline in einem adaptiven Formular anzuzeigen.

## Platzhalterbild hinzufügen

Der erste Schritt besteht darin, der Bedienfeldkomponente ein Platzhalter-div vorzuhängen. Im Code unter wird die Bedienfeldkomponente durch den CSS-Klassennamen des Foto-Uploads identifiziert. Die JavaScript-Funktion ist Teil der Client-Bibliothek, die mit den adaptiven Formularen verknüpft ist. Diese Funktion wird im initialize-Ereignis der Dateianlagenkomponente aufgerufen.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Inline-Bild anzeigen

Nachdem der Benutzer das Bild ausgewählt hat, wird das ausgeblendete Feld ImageName mit dem ausgewählten Bildnamen ausgefüllt. Dieser Bildname wird dann an die Funktion damURLToFile übergeben, die die Funktion createFile aufruft, um eine URL in einen Blob für FileReader.readAsDataURL() zu konvertieren.

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

### Auf Ihrem Server bereitstellen

* Laden Sie die [Client-Bibliothek und Beispielbilder](assets/InlineDAMImage.zip) auf Ihrer AEM-Instanz mit AEM Package Manager.
* Laden Sie die [Beispielformular](assets/FieldInspectionForm.zip) auf Ihrer AEM Instanz mithilfe AEM Package Manager.
* Zeigen Sie Ihren Browser auf [FileInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Wählen Sie eine der Einstellungen aus
* Das Bild sollte im Formular angezeigt werden