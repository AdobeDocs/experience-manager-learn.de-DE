---
title: Erstellen eines Servlets
description: Erstellen eines Servlets zur Verarbeitung der POST-Anfragen zum Speichern der Formulardaten
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6539
thumbnail: 6539.pg
topic: Development
role: Developer
level: Experienced
exl-id: a24ea445-3997-4324-99c4-926b17c8d2ac
duration: 41
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '88'
ht-degree: 100%

---

# Erstellen eines Servlets

Der nächste Schritt besteht darin, ein Servlet zu erstellen, das die entsprechenden Methoden unseres benutzerdefinierten OSGi-Dienstes aufruft. Das Servlet hat Zugriff auf die Daten des adaptiven Formulars und auf Informationen zu Dateianhängen.  Das Servlet gibt eine eindeutige Anwendungs-ID zurück, die zum Abrufen des teilweise ausgefüllten adaptiven Formulars verwendet werden kann.

Dieses Servlet wird aufgerufen, wenn die Benutzerin bzw. der Benutzer im adaptiven Formular auf die Schaltfläche „Speichern und Beenden“ klickt

```java
package saveandresume.core.servlets;

import java.io.PrintWriter;

import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
  private Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
  private static final long serialVersionUID = 1 L;
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;

  public void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    final String afData = request.getParameter("data");
    final String tel = request.getParameter("mobileNumber");
    log.debug("$$$The telephone number is  " + tel);
    log.debug("The request parameter data is  " + afData);
    try {
      JsonObject fileMap = JsonParser.parseString(request.getParameter("fileMap")).getAsJsonObject();
      log.debug("The file map is: " + fileMap.toString());
      String newFileMap = saveAndFetchFromDB.storeAFAttachments(fileMap, request);
      String application_id = saveAndFetchFromDB.storeFormData(afData, newFileMap, tel);
      log.debug("The application id:  " + application_id);
      JsonObject jsonObject = new JsonObject();
      jsonObject.addProperty("applicationID", application_id);
      response.setContentType("application/json");
      response.setHeader("Cache-Control", "nocache");
      response.setCharacterEncoding("utf-8");
      PrintWriter out = null;
      out = response.getWriter();
      out.println(jsonObject.toString());
    } catch (Exception ex) {
      log.error(ex.getMessage());
    }
  }

}
```

## Nächste Schritte

[Rendern eines Formulars mit gespeicherten Formulardaten](./retrieve-saved-form.md)
