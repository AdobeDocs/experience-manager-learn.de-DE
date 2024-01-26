---
title: AMS Dispatcher Standard-Datei-Layout
description: Grundlegendes zum Apache- und Dispatcher-Datei-Layout.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 354
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 100%

---

# Allgemeiner Dateiaufbau

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Was genau ist der „Dispatcher“?](./what-is-the-dispatcher.md)

In diesem Dokument werden der Satz der standardmäßigen AMS-Konfigurationsdatei und die Überlegungen zu diesem Konfigurationsstandard erläutert

## Standard-Ordnerstruktur von Enterprise Linux

In AMS verwendet die Basisinstallation Enterprise Linux als Basissystem. Bei der Installation des Apache-Webservers ist eine standardmäßige Installationsdatei festgelegt. Im Folgenden finden Sie die Standarddateien, die installiert werden, indem die grundlegenden RPMs installiert werden, die vom yum-Repository bereitgestellt werden

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
- Automatisch vertraut für alle, die in der Vergangenheit an Enterprise Linux HTTPD-Installationen gearbeitet haben
- Ermöglicht Patch-Zyklen, die vom Betriebssystem vollständig unterstützt werden, ohne dass Konflikte oder manuelle Anpassungen auftreten
- Vermeidet SELinux-Verstöße gegen falsch gekennzeichnete Dateikontexte

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Die Bilder der Adobe Managed Services-Server verfügen in der Regel über kleine Root-Laufwerke des Betriebssystems. Wir legen unsere Daten in einem separaten Volume ab, das normalerweise in `/mnt` gemountet wird 
Dann verwenden wir dieses Volume anstelle der Standardverzeichnisse für die folgenden Verzeichnisse

`DocumentRoot`
- Standard:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Standard: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Denken Sie daran, dass die alten und neuen Verzeichnisse dem ursprünglichen Bereitstellungspunkt zugeordnet werden, um Verwirrung zu vermeiden.
Die Verwendung eines separaten Volumes ist nicht unbedingt erforderlich, aber erwähnenswert.
</div>

## AMS-Add-ons

AMS fügt etwas zur Basisinstallation des Apache-Webservers hinzu.

### Dokumentenstämme

AMS-Standarddokumentenstämme:
- Author:
   - `/mnt/var/www/author/`
- Publish:
   - `/mnt/var/www/html/`
- Auffangfunktion und Wartung der Integritätsprüfungen
   - `/mnt/var/www/default/`

### Staging und Aktivierung von VirtualHost-Verzeichnissen

