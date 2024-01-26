---
title: AEM Forms mit JSON-Schema und -Daten [Teil 2]
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 100%

---

# Speichern gesendeter Daten in Datenbank


>[!NOTE]
>
>Es wird empfohlen, MySQL 8 als Ihre Datenbank zu verwenden, da es den JSON-Datentyp unterstützt. Außerdem müssen Sie den entsprechenden Treiber für MySQL DB installieren. Ich habe den Treiber verwendet, der hier verfügbar ist: https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Um die gesendeten Daten in der Datenbank zu speichern, schreiben wir ein Servlet, um die gebundenen Daten sowie den Namen und den Speicher des Formulars zu extrahieren. Der vollständige Code zur Verarbeitung der Formularübermittlung und zum Speichern der afBoundData in der Datenbank ist unten angegeben.

Wir haben eine benutzerdefinierte Übermittlung erstellt, um die Formularübermittlung zu handhaben. In der post.POST.jsp dieses benutzerdefinierten Übermittlungsvorgangs senden wir die Anfrage an unser Servlet weiter.

Weiterführende Informationen zu benutzerdefinierten Übermittlungsaktionen finden Sie in diesem [Artikel](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

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

![Connection Pool](assets/connectionpooled.gif)

Gehen Sie wie folgt vor, um das in Ihrem System zu verwenden

* [Laden Sie die ZIP-Datei herunter und entpacken Sie sie ](assets/aemformswithjson.zip)
* Erstellen Sie ein adaptives Formular mit JSON-Schema. Sie können das JSON-Schema verwenden, das als Teil dieses Artikel-Assets bereitgestellt wird. Stellen Sie sicher, dass die Übermittlungsaktion des Formulars entsprechend konfiguriert ist. Die Übermittlungsaktion muss gemäß „CustomSubmitHelpx“ konfiguriert werden.
* Erstellen Sie ein Schema in Ihrer MySQL-Instanz, indem Sie die Datei schema.sql mit dem MySQL Workbench-Tool importieren. Die schema.sql-Datei wird Ihnen auch im Rahmen dieses Tutorials-Assets zur Verfügung gestellt.
* Konfigurieren Sie die Apache Sling Connection Pooled DataSource über die Felix-Web-Konsole
* Vergewissern Sie sich, dass Sie Ihren Datenquellennamen „aemformswithjson“ nennen. Dies ist der Name, der vom Beispiel-OSGi-Bundle verwendet wird, das Ihnen bereitgestellt wird.
* Eigenschaften finden Sie in der Abbildung oben. Dies setzt voraus, dass Sie MySQL als Ihre Datenbank verwenden werden.
* Stellen Sie das oder die OSGi-Bundle/-s bereit, das oder die als Teil dieser Artikel-Assets bereitgestellt wird/werden.
* Sehen Sie sich eine Vorschau des Formulars an und senden Sie es.
* Die JSON-Daten werden in der Datenbank gespeichert, die beim Import der „schema.sql“-Datei erstellt wurde.
