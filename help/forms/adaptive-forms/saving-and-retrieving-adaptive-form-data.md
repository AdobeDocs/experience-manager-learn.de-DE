---
title: Speichern und Abrufen von Daten aus adaptiven Formularen
description: Speichern und Abrufen von Daten aus adaptiven Formularen aus der Datenbank. Mit dieser Funktion können Personen, die ein Formular ausfüllen, das Formular speichern und zu einem späteren Zeitpunkt weiter ausfüllen.
feature: Adaptive Forms
topic: Development
role: Developer
type: Tutorial
version: 6.4,6.5
last-substantial-update: 2019-06-09T00:00:00Z
duration: 691
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '614'
ht-degree: 100%

---


# Speichern und Abrufen von Daten aus adaptiven Formularen

In diesem Artikel werden Sie durch die Schritte geführt, die zum Speichern und Abrufen von Daten aus adaptiven Formularen aus der Datenbank erforderlich sind. Die MySQL-Datenbank wurde zum Speichern der Daten aus adaptiven Formularen verwendet. Allgemein gesprochen sind die folgenden Schritte erforderlich, um den Anwendungsfall zu erreichen:

* [Konfigurieren einer Datenquelle](#Configure-Data-Source)
* [Erstellen eines Servlets zum Schreiben von Daten in die Datenbank](#create-servlet)
* [Erstellen eines OSGi-Dienstes zum Abrufen gespeicherter Daten](#create-osgi-service)
* [Erstellen einer Client-Bibliothek](#create-client-library)
* [Erstellen einer adaptiven Formularvorlage und Seitenkomponente](#form-template-and-page-component)
* [Funktionsnachweis](#capability-demo)
* [Bereitstellen auf Ihrem Server](#deploy-on-your-server)

## Konfigurieren einer Datenquelle {#Configure-Data-Source}

Die Apache Sling Connection Pooled DataSource ist so konfiguriert, dass sie auf die Datenbank verweist, die zum Speichern der Daten aus adaptiven Formularen verwendet wird. Der folgende Screenshot zeigt die Konfiguration für meine Instanz. Die folgenden Eigenschaften können kopiert und eingefügt werden

* Datenquellenname: aemformstutorial – Dies ist der Name, der in meinem Code verwendet wird.

* JDBC-Treiberklasse: com.mysql.jdbc.Driver

* JDBC-Verbindungs-URL :jdbc:mysql://localhost:3306/aemformstutorial

![connectionpool](assets/storingdata.PNG)

### Erstellen eines Servlets {#create-servlet}

Im Folgenden finden Sie den Code des Servlets, das Daten aus adaptiven Formularen in die Datenbank einfügt/aktualisiert. Die Apache Sling Connection Pooled DataSource wird mit dem AEM ConfigMgr konfiguriert, was in Zeile 26 referenziert wird. Der Rest des Codes ist ziemlich einfach. Der Code fügt entweder eine neue Zeile in die Datenbank ein oder aktualisiert eine vorhandene Zeile. Die gespeicherten Daten des adaptiven Formulars sind mit einer GUID verknüpft. Dieselbe GUID wird dann zum Aktualisieren der Formulardaten verwendet.

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

## Erstellen eines OSGi-Dienstes zum Abrufen von Daten {#create-osgi-service}

Der folgende Code wurde geschrieben, um die gespeicherten Daten des adaptiven Formulars abzurufen. Über eine einfache Abfrage werden die mit einer bestimmten GUID verknüpften adaptiven Formulardaten abgerufen. Die abgerufenen Daten werden dann an die aufrufende Anwendung zurückgegeben. Auf dieselbe Datenquelle, die im ersten Schritt erstellt wurde, wird in diesem Code verwiesen.

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

## Erstellen einer Client-Bibliothek {#create-client-library}

Die AEM Client-Bibliothek verwaltet den gesamten Client-seitigen JavaScript-Code. Für diesen Artikel habe ich ein einfaches JavaScript erstellt, um die Daten aus adaptiven Formularen mithilfe der Guide Bridge-API abzurufen. Sobald die Daten des adaptiven Formulars abgerufen wurden, erfolgt der POST-Aufruf an das Servlet, um die Daten des adaptiven Formulars entweder in die Datenbank einzufügen oder zu aktualisieren. Die Funktion getALLUrlParams gibt die Parameter in der URL zurück. Sie wird verwendet, wenn Sie die Daten aktualisieren möchten. Der Rest der Funktionalität wird im Code verarbeitet, der mit dem Klick-Ereignis der .savebutton-Klasse verknüpft ist. Wenn der GUID-Parameter in der URL vorhanden ist, müssen wir den Aktualisierungsvorgang durchführen, falls es sich nicht um einen Einfügevorgang handelt.

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

## Erstellen einer adaptiven Formularvorlage und Seitenkomponente {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=12&learn=on)

### Funktionsnachweis {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

#### Bereitstellen auf Ihrem Server {#deploy-on-your-server}

Um diese Funktion auf Ihrer AEM Forms-Instanz zu testen, führen Sie die folgenden Schritte aus

* [Laden Sie DemoAssets.zip auf Ihr lokales System herunter und entpacken Sie es.](assets/demoassets.zip)
* Stellen Sie die Bundles „techmarketingdemos.jar“ und „mysqldriver.jar“ mithilfe der Felix-Webkonsole bereit und starten Sie sie.
*** Importieren Sie aemformstutorial.sql mit MYSQL Workbench. Dadurch werden das erforderliche Schema und die Tabellen in Ihrer Datenbank erstellt
* Importieren Sie StoreAndRetrieve.zip mit AEM Package Manager. Dieses Paket enthält die Vorlage für adaptive Formulare, die Client-Bibliothek für Seitenkomponenten sowie die Beispielkonfiguration für adaptive Formulare und Datenquellen.
* Melden Sie sich bei configMgr an. Suchen Sie nach „Apache Sling Connection Pooled DataSource“. Öffnen Sie den mit aemformstutorial verknüpften Datenquelleneintrag und geben Sie den Benutzernamen und das Passwort für Ihre Datenbankinstanz ein.
* Öffnen Sie das adaptive Formular.
* Geben Sie einige Daten ein und klicken Sie auf die Schaltfläche „Speichern und später fortfahren“.
* Sie sollten eine URL mit einer GUID zurückerhalten.
* Kopieren Sie die URL und fügen Sie sie in eine neue Registerkarte des Browsers ein.
* Das adaptive Formular sollte mit den Daten aus dem vorherigen Schritt gefüllt werden**