Mit den folgenden Verzeichnissen können Sie Konfigurationsdateien mit einem Staging-Bereich erstellen, in dem Sie Dateien bearbeiten und nur dann aktivieren können, wenn sie bereit sind.
- `/etc/httpd/conf.d/available_vhosts/`
   - Dieser Ordner hostet alle VirtualHost-/Dateien mit dem Namen `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Wenn Sie bereit sind, die `.vhost`-Dateien zu verwenden, müssen Sie sie im Ordner `available_vhosts` mit einem relativen Pfad in das Verzeichnis `enabled_vhosts` verlinken

### Zusätzliche `conf.d`-Verzeichnisse

Es gibt zusätzliche Elemente, die in Apache-Konfigurationen häufig vorkommen. Wir haben Unterverzeichnisse erstellt, um eine saubere Möglichkeit zu bieten, diese Dateien zu trennen und nicht alle Dateien in einem Verzeichnis zu haben.

#### Schreibt Verzeichnis neu

Dieses Verzeichnis kann alle von Ihnen erstellten `_rewrite.rules`-Dateien enthalten, die Ihre typische RewriteRule-Syntax enthalten, die das Modul [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) des Apache-Webservers aktiviert

- `/etc/httpd/conf.d/rewrites/`

#### Whitelists-Verzeichnis

Dieses Verzeichnis kann alle von Ihnen erstellten `_whitelist.rules`-Dateien enthalten, die Ihre typische `IP Allow`- oder `Require IP`-Syntax enthalten, die die [Zugriffskontrollen](https://httpd.apache.org/docs/2.4/howto/access.html) des Apache-Webservers beeinflussen.

- `/etc/httpd/conf.d/whitelists/`

#### Variablenverzeichnis

Dieser Ordner kann alle `.vars`-Dateien enthalten, die Sie erstellen und die Variablen enthalten, die Sie in Ihren Konfigurationsdateien verwenden können

- `/etc/httpd/conf.d/variables/`

### Dispatcher Module-spezifisches Konfigurationsverzeichnis

Der Apache-Webserver ist sehr erweiterbar. Wenn ein Modul viele Konfigurationsdateien hat, besteht die Best Practise darin, einen eigenen Konfigurationsordner unter dem Installationsgrundordner zu erstellen, anstatt den Standardordner zu überlasten.

Wir befolgen die Best Practice und haben unseren eigenen erstellt.

#### Verzeichnis der Modulkonfigurationsdatei

- `/etc/httpd/conf.dispatcher.d/`

#### Staging und aktivierte Farm

Mit den folgenden Verzeichnissen können Sie Konfigurationsdateien mit einem Staging-Bereich erstellen, in dem Sie Dateien bearbeiten und nur dann aktivieren können, wenn sie bereit sind.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Dieser Ordner hostet alle Ihre `/myfarm {`-Dateien namens `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Wenn Sie die Farm-Datei verwenden möchten, verknüpfen Sie sie im Ordner „available_farms“ mit einem relativen Pfad zum Ordner „enabled_farms“.

### Zusätzliche `conf.dispatcher.d`-Verzeichnisse

Es gibt zusätzliche Teile, die Unterabschnitte der Dateikonfigurationen der Dispatcher-Farm sind. Wir haben Unterverzeichnisse erstellt, um eine saubere Möglichkeit zu bieten, diese Dateien zu trennen und nicht alle Dateien in einem Verzeichnis zu haben.

#### Cache-Verzeichnis

Dieses Verzeichnis enthält alle `_cache.any`- und `_invalidate.any`-Dateien, die Sie erstellen und die Ihre Regeln dahingehend enthalten, wie Sie möchten, dass das Modul Caching-Elemente verarbeitet, die aus AEM stammen, sowie die Syntax der Invalidierungsregeln. Weitere Informationen zu diesem Abschnitt finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#configuring-the-dispatcher-cache-cache).

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Verzeichnis der Client-Header

Dieses Verzeichnis kann alle von Ihnen erstellten `_clientheaders.any`-Dateien enthalten, die an AEM durchgeleitet werden sollen, wenn eine Anfrage eingeht. Weitere Informationen zu diesem Abschnitt finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Filterverzeichnis

Dieses Verzeichnis kann alle von Ihnen erstellten `_filters.any`-Dateien enthalten, die alle Ihre Filterregeln zum Blockieren oder Zulassen von Datenverkehr durch den Dispatcher zum Erreichen von AEM enthalten

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Render-Verzeichnis

Dieses Verzeichnis kann alle von Ihnen erstellten `_renders.any`-Dateien enthalten, die die Verbindungsdetails zu den einzelnen Backend-Servern enthalten, von denen der Dispatcher Inhalte abrufen wird

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Vhosts-Verzeichnis

Dieses Verzeichnis kann alle von Ihnen erstellten `_vhosts.any`-Dateien enthalten, die eine Liste der Domain-Namen und Pfade für die Zuordnung einer bestimmten Farm zu einem bestimmten Backend-Server enthalten

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Vollständige Ordnerstruktur

AMS hat jede Datei mit benutzerdefinierten Dateierweiterungen strukturiert, um Probleme/Konflikte mit Namespaces und generell Verwirrung zu vermeiden.

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

## Um es möglichst ideal zu halten

Enterprise Linux verfügt über Patch-Zyklen für das Apache Webserver-Paket (httpd).

Je weniger installierte Standarddateien Sie ändern, desto besser. Wenn nämlich gepatchte Sicherheitskorrekturen oder Konfigurationsverbesserungen über den Befehl RPM / Yum angewendet werden, werden die Korrekturen nicht über eine geänderte Datei hinweg angewendet.

Stattdessen wird eine `.rpmnew`-Datei neben dem Original erstellt. Dies bedeutet, dass Sie einige Änderungen verpassen, die Sie möglicherweise gewünscht haben, und mehr Datenmüll in Ihren Konfigurationsordnern erzeugt haben.

D. h. der RPM wird während der Update-Installation nach `httpd.conf` schauen, und wenn diese sich im Zustand `unaltered` befindet, wird er die Datei *ersetzen*, sodass Sie die wichtigen Updates erhalten. Wenn die `httpd.conf` `altered` war, dann *wird die Datei nicht ersetzt* und stattdessen eine Referenzdatei mit dem Namen `httpd.conf.rpmnew` erstellt. Die vielen gewünschten Korrekturen werden in dieser Datei enthalten sein, die beim Start des Dienstes nicht angewendet wird.

Enterprise Linux wurde ordnungsgemäß eingerichtet, um diesen Anwendungsfall besser zu handhaben. Ihnen werden dort Bereiche gegeben, in denen Sie die für Sie festgelegten Standardeinstellungen erweitern oder überschreiben können. In der Basisinstallation von httpd finden Sie die Datei `/etc/httpd/conf/httpd.conf`, in der es eine Syntax wie folgt gibt:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

Die Idee ist, dass Apache möchte, dass Sie die Module und Konfigurationen erweitern, indem Sie neue Dateien in den Verzeichnissen `/etc/httpd/conf.d/` und `/etc/httpd/conf.modules.d/` mit der Dateierweiterung `.conf` hinzufügen

Als perfektes Beispiel für das Hinzufügen des Dispatcher-Moduls zu Apache würden Sie eine Modul-`.so`-Datei in ` /etc/httpd/modules/` erstellen und sie dann einschließen, indem Sie eine Datei in `/etc/httpd/conf.modules.d/02-dispatcher.conf` mit dem Inhalt zum Laden Ihrer Modul-`.so`-Datei hinzufügen

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Wir haben keine bereits vorhandenen Dateien geändert, die Apache bereitgestellt hat. Stattdessen haben wir unsere zu den Verzeichnissen hinzugefügt, für die sie bestimmt waren.
</div><br/>

Jetzt nutzen wir unser Modul in unserer Datei <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b>, die unser Modul initialisiert und die erste modulspezifische Konfigurationsdatei lädt

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Auch hier werden Sie feststellen, dass wir Dateien und Module hinzugefügt, aber keine Originaldateien verändert haben. Dadurch erhalten wir die gewünschte Funktionalität und werden davor geschützt, dass wir gewünschte Patches verpassen. Außerdem wird bei jeder Aktualisierung des Pakets ein Höchstmaß an Kompatibilität gewährleistet.

[Weiter -> Erläuterung der Konfigurationsdateien](./explanation-config-files.md)
