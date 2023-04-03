---
title: Einrichten von Sling Dynamic Include für AEM
description: Ein Video-Beispiel zur Installation und Verwendung von Apache Sling Dynamic Include mit AEM Dispatcher, der auf dem Apache HTTP Web Server ausgeführt wird.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 8%

---

# Setup [!DNL Sling Dynamic Include]

Video-Anleitung zur Installation und Verwendung von [!DNL Apache Sling Dynamic Include] mit [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=de) läuft [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Stellen Sie sicher, dass die neueste Version von AEM Dispatcher lokal installiert ist.

1. Laden Sie die [[!DNL Sling Dynamic Include] Bundle](https://sling.apache.org/downloads.cgi).
1. Konfigurieren [!DNL Sling Dynamic Include] über die [!DNL OSGi Configuration Factory] at **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Oder erstellen Sie, um eine AEM Code-Basis hinzuzufügen, die entsprechenden **sling:OsgiConfig** Knoten unter:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Optional) Wiederholen Sie den letzten Schritt, um Komponenten zuzulassen für [gesperrter (anfänglicher) Inhalt bearbeitbarer Vorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) über [!DNL SDI] sowie Der Grund für die zusätzliche Konfiguration besteht darin, dass gesperrte Inhalte bearbeitbarer Vorlagen aus `/conf` anstelle von `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Aktualisieren [!DNL Apache HTTPD Web server]s `httpd.conf` -Datei, um die [!DNL Include] -Modul.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aktualisieren Sie die [!DNL vhost] -Datei, die die Richtlinien einhält.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Aktualisieren Sie die Konfigurationsdatei &quot;dispatcher.any&quot;, um sie zu unterstützen (1). `nocache` Selektoren und (2) aktivieren TTL-Unterstützung.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Verlassen des Verlaufs `*` in der Globe `*.nocache.html*` -Regel, kann zu [Probleme in Anforderungen für Unter-Ressourcen](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Immer neu starten [!DNL Apache HTTP Web Server] , nachdem Sie Änderungen an den Konfigurationsdateien oder der `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Wenn Sie [!DNL Sling Dynamic Includes] für die Bereitstellung von Edge-Side Includes (ESI) müssen Sie dann sicherstellen, dass Sie die relevanten [Antwortheader im Dispatcher-Cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Mögliche Kopfzeilen sind:
>
>* &quot;Cache-Control&quot;
>* &quot;content-disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Ablaufdatum&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-content-type-options&quot;
>* &quot;Last-Modified&quot;
>


## Unterstützende Materialien

* [Herunterladen des Sling Dynamic Include-Bundles](https://sling.apache.org/downloads.cgi)
* [Dokumentation zu Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
