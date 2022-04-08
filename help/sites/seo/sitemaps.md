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
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Sitemaps

Erfahren Sie, wie Sie Ihr SEO durch die Erstellung von Sitemaps für AEM Sites verbessern können.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Ressourcen

+ [AEM Sitemap-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap-Dokumentation](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap-Dokumentation](https://www.sitemaps.org/protocol.html)
+ [Dokumentation zur Sitemap-Indexdatei &quot;Sitemap&quot;](https://www.sitemaps.org/protocol.html#index)
+ [Hersteller](http://www.cronmaker.com/)

## Konfigurationen

### OSGi-Konfiguration des Sitemap Scheduler

Definiert die [OSGi-Werkskonfiguration](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) für die Häufigkeit (mithilfe von [Cron-Ausdrücke](http://www.cronmaker.com)) werden Sitemaps neu/generiert und in AEM zwischengespeichert.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher-Zulassungsfilterregel

Erlauben Sie HTTP-Anforderungen für die Sitemap-Index- und Sitemap-Dateien.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache-Webserver-Neuschreibungsregel

Sichern `.xml` sitemap-HTTP-Anfragen werden an die richtige zugrunde liegende AEM weitergeleitet. Wenn keine URL-Verkürzung verwendet wird oder Sling-Zuordnungen zum Erzielen einer URL-Verkürzung verwendet werden, ist diese Konfiguration nicht erforderlich.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
