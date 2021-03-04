---
title: AEM Forms mit JSON-Schema und -Daten[Teil 2]
seo-title: AEM Forms mit JSON-Schema und -Daten[Teil 2]
description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
seo-description: Mehrteilige Übung, um Sie durch die Schritte zu führen, die beim Erstellen eines adaptiven Formulars mit JSON-Schema und beim Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---


# Speichern gesendeter Daten in der Datenbank


>[!NOTE]
>
>Es wird empfohlen, MySQL 8 als Datenbank zu verwenden, da es den JSON-Datentyp unterstützt. Außerdem müssen Sie den entsprechenden Treiber für MySQL DB installieren. Ich habe den unter diesem Speicherort verfügbaren Treiber verwendet https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Um die gesendeten Daten in der Datenbank zu speichern, schreiben wir ein Servlet, um die gebundenen Daten sowie den Formulardamen und die Speicherung zu extrahieren. Der vollständige Code zur Verarbeitung der Formularübermittlung und zur Speicherung der afBoundData in der Datenbank ist unten aufgeführt.

Wir haben eine benutzerdefinierte Übermittlung erstellt, um die Formularübermittlung zu bearbeiten. In diesem benutzerspezifischen submit&#39;s post.POST.jsp senden wir die Anfrage an unser Servlet.

Weitere Informationen zu benutzerdefinierten Übermittlungsbedingungen finden Sie in diesem [Artikel](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/stobekräfsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionPool](assets/connectionpooled.gif)

Gehen Sie wie folgt vor, um das System funktionsfähig zu machen

* [Herunterladen und Entpacken der ZIP-Datei](assets/aemformswithjson.zip)
* Erstellen Sie adaptives Formular mit JSON-Schema. Sie können das JSON-Schema als Teil dieser Artikelelemente verwenden. Stellen Sie sicher, dass die Sendeaktion des Formulars ordnungsgemäß konfiguriert ist. Die Übermittlungsaktion muss mit &quot;CustomSubmitHelpx&quot;konfiguriert werden.
* Erstellen Sie ein Schema in Ihrer MySQL-Instanz, indem Sie die Datei &quot;Schema.sql&quot;mit dem Tool &quot;MySQL Workbench&quot;importieren. Die Datei &quot;Schema.sql&quot;wird Ihnen auch als Teil dieser Lernelemente bereitgestellt.
* Apache Sling Connection Pooled DataSource über die Felix-Webkonsole konfigurieren
* Achten Sie darauf, den Namen Ihrer Datenquelle &quot;aemformswithjson&quot;zu nennen. Dies ist der Name, der vom Beispiel-OSGi-Bundle verwendet wird, das Ihnen zur Verfügung gestellt wird
* Die Eigenschaften finden Sie im obigen Bild. Dies setzt voraus, dass Sie MySQL als Datenbank verwenden werden.
* Stellen Sie die OSGi-Bundles bereit, die als Teil dieser Artikelelemente bereitgestellt werden.
* Vorschau des Formulars und Senden.
* Die JSON-Daten werden in der Datenbank gespeichert, die beim Importieren der Datei &quot;Schema.sql&quot;erstellt wurde.
