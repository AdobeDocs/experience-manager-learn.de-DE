---
title: Anpassen der Benachrichtigung „Aufgabe zuweisen“
description: Einschließen von Formulardaten in E-Mail-Benachrichtungen „Aufgabe zuweisen“
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 144
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '438'
ht-degree: 100%

---

# Anpassen der Benachrichtigung „Aufgabe zuweisen“

Die Komponente „Aufgabe zuweisen“ wird verwendet, um Workflow-Teilnehmenden Aufgaben zuzuweisen. Wenn eine Aufgabe einer Benutzerin bzw. einem Benutzer oder einer Gruppe zugewiesen wird, wird eine E-Mail-Benachrichtigung an die definierten Benutzerin bzw. den definierten Benutzer oder die definierten Gruppenmitglieder gesendet.
Diese E-Mail-Benachrichtigung enthält normalerweise dynamische Daten zur Aufgabe. Diese dynamischen Daten werden mithilfe der vom System generierten [Metadateneigenschaften](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html?lang=de#using-system-generated-metadata-in-an-email-notification) abgerufen.
Um Werte aus den übermittelten Formulardaten in die E-Mail-Benachrichtigung einzuschließen, müssen wir benutzerdefinierte Metadateneigenschaften erstellen und diese dann in der E-Mail-Vorlage verwenden.



## Erstellen benutzerdefinierter Metadateneigenschaften

Es wird empfohlen, eine OSGi-Komponente zu erstellen, die die getUserMetadata-Methode von [WorkitemUserMetadataService](https://helpx.adobe.com/de/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--) implementiert.

Mit dem folgenden Code werden vier Metadateneigenschaften (_firstName_,_lastName_,_reason_ und _amountRequested_) erstellt. Die zugehörigen Werte werden dabei anhand der übermittelten Daten festgelegt. Beispielsweise ist für den Wert der Metadateneigenschaft _firstName_ der Wert des Elements „firstName“ aus den übermittelten Daten festgelegt. Im folgenden Code wird davon ausgegangen, dass die übermittelten Daten des adaptiven Formulars im XML-Format vorliegen. Adaptive Formulare, die auf einem JSON-Schema oder Formulardatenmodell basieren, generieren Daten im JSON-Format.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## Verwenden benutzerdefinierter Metadateneigenschaften in der E-Mail-Vorlage für Aufgabenbenachrichtigungen

In der E-Mail-Vorlage können Sie die Metadateneigenschaft mithilfe der folgenden Syntax einschließen, wobei „amountRequested“ der Metadateneigenschaft `${amountRequested}` entspricht.

## Konfigurieren von „Aufgabe zuweisen“ zur Verwendung benutzerdefinierter Metadateneigenschaften

Nachdem die OSGi-Komponente erstellt und auf dem AEM-Server bereitgestellt wurde, konfigurieren Sie die Komponente „Aufgabe zuweisen“ wie unten gezeigt, um benutzerdefinierte Metadateneigenschaften zu verwenden.


![Aufgabenbenachrichtigung](assets/task-notification.PNG)

## Aktivieren der Verwendung benutzerdefinierter Metadateneigenschaften

![Benutzerdefinierte Metadateneigenschaften](assets/custom-meta-data-properties.PNG)

## Gehen Sie wie folgt vor, um dies auf Ihrem Server auszuprobieren:

* [Konfigurieren des Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=de#configuring-the-mail-service)
* Verknüpfen Sie eine gültige E-Mail-ID mit [der Admin-Benutzerin oder dem Admin-Benutzer](http://localhost:4502/security/users.html).
* Laden Sie die Datei [workflow-and-task-notification-template.zip](assets/workflow-and-task-notification-template.zip) herunter und installieren Sie sie mit [Package Manager](http://localhost:4502/crx/packmgr/index.jsp).
* Laden Sie das [adaptive Formular](assets/request-travel-authorization.zip) herunter und importieren Sie es über die Benutzeroberfläche [Formulare und Dokumente](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) in AEM.
* Implementieren und starten Sie das [benutzerdefinierte Bundle](assets/work-items-user-service-bundle.jar) mithilfe der [Web-Konsole](http://localhost:4502/system/console/bundles).
* [Zeigen Sie das Formular in einer Vorschau an und senden Sie es ab.](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Bei der Formularübermittlung wird die Benachrichtigung zur Aufgabenzuweisung an die E-Mail-ID gesendet, die mit der Admin-Benutzerin bzw. dem Admin-Benutzer verknüpft ist. Der folgende Screenshot zeigt eine beispielhafte Benachrichtigung zur Aufgabenzuweisung.

![Benachrichtigung](assets/task-nitification-email.png)

>[!NOTE]
>Die E-Mail-Vorlage für die Benachrichtigung „Aufgabe zuweisen“ muss im folgenden Format vorliegen:
>
> subject=Task Assigned – `${workitem_title}`
>
> „message=String“ steht dabei für Ihre E-Mail-Vorlage ohne Zeilenvorschubzeichen.

## Aufgabenkommentare in E-Mail-Benachrichtigungen „Aufgabe zuweisen“

In einigen Fällen können Sie die Kommentare von vorherigen Aufgabenbesitzenden in nachfolgende Aufgabenbenachrichtigungen einschließen. Den Code zum Erfassen des letzten Kommentars der Aufgabe finden Sie im Folgenden:

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

Das Bundle mit dem obigen Code kann [hier](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar) heruntergeladen werden.
