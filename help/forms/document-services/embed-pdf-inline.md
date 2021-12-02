---
title: 'Datensatzdokument inline anzeigen '
description: Zusammenführen von adaptiven Formulardaten mit der XDP-Vorlage und Anzeigen der PDF inline mithilfe der Document Cloud-Einbettungs-PDF-API.
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9411
source-git-commit: 7f9a7951b2d9bb780d5374f17bb289c38b2e2ae7
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 2%

---


# DoR inline anzeigen

Ein gängiger Anwendungsfall besteht darin, ein PDF-Dokument mit den vom Benutzer eingegebenen Daten anzuzeigen.

Um dieses Anwendungsbeispiel zu erstellen, haben wir die Funktion [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Die folgenden Schritte wurden ausgeführt, um die Integration abzuschließen

## Erstellen Sie eine benutzerdefinierte Komponente, um die PDF inline anzuzeigen

Eine benutzerdefinierte Komponente (embed-pdf) wurde erstellt, um das vom POST-Aufruf zurückgegebene PDF-Dokument einzubetten.

## Client-Bibliothek

Der folgende Code wird ausgeführt, wenn die Variable `viewPDF` auf das Kontrollkästchen klicken. Wir übergeben die Daten des adaptiven Formulars, den Vorlagennamen an den Endpunkt, um das PDF zu generieren. Das generierte PDF-Dokument wird dann dem Benutzer mit der JavaScript-Bibliothek &quot;pdf einbetten&quot;angezeigt.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## Beispieldaten für das XDP generieren

* Öffnen Sie die XDP in AEM Forms Designer.
* Datei auswählen | Formulareigenschaften | Vorschau
* Klicken Sie auf Vorschaudaten generieren .
* Klicken Sie auf Generieren
* Geben Sie einen aussagekräftigen Dateinamen an, z. B. &quot;form-data.xml&quot;

## XSD aus den XML-Daten generieren

Sie können jedes der kostenlosen Online-Tools verwenden, um [XSD generieren](https://www.freeformatter.com/xsd-generator.html) aus den XML-Daten, die im vorherigen Schritt generiert wurden.

## Vorlage hochladen

Stellen Sie sicher, dass Sie die xdp-Vorlage in [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) über die Schaltfläche &quot;Erstellen&quot;


## Adaptives Formular erstellen

Erstellen Sie ein adaptives Formular basierend auf der XSD aus dem vorherigen Schritt.
Fügen Sie dem adaptiven Formular eine neue Registerkarte hinzu. Fügen Sie dieser Registerkarte eine Checkbox-Komponente und eine Einbettungs-PDF-Komponente hinzu. Vergewissern Sie sich, dass Sie das Kontrollkästchen viewPDF nennen.
Konfigurieren Sie die Komponente &quot;embed-pdf&quot;, wie im Screenshot unten dargestellt
![embed-pdf](assets/embed-pdf-configuration.png)

**Einbetten des PDF-API-Schlüssels** - Dies ist der Schlüssel, den Sie zum Einbetten des PDF-Dokuments verwenden können. Dieser Schlüssel funktioniert nur mit localhost. Sie können [eigenen Schlüssel](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) und ordnen Sie sie einer anderen Domäne zu.

**Endpunkt, der pdf zurückgibt** - Dies ist das benutzerdefinierte Servlet, das die Daten mit der XDP-Vorlage zusammenführt und das PDF-Dokument zurückgibt.

**Vorlagenname** - Dies ist der Pfad zur xdp. In der Regel wird sie im Ordner &quot;formsanddocuments&quot;gespeichert.

**PDF-Dateiname** - Dies ist die Zeichenfolge, die in der Einbettungs-PDF-Komponente angezeigt wird.

## Benutzerdefiniertes Servlet erstellen

Ein benutzerdefiniertes Servlet wurde erstellt, um die Daten mit der XDP-Vorlage zusammenzuführen und das PDF-Dokument zurückzugeben. Der Code, um dies zu erreichen, ist unten aufgeführt. Das benutzerdefinierte Servlet ist Teil der [embedpdf bundle](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
         response.setContentType("application/pdf");
         response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
         response.setContentLength((int) fileInputStream.available());
         OutputStream responseOutputStream = response.getOutputStream();
         int bytes;
         while ((bytes = fileInputStream.read()) != -1) {
            responseOutputStream.write(bytes);
         }
         responseOutputStream.flush();
         responseOutputStream.close();

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## Bereitstellen des Beispiels auf Ihrem Server

Führen Sie die folgenden Schritte aus, um dies auf Ihrem lokalen Server zu testen:

1. [Herunterladen und Installieren des embedpdf-Bundles](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Dies verfügt über das Servlet, um die Daten mit der XDP-Vorlage zusammenzuführen und die PDF-Datei zurückzustreamen.
1. Fügen Sie den Pfad /bin/getPDFToEmbed im Abschnitt &quot;Ausgeschlossene Pfade&quot;der Adobe Granite CSRF Filter mit dem [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). In Ihrer Produktionsumgebung wird die Verwendung der [CSRF-Schutzrahmen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [Client-Bibliothek und benutzerdefinierte Komponente importieren](assets/embed-pdf.zip)
1. [Importieren des adaptiven Formulars und der Vorlage](assets/embed-pdf-form-and-xdp.zip)
1. [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Füllen Sie einige Formularfelder aus.
1. Registerkarte zur Registerkarte PDF anzeigen . Aktivieren Sie das Kontrollkästchen PDF-Ansicht . Im Formular sollte ein PDF-Dokument angezeigt werden, das mit den Daten des adaptiven Formulars gefüllt ist.
