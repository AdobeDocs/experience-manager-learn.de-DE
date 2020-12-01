---
title: AEM Workflow bei der Übermittlung des HTML5-Formulars auslösen
seo-title: AEM Workflow bei der Übermittlung von HTML5-Formularen auslösen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie ein mobiles Formular, um AEM Workflow auszulösen
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie ein mobiles Formular, um AEM Workflow auszulösen
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---


# Benutzerdefiniertes Profil erstellen

In diesem Teil erstellen wir ein [benutzerdefiniertes Profil.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Ein Profil ist für die Wiedergabe der XDP als HTML verantwortlich. Für die Wiedergabe von XDP-Dateien als HTML wird standardmäßig ein Profil bereitgestellt. Es stellt eine benutzerdefinierte Version des Mobile Forms Rendition-Dienstes dar. Mit dem Mobile Form Rendition-Dienst können Sie Erscheinungsbild, Verhalten und Interaktionen des Mobile Forms anpassen. In unserem benutzerspezifischen Profil erfassen wir die Daten, die im Mobile Form mit der guidebridge API ausgefüllt werden. Diese Daten werden dann an ein benutzerdefiniertes Servlet gesendet, das dann eine interaktive PDF generiert und an die aufrufende Anwendung zurückgesendet wird.

Rufen Sie die Formulardaten mit der JavaScript-API ab. `formBridge` Wir verwenden die `getDataXML()`-Methode:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

In der Erfolgshandler-Methode führen wir einen Aufruf an das benutzerdefinierte Servlet durch, das in AEM ausgeführt wird. Dieses Servlet gibt interaktive PDF-Dateien mit den Daten aus dem mobilen Formular aus und zurück

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

## Interaktive PDF generieren

Im Folgenden finden Sie den Servlet-Code, der für die Wiedergabe interaktiver PDF und die Rückgabe der PDF an die aufrufende Anwendung verantwortlich ist. Das Servlet ruft die Methode `mobileFormToInteractivePdf` des benutzerdefinierten DocumentServices OSGi-Dienstes auf.

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

### Interaktive PDF rendern

Im folgenden Code wird die [Forms-Dienst-API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) verwendet, um interaktive PDF-Dateien mit den Daten aus dem mobilen Formular wiederzugeben.

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

Zur Ansicht der Möglichkeit, interaktive PDF-Dateien von teilweise ausgefüllten mobilen Formularen herunterzuladen, klicken Sie [hier](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Nach dem Herunterladen der PDF-Datei müssen Sie die PDF-Datei senden, um einen AEM Workflow auszulösen. In diesem Arbeitsablauf werden die Daten aus der gesendeten PDF zusammengeführt und nicht interaktive PDF zur Überprüfung generiert.

Das für diesen Anwendungsfall erstellte benutzerdefinierte Profil steht als Teil dieser Übungselemente zur Verfügung.
