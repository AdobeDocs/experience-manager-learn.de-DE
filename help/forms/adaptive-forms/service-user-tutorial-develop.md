---
title: Entwicklung mit Dienstbenutzern in AEM Forms
seo-title: Entwicklung mit Dienstbenutzern in AEM Forms
description: Dieser Artikel führt Sie durch die Erstellung eines Dienstbenutzers in AEM Forms
seo-description: Dieser Artikel führt Sie durch die Erstellung eines Dienstbenutzers in AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Entwicklung mit Dienstbenutzern in AEM Forms

Dieser Artikel führt Sie durch die Erstellung eines Dienstbenutzers in AEM Forms

In früheren Versionen von Adobe Experience Manager (AEM) wurde der Verwaltungsressourcen-Auflöser für die Backend-Verarbeitung verwendet, für die der Zugriff auf das Repository erforderlich war. Die Verwendung des Verwaltungsressourcenauflösers wird in AEM 6.3 nicht mehr unterstützt. Stattdessen wird ein Systembenutzer mit bestimmten Berechtigungen im Repository verwendet.

Dieser Artikel erläutert schrittweise die Erstellung eines Systembenutzers und die Konfiguration der Eigenschaften der Benutzerzuordnung.

1. Navigieren Sie zu [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Als &#39; admin &#39; anmelden
1. Klicken Sie auf &#39; Benutzerverwaltung &#39;
1. Klicken Sie auf &quot; Systembenutzer erstellen&quot;
1. Legen Sie den Typ &quot;userid&quot;als &quot;data&quot;fest und klicken Sie auf das grüne Symbol, um den Vorgang zum Erstellen des Systembenutzers abzuschließen.
1. [Open configMgr](http://localhost:4502/system/console/configMgr)
1. Suchen Sie nach &#39; Apache Sling Service User Mapper Service &#39; und klicken Sie auf , um die Eigenschaften zu öffnen
1. Klicken Sie auf das Symbol *+* (Plus), um die folgende Dienstzuordnung hinzuzufügen

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Klicken Sie auf &#39; Speichern &#39;

In der obigen Konfiguration ist DevelopingWithServiceUser.core der symbolische Name des Bundles. getresourceresolver ist der Unterdienstname.data ist der Systembenutzer, der im vorherigen Schritt erstellt wurde.

Wir können auch Resource Resolver im Namen des fd-service-Benutzers abrufen. Dieser Dienstbenutzer wird für Dokument-Services verwendet. Wenn Sie z. B. Nutzungsrechte zertifizieren/anwenden usw. möchten, verwenden wir den Ressourcen-Auflöser des fd-service-Benutzers, um die Vorgänge auszuführen

1. [Laden Sie die mit diesem Artikel verknüpfte ZIP-Datei herunter und dekomprimieren Sie sie.](assets/developingwithserviceuser.zip)
1. Navigate to [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. OSGi-Bundle hochladen und Beginn daraus erstellen
1. Stellen Sie sicher, dass das Bundle sich im aktiven Status befindet.
1. Sie haben jetzt erfolgreich einen *Systembenutzer* erstellt und auch das *Dienstbenutzer-Bundle* bereitgestellt.

   Um Zugriff auf &quot;/content&quot;zu gewähren, müssen Sie dem Systembenutzer (&quot;data&quot;) Leserechte für den Knoten &quot;content&quot;erteilen.

   1. Navigieren Sie zu [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Suchen Sie nach Benutzerdaten. Dies ist der gleiche Systembenutzer, den Sie im vorherigen Schritt erstellt haben.
   1. Klicken Sie mit der Dublette auf den Benutzer und dann auf die Registerkarte &quot;Berechtigungen&quot;
   1. Zugriff auf den Ordner &quot;content&quot;durch den Benutzer &quot;read&quot;.
   1. Verwenden Sie den folgenden Code, um mithilfe des Dienstbenutzers Zugriff auf den Ordner &quot;/content&quot;zu erhalten

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Wenn Sie in Ihrem Bundle auf die Datei /content/dam/data.json zugreifen möchten, verwenden Sie den folgenden Code. Dieser Code setzt voraus, dass Sie dem Benutzer &quot;data&quot;auf dem Knoten /content/dam/ Leserechte erteilt haben

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   Der vollständige Code der Implementierung ist unten aufgeführt

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
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

