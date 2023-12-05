---
title: Einrichten von Sling Dynamic Include für AEM
description: Eine Videoanleitung zur Installation und Verwendung von Apache Sling Dynamic Include mit AEM Dispatcher auf einem Apache-HTTP-Webserver.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 910
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Setup [!DNL Sling Dynamic Include]

Eine Videoanleitung zur Installation und Verwendung von [!DNL Apache Sling Dynamic Include] mit [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=de) auf einem [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Stellen Sie sicher, dass die neueste Version von AEM Dispatcher lokal installiert ist.

1. Laden Sie das [[!DNL Sling Dynamic Include] Bundle](https://sling.apache.org/downloads.cgi) herunter und installieren Sie es.
1. Konfigurieren Sie [!DNL Sling Dynamic Include] über die [!DNL OSGi Configuration Factory] unter **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Oder erstellen Sie, um eine AEM-Code-Basis hinzuzufügen, den entsprechenden **sling:OsgiConfig**-Knoten unter:

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

1. (Optional) Wiederholen Sie den letzten Schritt, um zuzulassen, dass Komponenten zu [gesperrten (anfänglichen) Inhalten bearbeitbarer Vorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) ebenfalls über [!DNL SDI] bereitgestellt werden. Der Grund für die zusätzliche Konfiguration besteht darin, dass gesperrte Inhalte bearbeitbarer Vorlagen über `/conf` und nicht über `/content` bereitgestellt werden.

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

1. Aktualisieren Sie die Datei `httpd.conf` des [!DNL Apache HTTPD Web server]s, um das Modul [!DNL Include] zu aktivieren.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aktualisieren Sie die [!DNL vhost]-Datei, damit die Include-Anweisungen befolgt werden.

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

1. Aktualisieren Sie die Konfigurationsdatei „dispatcher.any“, um erstens `nocache`-Selektoren zu unterstützen und zweitens die TTL-Unterstützung zu aktivieren.

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
   > Wird das nachstehende `*` in der glob-Regel `*.nocache.html*` belassen, kann dies zu [Problemen bei Anfragen für Unterressourcen](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16) führen.

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Starten Sie den [!DNL Apache HTTP Web Server] immer neu, nachdem Sie Änderungen an den Konfigurationsdateien oder an `dispatcher.any` vorgenommen haben.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Wenn Sie [!DNL Sling Dynamic Includes] zur Bereitstellung von Edge-Side Includes (ESI) verwenden, müssen Sie sicherstellen, dass die relevanten [Antwort-Header im Dispatcher-Cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#CachingHTTPResponseHeaders) zwischengespeichert werden. Mögliche Header sind:
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Ablaufdatum&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>

## Hilfsmaterialien

* [Download des Sling Dynamic Include-Bundles](https://sling.apache.org/downloads.cgi)
* [Dokumentation zu Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
