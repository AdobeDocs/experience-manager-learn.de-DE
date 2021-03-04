---
title: Signaturstatus des Formulars in der Datenbank aktualisieren
description: Signaturstatus des signierten Formulars in der Datenbank mithilfe des AEM-Workflows aktualisieren
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 4%

---


# Signaturstatus aktualisieren

Der UpdateSignatureStatus-Arbeitsablauf wird ausgelöst, wenn der Benutzer die Signaturzeremonie abgeschlossen hat. Im Folgenden wird der Workflow beschrieben

![main-workflow](assets/update-signature.PNG)

&quot;Signaturstatus aktualisieren&quot;ist ein benutzerdefinierter Prozessschritt.
Der Hauptgrund für die Implementierung eines benutzerdefinierten Prozessschritts ist die Erweiterung eines AEM Arbeitsablaufs. Im Folgenden finden Sie den benutzerspezifischen Code zur Aktualisierung des Signaturstatus.
Code in diesem benutzerdefinierten Prozessschritt verweist auf den SignMultipleForms-Dienst.


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Assets

Der Arbeitsablauf zum Aktualisieren des Signaturstatus kann [von hier heruntergeladen werden](assets/update-signature-status-workflow.zip)

