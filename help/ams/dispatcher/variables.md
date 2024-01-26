---
title: Verwenden und Verstehen von Variablen in Ihrer AEM Dispatcher-Konfiguration
description: Erfahren Sie, wie Sie Variablen in den Konfigurationsdateien der Apache- und Dispatcher-Module verwenden können, um diese auf die nächste Stufe zu bringen.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 307
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 100%

---

# Verwenden und Verstehen von Variablen

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Grundlegendes zum Cache](./understanding-cache.md)

In diesem Dokument wird erläutert, wie Sie die Leistungsfähigkeit von Variablen auf dem Apache-Webserver und in Ihren Dispatcher-Modulkonfigurationsdateien nutzen können.

## Variablen

Apache unterstützt Variablen. Ab Version 4.1.9 gilt dies auch für das Dispatcher-Modul.

Wir können diese für eine Menge nützlicher Dinge einsetzen, z. B.:

- Sicherstellen, dass alles, was umgebungsspezifisch ist, nicht inline in den Konfigurationen ist, sondern extrahiert wird, damit Konfigurationsdateien aus der Entwicklungsumgebung in der Produktionsumgebung mit derselben funktionalen Ausgabe funktionieren
- Umschalten von Funktionen und Ändern der Protokollebenen von unveränderlichen Dateien, die von AMS bereitgestellt werden und die Sie nicht ändern dürfen.
- Ändern, welche „includes“ basierend auf Variablen wie `RUNMODE` und `ENV_TYPE` verwendet werden sollen
- Abgleichen der DNS-Namen von `DocumentRoot` und `VirtualHost` zwischen Apache- und Modulkonfigurationen

## Verwenden von grundlegenden Variablen

Da die Basisdateien von AMS schreibgeschützt und unveränderlich sind, gibt es Funktionen, die deaktiviert/aktiviert sowie durch Bearbeiten der von ihnen verwendeten Variablen konfiguriert werden können.

### Grundlegende Variablen

AMS-Standardvariablen werden in der Datei `/etc/httpd/conf.d/variables/ootb.vars` deklariert. Diese Datei kann nicht bearbeitet werden, ist aber vorhanden, um sicherzustellen, dass Variablen keine Nullwerte aufweisen. Sie werden zuerst vor und dann nach dem Einschließen von `/etc/httpd/conf.d/variables/ams_default.vars` einbezogen. Sie können diese Datei bearbeiten, um die Werte dieser Variablen zu ändern, oder sogar dieselben Variablennamen und -werte in Ihre eigene Datei aufnehmen.

Im Folgenden finden Sie ein Beispiel für den Inhalt der Datei `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Beispiel 1 – Erzwingen von SSL

Die oben aufgeführten Variablen, `AUHOR_FORCE_SSL` bzw. `PUBLISH_FORCE_SSL`, können auf 1 gesetzt werden, um Neuschreibungsregeln anzuwenden, durch die das Umleiten von Endbenutzenden bei HTTP-Anfragen zu HTTPS erzwungen wird.

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

Wie zu sehen ist, enthält das include-Element der Neuschreibungsregeln den Code, um den Browser der Endbenutzenden umzuleiten, aber erst durch Festlegen der Variable auf den Wert 1 wird die Verwendung bzw. Nichtverwendung der Datei ermöglicht.

### Beispiel 2 – Protokollierungsebene

Über die Variablen `DISP_LOG_LEVEL` können Sie die gewünschte Protokollebene für die laufende Konfiguration festlegen.

Im Folgenden finden Sie ein Syntaxbeispiel, das in den grundlegenden AMS-Konfigurationsdateien vorhanden ist:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Wenn Sie die Dispatcher-Protokollierungsebene erhöhen müssen, aktualisieren Sie einfach in `ams_default.vars` die Variable `DISP_LOG_LEVEL` auf die gewünschte Ebene.

Beispielwerte können eine Ganzzahl oder ein Wort sein:

| Protokollebene | Ganzzahlwert | Wortwert |
| --- | --- | --- |
| Trace | 4 | trace |
| Debuggen | 3 | debug |
| Informationen | 2 | info |
| Warnung | 1 | warn |
| Fehler | 0 | error |

### Beispiel 3 – Whitelists

Die Variablen `AUTHOR_WHITELIST_ENABLED` und `PUBLISH_WHITELIST_ENABLED` können auf 1 gesetzt werden, um Neuschreibungsregeln zu aktivieren, die Regeln zum Zulassen oder Sperren des Endbenutzer-Traffics auf der Grundlage der IP-Adresse enthalten. Das Einschalten dieser Funktion muss auch mit dem Erstellen einer Whitelist-Regeldatei kombiniert werden, die berücksichtigt werden soll.

Hier einige Syntaxbeispiele, wie die Variable die Einbindung der Whitelist-Dateien ermöglicht, sowie ein Beispiel für eine Whitelist-Datei

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

Wie Sie sehen können, erzwingt `sample_whitelist.rules` die IP-Beschränkung, aber wenn Sie die Variable umschalten, kann sie in `sample.vhost` aufgenommen werden.

## Platzieren der Variablen

### Argumente zum Starten von Webservern

AMS setzt Server-/topologiespezifische Variablen in die Startargumente des Apache-Prozesses in der Datei `/etc/sysconfig/httpd`

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

Diese können Sie zwar nicht ändern, Sie sollten sie aber in Ihren Konfigurationsdateien verwenden

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Da diese Datei nur beim Starten des Dienstes aufgenommen wird, ist ein Neustart des Dienstes erforderlich, um Änderungen zu übernehmen. Das bedeutet, dass ein Neuladen nicht ausreicht, sondern wirklich ein Neustart erforderlich ist.
</div>

### Variablendateien (`.vars`)

Benutzerdefinierte Variablen, die von Ihrem Code bereitgestellt werden, sollten in `.vars`-Dateien innerhalb des Verzeichnisses `/etc/httpd/conf.d/variables/` gespeichert werden.

Diese Dateien können beliebige benutzerdefinierte Variablen enthalten. Einige Syntaxbeispiele finden Sie in den folgenden Beispieldateien:

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

Wenn Sie Ihre eigenen Variablendateien erstellen, benennen Sie sie entsprechend ihrem Inhalt und halten Sie sich an die im Handbuch [hier](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html?lang=de#naming-convention) angegebenen Namensstandards. Im obigen Beispiel können Sie sehen, dass die Variablendatei die verschiedenen DNS-Einträge als Variablen hostet, die in den Konfigurationsdateien verwendet werden sollen.

## Verwenden von Variablen

Nachdem Sie Ihre Variablen in Ihren Variablendateien definiert haben, sollten Sie wissen, wie Sie sie in Ihren anderen Konfigurationsdateien ordnungsgemäß verwenden können.

Zur Veranschaulichung eines geeigneten Anwendungsfalls werden wir die oben genannten `.vars`-Dateien verwenden.

Da wir alle Umgebungsvariablen global einbeziehen wollen, erstellen wir die Datei `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Wir wissen, dass der httpd-Dienst beim Starten die von AMS in `/etc/sysconfig/httpd` gesetzten Variablen einzieht und die Variablen `ENV_TYPE` und `RUNMODE` enthält.

