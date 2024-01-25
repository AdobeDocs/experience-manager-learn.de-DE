---
title: Erstellen Ihres ersten Servlets in AEM Forms
description: Erstellen Sie Ihr erstes Sling-Servlet, um Daten mit einer Formularvorlage zusammenzuführen.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
duration: 68
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 100%

---

# Sling-Servlet

Ein Servlet ist eine Klasse, die verwendet wird, um die Funktionen von Servern zu erweitern, die Anwendungen hosten, auf die über ein Anfrage-Antwort-Programmiermodell zugegriffen wird. Für solche Anwendungen definiert die Servlet-Technologie HTTP-spezifische Servlet-Klassen.
Alle Servlets müssen die Servlet-Schnittstelle implementieren, die Lebenszyklusmethoden definiert.


Ein Servlet in AEM kann als OSGi-Dienst registriert werden: Sie können das SlingSafeMethodsServlet für schreibgeschützte Implementierung oder das SlingAllMethodsServlet erweitern, um alle RESTful-Vorgänge zu implementieren.

## Servlet-Code

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## Erstellen und Bereitstellen

Gehen Sie wie folgt vor, um Ihr Projekt zu erstellen:

* Öffnen Sie ein **Eingabeaufforderungsfenster**.
* Navigieren Sie zu `c:\aemformsbundles\mysite\core`.
* Führen Sie den Befehl `mvn clean install -PautoInstallBundle` aus.
* Der obige Befehl erstellt das Bundle automatisch und stellt es in Ihrer AEM-Instanz auf localhost:4502 bereit.

Das Bundle ist auch am folgenden Speicherort verfügbar: `C:\AEMFormsBundles\mysite\core\target`. Das Bundle kann auch mithilfe der [Felix-Web-Konsole](http://localhost:4502/system/console/bundles) in AEM bereitgestellt werden.


## Testen des Servlet-Resolvers

Verweisen Sie den Browser auf die [Servlet-Resolver-URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Dies teilt Ihnen das Servlet mit, das für einen bestimmten Pfad aufgerufen wird, wie im Screenshot unten dargestellt
![servlet-resolver](assets/servlet-resolver.JPG)

## Testen des Servlets mit Postman

![Testen des Servlets mit Postman](assets/test-servlet-postman.JPG)

## Nächste Schritte

[Einschließen von JAR-Dateien von Drittanbietern](./include-third-party-jars.md)

