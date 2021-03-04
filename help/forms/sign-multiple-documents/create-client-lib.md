---
title: Client-Bibliothek erstellen
description: Client-Bibliothekscode zum Abrufen des nächsten zu signierenden Formulars
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '86'
ht-degree: 5%

---

# Client-Bibliothek erstellen

Erstellen Sie eine benutzerdefinierte Client-Bibliothek (kurz clientlib), um die URL-Parameter zu extrahieren, die im GET-Aufruf übergeben werden. Der GET-Aufruf wird an ein Servlet gesendet, das auf &quot;/bin/getnextformtosign&quot;gemountet ist und die URL des nächsten Formulars zurückgibt, das im Paket signiert werden soll.

Der folgende Code wird in der JavaScript-Funktion clientlib verwendet


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

[Die clientlib können Sie hier herunterladen](assets/get-next-form-client-lib.zip)
