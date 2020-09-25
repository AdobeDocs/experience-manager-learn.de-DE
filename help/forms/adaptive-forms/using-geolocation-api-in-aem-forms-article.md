---
title: Verwenden von Geolocation-APIs in adaptiven Forms
seo-title: Verwenden von Geolocation-APIs in adaptiven Forms
description: Ausfüllen von Adressfeldern in Ihrem Formular mithilfe der API
seo-description: Ausfüllen von Adressfeldern in Ihrem Formular mithilfe der API
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 3%

---


# Verwenden von Geolocation-APIs in adaptiven Forms{#using-geolocation-api-s-in-adaptive-forms}

Besuchen Sie die [AEM Forms-Beispielseite](https://forms.enablementadobe.com/content/samples/samples.html?query=0) für einen Link zu einer Live-Demo dieser Funktion.

In diesem Artikel betrachten wir die Google Geolocation API, um Felder eines adaptiven Formulars zu füllen. Dieser Anwendungsfall wird häufig verwendet, wenn Sie die aktuellen Adressfelder in einem Formular ausfüllen möchten.

Die folgenden Schritte wurden zur Verwendung der Geolocation API in Adaptive Forms durchgeführt.

1. [Verwenden Sie den API-Schlüssel](https://developers.google.com/maps/documentation/javascript/get-api-key) von Google, um die Google Maps-Plattform zu verwenden. Sie erhalten einen Testschlüssel, der 1 Jahr gültig ist.

1. Das adaptive Formularfragment wurde mit Feldern erstellt, die die aktuelle Adresse enthalten.

1. Die Geolocation-API wurde auf dem click-Ereignis des Bildobjekts des adaptiven Formulars aufgerufen

1. Die vom API-Aufruf zurückgegebenen JSON-Daten wurden analysiert und die Feldwerte für adaptive Formulare wurden entsprechend festgelegt.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![Felder mit geoloaction api füllen](assets/capture-4.gif)

In Zeile 1 verwenden wir die HTML Geolocation API wird verwendet, um den aktuellen Ort abzurufen. Sobald die aktuelle Position abgerufen wurde, übergeben wir die aktuelle Position an die Funktion showPosition.

In der Funktion showPosition verwenden wir die Google API, um die Adressdetails für die angegebene Breiten- und Längengrad abzurufen.

Das von der API zurückgegebene JSON wird dann analysiert, um die Felder für das adaptive Formular festzulegen.

>[!NOTE]
>
>Zu Testzwecken können Sie das HTTP-Protokoll mit localhost in der URL verwenden.
>
>Für den Produktionsserver müssen Sie SSL aktivieren, damit Ihr AEM Server diese Funktion nutzen kann.
>
>Das mit diesem Artikel verknüpfte Muster wurde mit der US-Adresse getestet. Wenn Sie diese Funktion an anderen geografischen Orten verwenden möchten, müssen Sie die JSON-Analyse möglicherweise anpassen.

Gehen Sie wie folgt vor, um diese Funktion auf Ihren Server zu laden

* Installieren und Beginn AEM Forms Server.

>!![NOTE] Diese Funktion wurde unter AEM Forms 6.3 und höher getestet.
* [Google API-Schlüssel](https://developers.google.com/maps/documentation/javascript/get-api-key)abrufen.
* [Importieren Sie die Assets, die sich auf diesen Artikel beziehen, in AEM.](assets/geolocationapi.zip)
* [Öffnen Sie das adaptive Formularfragment im Bearbeitungsmodus.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Öffnen Sie den Regeleditor für die Komponente &quot;Bildauswahl&quot;.
* Ersetzen Sie &lt;your_api_key> durch den Google API-Schlüssel.
* Speichern Sie Ihre Änderungen.
* [Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Klicken Sie auf das Symbol &quot;Geolocation&quot;.
* Ihr Formular sollte mit Ihrem aktuellen Speicherort ausgefüllt werden.
