---
title: Sling Dynamic Include für AEM einrichten
description: Ein Video-Durchgang zum Installieren und Verwenden von Apache Sling Dynamic Include mit AEM Dispatcher, der auf dem Apache HTTP Web Server ausgeführt wird.
version: 6.3, 6.4, 6.5
sub-product: Stiftung, Standorte
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 9%

---


# Setup [!DNL Sling Dynamic Include]

Ein Video-Durchgang zum Installieren und Verwenden [!DNL Apache Sling Dynamic Include] mit [AEM Dispatcher](https://docs.adobe.com/content/help/de-DE/experience-manager-dispatcher/using/dispatcher.html) , auf dem ausgeführt wird [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Stellen Sie sicher, dass die neueste Version von AEM Dispatcher lokal installiert ist.

1. Laden Sie das [[!DNL Sling Dynamic Include] Bundle herunter und installieren Sie es](https://sling.apache.org/downloads.cgi).
1. Konfigurieren Sie [!DNL Sling Dynamic Include] die Konfiguration über [!DNL OSGi Configuration Factory] http://&lt;Host>:&lt;Port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration ****.

   Um einer AEM-Codebasis hinzuzufügen, erstellen Sie den entsprechenden Knoten **sling:OsgiConfig** unter:

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

1. (Optional) Wiederholen Sie den letzten Schritt, damit Komponenten auf [gesperrten (anfänglichen) Inhalten bearbeitbarer Vorlagen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) auch über [!DNL SDI] bereitgestellt werden können. Der Grund für die zusätzliche Konfiguration ist, dass gesperrter Inhalt von bearbeitbaren Vorlagen aus bereitgestellt wird `/conf` anstelle von `/content`.

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

1. Aktualisieren Sie [!DNL Apache HTTPD Web server]die `httpd.conf` -Datei, um das [!DNL Include] Modul zu aktivieren.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Aktualisieren Sie die [!DNL vhost] Datei, um Richtlinien einzuhalten.

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

1. Aktualisieren Sie die Konfigurationsdatei dispatcher.any, um (1) `nocache` Selektoren zu unterstützen und (2) die TTL-Unterstützung zu aktivieren.

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
   > Wenn Sie das nachgestellte Element `*` in der oben genannten Globus- `*.nocache.html*` Regel deaktivieren, kann dies zu [Problemen bei Anfragen nach Unterressourcen](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)führen.

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Nach Änderungen [!DNL Apache HTTP Web Server] an den Konfigurationsdateien oder der `dispatcher.any`Variablen müssen Sie immer neu starten.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Wenn Sie [!DNL Sling Dynamic Includes] zum Bereitstellen von Edge-Includes (ESI) verwenden, stellen Sie sicher, dass die entsprechenden [Antwortheader im Dispatcher-Cache zwischengespeichert werden](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Folgende Kopfzeilen sind möglich:
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Ablaufdatum&quot;
>* &quot;Zuletzt geändert&quot;
>* ‚ETag‘
>* &quot;X-Content-Type-Options&quot;
>* &quot;Zuletzt geändert&quot;

>



## Begleitmaterialien

* [Download Sling Dynamic Include Bundle](https://sling.apache.org/downloads.cgi)
* [Dokumentation zu Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
