---
title: AEM Forms mit JSON-Schema und Daten [Teil 3]
description: Mehrteiliges Tutorial, um Sie durch die Schritte zu führen, die zum Erstellen eines adaptiven Formulars mit JSON-Schema und zum Abfragen der gesendeten Daten erforderlich sind.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 412eea77-3cf4-43bb-9d2f-ae860cd9d3be
duration: 136
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 100%

---

# Speichern des JSON-Schemas in der Datenbank {#storing-json-schema-in-database}


Um die gesendeten Daten abfragen zu können, müssen wir das mit dem übermittelten Formular verknüpfte JSON-Schema speichern. Das JSON-Schema wird in Query Builder zum Erstellen der Abfrage verwendet.

Wenn ein adaptives Formular gesendet wird, überprüfen wir, ob das zugehörige JSON-Schema in der Datenbank enthalten ist. Wenn das JSON-Schema nicht vorhanden ist, rufen wir das JSON-Schema ab und speichern das Schema in der entsprechenden Tabelle. Wir verknüpfen auch den Formularnamen mit dem JSON-Schema. Der folgende Screenshot zeigt die Tabelle, in der die JSON-Schemata gespeichert sind.

![jsonschema](assets/jsonschemas.gif)

```java
public String getJSONSchema(String afPath) {
  // TODO Auto-generated method stub
  afPath = afPath.replaceAll("/content/dam/formsanddocuments/", "/content/forms/af/");
  Resource afResource = getResolver.getServiceResolver().getResource(afPath + "/jcr:content/guideContainer");
  javax.jcr.Node resNode = afResource.adaptTo(Node.class);
  String schemaNode = null;
  try {
   schemaNode = resNode.getProperty("schemaRef").getString();
  } catch (ValueFormatException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (PathNotFoundException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (RepositoryException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  if (schemaNode.startsWith("/content/dam")) {
   log.debug("The schema is in the dam");
   afResource = getResolver.getServiceResolver()
     .getResource(schemaNode + "/jcr:content/renditions/original/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }
  if (schemaNode.startsWith("/assets")) {
   afResource = getResolver.getServiceResolver()
     .getResource(afPath + "/jcr:content/guideContainer/" + schemaNode + "/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }

  return null;

 }
```

>[!NOTE]
>
>Beim Erstellen eines adaptiven Formulars können Sie entweder das JSON-Schema im Repository verwenden oder ein JSON-Schema hochladen. Der obige Code funktioniert in beiden Fällen.

Das abgerufene Schema wird mithilfe der standardmäßigen JDBC-Vorgänge in der Datenbank gespeichert. Mit dem folgenden Code wird das Schema in die Datenbank eingefügt:

```java
public void insertJsonSchema(JSONObject jsonSchema, String afForm) {
  log.debug("$$$$ in insert Schema" + afForm);
  log.debug("$$$$$ The jsonSchema is  " + jsonSchema);
  Connection con = getConnection();
  log.debug("$$$$ got connection is insertJsonSchema");
  String insertTableSQL = "INSERT INTO leads.jsonschemas(jsonschema,formname) VALUES(?,?)";
  PreparedStatement pstmt = null;
  try {

   // org.json.JSONObject jsonSchemaObj = new
   // org.json.JSONObject(jsonSchema);
   pstmt = con.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonSchema.toString());
   pstmt.setString(2, afForm);
   log.debug("Executing the insert  json schema statment  " + pstmt.executeUpdate());
   con.commit();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } finally {
   if (con != null) {
    try {
     con.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }

 }
```

Zusammenfassend wurden bisher folgende Schritte durchgeführt:

* Erstellen eines adaptiven Formulars basierend auf einem JSON-Schema
* Speichern des mit dem Formular verknüpften JSON-Schemas in der Datenbank bei der ersten Formularübermittlung
* Speichern der gebundenen Daten des adaptiven Formulars in der Datenbank

Der nächste Schritt besteht dann in der Verwendung von Query Builder zum Anzeigen der Felder, die basierend auf dem JSON-Schema durchsucht werden sollen.
