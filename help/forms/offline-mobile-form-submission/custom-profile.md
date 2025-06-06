---
title: Auslösen des AEM-Workflows bei der Übermittlung von HTML5-Formularen – Erstellen eines benutzerdefinierten Profils
description: Erstellen Sie ein benutzerdefiniertes Profil, um ein interaktives PDF mit den Daten aus einem teilweise ausgefüllten HTML5-Formular herunterzuladen.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '244'
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
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Generieren einer interaktiven PDF

Im Folgenden finden Sie den Servlet-Code, der für das Rendern interaktiver PDF-Dateien und die Rückgabe des PDF-Dokuments an die aufrufende Anwendung verantwortlich ist. Das Servlet ruft die Methode `mobileFormToInteractivePdf` des benutzerdefinierten DocumentServices-OSGi-Dienstes auf.

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### Rendern einer interaktiven PDF

Der folgende Code verwendet die [Forms-Service-API](https://helpx.adobe.com/de/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html), um interaktive PDFs mit den Daten aus dem Mobile-Formular zu rendern.

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## Nächste Schritte

[Durchführen der Formularübermittlung](./handle-form-submission.md)
