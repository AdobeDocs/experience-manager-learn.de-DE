---
title: AMS Dispatcher Basic File Layout
description: Grundlegendes zum Apache- und Dispatcher-Dateilayout.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1163'
ht-degree: 2%

---


# Grundlegendes Dateilayout

[Inhalt](./overview.md)

[&lt;- Zurück: Was ist der Dispatcher?](./what-is-the-dispatcher.md)

In diesem Dokument werden der Satz der standardmäßigen AMS-Konfigurationsdatei und die Überlegungen zu diesem Konfigurationsstandard erläutert

## Standard-Ordnerstruktur von Enterprise Linux

In AMS verwendet die Basisinstallation Enterprise Linux als Basissystem. Bei der Installation des Apache Webservers ist eine standardmäßige Installationsdatei festgelegt. Im Folgenden finden Sie die Standarddateien, die installiert werden, indem die grundlegenden RPMs installiert werden, die vom yum-Repository bereitgestellt werden

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Wenn Sie dem Installationsentwurf/der Installationsstruktur folgen und diese berücksichtigen, profitieren wir von den folgenden Vorteilen:

- Einfachere Unterstützung eines vorhersehbaren Layouts
- Automatisch mit allen Benutzern vertraut, die in der Vergangenheit an Enterprise Linux HTTPD-Installationen gearbeitet haben
- Ermöglicht Patchzyklen, die vom Betriebssystem vollständig unterstützt werden, ohne dass Konflikte oder manuelle Anpassungen auftreten
- Vermeidet SELinux-Verstöße gegen falsch gekennzeichnete Dateikontexte

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Die Bilder der Adobe Managed Services-Server verfügen in der Regel über kleine Root-Laufwerke des Betriebssystems.  Wir stellen unsere Daten in ein separates Volume ein, das normalerweise in "/mnt"bereitgestellt wird. Dann verwenden wir dieses Volumen anstelle der Standardwerte für die folgenden Standardverzeichnisse.

`DocumentRoot`
- Standard:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Standard: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Denken Sie daran, dass die alten und neuen Verzeichnisse dem ursprünglichen Bereitstellungspunkt zugeordnet werden, um Verwirrung zu vermeiden.
Die Verwendung eines separaten Volumens ist nicht unbedingt erforderlich, aber es ist wichtig
</div>

## AMS-Add-ons

AMS fügt zur Basisinstallation des Apache-Webservers hinzu.

### Dokumentenstamme

AMS-Standarddokumentstämme:
- Autor:
   - `/mnt/var/www/author/`
- Veröffentlichen:
   - `/mnt/var/www/html/`
- Wartung von &quot;Alles abfangen&quot;und &quot;Konsistenzprüfung&quot;
   - `/mnt/var/www/default/`

### Staging und Aktivierung von VirtualHost-Verzeichnissen

