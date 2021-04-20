---
title: Barcode-Dienst mit adaptivem Forms
seo-title: Barcode-Dienst mit adaptivem Forms
description: Barcode-Dienst zum Dekodieren von Barcodes und Ausfüllen von Formularfeldern aus den extrahierten Daten
seo-description: Barcode-Dienst zum Dekodieren von Barcodes und Ausfüllen von Formularfeldern aus den extrahierten Daten
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# Barcode-Dienst mit adaptivem Forms{#barcode-service-with-adaptive-forms}

In diesem Artikel wird die Verwendung des Barcode-Dienstes zum Ausfüllen des adaptiven Formulars erläutert. Der Verwendungsfall lautet wie folgt:

1. Der Benutzer fügt PDF mit Barcode als Anhang für ein adaptives Formular hinzu.
1. Der Pfad der Anlage wird an das Servlet gesendet
1. Das Servlet hat den Barcode dekodiert und gibt die Daten im JSON-Format zurück
1. Das adaptive Formular wird dann mit den dekodierten Daten ausgefüllt

Der folgende Code dekodiert den Barcode und füllt ein JSON-Objekt mit den dekodierten Werten. Das Servlet gibt dann das JSON-Objekt in seiner Antwort auf die aufrufende Anwendung zurück.

Sie können diese Funktion live sehen, besuchen Sie bitte das Portal [samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) und suchen Sie nach der Barcode Service Demo

```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

Im Folgenden finden Sie den Servlet-Code. Dieses Servlet wird aufgerufen, wenn der Benutzer dem adaptiven Formular eine Anlage hinzufügt. Das Servlet gibt das JSON-Objekt zurück zur aufrufenden Anwendung. Die aufrufende Anwendung füllt dann das adaptive Formular mit den Werten, die aus dem JSON-Objekt extrahiert wurden.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }

 }

}
```

Der folgende Code ist Teil der Client-Bibliothek, auf die vom adaptiven Formular verwiesen wird. Wenn ein Benutzer die Anlage zum adaptiven Formular hinzufügt, wird dieser Code ausgelöst. Der Code führt einen GET-Aufruf an das Servlet mit dem Pfad der Anlage durch, der im Anforderungsparameter übergeben wird. Die vom Servlet-Aufruf empfangenen Daten werden dann zum Ausfüllen des adaptiven Formulars verwendet.

```
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>Das in diesem Paket enthaltene adaptive Formular wurde mit AEM Forms 6.4 erstellt. Wenn Sie dieses Paket in der AEM Forms 6.3 Umgebung verwenden möchten, erstellen Sie das adaptive Formular in AEM 6.3

Zeile 12 - Benutzerdefinierter Code zum Abrufen des Dienstauflösers. Dieses Bundle ist Teil dieser Artikel-Assets.

Zeile 23 - Aufruf der DocumentServices-Methode &quot;extractBarCode&quot;, um das JSON-Objekt mit dekodierten Daten zu füllen

Gehen Sie wie folgt vor, um dieses System auf Ihrem System laufen zu lassen

1. [Laden Sie BarcodeService.](assets/barcodeservice.zip) zipand herunter und importieren Sie mit dem Package Manager in AEM
1. [Herunterladen und Installieren des benutzerdefinierten DocumentServices-Bundles](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Herunterladen und Installieren des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [PDF-Musterformular herunterladen](assets/barcode.pdf)
1. Verweisen Sie Ihren Browser auf das adaptive Beispielformular [](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Hochladen der bereitgestellten Musterdatei
1. Sie sollten die Formulare sehen, die mit den Daten gefüllt sind

