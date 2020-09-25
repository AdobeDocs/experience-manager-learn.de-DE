---
title: Speichern und Abrufen von adaptiven Formulardaten
seo-title: Speichern und Abrufen von adaptiven Formulardaten
description: Speichern und Abrufen adaptiver Formulardaten aus der Datenbank. Mit dieser Funktion können Formularausfüller das Formular speichern und das Formular zu einem späteren Zeitpunkt ausfüllen.
seo-description: Speichern und Abrufen adaptiver Formulardaten aus der Datenbank. Mit dieser Funktion können Formularausfüller das Formular speichern und das Formular zu einem späteren Zeitpunkt ausfüllen.
feature: adaptive-forms
topics: developing
audience: developer,implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---


# Speichern und Abrufen von adaptiven Formulardaten

Dieser Artikel führt Sie durch die Schritte zum Speichern und Abrufen adaptiver Formulardaten aus der Datenbank. Die Daten des adaptiven Formulars wurden in der MySQL-Datenbank gespeichert. Auf hoher Ebene werden die folgenden Schritte unternommen, um den Verwendungsfall zu erreichen:

* [Datenquelle konfigurieren](#Configure-Data-Source)
* [Servlet zum Schreiben von Daten in die Datenbank erstellen](#create-servlet)
* [OSGI-Dienst zum Abrufen gespeicherter Daten erstellen](#create-osgi-service)
* [Client-Bibliothek erstellen](#create-client-library)
* [Erstellen von Vorlagen für adaptive Formulare und Seitenkomponenten](#form-template-and-page-component)
* [Funktionsnachweis](#capability-demo)
* [Auf dem Server bereitstellen](#deploy-on-your-server)

## Datenquelle konfigurieren {#Configure-Data-Source}

Apache Sling Connection Pooled DataSource ist so konfiguriert, dass sie auf die Datenbank verweist, in der die Daten des adaptiven Formulars gespeichert werden. Der folgende Screenshot zeigt die Konfiguration für meine Instanz. Die folgenden Eigenschaften können kopiert und eingefügt werden

* Datasource Name:aemformstutorial - Dies ist der Name, der in meinem Code verwendet wird.

* JDBC-Treiberklasse:com.mysql.jdbc.Driver

* JDBC-Verbindungs-URL:jdbc:mysql://localhost:3306/aemformstutorial

![connectionPool](assets/storingdata.PNG)

### Servlet erstellen {#create-servlet}

Im Folgenden sehen Sie den Code des Servlets, mit dem adaptive Formulardaten in die Datenbank eingefügt/aktualisiert werden. Die gepoolte DataSource für die Apache Sling-Verbindung wird mithilfe von AEM ConfigMgr konfiguriert und dasselbe wird in Zeile 26 referenziert. Der Rest des Codes ist ziemlich einfach. Der Code fügt entweder eine neue Zeile in die Datenbank ein oder aktualisiert eine vorhandene Zeile. Die gespeicherten Daten des adaptiven Formulars sind mit einer GUID verknüpft. Die gleiche GUID wird dann verwendet, um die Formulardaten zu aktualisieren.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
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
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
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
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
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
            return randomUUIDString;
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
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## OSGI-Dienst zum Abrufen von Daten erstellen {#create-osgi-service}

Der folgende Code wurde geschrieben, um die gespeicherten Daten des adaptiven Formulars abzurufen. Eine einfache Abfrage wird verwendet, um die mit einer bestimmten GUID verknüpften adaptiven Formulardaten abzurufen. Die abgerufenen Daten werden dann an die aufrufende Anwendung zurückgegeben. Dieselbe Datenquelle, die im ersten Schritt erstellt wurde und auf die in diesem Code verwiesen wird.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Client-Bibliothek erstellen {#create-client-library}

AEM Client Library verwaltet den gesamten clientseitigen JavaScript-Code. Für diesen Artikel habe ich ein einfaches JavaScript erstellt, um die adaptiven Formulardaten mit der Guide Bridge-API abzurufen. Sobald die Daten des adaptiven Formulars abgerufen wurden, wird die POST an das Servlet gesendet, um die Daten des adaptiven Formulars in die Datenbank einzufügen oder zu aktualisieren. Die Funktion getALLUrlParams gibt die Parameter in der URL zurück. Dies wird verwendet, wenn Sie die Daten aktualisieren möchten. Der Rest der Funktionalität wird im Code behandelt, der mit dem click-Ereignis der .savebutton-Klasse verknüpft ist. Wenn der guid-Parameter in der URL vorhanden ist, müssen wir den Aktualisierungsvorgang durchführen, wenn nicht ein Einfügevorgang.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Erstellen von Vorlagen für adaptive Formulare und Seitenkomponenten {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=9&learn=on)

### Nachweis der Fähigkeit {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)

#### Auf dem Server bereitstellen {#deploy-on-your-server}

Gehen Sie wie folgt vor, um diese Funktion auf Ihrer AEM Forms-Instanz zu testen

* [Laden Sie die Datei DemoAssets.zip auf Ihr lokales System herunter und dekomprimieren Sie sie.](assets/demoassets.zip)
* Stellen Sie die Pakete techmarketingdemos.jar und mysqldriver.jar mithilfe der Felix-Webkonsole bereit und Beginn bereit.
*** Importieren Sie aemformstutorial.sql mit MYSQL Workbench. Dadurch werden die erforderlichen Schemas und Tabellen in Ihrer Datenbank erstellt.
* Importieren Sie &quot;StoreAndRetrieve.zip&quot;mit AEM Package Manager. Dieses Paket enthält die Vorlage für das adaptive Formular, die Client-Bibliothek für die Seitenkomponente sowie die Beispiel-Konfiguration für das adaptive Formular und die Datenquelle.
* Melden Sie sich bei configMgr an. Suchen Sie nach &quot;Apache Sling Connection Pooled DataSource. Öffnen Sie den mit aemformstutorial verknüpften Datenquelleneintrag und geben Sie den Benutzernamen und das Kennwort für Ihre Datenbankinstanz ein.
* Adaptives Formular öffnen
* Füllen Sie einige Details aus und klicken Sie auf die Schaltfläche &quot;Speichern und weiter&quot;
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie auf der Registerkarte eines neuen Browsers ein
* Adaptives Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden**
