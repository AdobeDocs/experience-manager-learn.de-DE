---
title: Trigger AEM Workflow beim Übermitteln von HTML5-Formularen - Erstellen eines benutzerdefinierten Profils
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offline-Modus fort und senden Sie das mobile Formular an den Trigger AEM Workflow
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Benutzerdefiniertes Profil erstellen

In diesem Teil erstellen wir eine [benutzerdefiniertes Profil.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Ein Profil ist für die Wiedergabe der XDP als HTML verantwortlich. Ein Standardprofil wird standardmäßig bereitgestellt, um XDPs als HTML zu rendern. Es stellt eine angepasste Version des Mobile Forms Rendition-Diensts dar. Sie können den Mobile Form Rendition-Dienst verwenden, um Erscheinungsbild, Verhalten und Interaktionen von Mobile Forms anzupassen. In unserem benutzerdefinierten Profil erfassen wir die im Mobile-Formular ausgefüllten Daten mithilfe der guidebridge-API. Diese Daten werden dann an ein benutzerdefiniertes Servlet gesendet, das dann eine interaktive PDF generiert und an die aufrufende Anwendung streamt.

Rufen Sie die Formulardaten mit dem `formBridge` JavaScript-API. Wir nutzen die `getDataXML()` -Methode:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

In der Erfolgshandler-Methode rufen wir ein benutzerdefiniertes Servlet auf, das in AEM ausgeführt wird. Dieses Servlet gibt interaktive PDF-Dateien mit den Daten aus dem mobilen Formular wieder und gibt sie zurück

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

Im Folgenden finden Sie den Servlet-Code, der für das Rendern interaktiver PDF-Dateien und die Rückgabe des PDF-Dokuments an die aufrufende Anwendung verantwortlich ist. Das Servlet ruft `mobileFormToInteractivePdf` -Methode des benutzerdefinierten DocumentServices-OSGi-Dienstes.

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

Der folgende Code verwendet die [Forms-Dienst-API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) um interaktive PDF mit den Daten aus dem mobilen Formular zu rendern.

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

So zeigen Sie die Möglichkeit zum Herunterladen interaktiver PDF von teilweise ausgefülltem Mobilformular an: [Bitte klicken Sie hier](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Nach dem Herunterladen der PDF besteht der nächste Schritt darin, die PDF an Trigger und einen AEM Workflow zu senden. Dieser Workflow führt die Daten aus der gesendeten PDF zusammen und generiert eine nicht interaktive PDF zur Überprüfung.

Das für diesen Anwendungsfall erstellte benutzerdefinierte Profil ist als Teil dieses Tutorials verfügbar.
