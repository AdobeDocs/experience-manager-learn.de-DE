---
title: Rendern von XDP in PDF mit Verwendungsrechten
description: Verwendungsrechte auf PDF anwenden
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# Rendern von XDP in PDF mit Verwendungsrechten{#rendering-xdp-into-pdf-with-usage-rights}

Ein gängiges Anwendungsbeispiel besteht darin, xdp in PDF zu rendern und Reader Extensions auf die gerenderte PDF anzuwenden.

Wenn ein Benutzer beispielsweise im Forms Portal von AEM Forms auf XDP klickt, kann XDP gerendert werden, während PDF und Leser die PDF erweitern.


Um dieses Anwendungsbeispiel zu erstellen, müssen wir die folgenden Schritte ausführen.

* Fügen Sie dem Benutzer &quot;fd-service&quot;das Reader Extensions-Zertifikat hinzu. Die Schritte zum Hinzufügen von Reader Extensions-Berechtigungen werden aufgelistet [here](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=de)


* Weitere Informationen finden Sie im Video unter [Konfigurieren von Reader Extensions-Berechtigungen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* Erstellen Sie einen benutzerdefinierten OSGi-Dienst, der Nutzungsrechte rendert und anwendet. Der Code, um dies zu erreichen, ist unten aufgeführt

## XDP rendern und Verwendungsrechte anwenden {#render-xdp-and-apply-usage-rights}

* Zeile 7: Mit renderPDFForm des FormsService generieren wir PDF aus der XDP.

* Zeilen 8-14: Die entsprechenden Verwendungsrechte werden festgelegt. Diese Verwendungsrechte werden aus den OSGi-Konfigurationseinstellungen abgerufen.

* Zeile 20: Verwenden Sie den ResourceResolver, der mit dem fd-service des Dienstbenutzers verknüpft ist.

* Zeile 24: Die Methode secureDocument von DocumentAssuranceService wird verwendet, um die Verwendungsrechte anzuwenden

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

Der folgende Screenshot zeigt die angezeigten Konfigurationseigenschaften. Die meisten allgemeinen Verwendungsrechte werden durch diese Konfiguration offen gelegt.

![Konfigurationseigenschaften](assets/configurationproperties.gif)

Der folgende Code zeigt den Code, der zum Erstellen der OSGi-Konfigurationseinstellungen verwendet wird

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## Erstellen eines Servlets zum Streamen des PDF {#create-servlet-to-stream-the-pdf}

Der nächste Schritt besteht darin, ein Servlet mit einer GET-Methode zu erstellen, um dem Leser das erweiterte PDF zurückzugeben. In diesem Fall wird der Benutzer aufgefordert, die PDF in seinem Dateisystem zu speichern. Dies liegt daran, dass die PDF als dynamische PDF gerendert wird und die PDF-Viewer, die mit den Browsern geliefert werden, keine dynamischen PDFs verarbeiten.

Im Folgenden finden Sie den Code für das Servlet. Wir übergeben den Pfad der XDP im CRX-Repository an dieses Servlet.

Dann rufen wir die Methode renderAndExtendXdp von com.aemformsssamples.documentservices.core.DocumentServices auf.

Die erweiterte PDF des Lesers wird dann an die aufrufende Anwendung gestreamt

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

Um dies auf Ihrem lokalen Server zu testen, führen Sie die folgenden Schritte aus
1. [Herunterladen und Installieren des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Herunterladen und Installieren des AEMFormsDocumentServices-Bundles](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Herunterladen des HTML-Codes der benutzerdefinierten Portalvorlage](assets/render-and-extend-template.zip)
1. [Laden Sie die mit diesem Artikel verknüpften Assets mit Package Manager in AEM herunter und importieren Sie sie.](assets/renderandextendxdp.zip)
   * Dieses Paket enthält Beispielportal und XDP-Datei
1. Hinzufügen des Reader Extensions-Zertifikats zum Benutzer &quot;fd-service&quot;
1. Zeigen Sie Ihren Browser auf [Portal-Webseite](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Klicken Sie auf das PDF-Symbol, um die xdp als pdf-Datei mit angewendeten Verwendungsrechten zu rendern.
