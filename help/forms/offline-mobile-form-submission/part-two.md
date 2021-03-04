---
title: Arbeitsablauf für Trigger AEM HTML5-Formularübermittlung
seo-title: Trigger AEM Workflow bei der Übermittlung von HTML5-Formularen
description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
seo-description: Fahren Sie mit dem Ausfüllen des mobilen Formulars im Offlinemodus fort und senden Sie das Mobile-Formular an den Trigger AEM Arbeitsablauf
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# PDF-Übermittlung verarbeiten

In diesem Teil erstellen wir ein einfaches Servlet, das auf AEM Publish ausgeführt wird, um die PDF-Übermittlung von Acrobat/Reader zu verarbeiten. Dieses Servlet stellt wiederum eine HTTP-POST an ein Servlet, das in einer AEM Autoreninstanz ausgeführt wird, die für das Speichern der gesendeten Daten als `nt:file`-Knoten im Repository von AEM Author zuständig ist.

Im Folgenden finden Sie den Code des Servlets, das die PDF-Übermittlung verarbeitet. In diesem Servlet führen wir einen POST-Aufruf an ein Servlet aus, das auf **/bin/startworkflow** in einer AEM Author-Instanz gemountet ist. Dieses Servlet speichert die Formulardaten im Repository von AEM Author.


## AEM Publish-Servlet

```java
package com.aemforms.handlepdfsubmission.core.servlets;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import javax.servlet.ServletInputStream;
import javax.servlet.ServletOutputStream;

import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
  service={Servlet.class}, 
  property={
    "sling.servlet.methods=post", 
    "sling.servlet.paths=/bin/handlepdfsubmit"
  }
)
public class HandlePDFSubmission extends SlingAllMethodsServlet {

  private static Logger logger = LoggerFactory.getLogger(HandlePDFSubmission.class);
  
  protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
    ByteArrayOutputStream result = new ByteArrayOutputStream();
    try {
       ServletInputStream is = request.getInputStream();
       byte[] buffer = new byte[1024];
       int length;
       while ((length = is.read(buffer)) != -1) {
         result.write(buffer, 0, length);
       }
       logger.debug(result.toString(StandardCharsets.UTF_8.name()));
     } catch (IOException e1) {
        logger.error("An error occurred", e1);
     }

     HttpPost postReq = new HttpPost("http://localhost:4502/bin/startworkflow");
     // This is the base64 encoding of the admin credetnials. This call should be made over HTTPS in production scenarios to avoid leaking credentials.
     postReq.addHeader("Authorization", "Basic YWRtaW46YWRtaW4=");
     
     CloseableHttpClient httpClient = HttpClients.createDefault();
     List<NameValuePair> urlParameters = new ArrayList<NameValuePair>();
     
     logger.debug("added url parameters");
     
     try {
        urlParameters.add(new BasicNameValuePair("xmlData", result.toString(StandardCharsets.UTF_8.name())));
        postReq.setEntity(new UrlEncodedFormEntity(urlParameters));
        httpClient.execute(postReq);
        logger.debug("Sent request to author instance");
        ServletOutputStream sout = response.getOutputStream();
        sout.print("Your form was successfully submitted");
     } catch (UnsupportedEncodingException | ClientProtocolException | IOException e) {
        logger.error("An error occurred", e)
     }
}
```

## AEM Authoring-Servlet

Der nächste Schritt besteht darin, die gesendeten Daten im Repository von AEM Author zu speichern. Das auf `/bin/startworkflow` bereitgestellte Servlet speichert die gesendeten Daten.

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

@Component(
    service = {Servlet.class},
    property = {
            "sling.servlet.methods=get",
            "sling.servlet.paths=/bin/startworkflow"
    }
)
public class StartWorkflow extends SlingAllMethodsServlet {
        private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

        @Reference
        private GetResolver getResolver;

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
            String xmlData = null;
            System.out.println("in start workflow");
            response.setContentType("text/html;charset=UTF-8");

            if (request.getParameter("xmlData") != null) {
                logger.debug("The form was submitted from Acrobat/Reader");
                xmlData = request.getParameter("xmlData");
                System.out.println("in start workflow" + xmlData);
            } else {
                logger.debug("Mobile Form submission");
                StringBuffer stringBuffer = new StringBuffer();
                String line = null;

                try {
                    InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
                    BufferedReader reader = new BufferedReader(isReader);
                    while ((line = reader.readLine()) != null)
                        stringBuffer.append(line);
                } catch (Exception e) {
                    logger.debug("Error" + e.getMessage());
                }

                xmlData = new String(stringBuffer);
            }

            Resource r = getResolver.getFormsServiceResolver().getResource("/content/pdfsubmissions");
            Session session = r.getResourceResolver().adaptTo(Session.class);
            logger.debug("Got reosurce pdfsubmissions" + r.getPath());
            UUID uidName = UUID.randomUUID();
            Node xmlDataFilesNode = r.adaptTo(Node.class);
            InputStream is = new ByteArrayInputStream(xmlData.getBytes());
            Binary binary;

            try {
                Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                logger.debug("Added nt file node");
                Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                logger.debug("Added jcr content");
                binary = session.getValueFactory().createBinary(is);
                jcrContent.setProperty("jcr:data", binary);
                session.save();
            } catch (RepositoryException e) {
                logger.error("Unable to store data to JCR", e);
            }

            response.setContentType("text/plain;charset=UTF-8");

            try {
                ServletOutputStream sout = response.getOutputStream();
                sout.print("Your form was successfully submitted");
            } catch (IOException e) {
                logger.error("Unable write response", e);
            }
        }
}
```

Ein AEM Workflow-Starter wird für den Trigger jedes Mal konfiguriert, wenn eine neue Ressource des Typs `nt:file` unter dem Knoten `/content/pdfsubmissions` erstellt wird. Dieser Arbeitsablauf erstellt nicht interaktive oder statische PDF-Dateien, indem die gesendeten Daten mit der xdp-Vorlage zusammengeführt werden. Die generierte PDF wird dann einem Benutzer zur Überprüfung und Genehmigung zugewiesen.

Um die gesendeten Daten unter dem Knoten `/content/pdfsubmissions` zu speichern, verwenden wir den Dienst `GetResolver` OSGi, um die gesendeten Daten mit dem `fd-service`-Systembenutzer zu speichern, der in jeder AEM Forms-Installation verfügbar ist.

