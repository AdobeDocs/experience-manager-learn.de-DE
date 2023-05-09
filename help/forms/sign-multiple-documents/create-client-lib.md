---
title: Client-Bibliothek erstellen
description: Client-Bibliotheks-Code zum Abrufen des nächsten zu signierenden Formulars
feature: Adaptive Forms
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# Client-Bibliothek erstellen

Erstellen Sie eine benutzerdefinierte Client-Bibliothek, kurz clientlib , um die URL-Parameter zu extrahieren, die diese Parameter im GET-Aufruf übergeben. Der GET-Aufruf wird an ein Servlet gesendet, das auf /bin/getnextformtosign bereitgestellt wird. Dadurch wird die URL des nächsten Formulars zurückgegeben, das im Paket signiert werden soll.

Im Folgenden finden Sie den Code, der in der clientlib-JavaScript-Funktion verwendet wird


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[Die clientlib kann von hier heruntergeladen werden](assets/get-next-form-client-lib.zip)

## Nächste Schritte

[Benutzerdefinierte Formularvorlage für diesen Anwendungsfall erstellen](./create-af-template.md)