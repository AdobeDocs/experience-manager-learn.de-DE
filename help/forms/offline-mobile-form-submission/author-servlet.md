---
title: Trigger AEM Workflow bei der HTML 5-Formularübermittlung - Formularübermittlung handhaben
description: Erfahren Sie, wie Sie AEM Workflow beim Senden des HTML5-Formulars Trigger und die gesendeten Daten im Repository speichern können.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
exl-id: eafeafe1-7a72-4023-b5bb-d83b056ba207
duration: 116
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 29%

---


# Gesendete Daten speichern

Der nächste Schritt besteht darin, die gesendeten Daten im Repository von AEM Author zu speichern. Das auf `/bin/startworkflow` bereitgestellte Servlet speichert die gesendeten Daten.
Ein AEM Workflow-Starter wird so konfiguriert, dass er jedes Mal, wenn eine neue Ressource des Typs `nt:file` unter dem Knoten &lt;node_to_store_sent_data> erstellt wird, zum Trigger wird. Dieser Workflow erstellt eine nicht-interaktive oder statische PDF, indem die gesendeten Daten mit der xdp-Vorlage zusammengeführt werden. Die generierte PDF wird dann der Benutzerin bzw. dem Benutzer zur Überprüfung und Genehmigung zugewiesen.

Um die übermittelten Daten im Knoten &lt;node_to_store_sent_data> zu speichern, verwenden wir den OSGi-Dienst `GetResolver`, der es uns ermöglicht, die übermittelten Daten mithilfe des in jeder AEM Forms-Installation verfügbaren Systembenutzers `fd-service` zu speichern.

Der Knoten, unter dem die gesendeten Daten gespeichert werden, kann mithilfe von ConfigMgr konfiguriert werden, wie unter [Bereitstellen von Beispiel-Assets](./deploy-assets.md) beschrieben.

```java
package com.aemforms.mobileforms.core.servlets;

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
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import org.apache.sling.api.servlets.SlingAllMethodsServlet;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/startworkflow"})
public class StartWorkflow extends SlingAllMethodsServlet {

    private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

    @Reference
    private GetResolver getResolver;
    
    @Reference
    private AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;
    
    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
    {
            String xmlData = null;
            logger.debug("in start workflow");
            response.setContentType("text/html;charset=UTF-8");
            if (request.getParameter("xmlData") != null)
                {
                    logger.debug("The form was submitted from Acrobat/Reader");
                    xmlData = request.getParameter("xmlData");
                    logger.debug("In start workflow" + xmlData);
                }
                logger.debug("Trying to get resource  "+aemServerCredentialsConfigurationService.getFolderPath());
                Resource r = getResolver.getFormsServiceResolver().getResource(aemServerCredentialsConfigurationService.getFolderPath());
                String responseMessage =null;
                if(r!= null)
                {

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
                        } catch (RepositoryException e)
                        {
                            throw new RuntimeException(e);
                        }
                    responseMessage = "Your form was successfully submitted";
                }else
                {
                    logger.debug("The resource IS NULL");
                    responseMessage = "Error is processing your submission!!! Please contact the administrator";
                }

        try {
            response.setContentType("text/plain");
            response.getWriter().write(responseMessage);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

## Nächste Schritte

[Workflow-Starter und Workflow](./review-workflow.md)