Mit den folgenden Verzeichnissen können Sie Konfigurationsdateien mit einem Staging-Bereich erstellen, in dem Sie Dateien bearbeiten und nur dann aktivieren können, wenn sie bereit sind.
- `/etc/httpd/conf.d/available_vhosts/`
   - Dieser Ordner hostet alle VirtualHost-/Dateien mit dem Namen `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Wenn Sie bereit sind, die `.vhost` -Dateien, die Sie in der `available_vhosts` -Ordner sie mithilfe eines relativen Pfads in `enabled_vhosts` directory

### Zusätzliche `conf.d` Verzeichnisse

Es gibt zusätzliche Elemente, die in Apache-Konfigurationen häufig vorkommen. Wir haben Unterverzeichnisse erstellt, um eine saubere Möglichkeit zu bieten, diese Dateien zu trennen und nicht alle Dateien in einem Verzeichnis zu haben

#### Verzeichnis neu schreibt

Dieser Ordner kann alle `_rewrite.rules` Dateien, die Sie erstellen und die Ihre typische RewriteRulesyntax enthalten, die für Apache-Webserver verwendet wird [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) Modul

- `/etc/httpd/conf.d/rewrites/`

#### Whitelist-Verzeichnis

Dieser Ordner kann alle `_whitelist.rules` von Ihnen erstellte Dateien, die Ihre `IP Allow` oder `Require IP`Syntax, mit der Apache-Webserver verbunden werden [Zugriffssteuerungen](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Variablenverzeichnis

Dieser Ordner kann alle `.vars` Dateien, die Sie erstellen und die Variablen enthalten, die Sie in Ihren Konfigurationsdateien verwenden können

- `/etc/httpd/conf.d/variables/`

### Dispatcher Module-spezifisches Konfigurationsverzeichnis

Der Apache-Webserver ist sehr erweiterbar. Wenn ein Modul viele Konfigurationsdateien hat, empfiehlt es sich, einen eigenen Konfigurationsordner unter dem Installationsgrundordner zu erstellen, anstatt den Standardordner zu überlasten.

Wir befolgen die Best Practice und haben unsere eigene

#### Verzeichnis der Modulkonfigurationsdatei

- `/etc/httpd/conf.dispatcher.d/`

#### Staging und aktivierte Landwirtschaft

Mit den folgenden Verzeichnissen können Sie Konfigurationsdateien mit einem Staging-Bereich erstellen, in dem Sie Dateien bearbeiten und nur dann aktivieren können, wenn sie bereit sind.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Dieser Ordner hostet alle Ihre `/myfarm {` Dateien namens `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Wenn Sie die Farm-Datei verwenden möchten, verknüpfen Sie sie im Ordner &quot;available_farms&quot;mit einem relativen Pfad zum Ordner &quot;enabled_farms&quot;.

### Zusätzliche `conf.dispatcher.d` Verzeichnisse

Es gibt zusätzliche Teile, die Unterabschnitte der Dateikonfigurationen der Dispatcher-Farm sind. Wir haben Unterverzeichnisse erstellt, um eine saubere Möglichkeit zu bieten, diese Dateien zu trennen und nicht alle Dateien in einem Verzeichnis zu haben

#### Cache-Verzeichnis

Dieses Verzeichnis enthält alle `_cache.any`, `_invalidate.any` -Dateien, die Sie erstellen und die Ihre Regeln enthalten, wie Sie möchten, dass das -Modul Caching-Elemente verarbeitet, die aus AEM stammen, sowie die Syntax der Invalidierungsregeln.  Weitere Informationen zu diesem Abschnitt finden Sie hier . [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Verzeichnis der Client-Header

Dieser Ordner kann alle `_clientheaders.any` -Dateien erstellen, die Listen von Client-Headern enthalten, die an AEM weitergegeben werden sollen, wenn eine Anforderung eingeht.  Weitere Informationen zu diesem Abschnitt finden Sie hier . [here](https://docs.adobe.com/content/help/de-DE/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-http-headers-to-pass-through-clientheaders)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Filterverzeichnis

Dieser Ordner kann alle `_filters.any` -Dateien, die Sie erstellen und die alle Filterregeln enthalten, um den Datenverkehr durch den Dispatcher zu blockieren oder zu ermöglichen, AEM zu erreichen

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Render Directory

Dieser Ordner kann alle `_renders.any` Dateien, die Sie erstellen und die Verbindungsdetails zu jedem Backend-Server enthalten, von dem der Dispatcher Inhalte von

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts-Verzeichnis

Dieser Ordner kann alle `_vhosts.any` Dateien, die Sie erstellen und die eine Liste der Domänennamen und Pfade enthalten, die einer bestimmten Farm mit einem bestimmten Back-End-Server zugeordnet werden sollen

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Vollständige Ordnerstruktur

AMS hat jede Datei mit benutzerdefinierten Dateierweiterungen strukturiert, um Namespace-Probleme/-Konflikte und Verwirrung zu vermeiden.

Im Folgenden finden Sie ein Beispiel für einen Standarddateisatz aus einer AMS-Standardbereitstellung:

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## Ideal beibehalten

Enterprise Linux verfügt über Patchzyklen für das Apache Webserver-Paket (httpd).

Je weniger installierte Standarddateien Sie ändern, desto besser. Wenn es gepatchte Sicherheitskorrekturen oder Konfigurationsverbesserungen gibt, werden diese über den RPM/Yum-Befehl angewendet und nicht über eine geänderte Datei angewendet.

Stattdessen wird eine `.rpmnew` -Datei neben dem Original.  Dies bedeutet, dass Sie einige Änderungen verpassen, die Sie möglicherweise gewünscht haben, und mehr Müll in Ihren Konfigurationsordnern erstellt haben.

Das RPM während der Installation der Aktualisierung wird sich also mit `httpd.conf` , wenn es sich im `unaltered` angeben, wird *replace* und Sie erhalten die wichtigsten Updates.  Wenn die Variable `httpd.conf` was `altered` dann *will nicht ersetzen* die Datei und erstellt stattdessen eine Referenzdatei mit dem Namen `httpd.conf.rpmnew` und die vielen gewünschten Fehlerbehebungen werden in dieser Datei enthalten sein, die nicht für das Starten des Dienstes gilt.

Enterprise Linux wurde ordnungsgemäß eingerichtet, um diesen Anwendungsfall besser zu handhaben.  Sie geben Ihnen Bereiche, in denen Sie die für Sie festgelegten Standardeinstellungen erweitern oder überschreiben können.  In der Basisinstallation von httpd finden Sie die Datei `/etc/httpd/conf/httpd.conf`und hat eine Syntax wie:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Die Idee ist, dass Apache Sie beim Hinzufügen neuer Dateien zum `/etc/httpd/conf.d/` und `/etc/httpd/conf.modules.d/` Verzeichnisse mit einer Dateierweiterung von `.conf`

Als perfektes Beispiel beim Hinzufügen des Dispatcher-Moduls zu Apache würden Sie ein Modul erstellen `.so` Datei in ` /etc/httpd/modules/` und schließen Sie sie dann ein, indem Sie eine Datei in `/etc/httpd/conf.modules.d/02-dispatcher.conf` mit den Inhalten zum Laden Ihres Moduls `.so` file

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Wir haben keine bereits vorhandenen Dateien geändert, die Apache bereitgestellt hat.  Stattdessen haben wir unsere zu den Verzeichnissen hinzugefügt, in die sie gehen sollten.
</div><br/>

Jetzt nutzen wir unser Modul in unserer Datei <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> , das unser Modul initialisiert und die anfängliche modulspezifische Konfigurationsdatei lädt

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Auch hier werden Sie feststellen, dass wir Dateien und Module hinzugefügt, aber keine Originaldateien verändert haben.  Dies gibt uns die gewünschte Funktionalität und schützt uns vor fehlenden Patchfixes und hält die höchste Kompatibilität mit jedem Upgrade des Pakets.

[Weiter -> Erläuterung der Konfigurationsdateien](./explanation-config-files.md)