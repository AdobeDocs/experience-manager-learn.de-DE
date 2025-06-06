---
title: Erstellen eines OSGi-Dienstes zum Exportieren von Daten aus einem PDF-Formular
description: Exportieren von Daten aus einem PDF-Formular mithilfe der FormsService-API
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: c3032669-154c-4565-af6e-32d94e975e37
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '135'
ht-degree: 100%

---

# Daten exportieren

Der erste Schritt zum Ausfüllen eines adaptiven Formulars aus einer PDF besteht darin, die Daten aus der angegebenen PDF-Datei zu exportieren und im AEM-Repository zu speichern.

Der folgende Code wurde geschrieben, um die Daten aus dem hochgeladenen PDF-Dokument zu extrahieren und so das korrekte Format zu erhalten, das zum Ausfüllen des adaptiven Formulars verwendet werden kann

```java
public String getFormData(Document pdfForm) {
   DocumentBuilderFactory factory = null;
   DocumentBuilder builder = null;
   org.w3c.dom.Document xmlDocument = null;

   try {
      Document xmlData = formsService.exportData(pdfForm, DataFormat.Auto);
      xmlData.copyToFile(new File("xmlData.xml"));
      factory = DocumentBuilderFactory.newInstance();
      factory.setNamespaceAware(true);
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlData.getInputStream());

      org.w3c.dom.Node xdpNode = xmlDocument.getDocumentElement();
      log.debug("Got xdp " + xdpNode.getNodeName());
      org.w3c.dom.Node datasets = getChildByTagName(xdpNode, "xfa:datasets");
      log.debug("Got datasets " + datasets.getNodeName());
      org.w3c.dom.Node data = getChildByTagName(datasets, "xfa:data");
      log.debug("Got data " + data.getNodeName());
      org.w3c.dom.Node topmostSubform = getChildByTagName(data, "topmostSubform");

      if (topmostSubform != null) {

         org.w3c.dom.Document newXmlDocument = builder.newDocument();
         org.w3c.dom.Node importedNode = newXmlDocument.importNode(topmostSubform, true);
         newXmlDocument.appendChild(importedNode);
         Document aemFDXmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXmlDocument);
         aemFDXmlDocument.copyToFile(new File("aemFDXmlDocument.xml"));
         // saveDocumentInCrx is an utility method of the custom DocumentServices service. 
         return documentServices.saveDocumentInCrx("/content/exporteddata", ".xml", aemFDXmlDocument);
      }

   } catch (Exception e) {
      log.debug("Error:  " + e.getMessage());

   }
   return null;
}
```

Im Folgenden finden Sie die Dienstprogrammfunktion, die geschrieben wurde, um das _**topmostSubForm**_ mit den entsprechenden Namespaces zu extrahieren.

```java
private static org.w3c.dom.Node getChildByTagName(org.w3c.dom.Node parent, String tagName) {
   NodeList nodeList = parent.getChildNodes();
   for (int i = 0; i < nodeList.getLength(); i++) {
      org.w3c.dom.Node node = nodeList.item(i);
      if (node.getNodeType() == org.w3c.dom.Node.ELEMENT_NODE && node.getNodeName().equals(tagName)) {
         return node;
      }
   }
   return null;
}
```

Die extrahierten Daten werden im Knoten /content/exporteddata im CRX-Repository gespeichert. Der Dateipfad der exportierten Daten wird dann zum Ausfüllen des adaptiven Formulars an die aufrufende Anwendung zurückgegeben.

## Nächste Schritte

[Importieren von Daten aus der PDF-Datei](./populate-adaptive-form.md)
