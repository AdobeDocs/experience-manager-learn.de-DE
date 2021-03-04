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
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---


# Rendern von XDP in PDF mit Verwendungsrechten{#rendering-xdp-into-pdf-with-usage-rights}

Ein gängiger Anwendungsfall besteht darin, xdp in PDF zu rendern und Reader Extensions auf die gerenderte PDF anzuwenden.

Wenn ein Benutzer beispielsweise im Forms Portal von AEM Forms auf XDP klickt, können wir XDP als PDF wiedergeben und die PDF-Datei durch Reader erweitern.

Um diese Funktion zu testen, können Sie diesen [Link](https://forms.enablementadobe.com/content/samples/samples.html?query=0) ausprobieren. Der Beispielname lautet &quot;Render XDP with RE&quot;.

Um diesen Verwendungsfall zu erreichen, müssen wir die folgenden Schritte ausführen.

* hinzufügen das Reader Extensions-Zertifikat auf &quot;fd-service&quot;-Benutzer. Die Schritte zum Hinzufügen der Berechtigung für Reader-Erweiterungen sind [hier](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) aufgelistet

* Erstellen Sie einen benutzerdefinierten OSGi-Dienst, der Verwendungsrechte wiedergibt und anwendet. Der Code, um dies zu erreichen, ist unten aufgeführt

## XDP rendern und Verwendungsrechte anwenden {#render-xdp-and-apply-usage-rights}

* Zeile 7: Mit dem renderPDFForm des FormsService generieren wir PDF aus der XDP.

* Zeilen 8-14: Die entsprechenden Verwendungsrechte werden festgelegt. Diese Verwendungsrechte werden aus den OSGi-Konfigurationseinstellungen abgerufen.

* Zeile 20: Verwenden Sie den mit dem Dienst fd-service des Dienstbenutzers verknüpften Ressourcenordner

* Zeile 24: Die secureDocument-Methode von DocumentAssuranceService wird verwendet, um die Verwendungsrechte anzuwenden

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

![](assets/configurationproperties.gif)

Der folgende Code zeigt den Code zum Erstellen der OSGi-Konfigurationseinstellungen

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

## Erstellen eines Servlets zum Streamen der PDF {#create-servlet-to-stream-the-pdf}

Im nächsten Schritt erstellen Sie ein Servlet mit einer GET, um dem Leser die erweiterte PDF-Datei zurückzugeben. In diesem Fall wird der Benutzer aufgefordert, die PDF-Datei in seinem Dateisystem zu speichern. Das liegt daran, dass die PDF-Datei als dynamisches PDF gerendert wird und die mit den Browsern mitgelieferten PDF-Viewer keine dynamischen PDF-Dateien verarbeiten.

Im Folgenden finden Sie den Code für das Servlet. Wir übergeben den Pfad der XDP im CRX-Repository an dieses Servlet.

Anschließend rufen wir die Methode renderAndExtendXdp von com.aemformssamples.documentservices.core.DocumentServices auf.

Die erweiterte PDF-Datei des Lesers wird dann in die aufrufende Anwendung gestreamt

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

Gehen Sie wie folgt vor, um dies auf Ihrem lokalen Server zu testen
1. [Herunterladen und Installieren des DevelopingWithServiceUser-Bundles](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Herunterladen und Installieren des AEMFormsDocumentServices-Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Herunterladen und Importieren der mit diesem Artikel zusammenhängenden Assets in AEM Package Manager](assets/renderandextendxdp.zip)
   * Dieses Paket enthält eine Beispielportal- und XDP-Datei
1. hinzufügen Reader Extensions-Zertifikat auf &quot;fd-service&quot;-Benutzer
1. Verweisen Sie Ihren Browser auf die [Portal-Webseite](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Klicken Sie auf das PDF-Symbol, um die xdp-Datei zu rendern und die PDF-Datei abzurufen, die Reader Extended ist.



