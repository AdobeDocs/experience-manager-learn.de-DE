---
title: Tagging und Speichern von AEM Forms-Datensatzdokumenten in DAM
description: In diesem Artikel wird der Anwendungsfall zum Speichern und Tagging des von AEM Forms generierten Datensatzdokuments (Document of Record, DoR) in AEM DAM erläutert. Das Tagging des Dokuments erfolgt auf Basis der übermittelten Formulardaten.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 237
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 100%

---

# Tagging und Speichern von AEM Forms-Datensatzdokumenten in DAM {#tagging-and-storing-aem-forms-dor-in-dam}

In diesem Artikel wird der Anwendungsfall zum Speichern und Tagging des von AEM Forms generierten Datensatzdokuments (Document of Record, DoR) in AEM DAM erläutert. Das Tagging des Dokuments erfolgt auf Basis der übermittelten Formulardaten.

Kundinnen und Kunden wünschen sich häufig, das von AEM Forms generierte DoR in AEM DAM speichern und mit Tags versehen zu können. Das Tagging des Dokuments muss auf den übermittelten Daten des adaptiven Formulars basieren. Wenn der Beschäftigungsstatus in den übermittelten Daten beispielsweise „Im Ruhestand“ lautet, soll das Dokument mit einem entsprechenden Tag versehen und in DAM gespeichert werden.

Der Anwendungsfall sieht folgendermaßen aus:

* Eine Benutzerin oder ein Benutzer füllt das adaptive Formular aus. Im adaptiven Formular werden der Familienstand (z. B. „Ledig“) und der Beschäftigungsstatus (z. B. „Im Ruhestand“) der Benutzerin oder des Benutzers erfasst.
* Beim Übermitteln des Formulars wird ein AEM-Workflow ausgelöst. Dieser Workflow versieht das Dokument mit Tags für den Familienstand („Ledig“) sowie den Beschäftigungsstatus („Im Ruhestand“) und speichert das Dokument in DAM.
* Sobald das Dokument in DAM gespeichert ist, sollten Admins das Dokument anhand dieser Tags durchsuchen können. Beispielsweise würde bei einer Suche nach „Ledig“ oder „Im Ruhestand“ die entsprechenden DoRs zurückgegeben werden.

Für diesen Anwendungsfall wurde ein benutzerdefinierter Prozessschritt geschrieben. In diesem Schritt rufen wir die Werte der entsprechenden Datenelemente aus den übermittelten Daten ab. Anschließend erstellen wir die Tag-Kachel mit diesem Wert. Wenn der Wert des Familienstand-Elements beispielsweise „Ledig“ lautet, wird der Tag-Titel zu „Peak:EmploymentStatus/Single“ (also Peak:Beschäftigungsstatus/Ledig). ** Mithilfe der TagManager-API finden wir das Tag und wenden es auf das DoR an.

Im Folgenden finden Sie den vollständigen Code zum Tagging und Speichern des Datensatzdokuments in AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Um dieses Beispiel auf Ihrem System verwenden zu können, führen Sie die folgenden Schritte aus:
* [Stellen Sie das Bundle „DevelopingWithServiceUser“ bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Laden Sie das setvalue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle, das die Tags aus den übermittelten Formulardaten festlegt.

* [Laden Sie das adaptive Beispielformular herunter.](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Navigieren Sie zu „Formulare und Dokumente“.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Klicken Sie auf „Erstellen > Datei hochladen“ und laden Sie die Datei „tag-and-store-in-dam-adaptive-form.zip“ hoch.

* [Importieren Sie die Artikel-Assets](assets/tag-and-store-in-dam-assets.zip) mit AEM Package Manager.
* Öffnen Sie das [adaptive Formular im Vorschaumodus](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Füllen Sie alle Felder aus** und übermitteln Sie das Formular.
* [Navigieren Sie zum Peak-Ordner in DAM](http://localhost:4502/assets.html/content/dam/Peak). Das DoR sollte im Peak-Ordner zu sehen sein. Überprüfen Sie die Eigenschaften des Dokuments. Es sollte entsprechend mit Tags versehen sein.
Herzlichen Glückwunsch!! Sie haben das Muster erfolgreich auf Ihrem System installiert.

* Sehen wir uns den [Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) an, der bei der Formularübermittlung ausgelöst wird.
* Im ersten Workflow-Schritt wird ein eindeutiger Dateiname erstellt, indem der Name der Antragstellerin oder des Antragstellers und der Kreis, indem sich der Wohnsitz befindet, miteinander verkettet werden.
* Im zweiten Workflow-Schritt werden die Tag-Hierarchie und die mit Tags zu versehenen Formularfeldelemente weitergegeben. Durch den Prozessschritt wird der Wert aus den übermittelten Daten extrahiert und der Tag-Titel für das Dokument erstellt.
* Wenn Sie das DoR in einem anderen Ordner in DAM speichern möchten, geben Sie den Ordnerspeicherort mithilfe der Konfigurationseigenschaften an, wie im Screenshot unten angegeben.

Die anderen beiden Parameter sind spezifisch für das DoR und den Datendateipfad, wie in den Übermittlungsoptionen für adaptive Formulare angegeben. Stellen Sie sicher, dass die hier angegebenen Werte mit den Werten übereinstimmen, die Sie in den Übermittlungsoptionen für adaptive Formulare festgelegt haben.

![DoR-Tagging](assets/tag_dor_service_configuration.gif)
