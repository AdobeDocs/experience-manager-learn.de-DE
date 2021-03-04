---
title: OSGi-Dienst erstellen
description: OSGi-Dienst zum Speichern der zu signierenden Formulare erstellen
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: Entwicklung
role: Entwickler
level: Erfahren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 1%

---


# OSGi-Dienst erstellen

Der folgende Code wurde geschrieben, um die zu unterschreibenden Formulare zu speichern. Jedes zu signierende Formular ist mit einer eindeutigen ID und einer Kunden-ID verknüpft. So können einem oder mehreren Formularen dieselbe Kunden-ID zugeordnet werden, dem Formular wird jedoch eine eindeutige GUID zugewiesen.

## Benutzeroberfläche

Im Folgenden finden Sie die verwendete Schnittstellendeklaration

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



## Daten einfügen

Die Methode zum Einfügen von Daten fügt eine Zeile in die Datenbank ein, die von der Datenquelle identifiziert wird. Jede Zeile in der Datenbank entspricht einem Formular und wird durch eine GUID und eine Kunden-ID eindeutig identifiziert. Die Formulardaten und die Formular-URL werden auch in dieser Zeile gespeichert. Die Statusspalte gibt an, ob das Formular ausgefüllt und unterzeichnet wurde. Der Wert 0 bedeutet, dass das Formular noch nicht unterzeichnet werden muss.

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


## Formulardaten abrufen

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

## Signaturstatus aktualisieren

Der erfolgreiche Abschluss der Unterzeichnungszeremonie Trigger und AEM Arbeitsablauf für das Formular. Der erste Schritt im Workflow ist ein Prozessschritt, der den Status in der Datenbank für die durch die GUID und Kunden-ID identifizierte Zeile aktualisiert. Der Wert des unterschriebenen Elements in den Formulardaten wurde auch auf Y gesetzt, um anzugeben, dass das Formular ausgefüllt und unterzeichnet wurde. Das adaptive Formular wird mit diesen Daten ausgefüllt und der Wert des unterschriebenen Datenelements in den XML-Daten wird verwendet, um die entsprechende Meldung anzuzeigen. Der Code updateSignatureStatus wird im benutzerdefinierten Prozessschritt aufgerufen.


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

## Nächstes Formular zum Signieren abrufen

Der folgende Code wurde verwendet, um das nächste Formular zum Signieren für eine bestimmte customerID mit dem Status 0 abzurufen. Wenn die sql-Abfrage keine Zeilen zurückgibt, geben wir die Zeichenfolge **&quot;AllDone&quot;** zurück, die angibt, dass es keine weiteren Formulare zum Signieren für die angegebene Kunden-ID gibt.

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

Das OSGi-Bundle mit den oben genannten Diensten kann [von hier heruntergeladen werden](assets/sign-multiple-forms.jar)