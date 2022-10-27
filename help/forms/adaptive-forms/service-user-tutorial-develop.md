---
title: Entwickeln mit Dienstbenutzern in AEM Forms
description: Dieser Artikel führt Sie durch den Prozess der Erstellung eines Dienstbenutzers in AEM Forms
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 2%

---

# Entwickeln mit Dienstbenutzern in AEM Forms

Dieser Artikel führt Sie durch den Prozess der Erstellung eines Dienstbenutzers in AEM Forms

In früheren Versionen von Adobe Experience Manager (AEM) wurde der administrative Ressourcen-Resolver für die Backend-Verarbeitung verwendet, für die der Zugriff auf das Repository erforderlich war. Die Verwendung des administrativen Ressourcen-Resolvers wird in AEM 6.3 nicht mehr unterstützt. Stattdessen wird ein Systembenutzer mit spezifischen Berechtigungen im Repository verwendet.

Weitere Informationen zu [Erstellen und Verwenden von Dienstbenutzern in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

Dieser Artikel erläutert die Erstellung eines Systembenutzers und die Konfiguration der Eigenschaften des Benutzer-Mappers.

1. Navigieren Sie zu [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Als &quot;Administrator&quot;anmelden
1. Klicken Sie auf &quot;Benutzerverwaltung&quot;
1. Klicken Sie auf &quot; Systembenutzer erstellen &quot;
1. Legen Sie den Benutzereridtyp auf &quot;Daten&quot;fest und klicken Sie auf das grüne Symbol, um die Erstellung des Systembenutzers abzuschließen.
1. [Öffnen Sie configMgr](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach _Apache Sling Service User Mapper Service_ und klicken Sie auf , um die Eigenschaften zu öffnen
1. Klicken Sie auf *+* Symbol (Plus) zum Hinzufügen der folgenden Dienstzuordnung

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Klicken Sie auf Speichern .

In der obigen Konfiguration ist die Einstellung DevelopingWithServiceUser.core der symbolische Name des Bundles. getresourceresolver ist der Subdienstname.data ist der Systembenutzer, der im vorherigen Schritt erstellt wurde.

Wir können auch den Resource Resolver im Namen des fd-service-Benutzers erhalten. Dieser Dienstbenutzer wird für Document Services verwendet. Wenn Sie z. B. Nutzungsrechte zertifizieren/anwenden usw. möchten, verwenden wir den Ressourcen-Resolver des fd-service-Benutzers, um die Vorgänge auszuführen.

1. [Laden Sie die mit diesem Artikel verknüpfte ZIP-Datei herunter und entpacken Sie sie.](assets/developingwithserviceuser.zip)
1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Laden Sie das OSGi-Bundle hoch und starten Sie es.
1. Stellen Sie sicher, dass das Bundle sich im aktiven Status befindet.
1. Sie haben jetzt erfolgreich eine *Systembenutzer* und bereitgestellt *Service User Bundle*.

   Um Zugriff auf /content zu gewähren, erteilen Sie dem Systembenutzer (&#39; data &#39;) Leseberechtigungen für den Inhaltsknoten.

   1. Navigieren Sie zu [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Suchen Sie nach Benutzerdaten &quot;. Dies ist derselbe Systembenutzer, den Sie im vorherigen Schritt erstellt haben.
   1. Doppelklicken Sie auf den Benutzer und klicken Sie dann auf die Registerkarte Berechtigungen .
   1. Gewähren Sie &quot; read &quot; Zugriff auf den Ordner &quot;content &quot;.
   1. Verwenden Sie den folgenden Code, um mithilfe des Dienstbenutzers Zugriff auf den Ordner /content zu erhalten



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Wenn Sie auf die Datei /content/dam/data.json in Ihrem Bundle zugreifen möchten, verwenden Sie den folgenden Code. Dieser Code setzt voraus, dass Sie dem Benutzer &quot;data&quot;im Knoten /content/dam/ Leseberechtigungen erteilt haben

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

Der vollständige Code der Implementierung ist unten aufgeführt.

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
