---
title: Rendern von XDP in PDF mit Verwendungsrechten
seo-title: Rendern von XDP in PDF mit Verwendungsrechten
description: Verwendungsrechte auf PDF anwenden
seo-description: Verwendungsrechte auf PDF anwenden
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Formularservice
topic: Entwicklung
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 12%

---


# Anwenden von Reader-Erweiterungen

Mit Reader Extensions können Sie Verwendungsrechte für PDF-Dokumente bearbeiten. Nutzungsrechte gelten für Funktionen, die standardmäßig in Acrobat, nicht jedoch in Adobe Reader zur Verfügung stehen. Die von Reader Extensions gesteuerte Funktion ermöglicht das Hinzufügen von , das Ausfüllen von Formularen und das Speichern von Dokumenten. PDF-Dokumente, für die Nutzungsrechte gelten, werden als Dokumente mit aktivierten Nutzungsrechten bezeichnet. Benutzer, die ein PDF-Dokument mit aktivierten Nutzungsrechten in Adobe Reader öffnen, können Vorgänge durchführen, die für dieses Dokument aktiviert sind.
Um diese Funktion zu testen, können Sie [link](https://forms.enablementadobe.com/content/samples/samples.html?query=0) ausprobieren. Der Beispielname lautet &quot;Render XDP with RE&quot;

Um dieses Anwendungsbeispiel zu erstellen, müssen wir Folgendes tun:
* Fügen Sie dem Benutzer &quot;fd-service&quot;das Reader Extensions-Zertifikat hinzu. Die Schritte zum Hinzufügen von Reader Extensions-Anmeldedaten sind [hier](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) aufgeführt.

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
import java.util.Enumeration;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFormatException;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = Servlet.class, property = {
sling.servlet.methods=get", "sling.servlet.paths=/bin/applyrights"
})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {
  @Reference
  GetResolver getResolver;
  @Reference
  ApplyUsageRights applyRights;
  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
  try {
        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
        InputSource is = new InputSource();
        is.setCharacterStream(new StringReader(xmlString));
  return db.parse(is);
} catch (ParserConfigurationException e) {
    e.printStackTrace();
} catch (SAXException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
return null;
}
@Override
protected void doGet(SlingHttpServletRequest request,SlingHttpServletResponse response)
{
  doPost(request,response);
}
@Override
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  UsageRights usageRights = new UsageRights();
  String submittedData = request.getParameter("jcr:data");
  org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
  String formFill = submittedXml.getElementsByTagName("formfill").item(0).getTextContent();
  usageRights.setEnabledFormDataImportExport(true);
  usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
  usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
  usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
  usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
  String attachmentPath = submittedXml.getElementsByTagName("attachmentpath").item(0).getTextContent();
  Node pdfNode = getResolver.getFormsServiceResolver().getResource(attachmentPath).adaptTo(Node.class);
  javax.jcr.Node jcrContent = null;
try {
    jcrContent = pdfNode.getNode("jcr:content");
    } catch (RepositoryException e1) {
      e1.printStackTrace();
    }

try {
    InputStream is = jcrContent.getProperty("jcr:data").getBinary().getStream();
    Session jcrSession = getResolver.getFormsServiceResolver().adaptTo(Session.class);
    Document documentToReaderExtend = new Document(is);
    documentToReaderExtend =  applyRights.applyUsageRights(documentToReaderExtend,usageRights);
    documentToReaderExtend.copyToFile(new File("c:\\scrap\\doctore.pdf"));
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
  } catch (ValueFormatException e) {
      e.printStackTrace();
  } catch (PathNotFoundException e) {
      e.printStackTrace();
  } catch (RepositoryException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
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



