---
title: Verwenden des Assembler-Dienstes in AEM Forms
description: Zusammenstellen mehrerer PDF-Dateien mit dem Assembler-Dienst in AEM Forms
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
duration: 74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '192'
ht-degree: 100%

---

# Verwenden des Assembler-Dienstes in AEM Forms{#using-assembler-service-in-aem-forms}

In diesem Artikel finden Sie die Assets, mit denen Sie mehrere PDF-Dateien in den Browser ziehen und die zusammengestellte PDF-Datei in Ihrem Dateisystem speichern können. Im Folgenden finden Sie den Code für das Servlet, das mithilfe des Browsers die hochgeladenen PDF-Dateien zusammenstellt.

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

Sie können diese Funktion wie folgt auf Ihrem AEM-Server verwenden:

* Laden Sie die Datei [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) auf Ihr lokales System herunter.
* Verwenden Sie [Package Manager](http://localhost:4502/crx/packmgr/index.jsp), um das Paket hochzuladen und zu installieren.
* Laden Sie das [Bundle für benutzerdefinierte Dokumentendienste](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) herunter.
* Laden Sie das Bundle zur [Entwicklung mit Dienstbenutzenden](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) herunter.
* Stellen Sie die Bundles bereit und starten Sie sie mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles).
* Verweisen Sie Ihren Browser auf [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html).
* Legen Sie einige PDF-Dateien per Drag &amp; Drop ab.

>[!NOTE]
>
>Stellen Sie sicher, dass Sie Ihre AEM Forms-Installation abgeschlossen haben. Alle Bundles müssen sich in einem aktiven Status befinden.
>
>Stellen Sie sicher, dass Sie Boot Delegate RSA und BouncyCastle-Bibliotheken hinzugefügt haben, wie unter [Installieren von AEM Forms](https://helpx.adobe.com/de/aem-forms/6-3/installing-configuring-aem-forms-osgi.html) beschrieben.
>
>**Einschränkungen für diese Demo**
>
> * Der Code verarbeitet keine XFA-basierten PDF-Dokumente.
>
> * Stellen Sie sicher, dass Sie nur PDF-Dateien per Drag &amp; Drop ablegen.
>
>
