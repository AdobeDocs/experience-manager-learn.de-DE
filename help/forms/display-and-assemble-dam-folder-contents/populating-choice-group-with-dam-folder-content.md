---
title: Hinzufügen von DAM-Ordnerelementen zur Auswahlgruppen-Komponente
description: Dynamisches Hinzufügen von Elementen zur Auswahlgruppen-Komponente
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
duration: 100
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Dynamisches Hinzufügen von Elementen zur Auswahlgruppen-Komponente

AEM Forms 6.5 bietet die Möglichkeit, Elemente dynamisch zu einer Auswahlgruppen-Komponente adaptiver Formulare (z. B. Kontrollkästchen, Optionsfeld und Bildliste) hinzuzufügen. In diesem Artikel werden wir uns das Anwendungsbeispiel zum Ausfüllen einer Auswahlgruppen-Komponente mit dem DAM-Ordnerinhalt ansehen. Im Screenshot befinden sich die 3 Dateien im Ordner „Newsletter“. Jedes Mal, wenn ein neuer Newsletter zum Ordner hinzugefügt wird, wird die Auswahlgruppen-Komponente aktualisiert, um den Inhalt automatisch aufzulisten. Die Benutzerin bzw. der Benutzer kann einen oder mehrere Newsletter zum Herunterladen auswählen.

![Regeleditor](assets/newsletters-download.png)

## Erstellen eines Servlets, um den DAM-Ordnerinhalt zurückgeben

Der folgende Code wurde geschrieben, um den DAM-Ordnerinhalt im JSON-Format zurückzugeben.

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## Erstellen einer Client-Bibliothek mit JavaScript-Funktion

Das Servlet wird über eine JavaScript-Funktion aufgerufen. Die Funktion gibt ein Array-Objekt zurück, das zum Ausfüllen der Auswahlgruppen-Komponente verwendet wird

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## Erstellen eines adaptiven Formulars

Erstellen Sie ein adaptives Formular und verknüpfen Sie es mit der Client-Bibliothek **listfolderAssets**. Fügen Sie Ihrem Formular eine Kontrollkästchen-Komponente hinzu. Verwenden Sie den Regeleditor, um die Optionen des Kontrollkästchens wie im Screenshot gezeigt auszufüllen.
![set-options](assets/set-options-newsletter.png)

Hier wird die Javascript-Funktion **getDAMFolderAssets** aufgerufen und der Pfad zu den Assets des DAM-Ordners übergeben, die im Formular aufgelistet werden sollen.

## Nächste Schritte

[Zusammenführen ausgewählter Assets](./assemble-selected-newsletters.md)
