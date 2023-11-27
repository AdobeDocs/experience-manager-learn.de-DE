---
title: Erstellen eines benutzerdefinierten Übermittlungsaktions-Handlers
description: Senden eines adaptiven Formulars an einen benutzerdefinierten Übermittlungs-Handler
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
source-git-commit: b770fc33ee0752911135d1a94144406bad8f295b
workflow-type: tm+mt
source-wordcount: '0'
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

## Erstellen eines benutzerdefinierten Übermittlungs-Handlers 

Erstellen Sie Ihre benutzerdefinierte Übermittlungsaktion im Ordner `apps/bankingapplication` auf die gleiche Weise wie in [früheren Versionen von AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=de). Für diese Anleitung erstelle ich einen Ordner mit dem Namen SubmitToAEMServlet unter dem Knoten `apps/bankingapplication` im CRX-Repository.

Der folgende Code in post.POST.jsp leitet die Anfrage einfach an das Servlet weiter, das auf /bin/formstutorial bereitgestellt wurde. Dies ist dasselbe Servlet, das im vorherigen Schritt erstellt wurde

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

Klicken Sie in Ihrem AEM-Projekt in IntelliJ mit der rechten Maustaste auf den `apps/bankingapplication`-Ordner, wählen Sie „Neu“ > „Paket“ aus und geben Sie „SubmitToAEMServlet“ nach „apps.bankingapplication“ im neuen Paketdialogfeld ein. Klicken Sie mit der rechten Maustaste auf den SubmitToAEMServlet-Knoten und wählen Sie „Repo“ > „Befehl abrufen“ aus, um das AEM-Projekt mit dem AEM Server-Repository zu synchronisieren.


## Konfigurieren eines adaptiven Formulars

Sie können jetzt jedes adaptive Formular so konfigurieren, dass es an den benutzerdefinierten Submit-Handler mit dem Namen **Senden an AEM-Servlet** übermittelt

## Nächste Schritte

[Servlet anhand des Ressourcentyps registrieren](./registering-servlet-using-resourcetype.md)
