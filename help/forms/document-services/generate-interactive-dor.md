---
title: Interaktives Datensatzdokument mit adaptiven Formulardaten generieren
description: Zusammenführen adaptiver Formulardaten mit der XDP-Vorlage, um herunterladbare PDF-Dateien zu generieren
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: 2ed78bb8b122acbe69e98d63caee1115615d568f
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---


# Interaktives DoR herunterladen

Ein gängiges Nutzungsszenario besteht darin, ein interaktives DoR mit den adaptiven Formulardaten herunterzuladen. Das heruntergeladene DoR wird dann mit Adobe Acrobat oder Adobe Reader abgeschlossen.

Um dieses Anwendungsbeispiel zu erstellen, müssen wir die folgenden Schritte ausführen

## Beispieldaten für das XDP generieren

* Öffnen Sie die XDP in AEM Forms Designer.
* Datei auswählen | Formulareigenschaften | Vorschau
* Klicken Sie auf Vorschaudaten generieren .
* Klicken Sie auf Generieren
* Geben Sie einen aussagekräftigen Dateinamen an, z. B. &quot;form-data.xml&quot;

## XSD aus den XML-Daten generieren

Sie können jedes der kostenlosen Online-Tools verwenden, um [XSD generieren](https://www.freeformatter.com/xsd-generator.html) aus den XML-Daten, die im vorherigen Schritt generiert wurden.

## Adaptives Formular erstellen

Erstellen Sie ein adaptives Formular basierend auf der XSD aus dem vorherigen Schritt. Verknüpfen Sie das Formular mit der Client-Bibliothek &quot;irs&quot;. Diese Client-Bibliothek enthält den Code, um einen POST-Aufruf an das Servlet durchzuführen, der die PDF an die aufrufende Anwendung zurückgibt. Der folgende Code wird ausgelöst, wenn die _PDF herunterladen_ angeklickt wird

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Benutzerdefiniertes Servlet erstellen

Erstellen Sie ein benutzerdefiniertes Servlet, das die Daten mit der XDP-Vorlage zusammenführt und das PDF-Dokument zurückgibt. Der Code, um dies zu erreichen, ist unten aufgeführt. Das benutzerdefinierte Servlet ist Teil der [AEMFormsDocumentServices.core-1.0-SNAPSHOT Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

Im Beispielcode ist der Vorlagenname (f8918-r14e_redo-barcode_3 2.xdp) fest codiert. Sie können den Vorlagennamen einfach an das Servlet übergeben, damit dieser Code für alle Vorlagen verwendet werden kann.


## Bereitstellen des Beispiels auf Ihrem Server

Führen Sie die folgenden Schritte aus, um dies auf Ihrem lokalen Server zu testen:

1. [Herunterladen und Installieren des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Fügen Sie den folgenden Eintrag im Apache Sling Service User Mapper Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service hinzu.
1. [Herunterladen und Installieren des benutzerdefinierten Document Services-Bundles](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Dies verfügt über das Servlet, um die Daten mit der XDP-Vorlage zusammenzuführen und die PDF-Datei zurückzustreamen.
1. [Client-Bibliothek importieren](assets/irs.zip)
1. [Importieren des adaptiven Formulars](assets/f8918complete.zip)
1. [Importieren der XDP-Vorlage und des Schemas](assets/xdp-template-and-xsd.zip)
1. [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Füllen Sie einige Formularfelder aus.
1. Klicken Sie auf PDF herunterladen , um die PDF abzurufen.
