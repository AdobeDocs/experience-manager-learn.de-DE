---
title: Entwickeln mit Dienstbenutzenden in AEM Forms
description: Dieser Artikel führt Sie durch die Erstellung einer Dienstbenutzerin bzw. eines Dienstbenutzers in AEM Forms.
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
duration: 159
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 100%

---

# Entwickeln mit Dienstbenutzenden in AEM Forms

Dieser Artikel führt Sie durch die Erstellung einer Dienstbenutzerin bzw. eines Dienstbenutzers in AEM Forms.

In früheren Versionen von Adobe Experience Manager (AEM) wurde der administrative Ressourcen-Resolver für die Backend-Verarbeitung verwendet, wofür Zugriff auf das Repository benötigt wurde. Die Nutzung des administrativen Ressourcen-Resolvers wird in AEM 6.3 nicht mehr unterstützt. Stattdessen wird eine Systembenutzerin bzw. ein Systembenutzer mit bestimmten Berechtigungen im Repository verwendet.

Weitere Informationen finden Sie unter [Erstellen und Verwenden von Dienstbenutzenden in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html?lang=de).

Dieser Artikel erläutert, wie Sie eine Systembenutzerin bzw. einen Systembenutzer erstellen und User-Mapper-Eigenschaften konfigurieren.

1. Navigieren Sie zu [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp).
1. Melden Sie sich als „admin“ an.
1. Klicken Sie auf „Benutzerverwaltung“.
1. Klicken Sie auf „Systembenutzer erstellen“.
1. Legen Sie den Benutzer-ID-Typ auf „Daten“ fest und klicken Sie auf das grüne Symbol, um die Erstellung der Systembenutzerin bzw. des Systembenutzers abzuschließen.
1. [Öffnen Sie „configMgr“](http://localhost:4502/system/console/configMgr).
1. Suchen Sie nach _Apache Sling Service User Mapper Service_ und klicken Sie darauf, um die Eigenschaften zu öffnen.
1. Klicken Sie auf das Symbol *+* (Plus), um die folgende Dienstzuordnung hinzuzufügen.

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Klicken Sie auf „Speichern“.

In der obigen Konfigurationseinstellung ist „DevelopingWithServiceUser.core“ der symbolische Name des Bundles. „getresourceresolver“ ist der Subservice-Name und „data“ ist die Systembenutzerin bzw. der Systembenutzer, die bzw. der im vorherigen Schritt erstellt wurde.

Wir können auch den Ressourcen-Resolver im Namen der fd-service-Benutzerin bzw. des fd-service-Benutzers abrufen. Diese Dienstbenutzerin bzw. dieser Dienstbenutzer wird für die Dokumentendienste verwendet. Wenn Sie z. B. Nutzungsrechte zertifizieren/anwenden möchten, verwenden wir den Ressourcen-Resolver der fd-service-Benutzerin bzw. des fd-service-Benutzers, um die Vorgänge durchzuführen.

1. [Laden Sie die mit diesem Artikel verknüpfte ZIP-Datei herunter und entpacken Sie sie.](assets/developingwithserviceuser.zip)
1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Laden Sie das OSGi-Bundle hoch und starten Sie es.
1. Stellen Sie sicher, dass sich das Bundle in einem aktiven Status befindet.
1. Sie haben nun erfolgreich eine *Systembenutzerin bzw. einen Systembenutzer* erstellt und zudem das *Bundle für Dienstbenutzende* bereitgestellt.

   Um Zugriff auf „/content“ zu gewähren, erteilen Sie der Systembenutzerin bzw. dem Systembenutzer („data“) Leseberechtigungen für den Inhaltsknoten.

   1. Navigieren Sie zu [http://localhost:4502/useradmin](http://localhost:4502/useradmin).
   1. Suchen Sie nach der Benutzerin bzw. dem Benutzer „data“. Hierbei handelt es sich um dieselbe Systembenutzerin bzw. denselben Systembenutzer, die bzw. den Sie im vorherigen Schritt erstellt haben.
   1. Doppelklicken Sie auf die Benutzerin bzw. den Benutzer und klicken Sie dann auf die Registerkarte „Berechtigungen“.
   1. Gewähren Sie Lesezugriff auf den Ordner „/content“.
   1. Verwenden Sie den folgenden Code, um mithilfe der Dienstbenutzerin bzw. des Dienstbenutzers Zugriff auf den Ordner „/content“ zu erhalten.



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Verwenden Sie den folgenden Code, wenn Sie auf die Datei „/content/dam/data.json“ in Ihrem Paket zugreifen möchten. Dieser Code setzt voraus, dass Sie der Benutzerin bzw. dem Benutzer „data“ im Knoten „/content/dam/“ Leseberechtigungen erteilt haben.

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

Der vollständige Code der Implementierung steht nachfolgend:

```java
package com.mergeandfuse.getserviceuserresolver.impl;
import java.util.HashMap;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = GetResolver.class)
public class GetResolverImpl implements GetResolver {
        @Reference
        ResourceResolverFactory resolverFactory;

        @Override
        public ResourceResolver getServiceResolver() {
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
