---
title: Rendern von XDP in PDF mit Verwendungsrechten
description: Verwendungsrechte auf PDF anwenden
version: 6.4,6.5
feature: Reader Extensions
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 13%

---


# Anwenden von Reader-Erweiterungen

Mit Reader Extensions können Sie Verwendungsrechte für PDF-Dokumente bearbeiten. Nutzungsrechte gelten für Funktionen, die standardmäßig in Acrobat, nicht jedoch in Adobe Reader zur Verfügung stehen. Die von Reader Extensions gesteuerte Funktion ermöglicht das Hinzufügen von , das Ausfüllen von Formularen und das Speichern von Dokumenten. PDF-Dokumente, für die Nutzungsrechte gelten, werden als Dokumente mit aktivierten Nutzungsrechten bezeichnet. Benutzer, die ein PDF-Dokument mit aktivierten Nutzungsrechten in Adobe Reader öffnen, können Vorgänge durchführen, die für dieses Dokument aktiviert sind.
Um diese Funktion zu testen, können Sie [link](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html) ausprobieren.

Um dieses Anwendungsbeispiel zu erstellen, müssen wir Folgendes tun:
* [Fügen Sie dem ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) Reader das  `fd-service` Benutzererweiterungszertifikat hinzu.

* Erstellen Sie einen benutzerdefinierten OSGi-Dienst, der Verwendungsrechte auf die Dokumente anwendet. Der Code, um dies zu erreichen, ist unten aufgeführt

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## Erstellen eines Servlets zum Streamen der PDF {#create-servlet-to-stream-the-pdf}

Der nächste Schritt besteht darin, ein Servlet mit einer POST-Methode zu erstellen, um die Lesererweiterte PDF-Datei an den Benutzer zurückzugeben. In diesem Fall wird der Benutzer aufgefordert, die PDF-Datei in seinem Dateisystem zu speichern. Dies liegt daran, dass die PDF-Datei als dynamisches PDF gerendert wird und die PDF-Viewer, die mit den Browsern geliefert werden, keine dynamischen PDF-Dateien verarbeiten.

Im Folgenden finden Sie den Code für das Servlet. Das Servlet wird von der Aktion **customsubmit** des adaptiven Formulars aufgerufen.
Servlet erstellt das Objekt UsageRights und legt seine Eigenschaften auf der Grundlage der vom Benutzer im adaptiven Formular eingegebenen Werte fest. Das Servlet ruft dann die Methode **applyUsageRights** des zu diesem Zweck erstellten Dienstes auf.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;

import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];
                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);

                                        documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        documentToReaderExtend.close();
                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();
                                        documentToReaderExtend.close();
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
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

Führen Sie die folgenden Schritte aus, um dies auf Ihrem lokalen Server zu testen:
1. [Herunterladen und Installieren des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Laden Sie das Paket ares.ares.core-ares herunter und installieren Sie es](assets/ares.ares.core-ares.jar). Dies umfasst den benutzerdefinierten Dienst und das Servlet, um Verwendungsrechte anzuwenden und das PDF-Dokument zurückzustreamen.
1. [Importieren von Client-Bibliotheken und benutzerdefiniertem Senden](assets/applyaresdemo.zip)
1. [Importieren des adaptiven Formulars](assets/applyaresform.zip)
1. Hinzufügen des Reader Extensions-Zertifikats zum Benutzer &quot;fd-service&quot;
1. [Vorschau des adaptiven Formulars](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Wählen Sie die entsprechenden Berechtigungen aus und laden Sie die PDF-Datei hoch.
1. Klicken Sie auf Senden , um Reader Extended PDF zu erhalten


