---
title: AMS Dispatcher - Schreibgeschützte oder unveränderliche Dateien
description: Verstehen, warum einige Dateien schreibgeschützt oder nicht bearbeitbar sind und wie Sie die gewünschten funktionalen Änderungen vornehmen
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 1%

---


# Schreibgeschützte oder unveränderliche Dateien in AMS

[Inhalt](./overview.md)

[&lt;- Zurück: Allgemeine Protokolle](./common-logs.md)

## Beschreibung

In diesem Dokument wird beschrieben, welche Dateien gesperrt und nicht geändert werden und wie Sie die gewünschten Konfigurationseinstellungen ordnungsgemäß vornehmen.

Wenn AMS ein System bereitstellt, führen sie eine Basiskonfiguration ein, die alles funktionsfähig und sicher macht.  Dies sind Dinge, die AMS sicherstellen möchte, dass sie als Grundlage für Funktionalität und Sicherheit erhalten bleiben.  Um dies zu erreichen, werden einige Dateien als schreibgeschützt und unveränderlich markiert, um eine Änderung zu vermeiden.

Das Layout hindert Sie nicht daran, ihr Verhalten zu verändern und erforderliche Änderungen zu überschreiben.  Anstatt diese Dateien zu ändern, überlagern Sie Ihre eigene Datei, die das Original ersetzt.

Auf diese Weise können Sie sich auch sicher sein, dass die Dispatcher bei Patches von AMS mit den neuesten Fehlerbehebungen und Sicherheitsverbesserungen Ihre Dateien nicht verändern.  Dann können Sie weiterhin von den Verbesserungen profitieren und nur die gewünschten Änderungen übernehmen.
![Zeigt eine Bowling-Spur mit einer Kugel, die die Spur entlang rollt.  Der Ball hat einen Pfeil, der das Wort zeigt.  Die Bumper werden aufgezogen und haben die Worte unveränderliche Dateien über ihnen.](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Wie im obigen Bild gezeigt, halten unveränderliche Dateien Sie nicht davon ab, das Spiel zu spielen.  Sie halten Sie einfach davon ab, Ihre Leistung zu beeinträchtigen und halten Sie auf der Spur.  Diese Methode ermöglicht uns die wichtigsten Funktionen:

- Anpassungen werden in ihren eigenen sicheren Bereichen vorgenommen
- Die Überlagerung benutzerdefinierter Änderungen entspricht der Überlagerungsmethoden in AEM
- Das Patchen von AMS-Konfigurationen kann ohne Ändern von Anpassungen durchgeführt werden
- Das Testen der Basisinstallation und angepasster Konfigurationen kann gleichzeitig durchgeführt werden, um festzustellen, ob die Probleme durch Anpassungen verursacht werden oder etwas Anderes Welche Dateien?


Im Folgenden finden Sie eine typische Liste der Dateien, die mit einem Dispatcher bereitgestellt werden:

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Um festzustellen, welche Dateien unveränderlich sind, können Sie den folgenden Befehl auf einem Dispatcher ausführen, um ihn anzuzeigen:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Im Folgenden finden Sie eine Beispielantwort mit unveränderlichen Dateien:

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Änderungen vornehmen

### Variablen

Variablen ermöglichen es Ihnen, funktionale Änderungen vorzunehmen, ohne die Konfigurationsdateien selbst zu ändern.  Bestimmte Elemente der Konfiguration können mit der Anpassung der Variablenwerte angepasst werden.  Ein Beispiel, das wir aus der Datei hervorheben können `/etc/httpd/conf.d/dispatcher_vhost.conf` wird hier gezeigt:

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Erfahren Sie, wie die DispatcherLogLevel-Direktive eine Variable von `DISP_LOG_LEVEL` anstelle des Normalwerts, den Sie dort sehen würden.  Oberhalb dieses Codeabschnitts wird auch eine Include-Anweisung für eine Variablendatei angezeigt.  Die Variablendatei `/etc/httpd/conf.d/variables/ams_default.vars` ist, wo wir als Nächstes hinsehen wollen.  Im Folgenden finden Sie den Inhalt dieser Variablendatei:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Sie sehen oben, dass der aktuelle Wert von `DISP_LOG_LEVEL` ist `info`.  Wir können diese Einstellung anpassen, um die Werte oder Ebenen Ihrer Wahl zu verfolgen oder zu debuggen.  Überall, wo die Protokollebene gesteuert wird, wird sie automatisch angepasst.

### Überlagerungsmethode

Verstehen Sie die Include-Dateien der obersten Ebene, da Sie damit Ihre Anpassungen vornehmen können.  Zunächst ein einfaches Beispiel: Wir haben ein Szenario, in dem wir einen neuen Domänennamen hinzufügen möchten, auf den wir diesen Dispatcher verweisen möchten.  Das Domänenbeispiel, das wir verwenden werden, ist we-retail.adobe.com.  Kopieren Sie zunächst eine vorhandene Konfigurationsdatei in eine neue, in der wir unsere Änderungen hinzufügen können:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Wir haben die vorhandene Datei aem_publish.vhost kopiert, da sie bereits das enthält, was wir benötigen, um Dinge zum Laufen zu bringen, und wir möchten keinen bereits starken Start neu erfinden.  Jetzt bearbeiten wir die neue Datei weretail.vhost und nehmen die erforderlichen Änderungen vor.

Vorher:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Nachher:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Jetzt haben wir unsere `ServerName` und `ServerAlias` , um die neuen Domänennamen abzugleichen und andere Breadcrumb-Header zu aktualisieren.  Lassen Sie uns nun unsere neue Datei aktivieren, damit Apache wissen kann, wie sie unsere neue Datei verwenden kann:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Der Apache-Webserver weiß jetzt, dass die Domäne Traffic liefern soll. Wir müssen jedoch das Dispatcher-Modul darüber informieren, dass ein neuer Domänenname vorhanden ist, der berücksichtigt werden muss.  Wir beginnen mit der Erstellung eines neuen `*_vhost.any` file `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` und in dieser Datei werden wir den Domänennamen einfügen, den wir berücksichtigen möchten:

```
"we-retail.adobe.com"
```

Jetzt müssen wir eine neue Farm-Datei erstellen, die unsere neue vhost-Eintragsdatei verwendet, und wir beginnen mit dem Kopieren einer starken Start-Datei in unsere neue.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Zeigt die Änderungen an, die wir an dieser Farm-Datei vornehmen müssen

Vorher:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Nachher:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Jetzt haben wir den Farm-Namen aktualisiert und die darin verwendete Include-Datei im `/virtualhosts` -Abschnitt der Farm-Konfiguration.  Diese neue Farm-Datei muss aktiviert werden, damit sie in der laufenden Konfiguration verwendet werden kann:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Jetzt laden wir einfach den Webserver-Dienst neu und nutzen unsere neue Domain!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Beachten Sie, dass wir nur die Teile geändert haben, die wir ändern mussten, um die vorhandenen Includes und den Code zu nutzen, die mit den Basiskonfigurationsdateien geliefert wurden.  Wir müssen nur auf das Element eingehen, das wir ändern müssen.  Erleichtert die Arbeit und ermöglicht es uns, weniger Code zu verwalten
</div>