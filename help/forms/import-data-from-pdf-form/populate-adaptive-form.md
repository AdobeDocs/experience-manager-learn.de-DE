---
title: Füllen des adaptiven Formulars mit der Methode setData
description: Senden Sie die hochgeladene PDF-Datei zur Datenextraktion und füllen Sie das adaptive Formular mit den extrahierten Daten
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 4%

---

# Ajax-Aufruf

Wenn der Benutzer die PDF-Datei hochgeladen hat, müssen wir einen POST-Aufruf an ein Servlet durchführen und das hochgeladene PDF-Dokument in der POST-Anfrage übergeben. Die POST-Anfrage gibt einen Pfad zu den exportierten Daten im CRX-Repository zurück

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

Das Servlet, das auf **_/bin/ExtractDataFromPDF_** extrahiert die Daten aus der PDF-Datei und gibt den Pfad des CRX-Knotens zurück, in dem die extrahierten Daten gespeichert werden.
Die [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) -Methode verwendet wird, um die Daten des adaptiven Formulars festzulegen.

## Nächste Schritte

[Bereitstellen der Beispiel-Assets](./test-the-solution.md)


