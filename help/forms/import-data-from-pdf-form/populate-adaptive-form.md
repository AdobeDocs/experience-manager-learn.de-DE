---
title: Ausfüllen eines adaptiven Formulars mit der Methode „setData“
description: Senden der hochgeladenen PDF-Datei zur Datenextraktion und Ausfüllen des adaptiven Formulars mit den extrahierten Daten
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 33
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 100%

---

# Durchführen eines Ajax-Aufrufs

Wenn die Benutzerin bzw. der Benutzer die PDF-Datei hochgeladen hat, müssen wir einen POST-Aufruf an ein Servlet durchführen und das hochgeladene PDF-Dokument in der POST-Anfrage übergeben. Die POST-Anfrage gibt einen Pfad zu den exportierten Daten im CRX-Repository zurück.

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

Das auf **_/bin/ExtractDataFromPDF_** aufgesetzte Servlet extrahiert die Daten aus der PDF-Datei und gibt den Pfad des CRX-Knotens zurück, in dem die extrahierten Daten gespeichert werden.
Die Methode [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) wird dann verwendet, um die Daten des adaptiven Formulars festzulegen.

## Nächste Schritte

[Bereitstellen der Beispiel-Assets](./test-the-solution.md)
