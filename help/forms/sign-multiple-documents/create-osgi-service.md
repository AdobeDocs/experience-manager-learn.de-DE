---
title: Erstellen eines OSGi-Dienstes
description: Erstellen eines OSGi-Dienstes zum Speichern der zu signierenden Formulare
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
jira: KT-6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
duration: 188
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 100%

---

# Erstellen eines OSGi-Dienstes

Der folgende Code wurde zum Speichern der zu signierenden Formulare geschrieben: Jedes zu signierende Formular ist mit einer eindeutigen Kennung und einer Kunden-ID verknüpft. So können einem oder mehreren Formularen dieselbe Kunden-ID zugeordnet werden, dem Formular wird jedoch eine eindeutige GUID zugewiesen.

## Benutzeroberfläche

Die folgende Schnittstellendeklaration wurde verwendet.

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Einfügen von Daten

Die Methode zum Einfügen von Daten fügt eine Zeile in die Datenbank ein, die von der Datenquelle identifiziert wird. Jede Zeile in der Datenbank entspricht einem Formular und wird durch eine GUID und eine Kunden-ID eindeutig identifiziert. Die Formulardaten und die Formular-URL werden ebenfalls in dieser Zeile gespeichert. In der Statusspalte wird angezeigt, ob das Formular ausgefüllt und signiert wurde. Der Wert 0 zeigt an, dass das Formular noch nicht signiert wurde.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Abrufen von Formulardaten

Der folgende Code wird verwendet, um adaptive Formulardaten abzurufen, die mit der angegebenen GUID verknüpft sind. Die Formulardaten werden dann zum Vorausfüllen des adaptiven Formulars verwendet.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Aktualisieren des Signaturstatus

Der erfolgreiche Abschluss der Signaturzeremonie löst einen mit dem Formular verbundenen AEM-Workflow aus. Der erste Schritt im Workflow ist ein Prozessschritt, der den Status in der Datenbank für die Zeile aktualisiert, die durch die GUID und die Kunden-ID identifiziert wird. Außerdem setzen wir den Wert des signierten Elements in den Formulardaten auf Y, um anzugeben, dass das Formular ausgefüllt und signiert wurde. Das adaptive Formular wird mit diesen Daten ausgefüllt und der Wert des signierten Datenelements in den XML-Daten wird verwendet, um die entsprechende Nachricht anzuzeigen. Der Code „updateSignatureStatus“ wird aus dem benutzerdefinierten Prozessschritt aufgerufen.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Abrufen des nächsten Formulars zum Signieren

Der folgende Code wurde verwendet, um das nächste Formular zum Signieren für eine bestimmte customerID mit dem Status 0 abzurufen: Wenn die SQL-Abfrage keine Zeilen zurückgibt, wird die Zeichenkette **„AllDone“** zurückgegeben, die anzeigt, dass für die angegebene Kundennummer keine weiteren Formulare zum Unterschreiben mehr vorhanden sind.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

Das OSGi-Bundle mit den oben genannten Diensten kann [hier heruntergeladen werden](assets/sign-multiple-forms.jar)

## Nächste Schritte

[Erstellen eines Haupt-Workflows zur Verarbeitung der ersten Formularübermittlung](./create-main-workflow.md)
