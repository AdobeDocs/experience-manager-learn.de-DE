---
title: AMS Dispatcher – Schreibgeschützte bzw. unveränderliche Dateien
description: Gründe, warum einige Dateien schreibgeschützt bzw. nicht bearbeitbar sind, und Vornehmen der gewünschten funktionalen Änderungen
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '826'
ht-degree: 100%

---

# Schreibgeschützte bzw. unveränderliche Dateien in AMS

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Allgemeine Protokolle](./common-logs.md)

## Beschreibung

In diesem Dokument wird beschrieben, welche Dateien gesperrt sind und nicht geändert werden können und wie Sie die gewünschten Konfigurationseinstellungen vornehmen können.

Wenn AMS ein System bereitstellt, wird eine Basiskonfiguration eingeführt, die alle Bestandteile funktionsfähig und sicher macht. AMS möchte sicherstellen, dass diese Dinge als Grundlage für Funktionalität und Sicherheit erhalten bleiben. Dazu werden einige Dateien als schreibgeschützt und unveränderlich markiert, sodass sie nicht verändert werden können.

Das Layout hindert Sie jedoch nicht daran, ihr Verhalten zu ändern und erforderliche Änderungen zu überschreiben. Anstatt diese Dateien zu ändern, überlagern Sie dann Ihre eigene Datei, die das Original ersetzt.

So können Sie auch sicher sein, dass die Dispatcher bei Patches von AMS mit den neuesten Fehlerbehebungen und Sicherheitsverbesserungen Ihre Dateien nicht verändern. Dann können Sie weiterhin von den Verbesserungen profitieren und nur die gewünschten Änderungen übernehmen.
![Zeigt eine Bowling-Bahn mit einer Kugel, die die Bahn entlang rollt. Ein Pfeil mit dem Wort „you“ zeigt auf die Kugel. Die Stoßstangen an den Fehlwurfrinnen sind erhöht, und über ihnen stehen die Worte „immutable files“ .(unveränderliche Dateien).](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
Wie im obigen Bild gezeigt, bedeuten unveränderliche Dateien nicht, dass Sie das Spiel nicht spielen können. Sie verhindern nur, dass Ihre Leistung beeinträchtigt wird, und halten Sie in der Spur. Diese Methode unterstützt die wichtigsten Funktionen:

- Anpassungen werden in ihren eigenen sicheren Bereichen vorgenommen
- Die Überlagerung benutzerdefinierter Änderungen entspricht den Überlagerungsmethoden in AEM
- Das Patchen von AMS-Konfigurationen kann ohne Ändern von Anpassungen durchgeführt werden
- Das Testen der Basisinstallation gegenüber angepassten Konfigurationen kann gleichzeitig durchgeführt werden, sodass sich feststellen lässt, ob die Probleme durch Anpassungen verursacht werden oder etwas Anderes. Welche Dateien?


Es folgt eine Liste von Dateien, die meist mit einem Dispatcher bereitgestellt werden:

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

Um festzustellen, welche Dateien unveränderlich sind, können Sie den folgenden Befehl auf einem Dispatcher ausführen:

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

## Vornehmen von Änderungen

### Variablen

Variablen ermöglichen es Ihnen, funktionale Änderungen vorzunehmen, ohne die Konfigurationsdateien selbst zu ändern. Bestimmte Elemente der Konfiguration können durch Verändern der Variablenwerte angepasst werden. Hier ein Beispiel aus der Datei `/etc/httpd/conf.d/dispatcher_vhost.conf`:

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

Sie sehen die Anweisung „DispatcherLogLevel“ mit einer Variablen von `DISP_LOG_LEVEL` anstelle des normalen Werts. Oberhalb dieses Code-Abschnitts wird auch eine include-Anweisung für eine Variablendatei angezeigt. Als Nächstes schauen wir uns die Variablendatei `/etc/httpd/conf.d/variables/ams_default.vars` an. Hier der Inhalt dieser Variablendatei:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Sie sehen oben, dass der aktuelle Wert der `DISP_LOG_LEVEL`-Variablen `info` ist. Dies kann angepasst werden, um den gewünschten number-Wert bzw. die gewünschte Ebene zu verfolgen oder zu debuggen. Die Steuerung der Protokollebene wird nun automatisch angepasst.

### Überlagerungsmethode

Ein Verständnis der include-Dateien der obersten Ebene ist wichtig, da Sie für jegliche Anpassung bei diesen ansetzen. Zunächst ein einfaches Beispiel: Wir haben ein Szenario, in dem wir einen neuen Domain-Namen hinzufügen möchten, der auf diesen Dispatcher verweisen soll. Die Beispiel-Domain ist hierbei we-retail.adobe.com. Kopieren Sie zunächst eine vorhandene Konfigurationsdatei in eine neue, in der Änderungen hingefügt werden können:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Wir haben die vorhandene Datei „aem_publish.vhost“ kopiert, da sie bereits alles enthält, was wir für den Anfang brauchen. Jetzt bearbeiten wir die neue Datei „weretail.vhost“ und nehmen die erforderlichen Änderungen vor.

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

Jetzt sind `ServerName` und `ServerAlias` entsprechend der neuen Domain-Namen aktualisiert, ebenso andere Breadcrumb-Header. Lassen Sie uns nun unsere neue Datei aktivieren, sodass Apache sie entsprechend verwendet:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Der Apache-Webserver weiß nun, dass für die Domain Traffic bereitgestellt werden soll. Allerdings müssen wir das Dispatcher-Modul darüber informieren, dass ein neuer Domain-Name vorhanden ist, der berücksichtigt werden muss. Wir beginnen mit der Erstellung einer neuen `*_vhost.any`-Datei `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` und fügen in dieser Datei den zu berücksichtigenden Domain-Namen ein:

```
"we-retail.adobe.com"
```

Nun müssen wir eine neue Farm-Datei erstellen, die unsere neue vhost-Eintragsdatei verwendet. Als Erstes kopieren wir dazu eine solide Startdatei in unsere eigene neue Datei.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Sehen wir uns jetzt die Änderungen an, die wir an dieser Farm-Datei vornehmen müssen:

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

Nun haben wir den Farm-Namen und das darin verwendete include-Element im Abschnitt `/virtualhosts` der Farm-Konfiguration aktualisiert. Wir müssen diese neue Farm-Datei aktivieren, damit sie in der laufenden Konfiguration verwendet werden kann:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Nun laden wir einfach den Webserver-Dienst neu und nutzen unsere neue Domain.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Wir haben nur die Teile geändert, für die dies erforderlich gewesen ist, und die include-Elemente sowie den Code aus den Basiskonfigurationsdateien genutzt. Wir müssen nur das Element beschreiben, das wir ändern müssen. Das macht die Arbeit deutlich einfacher und bedeutet weniger Code, der gepflegt werden muss.
</div>

[Weiter -> Dispatcher-Konsistenzprüfung](./health-check.md)
