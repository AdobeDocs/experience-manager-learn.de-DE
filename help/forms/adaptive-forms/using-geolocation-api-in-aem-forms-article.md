---
title: Verwenden der Geolocation API in adaptiven Formularen
description: Ausfüllen der Adressfelder in Ihrem Formular mithilfe der Geolocation API
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '390'
ht-degree: 100%

---

# Verwenden der Geolocation API in adaptiven Formularen{#using-geolocation-api-s-in-adaptive-forms}

In diesem Artikel werden wir uns die Verwendung der Geolocation API von Google ansehen, um Felder eines adaptiven Formulars auszufüllen. Dieser Anwendungsfall wird häufig verwendet, wenn Sie die aktuellen Adressfelder in einem Formular ausfüllen möchten.

Die folgenden Schritte wurden zur Verwendung der Geolocation API in adaptiven Formularen ausgeführt.

1. [Rufen Sie den API-Schlüssel von Google ab](https://developers.google.com/maps/documentation/javascript/get-api-key), um die Google Maps-Plattform zu verwenden. Sie können einen Testschlüssel beziehen, der 1 Jahr gültig ist.

1. Das adaptive Formularfragment wurde mit Feldern für die aktuelle Adresse erstellt.

1. Die Geolocation API wurde beim Klick-Ereignis des Bildobjekts des adaptiven Formulars aufgerufen.

1. Die vom API-Aufruf zurückgegebenen JSON-Daten wurden analysiert und die Feldwerte für das adaptive Formular entsprechend festgelegt.

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

![Ausfüllen von Feldern mit der Geoloaction API](assets/capture-4.gif)

In Zeile 1 verwenden wir die HTML Geolocation API, um den aktuellen Standort zu ermitteln. Sobald der aktuelle Standort ermittelt wurde, geben wir diese Information an die showPosition-Funktion weiter.

In der showPosition-Funktion verwenden wir die Google-API, um die Adressdetails für den angegebenen Längen- und Breitengrad abzurufen.

Die von der API zurückgegebene JSON-Daten werden dann analysiert, um die Felder des adaptiven Formulars festzulegen.

>[!NOTE]
>
>Zu Testzwecken können Sie das HTTP-Protokoll mit „localhost“ in der URL verwenden.
>
>Für den Produktions-Server müssen Sie SSL für Ihren AEM-Server aktivieren, um diese Funktion nutzen zu können.
>
>Das mit diesem Artikel verbundene Beispiel wurde mit einer US-Adresse getestet. Wenn Sie diese Funktion an anderen geografischen Standorten verwenden möchten, müssen Sie möglicherweise das JSON-Parsen anpassen.

Gehen Sie wie folgt vor, um diese Funktion auf Ihrem Server bereitzustellen:

* Installieren und starten Sie den AEM Forms-Server.
> Diese Funktion wurde mit AEM Forms 6.3 und höher getestet.
* [Rufen Sie den Google-API-Schlüssel ab](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importieren Sie die mit diesem Artikel verbundenen Assets in AEM.](assets/geolocationapi.zip)
* [Öffnen Sie das adaptive Formularfragment im Bearbeitungsmodus.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Öffnen Sie den Regeleditor für die Komponente „Bildauswahl“.
* Ersetzen Sie „&lt;your_api_key>“ durch den Google-API-Schlüssel.
* Speichern Sie Ihre Änderungen.
* [Zeigen Sie das Formular in einer Vorschau an](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Klicken Sie auf das Symbol „Geolocation“.
* Ihr Formular sollte mit Ihrem aktuellen Standort ausgefüllt werden.
