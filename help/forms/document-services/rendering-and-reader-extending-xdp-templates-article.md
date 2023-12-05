---
title: Rendern von XDP in PDF mit Verwendungsrechten
description: Anwenden von Verwendungsrechten auf PDF-Dokumente
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
duration: 221
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 100%

---

# Rendern von XDP in PDF mit Verwendungsrechten{#rendering-xdp-into-pdf-with-usage-rights}

Ein gängiger Anwendungsfall besteht darin, XDP in PDF zu rendern und Reader Extensions auf die gerenderte PDF-Datei anzuwenden.

Wenn eine Benutzerin oder ein Benutzer beispielsweise im Formularportal von AEM Forms auf XDP klickt, kann eine XDP-Datei als PDF gerendert werden und Reader das PDF-Dokument erweitern.


Für diesen Anwendungsfall müssen wir die folgenden Schritte ausführen.

* Fügen Sie der Benutzerin bzw. dem Benutzer „fd-service“ das Reader Extensions-Zertifikat hinzu. Die Schritte zum Hinzufügen von Reader Extensions-Anmeldeinformationen sind [hier](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=de) aufgelistet.


* Weitere Informationen finden Sie außerdem im Video zum [Konfigurieren von Reader Extensions-Anmeldeinformationen](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=de).


* Erstellen Sie einen benutzerdefinierten OSGi-Dienst, der Verwendungsrechte rendert und anwendet. Der Code dafür ist unten aufgeführt.

## Rendern von XDP und Anwenden von Verwendungsrechten {#render-xdp-and-apply-usage-rights}

* Zeile 7: Mit renderPDFForm des Forms-Dienstes generieren wir eine PDF-Datei aus XDP.

* Zeilen 8–14: Die entsprechenden Verwendungsrechte werden festgelegt. Diese Verwendungsrechte werden aus den OSGi-Konfigurationseinstellungen abgerufen.

* Zeile 20: Verwenden Sie den ResourceResolver, der mit der Dienstbenutzerin bzw. dem Dienstbenutzer „fd-service“ verknüpft ist.

* Zeile 24: Über die secureDocument-Methode von „DocumentAssuranceService“ werden die Verwendungsrechte angewendet.

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

Der folgende Screenshot zeigt die bereitgestellten Konfigurationseigenschaften. Die meisten allgemeinen Verwendungsrechte werden durch diese Konfiguration bereitgestellt.

![Konfigurationseigenschaften](assets/configurationproperties.gif)

Im Folgenden finden Sie den Code, der zum Erstellen der OSGi-Konfigurationseinstellungen verwendet wird.

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

## Erstellen eines Servlets zum Streamen der PDF-Datei {#create-servlet-to-stream-the-pdf}

Der nächste Schritt besteht darin, ein Servlet mit einer GET-Methode zu erstellen, um die Reader-erweiterte PDF-Datei an die Benutzerin oder den Benutzer zurückzugeben. In diesem Fall wird die Benutzerin bzw. der Benutzer aufgefordert, die PDF-Datei im eigenen Dateisystem zu speichern. Dies liegt daran, dass die PDF-Datei als dynamische PDF gerendert wird und die in den Browsern enthaltenen PDF-Viewer keine dynamischen PDFs verarbeiten.

Im Folgenden finden Sie den Code für das Servlet. Wir geben den Pfad der XDP-Datei im CRX-Repository an dieses Servlet weiter.

Dann rufen wir die renderAndExtendXdp-Methode von „com.aemformssamples.documentservices.core.DocumentServices“ auf.

Die Reader-erweiterte PDF-Datei wird anschließend an die aufrufende Anwendung gestreamt.

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

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen Server zu testen:
1. [Laden Sie das DevelopingWithServiceUser-Bundle herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Laden Sie das AEMFormsDocumentServices-Bundle herunter und installieren Sie es.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Laden Sie die HTML-Seite der benutzerdefinierten Portalvorlage herunter.](assets/render-and-extend-template.zip)
1. [Laden Sie die mit diesem Artikel verbundenen Assets herunter und importieren Sie sie mit Package Manager in AEM.](assets/renderandextendxdp.zip)
   * Dieses Paket enthält die beispielhafte Portal- und XDP-Datei.
1. Fügen Sie das Reader Extensions-Zertifikat zur Benutzerin bzw. zum Benutzer „fd-service“ hinzu.
1. Verweisen Sie Ihren Browser auf die [Portal-Web-Seite](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html).
1. Klicken Sie auf das PDF-Symbol, um die XDP als PDF-Datei mit angewendeten Verwendungsrechten zu rendern.
