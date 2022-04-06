---
title: Speichern und Fortsetzen von Briefen
seo-title: Save and resume letters
description: Erfahren Sie, wie Sie Entwurfsbriefe speichern und abrufen
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Entwürfe speichern

Der folgende Code wird zum Speichern der Briefinstanz verwendet. Die Metadaten der Briefinstanz werden im _Abzüge_ Tabelle. Eine eindeutige Zeichenfolge (draftID) wird generiert und zurückgegeben. Diese eindeutige Zeichenfolge wird dann zum Abrufen der gespeicherten Briefinstanz verwendet.

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## Brief abrufen

Der folgende Code wurde geschrieben, um den gespeicherten Briefentwurf abzurufen.
Zum Laden gespeicherter Briefinstanzen müssen Sie die draftID angeben. Basierend auf dieser draftID rufen wir die Datenbank ab, um die zusätzlichen Metadaten zum Brief zu erhalten. Dieselbe draftID wird verwendet, um die Daten des Briefs zu erstellen, indem die entsprechende XML aus dem Dateisystem gelesen wird. Anschließend wird ein CCRDocumentInstance -Objekt erstellt und zurückgegeben.


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### Brief aktualisieren

Der folgende Code wurde verwendet, um die gespeicherte Briefinstanz zu aktualisieren. Die Daten des aktualisierten Briefs werden mithilfe der Brief-ID in das Dateisystem geschrieben.

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
		Document icData = letterInstanceToUpdate.getData();
		String draftID = letterInstanceToUpdate.getId();
		log.debug("updating letter instance with draft id =  "+draftID);
		try
			{
				icData.copyToFile(new File(draftID+".xml"));
			} 
		catch (IOException e)
			{
				log.debug("Error updating "+e.getMessage());;
			}
		
	}
```

### Alle gespeicherten Briefe abrufen

AEM Forms bietet keine vorkonfigurierte Benutzeroberfläche zum Auflisten der gespeicherten Briefe. Für diesen Artikel zeige ich die gespeicherten Briefinstanzen mithilfe eines adaptiven Formulars in einem tabellarischen Format auf.
Sie können die Abfrage so anpassen, dass die gespeicherten Briefinstanzen abgerufen werden. In diesem Beispiel frage ich mich nach der gespeicherten Briefinstanz durch &quot;admin&quot;.

```java
	public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
	  String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
	  Connection connection = getConnection();
	  Statement statement = null;
	  String documentID = "";
	  List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
	  String draftID;
	  String savedInstanceName = "";
	  try {
	    statement = connection.createStatement();
	    ResultSet rs = statement.executeQuery(selectStatement);
	    while (rs.next()) {
	      documentID = rs.getString("documentID");
	      draftID = rs.getString("draftID");
	      savedInstanceName = rs.getString("name");
	      Document draftData = new Document(new File(draftID + ".xml"));
	      CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
	      listOfDrafts.add(draftLetter);
	    }
	  } catch (SQLException e) {
	    log.debug("The error is " + e.getMessage());
	  } finally {
	    if (statement != null) {
	      try {
	        statement.close();
	      } catch (SQLException e) {
	        log.debug("error in closing statement" + e.getMessage());
	      }
	    }
	    if (connection != null) {
	      try {
	        connection.close();
	      } catch (SQLException e) {
	        log.debug("error in closing connection" + e.getMessage());
	      }
	    }
	  }

	  return listOfDrafts;
	}
```

### Eclipse-Projekt

Das Eclipse-Projekt mit Beispielimplementierung kann [heruntergeladen von hier](assets/icdrafts-eclipse-project.zip)

