---
title: AEM Forms mit JSON-Schema und -Daten[Teil 3]
seo-title: AEM Forms mit JSON-Schema und -Daten[Teil 3]
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
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---


# Speichern des JSON-Schemas in der Datenbank {#storing-json-schema-in-database}


Um Abfragen zu den gesendeten Daten vornehmen zu können, müssen wir das mit dem gesendeten Formular verknüpfte JSON-Schema speichern. Das JSON-Schema wird im Abfrage Builder zum Erstellen der Abfrage verwendet.

Wenn ein adaptives Formular gesendet wird, prüfen wir, ob sich das zugehörige JSON-Schema in der Datenbank befindet. Wenn das JSON-Schema nicht vorhanden ist, rufen wir das JSON-Schema ab und speichern das Schema in der entsprechenden Tabelle. Wir verknüpfen den Formularnamen auch mit dem JSON-Schema. Der folgende Screenshot zeigt die Tabelle, in der die JSON-Schema gespeichert werden.

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
>Beim Erstellen eines adaptiven Formulars können Sie entweder das JSON-Schema verwenden, das sich im Repository befindet, oder ein JSON-Schema hochladen. Der oben genannte Code funktioniert in beiden Fällen.

Das abgerufene Schema wird in der Datenbank unter Verwendung der standardmäßigen JDBC-Vorgänge gespeichert. Mit dem folgenden Code wird das Schema in die Datenbank eingefügt

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

Zusammenfassend haben wir bisher Folgendes getan

* Adaptives Formular basierend auf JSON-Schema erstellen
* Wenn das Formular zum ersten Mal gesendet wird, speichern wir das mit dem Formular verknüpfte JSON-Schema in der Datenbank.
* Die gebundenen Daten des adaptiven Formulars werden in der Datenbank gespeichert.

Als Nächstes verwenden Sie QueryBuilder, um die zu suchenden Felder basierend auf dem JSON-Schema anzuzeigen


