---
title: Sitemaps
description: Erfahren Sie, wie Sie Ihre SEO durch die Erstellung von Sitemaps für AEM Sites verbessern können.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 96%

---

# Sitemaps

Erfahren Sie, wie Sie Ihre SEO durch die Erstellung von Sitemaps für AEM Sites verbessern können.

>[!WARNING]
>
>In diesem Video wird die Verwendung von relativen URLs in der Sitemap veranschaulicht. Sitemaps [sollten absolute URLs verwenden](https://sitemaps.org/protocol.html). Informationen dazu, wie Sie absolute URLs aktivieren, finden Sie unter [Konfigurationen](#absolute-sitemap-urls), da dies im Video unten nicht behandelt wird.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Konfigurationen

### Absolute Sitemap-URLs{#absolute-sitemap-urls}

Die Sitemap von AEM unterstützt absolute URLs durch Verwendung von [Sling-Zuordnung](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Dies geschieht durch Erstellen von Zuordnungsknoten auf den AEM-Services, die Sitemaps generieren (normalerweise der AEM-Publish-Service).

Eine Beispieldefinition eines Sling-Zuordnungsknotens für `https://wknd.com` kann unter `/etc/map/https` wie folgt definiert werden:

| Pfad | Eigenschaftsname | Eigenschaftstyp | Eigenschaftswert |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Zeichenfolge | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Zeichenfolge | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Zeichenfolge | `wknd.com/$1` |

Die folgende Abbildung zeigt eine ähnliche Konfiguration, jedoch für `http://wknd.local` (eine lokale Zuordnung der Host-Namen, die auf `http` läuft).

![Konfiguration absoluter Sitemap-URLs](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### OSGi-Konfiguration der Sitemap-Planung

Definiert die [OSGi-Werkskonfiguration](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) für die Häufigkeit (mithilfe von [CRON-Ausdrücken](http://www.cronmaker.com/)), mit der Sitemaps generiert (oder neu generiert) und in AEM zwischengespeichert werden.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher-Zulassungsfilterregel

Zulassen von HTTP-Anfragen für die Sitemap-Index- und Sitemap-Dateien.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache-Webserver-Neuschreibungsregel

Stellen Sie sicher, dass `.xml` Sitemap-HTTP-Anfragen an die richtige zugrunde liegende AEM-Seite weitergeleitet werden.  Wenn keine URL-Verkürzung verwendet wird oder Sling-Zuordnungen zum Erzielen einer URL-Verkürzung verwendet werden, ist diese Konfiguration nicht erforderlich.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Ressourcen

+ [AEM-Dokumentation zu Sitemaps](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Apache Sling-Dokumentation zu Sitemaps](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Dokumentation zu Sitemaps von Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Dokumentation zu Sitemap-Indexdateien von Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)