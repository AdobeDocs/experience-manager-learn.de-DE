---
title: Verwenden der GuideBridge-API für den Zugriff auf Formulardaten
description: Greifen Sie mithilfe der GuideBridge-API auf Formulardaten und Anhänge für ein auf einer Kernkomponente basierendes adaptives Formular zu.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
source-git-commit: 73c15a195c438dd7a07142bba719c6f820bf298a
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# Verwenden der GuideBridge-API zur POST von Formulardaten

Das &quot;Speichern und Fortsetzen&quot;eines Formulars ermöglicht es Benutzern, den Fortschritt beim Ausfüllen des Formulars zu speichern und es zu einem späteren Zeitpunkt wieder aufzunehmen.
Die folgenden Schritte wurden zur Verwendung der Geolocation-API in Adaptive Forms ausgeführt. Um dieses Anwendungsbeispiel zu erstellen, müssen wir mithilfe der GuideBridge-API auf die Formulardaten zugreifen und sie zum Speichern und Abrufen an den REST-Endpunkt senden.

Die Formulardaten werden mithilfe des Regeleditors beim Klicken auf eine Schaltfläche gespeichert
![rule-editor](assets/rule-editor.png)

Die folgende JavaScript-Funktion wurde geschrieben, um die Daten an den angegebenen Endpunkt zu senden

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST end point
* @param {string} endpoint in Stringformat
* @return {string} 
 */

function submitFormDataAndAttachments(endpoint) {

    guideBridge.getFormDataObject({
        success: function(resultObj) {
            afFormData = resultObj.data.data;
            var formData = new FormData();
            formData.append("dataXml", afFormData);
            for (i = 0; i < resultObj.data.attachments.length; i++) {
                var attachment = resultObj.data.attachments[i];
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            }
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    console.log("successfully saved");
                    var fld = guideBridge.resolveNode("$form.confirmation");
                    return " Form data was saved successfully";

                }
            };
            xhttp.open('POST', endpoint)
            xhttp.send(formData);
        }
    });

}
```



## Server-seitiger Code

Der folgende serverseitige Java-Code wurde geschrieben, um die Formulardaten zu verarbeiten. Im Folgenden finden Sie das Java-Servlet, das in AEM ausgeführt wird und über den XHR-Aufruf im obigen JavaScript aufgerufen wird.

```java
package com.azuredemo.core.servlets;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;
@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1 L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());
   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List < RequestParameter > listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of list is " + listOfRequestParameters.size());
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("is this request parameter a form field?" + requestParameter.isFormField());
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
