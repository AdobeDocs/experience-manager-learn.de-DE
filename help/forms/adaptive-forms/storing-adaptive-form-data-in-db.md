---
title: Speichern von adaptiven Formulardaten
description: Speichern adaptiver Formulardaten in der Datenbank als Teil Ihres AEM-Workflows
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
duration: 146
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# Speichern übermittelter adaptiver Formulardaten in der Datenbank

Es gibt mehrere Möglichkeiten, die übermittelten Formulardaten in der Datenbank Ihrer Wahl zu speichern. Eine JDBC-Datenquelle kann verwendet werden, um die Daten direkt in der Datenbank zu speichern. Es kann ein benutzerdefiniertes OSGI-Bundle geschrieben werden, um die Daten in der Datenbank zu speichern. In diesem Artikel wird der benutzerdefinierte Prozessschritt im AEM-Workflow zum Speichern der Daten verwendet.
Der Anwendungsfall besteht darin, einen AEM-Workflow bei der Übermittlung eines adaptiven Formulars auszulösen. Über einen Schritt im Workflow werden dann die übermittelten Daten in der Datenbank gespeichert.



## JDBC Connection Pool

* Navigieren Sie zu [ConfigMgr](http://localhost:4502/system/console/configMgr).

   * Suchen Sie nach „JDBC Connection Pool“. Erstellen Sie einen neuen Day Commons JDBC Connection Pool. Geben Sie die spezifischen Einstellungen für Ihre Datenbank an.

   * ![JDBC Connection Pool – OSGi-Konfiguration](assets/aemformstutorial-jdbc.png)

## Angeben von Datenbankdetails

* Suchen Sie nach dem Abschnitt zum **Angeben von Datenbankdetails**.
* Geben Sie die spezifischen Eigenschaften für Ihre Datenbank an.
   * DataSourceName: Name der zuvor konfigurierten Datenquelle
   * TableName: Name der Tabelle, in der die AF-Daten gespeichert werden sollen
   * FormName: Spaltenname für den Namen des Formulars
   * ColumnName: Spaltenname für die AF-Daten

  ![Angeben der Datenbankdetails – OSGi-Konfiguration](assets/specify-database-details.png)



## Code für die OSGi-Konfiguration

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## Lesen von Konfigurationswerten

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## Code zum Implementieren des Prozessschritts

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## Bereitstellen der Beispiel-Assets

* Stellen Sie sicher, dass Sie Ihren JDBC Connection Pool konfiguriert haben.
* Geben Sie die Datenbankdetails mithilfe von configMgr an.
* [Laden Sie die ZIP-Datei herunter und extrahieren Sie den Inhalt auf Ihre Festplatte](assets/article-assets.zip)

   * Stellen Sie die JAR-Datei mit der [AEM-Web-Konsole](http://localhost:4502/system/console/bundles) bereit. Diese JAR-Datei enthält den Code zum Speichern der Formulardaten in der Datenbank.

   * Importieren Sie die beiden ZIP-Dateien in [AEM mit Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dadurch erhalten Sie den [Beispiel-Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) und das [adaptive Beispielformular](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html), das den Workflow bei der Formularübermittlung auslöst. Achten Sie auf die Prozessargumente im Workflow-Schritt. Diese Argumente geben den Formularnamen und den Namen der Datendatei mit den Daten des adaptiven Formulars an. Die Datendatei wird im Payload-Ordner des CRX-Repositorys gespeichert. Das [adaptive Formular](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) ist so konfiguriert, dass der AEM-Workflow bei der Übermittlung und die Datendateikonfiguration (data.xml) ausgelöst werden.

   * Sehen Sie sich das Formular in einer Vorschau an und füllen Sie es aus. Übermitteln Sie es anschließend. Es sollte eine neue, in Ihrer Datenbank erstellte Zeile angezeigt werden.

