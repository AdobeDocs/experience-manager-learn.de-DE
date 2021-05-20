---
title: Verwenden von Geolocation-APIs in Adaptive Forms
seo-title: Verwenden von Geolocation-APIs in Adaptive Forms
description: Füllen Sie Adressfelder in Ihrem Formular mithilfe der API für die Geolocation aus.
seo-description: Füllen Sie Adressfelder in Ihrem Formular mithilfe der API für die Geolocation aus.
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: Adaptive Formulare
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 4%

---


# Verwenden von Geolocation-APIs in Adaptive Forms{#using-geolocation-api-s-in-adaptive-forms}

Besuchen Sie die Seite [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) für einen Link zu einer Live-Demo dieser Funktion.

In diesem Artikel werden wir uns die Verwendung der Geolocation-API von Google ansehen, um Felder eines adaptiven Formulars auszufüllen. Dieser Anwendungsfall wird häufig verwendet, wenn Sie die aktuellen Adressfelder in einem Formular ausfüllen möchten.

Die folgenden Schritte wurden zur Verwendung der Geolocation-API in Adaptive Forms ausgeführt.

1. [Rufen Sie API-](https://developers.google.com/maps/documentation/javascript/get-api-key) Keywords von Google ab, um die Google Maps-Plattform zu verwenden. Sie erhalten einen Testschlüssel, der 1 Jahr gültig ist.

1. Das adaptive Formularfragment wurde mit Feldern für die aktuelle Adresse erstellt

1. Die Geolocation-API wurde beim Klicken-Ereignis des Bildobjekts des adaptiven Formulars aufgerufen

1. Die vom API-Aufruf zurückgegebenen JSON-Daten wurden analysiert und die Feldwerte für das adaptive Formular wurden entsprechend festgelegt.

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

![Felder werden mit der geoloaction-API ausgefüllt](assets/capture-4.gif)

In Zeile 1 verwenden wir die HTML Geolocation-API, um den aktuellen Speicherort abzurufen. Sobald der aktuelle Speicherort abgerufen wurde, übergeben wir den aktuellen Speicherort an die Funktion showPosition .

In der Funktion showPosition verwenden wir die Google-API, um die Adressdetails für den angegebenen Längen- und Breitengrad abzurufen.

Die von der API zurückgegebene JSON-Datei wird dann analysiert, um die Felder des adaptiven Formulars festzulegen.

>[!NOTE]
>
>Zu Testzwecken können Sie das HTTP-Protokoll mit localhost in der URL verwenden.
>
>Für den Produktionsserver müssen Sie SSL für Ihren AEM-Server aktivieren, um diese Funktion zu erhalten.
>
>Das mit diesem Artikel verknüpfte Beispiel wurde mit der US-Adresse getestet. Wenn Sie diese Funktion an anderen geografischen Standorten verwenden möchten, müssen Sie möglicherweise das JSON-Parsing anpassen.

Gehen Sie wie folgt vor, um diese Funktion auf Ihren Server zu übertragen:

* Installieren und starten Sie den AEM Forms-Server.

>!![NOTE] Diese Funktion wurde mit AEM Forms 6.3 und höher getestet.
* [Google API-Schlüssel abrufen](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importieren Sie die mit diesem Artikel verknüpften Assets in AEM.](assets/geolocationapi.zip)
* [Öffnen Sie das adaptive Formularfragment im Bearbeitungsmodus.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Öffnen Sie den Regeleditor für die Komponente Bildauswahl .
* Ersetzen Sie &lt;Ihr_api_key> durch den Google API-Schlüssel.
* Speichern Sie Ihre Änderungen.
* [Vorschau des Formulars](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled) anzeigen.
* Klicken Sie auf das Symbol &quot;Geolocation&quot;.
* Ihr Formular sollte mit Ihrem aktuellen Speicherort ausgefüllt werden.
