---
title: Verwenden und Verstehen von Variablen in Ihrer AEM Dispatcher-Konfiguration
description: Erfahren Sie, wie Sie Variablen in den Konfigurationsdateien der Apache- und Dispatcher-Module verwenden können, um sie auf die nächste Ebene zu bringen.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# Verwenden und Verstehen von Variablen

[Inhalt](./overview.md)

[&lt;- Zurück: Grundlagen zum Cache](./understanding-cache.md)

In diesem Dokument wird erläutert, wie Sie die Leistungsfähigkeit von Variablen auf dem Apache-Webserver und in Ihren Dispatcher-Modulkonfigurationsdateien nutzen können.

## Variablen

Apache unterstützt Variablen und ab Version 4.1.9 des Dispather-Moduls unterstützt es sie auch!

Wir können diese nutzen, um eine Menge nützlicher Dinge zu tun, wie:

- Stellen Sie sicher, dass alles, was umgebungsspezifisch ist, nicht inline in den Konfigurationen ist, sondern extrahiert wird, um sicherzustellen, dass Konfigurationsdateien aus dev in prod mit derselben funktionalen Ausgabe funktionieren.
- Wechsel zu Funktionen und Änderung der Protokollebenen unveränderlicher Dateien, die AMS bereitstellt, lassen eine Änderung nicht zu.
- Ändern Sie, welche enthält, basierend auf Variablen wie `RUNMODE` und `ENV_TYPE`
- Übereinstimmung `DocumentRoot`&quot;s und `VirtualHost` DNS-Namen zwischen Apache-Konfigurationen und Modulkonfigurationen.

## Grundlegende Variablen verwenden

Da die Grundlinien-Dateien von AMS schreibgeschützt und unveränderlich sind, können Funktionen deaktiviert und aktiviert sowie durch Bearbeiten der von ihnen verwendeten Variablen konfiguriert werden.

### Basisvariablen

AMS-Standardvariablen werden in der Datei deklariert `/etc/httpd/conf.d/variables/ootb.vars`.  Diese Datei kann nicht bearbeitet werden, ist aber vorhanden, um sicherzustellen, dass Variablen keine Nullwerte aufweisen.  Sie werden zuerst einbezogen, nachdem wir sie eingefügt haben `/etc/httpd/conf.d/variables/ams_default.vars`.  Sie können diese Datei bearbeiten, um die Werte dieser Variablen zu ändern oder sogar dieselben Variablennamen und -werte in Ihre eigene Datei aufzunehmen!

Im Folgenden finden Sie ein Beispiel für den Inhalt der Datei `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Beispiel 1: SSL erzwingen

Die oben aufgeführten Variablen `AUHOR_FORCE_SSL`oder `PUBLISH_FORCE_SSL` kann auf 1 gesetzt werden, um Umschreibungsregeln zu umschreiben, die Endbenutzer zwingen, beim Eintreffen auf eine HTTP-Anfrage zu HTTPS umgeleitet zu werden

Die folgende Syntax der Konfigurationsdatei ermöglicht es, diesen Umschalter zu verwenden:

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Wie Sie sehen können, enthalten die Neuschreibungsregeln Folgendes: Der Code, um den Endbenutzer-Browser umzuleiten, aber die Variable, die auf 1 gesetzt ist, ermöglicht die Verwendung oder Nichtverwendung der Datei

### Beispiel 2: Protokollierungsstufe

Die Variablen `DISP_LOG_LEVEL` kann verwendet werden, um festzulegen, was Sie für die Protokollebene wünschen, die tatsächlich in der laufenden Konfiguration verwendet wird.

Im Folgenden finden Sie ein Syntaxbeispiel, das in den grundlegenden AMS-Konfigurationsdateien vorhanden ist:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Wenn Sie die Dispatcher-Protokollierungsstufe erhöhen müssen, aktualisieren Sie einfach die `ams_default.vars` Variable `DISP_LOG_LEVEL` auf die gewünschte Ebene.

Beispielwerte können eine Ganzzahl oder das Wort sein:

| Protokollebene | Ganzzahlwert | Wortwert |
| --- | --- | --- |
| Trace | 4 | trace |
| Debug | 3 | debug |
| Info | 2 | Informationen |
| Warnung | 1 | warn |
| Fehler | 0 | Fehler |

### Beispiel 3: Whitelists

Die Variablen `AUTHOR_WHITELIST_ENABLED` und `PUBLISH_WHITELIST_ENABLED` kann auf 1 gesetzt werden, um Umschreibungsregeln zu aktivieren, die Regeln enthalten, die den Endbenutzer-Traffic basierend auf IP-Adressen zulassen oder verbieten.  Das Umschalten dieser Funktion auf muss auch mit dem Erstellen einer Whitelist-Regeldatei kombiniert werden, damit sie eingeschlossen wird.

Im Folgenden finden Sie einige Syntaxbeispiele, wie die -Variable die Includes der Whitelist-Dateien und ein Beispiel für eine Whitelist-Datei aktiviert

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Sie können die `sample_whitelist.rules` erzwingt die IP-Beschränkung, aber durch Umschalten der Variable kann sie in die Variable `sample.vhost`

## Platzieren der Variablen

### Argumente zum Starten von Webservern

AMS setzt Server-/Topologie-spezifische Variablen in die Startargumente des Apache-Prozesses in die Datei `/etc/sysconfig/httpd`

Diese Datei enthält vordefinierte Variablen, wie im Folgenden gezeigt:

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

Dies können Sie nicht ändern, sind aber gut zu nutzen in Ihren Konfigurationsdateien

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Da diese Datei nur beim Starten des Dienstes eingeschlossen wird.  Um Änderungen aufzunehmen, ist ein Neustart des Dienstes erforderlich.  Das bedeutet, dass eine Neuladung nicht ausreicht, sondern stattdessen ein Neustart erforderlich ist
</div>

### Variablendateien (`.vars`)

Benutzerdefinierte Variablen, die von Ihrem Code bereitgestellt werden, sollten in `.vars` Dateien im Verzeichnis `/etc/httpd/conf.d/variables/`

Diese Dateien können beliebige benutzerdefinierte Variablen enthalten. Syntaxbeispiele finden Sie in den folgenden Beispieldateien

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Bei der Erstellung Ihrer eigenen Variablen benennen Sie sie anhand ihres Inhalts und folgen Sie den im Handbuch angegebenen Benennungsstandards. [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  Im obigen Beispiel können Sie sehen, dass die Variablendatei die verschiedenen DNS-Einträge als Variablen hostet, die in den Konfigurationsdateien verwendet werden sollen.

## Verwenden von Variablen

Nachdem Sie Ihre Variablen in Ihren Variablendateien definiert haben, sollten Sie wissen, wie Sie sie in Ihren anderen Konfigurationsdateien ordnungsgemäß verwenden können.

Wir verwenden das Beispiel `.vars` -Dateien aus dem obigen Abschnitt, um einen geeigneten Anwendungsfall zu veranschaulichen.

Wir möchten alle umgebungsbasierten Variablen global einbeziehen. Wir werden die Datei erstellen `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Wir wissen, dass der HTTPD-Dienst beim Start die von AMS festgelegten Variablen in abruft `/etc/sysconfig/httpd` und verfügt über den Variablensatz von `ENV_TYPE` und `RUNMODE`

