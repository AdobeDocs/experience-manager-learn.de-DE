---
title: Verwenden des Assembler-Dienstes in AEM Forms
description: Assembler-Dienst in AEM Forms zum Zusammenführen mehrerer PDF-Dateien verwenden
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 4%

---

# Verwenden des Assembler-Dienstes in AEM Forms{#using-assembler-service-in-aem-forms}

In diesem Artikel finden Sie die Assets, die zeigen, wie Sie mehrere PDF-Dateien per Drag-and-Drop in den Browser ziehen und die assemblierte PDF-Datei in Ihrem Dateisystem speichern können. Im Folgenden finden Sie den Code für das Servlet, das die mit dem Browser hochgeladenen PDF-Dateien zusammenstellt.

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

So können Sie diese Funktion auf Ihrem AEM-Server verwenden

* Laden Sie die [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) auf Ihr lokales System.
* Laden Sie das Paket hoch und installieren Sie es mithilfe des [Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* Download[Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Download [Entwickeln mit Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Stellen Sie die Bundles bereit und starten Sie sie mit dem [Felix-Webkonsole](http://localhost:4502/system/console/bundles)
* Zeigen Sie Ihren Browser auf [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Ziehen Sie einige Dateien mit PDF-Dateien per Drag-and-Drop

>[!NOTE]
>
>Stellen Sie sicher, dass die AEM Forms-Installation abgeschlossen ist. Alle Pakete müssen sich im aktiven Status befinden.
>
>Stellen Sie sicher, dass Sie - Boot Delegate RSA- und BouncyCastle-Bibliotheken hinzugefügt haben, wie in diesem [Installieren von AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Einschränkungen für diese Demo**
>
> * Der Code verarbeitet keine XFA-basierten PDF-Dokumente
>
> * Stellen Sie sicher, dass Sie nur PDF-Dateien per Drag-and-Drop verschieben
>
>

