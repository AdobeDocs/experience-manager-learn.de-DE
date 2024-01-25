---
title: Erstellen des Haupt-Workflows zum Auslösen des Signaturprozesses
description: Erstellen eines Workflows zum Speichern der Formulare zur Signierung in der Datenbank
feature: Adaptive Forms
version: 6.4,6.5
thumbnail: 6887.jpg
jira: KT-6887
topic: Development
role: Developer
level: Intermediate
exl-id: 338d9522-f6da-4aa7-b5d8-b9fff39ea94b
duration: 90
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 100%

---

# Erstellen eines Haupt-Workflows

Der Haupt-Workflow wird ausgelöst, wenn die Benutzerin oder der Benutzer das erste Formular sendet (**RefinanceForm**). Der Workflow läuft wie folgt ab.

![Haupt-Workflow](assets/main-workflow.PNG)

**Formulare zum Signieren speichern** ist ein benutzerdefinierter Prozessschritt.

Ein benutzerdefinierter Prozessschritt wird implementiert, um einen AEM-Workflow zu erweitern. Mit dem folgenden Code wird ein solcher benutzerdefinierter Prozessschritt implementiert. Der Code extrahiert die Namen der zu signierenden Formulare und gibt die gesendeten Formulardaten an die `insertData`-Methode im SignMultipleForms-Dienst weiter. Die `insertData`-Methode fügt dann die von der Datenquelle **aemformstutorial** identifizierten Zeilen in die Datenbank ein.

Der Code in diesem benutzerdefinierten Prozessschritt verweist auf den `SignMultipleForms`-Dienst.



```java
package com.aem.forms.signmultipleforms;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=StoreFormsToSign",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=StoreFormsToSign"
})
public class StoreFormsToSignWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(StoreFormsToSignWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log.debug("The payload  in StoreFormsToSign " + workItem.getWorkflowData().getPayload().toString());
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    String serverURL = arg2.get("PROCESS_ARGS", "string").toString();
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/formsToSign").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      log.debug("The form names to sign are  t" + node.getTextContent());
      String formNamesToSign[] = node.getTextContent().split(",");
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      DOMSource source = new DOMSource(xmlDocument);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      InputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
      StringWriter writer = new StringWriter();
      IOUtils.copy(inputStream, writer, Charset.defaultCharset());
      String formData = writer.toString();
      signMultipleForms.insertData(formNamesToSign, formData, serverURL, workItem, workflowSession);

  }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }
}
```




## Assets

Der in diesem Artikel verwendete Workflow „Mehrere Formulare signieren“ kann [hier heruntergeladen](assets/sign-multiple-forms-workflows.zip) werden.

>[!NOTE]
> Stellen Sie sicher, dass Sie den Day CQ Mail Service konfigurieren, um E-Mail-Benachrichtungen zu versenden. Die E-Mail-Vorlage wird auch im obigen Paket bereitgestellt.

## Nächste Schritte

[Aktualisieren des Signaturstatus beim Signieren eines Dokuments](./update-signature-status.md)