Wenn diese globale `.conf` -Datei abgerufen wird, wird sie frühzeitig abgerufen, da die Include-Reihenfolge der Dateien in `conf.d` ist alphanumerische Ladereihenfolge bedeutet 000 im Dateinamen, dass er vor den anderen Dateien im Verzeichnis geladen wird.

Die Include-Anweisung verwendet auch eine Variable im Dateinamen.  Dadurch kann sich ändern, welche Datei tatsächlich geladen wird, basierend darauf, welcher Wert in der Variablen `ENV_TYPE` und `RUNMODE` Variablen.

Wenn die Variable `ENV_TYPE` Wert ist `dev` dann wird die Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Wenn die Variable `ENV_TYPE` Wert ist `stage` dann wird die Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Wenn die Variable `RUNMODE` Wert ist `preview` dann wird die Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Wenn diese Datei eingeschlossen wird, können wir die darin gespeicherten Variablennamen verwenden.

In unserem `/etc/httpd/conf.d/available_vhosts/weretail.vhost` -Datei können wir die normale Syntax austauschen, die nur für dev funktioniert hat:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Mit einer neueren Syntax, die die Leistungsfähigkeit von Variablen nutzt, um für Entwicklung, Staging und Produktion zu funktionieren:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

In unserem `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` -Datei können wir die normale Syntax austauschen, die nur für dev funktioniert hat:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Mit einer neueren Syntax, die die Leistungsfähigkeit von Variablen nutzt, um für Entwicklung, Staging und Produktion zu funktionieren:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Diese Variablen können sehr häufig wiederverwendet werden, um laufende Einstellungen zu individualisieren, ohne dass je Umgebung unterschiedliche bereitgestellte Dateien benötigt werden.  Im Grunde nehmen Sie Ihre Konfigurationsdateien mit Variablen als Vorlage vor und schließen Dateien ein, die auf Variablen basieren.

## Anzeigen von Variablenwerten

Manchmal müssen wir bei der Verwendung von Variablen suchen, um zu sehen, welche Werte in unseren Konfigurationsdateien enthalten sein könnten.  Es gibt eine Möglichkeit, die aufgelösten Variablen anzuzeigen, indem Sie die folgenden Befehle auf dem Server ausführen:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Wie die Variablen in Ihrer kompilierten Apache-Konfiguration aussahen:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Aussehen der Variablen in Ihrer kompilierten Dispatcher-Konfiguration:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

In der Ausgabe der Befehle sehen Sie die Unterschiede zwischen der Variablen in der Konfigurationsdatei und der kompilierten Ausgabe.

Beispielkonfiguration

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

Führen Sie nun die Befehle aus, um die kompilierte Ausgabe anzuzeigen

Kompilierte Apache-Konfiguration:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Kompilierte Dispatcher-Konfiguration:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Nächste -> Flushing](./disp-flushing.md)