---
title: Erstellen Ihres ersten Servlets in AEM Forms
description: Erstellen Sie Ihr erstes Sling-Servlet, um Daten mit einer Formularvorlage zusammenzuführen.
feature: Adaptive Formulare
version: 6.4,6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# Sling Servlet

Ein Servlet ist eine Klasse, die verwendet wird, um die Funktionen von Servern zu erweitern, die Anwendungen hosten, auf die über ein Programmiermodell für Anforderungsantworten zugegriffen wird. Für solche Anwendungen definiert die Servlet-Technologie HTTP-spezifische Servlet-Klassen.
Alle Servlets müssen die Servlet-Schnittstelle implementieren, die Lebenszyklusmethoden definiert.


Ein Servlet in AEM kann als OSGi-Dienst registriert werden: Sie können SlingSafeMethodsServlet für schreibgeschützte Implementierung oder SlingAllMethodsServlet erweitern, um alle RESTful-Vorgänge zu implementieren.

## Servlet-Code

```java
import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

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

* Öffnen Sie **das Befehlszeilenfenster**
* Navigieren Sie zu `c:\aemformsbundles\learningaemforms\core`
* Führen Sie den Befehl `mvn clean install -PautoInstallBundle` aus.
* Mit dem obigen Befehl wird das Bundle automatisch erstellt und für Ihre AEM-Instanz bereitgestellt, die auf localhost:4502 ausgeführt wird.

Das Bundle ist auch am folgenden Speicherort `C:\AEMFormsBundles\learningaemforms\core\target` verfügbar. Das Bundle kann auch über die [Felix-Webkonsole in AEM bereitgestellt werden.](http://localhost:4502/system/console/bundles)


## Servlet-Resolver testen

Zeigen Sie Ihren Browser auf die [Servlet-Resolver-URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Dadurch erhalten Sie Informationen zum Servlet, das für einen bestimmten Pfad aufgerufen wird, wie im Screenshot unten dargestellt
![servlet-resolver](assets/servlet-resolver.JPG)

## Testen des Servlets mit Postman

![test-servlet-postman](assets/test-servlet-postman.JPG)
