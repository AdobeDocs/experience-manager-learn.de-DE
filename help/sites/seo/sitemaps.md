---
title: Sitemaps
description: Erfahren Sie, wie Sie Ihr SEO durch die Erstellung von Sitemaps für AEM Sites verbessern können.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# Sitemaps

Erfahren Sie, wie Sie Ihr SEO durch die Erstellung von Sitemaps für AEM Sites verbessern können.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Ressourcen

+ [AEM Sitemap-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap-Dokumentation](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM WCM-Kernkomponenten-GitHub](https://github.com/adobe/aem-core-wcm-components)
   + In Version 2.17.6 hinzugefügte Sitemap-Funktionen
+ [Sitemap.org Sitemap-Dokumentation](https://www.sitemaps.org/protocol.html)
+ [Dokumentation zur Sitemap-Indexdatei &quot;Sitemap&quot;](https://www.sitemaps.org/protocol.html#index)
+ [Hersteller](http://www.cronmaker.com/)

## Konfigurationen

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

Definiert die [OSGi-Werkskonfiguration](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) für die Häufigkeit (mithilfe von [Cron-Ausdrücke](http://www.cronmaker.com)) werden Sitemaps neu/generiert und in AEM zwischengespeichert.

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

Erlauben Sie HTTP-Anforderungen für die Sitemap-Index- und Sitemap-Dateien.

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

Sichern `.xml` sitemap-HTTP-Anfragen werden an die richtige zugrunde liegende AEM weitergeleitet. Wenn keine URL-Verkürzung verwendet wird oder Sling-Zuordnungen zum Erzielen einer URL-Verkürzung verwendet werden, ist diese Konfiguration nicht erforderlich.

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
