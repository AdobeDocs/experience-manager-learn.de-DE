---
title: Generieren von PDF-Dokumenten mit dem Ausgabe-Service
description: Zusammenführen von Daten mit der XDP-Vorlage zum Generieren nicht interaktiver PDFs
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 100%

---

# Generieren von PDF-Dokumenten mit dem Ausgabe-Service

Der [Ausgabe-Service](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) ist als OSGi-Dienst Teil der AEM-Dokumentendienste. Er unterstützt verschiedene Ausgabeformate und Design-Funktionen von AEM Forms Designer. Der Ausgabe-Service konvertiert XFA-Vorlagen und XML-Daten, um Druckdokumente in verschiedenen Formaten zu erstellen.

Der Ausgabe-Service in AEM Forms as a Cloud Service ähnelt dem in AEM Forms 6.5. Wenn Sie also mit der Verwendung des Ausgabe-Service in AEM Forms 6.5 vertraut sind, sollte der Übergang zu AEM Forms as a Cloud Service unkompliziert sein.

Mit dem Ausgabe-Service können Sie Anwendungen erstellen, die Folgendes ermöglichen:

+ Erzeugen fertiger Formulardokumente durch Füllen von Vorlagendateien mit XML-Daten.
+ Generieren von Ausgabeformularen in verschiedenen Formaten einschließlich nicht interaktiver PDF-, PostScript-, PCL- und ZPL-Druckdatenströme
+ Generieren von druckbaren PDFs aus XFA-Formular-PDFs
+ Generieren von PDF-, PostScript-, PCL- und ZPL-Dokumenten in großen Mengen durch Zusammenführen mehrerer Datensätze mit den bereitgestellten Vorlagen.

Dieser Dienst ist für die Verwendung im Kontext einer AEM Forms as a Cloud Service-Instanz konzipiert. Das folgende Code-Fragment generiert ein PDF-Dokument in einem Servlet mit dem `OutputService`.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
