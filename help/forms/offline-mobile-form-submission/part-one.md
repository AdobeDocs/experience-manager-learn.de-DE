---
title: Auslösen des AEM-Workflows beim Übermitteln von HTML5-Formularen – Erstellen eines benutzerdefinierten Profils
description: Fahren Sie mit dem Ausfüllen des Mobile-Formulars im Offline-Modus fort und übermitteln Sie das Mobile-Formular, um den AEM-Workflow auszulösen.
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 151
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 100%

---

# Erstellen eines benutzerdefinierten Profils

In diesem Teil erstellen wir ein [benutzerdefiniertes Profil.](https://helpx.adobe.com/de/livecycle/help/mobile-forms/creating-profile.html) Ein Profil ist für das Rendern der XDP als HTML verantwortlich. Ein vorkonfiguriertes Standardprofil wird bereitgestellt, um XDPs als HTML zu rendern. Es stellt eine angepasste Version des Ausgabedarstellungsdienstes für Mobile-Formulare dar. Sie können den Ausgabedarstellungsdienst für Mobile-Formulare verwenden, um Erscheinungsbild, Verhalten und Interaktionen von Mobile-Formularen anzupassen. In unserem benutzerdefinierten Profil erfassen wir die im Mobile-Formular ausgefüllten Daten mithilfe der GuideBridge-API. Diese Daten werden dann an ein benutzerdefiniertes Servlet gesendet, das daraufhin eine interaktive PDF generiert und diese an die aufrufende Anwendung streamt.

Rufen Sie die Formulardaten mit der `formBridge`-JavaScript-API ab. Wir nutzen die Methode `getDataXML()`:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

In der Erfolgs-Handler-Methode rufen wir ein benutzerdefiniertes Servlet auf, das in AEM ausgeführt wird. Dieses Servlet rendert interaktive PDF-Dateien mit den Daten aus dem Mobile-Formular und gibt sie zurück

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Generieren einer interaktiven PDF

Im Folgenden finden Sie den Servlet-Code, der für das Rendern interaktiver PDF-Dateien und die Rückgabe des PDF-Dokuments an die aufrufende Anwendung verantwortlich ist. Das Servlet ruft die Methode `mobileFormToInteractivePdf` des benutzerdefinierten DocumentServices-OSGi-Dienstes auf.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
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
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### Rendern einer interaktiven PDF

Der folgende Code verwendet die [Forms-Service-API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html), um interaktive PDFs mit den Daten aus dem Mobile-Formular zu rendern.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Um die Möglichkeit zum Herunterladen einer interaktiven PDF von teilweise ausgefüllten Mobile-Formularen anzusehen, [klicken Sie hier](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Nach dem Herunterladen der PDF besteht der nächste Schritt darin, die PDF zu übermitteln, um einen AEM-Workflow auszulösen. Dieser Workflow führt die Daten aus der gesendeten PDF zusammen und generiert eine nicht-interaktive PDF zur Überprüfung.

Das für diesen Anwendungsfall erstellte benutzerdefinierte Profil ist als Teil dieses Tutorials verfügbar.
