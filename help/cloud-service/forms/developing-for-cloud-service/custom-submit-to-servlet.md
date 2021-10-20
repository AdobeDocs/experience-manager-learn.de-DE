---
title: Benutzerdefinierten Übermittlungs-Handler erstellen
description: Senden des adaptiven Formulars an einen benutzerdefinierten Submit-Handler
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8852
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Erstellen eines Servlets zur Verarbeitung der gesendeten Daten

Starten Sie Ihr AEM-Banking-Projekt in IntelliJ.
Erstellen Sie ein einfaches Servlet, um die gesendeten Daten in die Protokolldatei auszugeben. Stellen Sie sicher, dass sich der Code im Kernprojekt befindet, wie im Screenshot unten dargestellt
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Benutzerdefinierte Übermittlung erstellen

Erstellen Sie Ihr benutzerdefiniertes Senden im Ordner &quot;app/bankingapplication&quot;auf die gleiche Weise wie im [frühere Versionen von AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
Der folgende Code in post.POST.jsp leitet die Anfrage einfach an das Servlet weiter, das auf /bin/formstutorial bereitgestellt wurde. Dies ist dasselbe Servlet, das im vorherigen Schritt erstellt wurde

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Adaptives Formular konfigurieren

Sie können Ihr adaptives Formular jetzt so konfigurieren, dass es an diesen benutzerdefinierten Submit-Handler mit dem Namen **Senden an AEM Servlet**



