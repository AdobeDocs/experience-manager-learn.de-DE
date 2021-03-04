---
title: Assembler-Dienst in AEM Forms verwenden
seo-title: Assembler-Dienst in AEM Forms verwenden
description: Assembler-Dienst in AEM Forms zum Zusammenstellen mehrerer PDF-Dateien verwenden
seo-description: Assembler-Dienst in AEM Forms zum Zusammenstellen mehrerer PDF-Dateien verwenden
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 4%

---


# Assembler-Dienst in AEM Forms verwenden{#using-assembler-service-in-aem-forms}

In diesem Artikel finden Sie die Assets, mit denen Sie demonstrieren können, wie Sie mehrere PDF-Dateien per Drag &amp; Drop in den Browser ziehen und die assemblierte PDF-Datei in Ihrem Dateisystem speichern können. Im Folgenden finden Sie den Code für das Servlet, das die mit dem Browser hochgeladenen PDF-Dateien zusammenstellt.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

So funktioniert diese Funktion auf Ihrem AEM

* Laden Sie die Datei [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) auf Ihr lokales System herunter.
* Laden Sie das Paket mit dem [Paketmanager](http://localhost:4502/crx/packmgr/index.jsp) hoch und installieren Sie es.
* Herunterladen[Benutzerdefiniertes Dokument Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Stellen Sie die Bundles mithilfe der felix-Webkonsole [bereit und Beginn bereit.](http://localhost:4502/system/console/bundles)
* Verweisen Sie Ihren Browser auf [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* PDF-Dateien per Drag&amp;Drop verschieben

>[!NOTE]
>
>Vergewissern Sie sich, dass die AEM Forms-Installation abgeschlossen ist. Alle Pakete müssen sich im aktiven Zustand befinden.
>
>Vergewissern Sie sich, dass Sie - Boot Delegate RSA- und BouncyCastle-Bibliotheken hinzugefügt haben, wie in dieser [Installation von AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html) beschrieben
>
>**Caveats für diese Demo**
>
> * Der Code behandelt keine XFA-basierten PDF-Dokumente
   >
   > 
* Stellen Sie sicher, dass nur PDF-Dateien per Drag &amp; Drop eingefügt werden
>
>