Wenn diese globale Datei `.conf` eingefügt wird, wird sie frühzeitig eingefügt, da die Reihenfolge der Dateien in `conf.d` eine alphanumerische Ladereihenfolge ist, d. h. 000 im Dateinamen stellt sicher, dass sie vor den anderen Dateien im Verzeichnis geladen wird.

Die include-Anweisung verwendet ebenfalls eine Variable im Dateinamen. Dadurch kann sich ändern, welche Datei tatsächlich geladen wird, basierend darauf, welcher Wert in den Variablen `ENV_TYPE` und `RUNMODE` steht.

Wenn der `ENV_TYPE`-Wert `dev` ist, wird die folgende Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Wenn der `ENV_TYPE`-Wert `stage` ist, wird die folgende Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Wenn der `RUNMODE`-Wert `preview` ist, wird die folgende Datei verwendet:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Wenn diese Datei eingeschlossen wird, können wir die darin gespeicherten Variablennamen verwenden.

In unserer `/etc/httpd/conf.d/available_vhosts/weretail.vhost`-Datei können wir die normale Syntax austauschen, die nur für die Entwicklung funktioniert:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Wir ersetzen sie durch eine neuere Syntax, die die Leistungsfähigkeit von Variablen nutzt, um für Entwicklung, Staging und Produktion zu funktionieren:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

In unserer `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any`-Datei können wir die normale Syntax austauschen, die nur für die Entwicklung funktioniert:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Wir ersetzen sie durch eine neuere Syntax, die die Leistungsfähigkeit von Variablen nutzt, um für Entwicklung, Staging und Produktion zu funktionieren:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Diese Variablen können sehr häufig wiederverwendet werden, um laufende Einstellungen zu individualisieren, ohne dass für jede Umgebung unterschiedliche Dateien bereitgestellt werden müssen. Im Wesentlichen gestalten Sie Ihre Konfigurationsdateien mithilfe von Variablen und include-Dateien auf der Grundlage von Variablen.

## Anzeigen von Variablenwerten

Bei der Verwendung von Variablen müssen wir manchmal nachsehen, welche Werte in unseren Konfigurationsdateien stehen könnten. Es gibt eine Möglichkeit, die aufgelösten Variablen anzuzeigen, indem Sie die folgenden Befehle auf dem Server ausführen:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

So sahen die Variablen in Ihrer kompilierten Apache-Konfiguration aus:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

So sahen die Variablen in Ihrer kompilierten Dispatcher-Konfiguration aus:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

In der Ausgabe der Befehle sehen Sie die Unterschiede zwischen der Variablen in der Konfigurationsdatei und der kompilierten Ausgabe.

Beispielkonfiguration

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
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

[Weiter -> Flushing](./disp-flushing.md)
