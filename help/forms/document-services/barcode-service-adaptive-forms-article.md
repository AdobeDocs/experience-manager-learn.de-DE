---
title: Barcode-Dienst mit adaptiven Formularen
description: Verwenden des Barcode-Dienstes, um den Barcode zu decodieren und Formularfelder aus den extrahierten Daten auszufüllen.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 169
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 100%

---

# Barcode-Dienst mit adaptiven Formularen{#barcode-service-with-adaptive-forms}

In diesem Artikel wird die Verwendung des Barcode-Dienstes zum Ausfüllen des adaptiven Formulars veranschaulicht. Der Anwendungsfall sieht folgendermaßen aus:

1. Die Benutzerin bzw. der Benutzer fügt PDF mit einem Barcode als Anhang des adaptiven Formulars hinzu
1. Der Pfad des Anhangs wird an das Servlet gesendet
1. Das Servlet hat den Barcode decodiert und gibt die Daten im JSON-Format zurück
1. Das adaptive Formular wird dann mit den decodierten Daten ausgefüllt

Der folgende Code decodiert den Barcode und füllt ein JSON-Objekt mit den decodierten Werten. Das Servlet gibt dann das JSON-Objekt in seiner Antwort auf die aufrufende Anwendung zurück.



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

Im Folgenden finden Sie den Servlet-Code. Dieses Servlet wird aufgerufen, wenn die Benutzerin bzw. der Benutzer dem adaptiven Formular eine Anlage hinzufügt. Das Servlet gibt das JSON-Objekt an die aufrufende Anwendung zurück. Die aufrufende Anwendung füllt dann das adaptive Formular mit den Werten, die aus dem JSON-Objekt extrahiert wurden.

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

Der folgende Code ist Teil der Client-Bibliothek, auf die vom adaptiven Formular verwiesen wird. Wenn eine Benutzerin bzw. ein Benutzer die Anlage zum adaptiven Formular hinzufügt, wird dieser Code ausgelöst. Der Code führt einen GET-Aufruf an das Servlet durch, wobei der Pfad der Anlage im Anfrageparameter übergeben wird. Die vom Servlet-Aufruf empfangenen Daten werden dann zum Ausfüllen des adaptiven Formulars verwendet.

```javascript
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
>Das in diesem Paket enthaltene adaptive Formular wurde mit AEM Forms 6.4 erstellt. Wenn Sie dieses Paket in der AEM Forms 6.3-Umgebung verwenden möchten, erstellen Sie das adaptive Formular in AEM Form 6.3

Zeile 12: Benutzerdefinierter Code zum Abrufen des Service-Resolvers. Dieses Bundle ist Teil der Assets dieses Artikels.

Zeile 23: Rufen Sie die Methode „DocumentServices extractBarCode“ auf, um das JSON-Objekt mit decodierten Daten auszufüllen.

Führen Sie die folgenden Schritte aus, um dies auf Ihrem System durchzuführen:

1. [Laden Sie BarcodeService.zip herunter](assets/barcodeservice.zip) und importieren Sie es mit Package Manager in AEM
1. [Laden Sie das Custom DocumentServices-Bundle herunter und installieren Sie es](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Laden Sie das DevelopingWithServiceUser-Bundle herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Laden Sie das PDF-Musterformular herunter](assets/barcode.pdf)
1. Rufen Sie mit Ihrem Browser das [Beispiel für ein adaptives Formular](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled) auf
1. Laden Sie das PDF-Musterformular hoch
1. Sie sollten die Formulare sehen, die mit den Daten gefüllt sind
