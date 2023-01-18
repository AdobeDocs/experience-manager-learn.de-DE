---
title: Erläuterung der Dispatcher-Konfigurationsdateien
description: Machen Sie sich mit Konfigurationsdateien, Benennungskonventionen und mehr vertraut.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# Erläuterung der Konfigurationsdateien

[Inhalt](./overview.md)

[&lt;- Zurück: Grundlegendes Dateilayout](./basic-file-layout.md)

In diesem Dokument werden die einzelnen Konfigurationsdateien aufgeschlüsselt und erläutert, die auf einem standardmäßigen Dispatcher-Server bereitgestellt werden, der in Adobe Managed Services bereitgestellt wird. Verwendung, Namenskonvention usw.

## Namenskonvention

Der Apache-Webserver kümmert sich bei der Zielgruppenbestimmung mit einer Datei nicht darum, was die Dateierweiterung einer Datei ist `Include` oder `IncludeOptional` -Anweisung.  Eine ordnungsgemäße Benennung mit Namen, die Konflikte und Verwirrung beseitigen, hilft einem <b>ton</b>. Die verwendeten Namen beschreiben den Umfang, in dem die Datei angewendet wird, und erleichtern das Leben. Wenn alles benannt ist `.conf` das wird wirklich verwirrend. Wir möchten schlecht benannte Dateien und Erweiterungen vermeiden.  Im Folgenden finden Sie eine Liste der verschiedenen benutzerdefinierten Dateierweiterungen und Benennungskonventionen, die in einem typischen AMS-konfigurierten Dispatcher verwendet werden.

## Dateien in &quot;conf.d/&quot;

| File | Dateiziel | Beschreibung |
| ---- | ---------------- | ----------- |
| DATEINAME`.conf` | `/etc/httpd/conf.d/` | Eine standardmäßige Enterprise Linux-Installation verwendet diese Dateierweiterung und den Include-Ordner als Ort, um in httpd.conf deklarierte Einstellungen zu überschreiben und Ihnen das Hinzufügen zusätzlicher Funktionen auf globaler Ebene in Apache zu ermöglichen. |
| DATEINAME`.vhost` | Staging: `/etc/httpd/conf.d/available_vhosts/`<br>Aktiv: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b> .vhost-Dateien werden nicht in den Ordner &quot;enabled_vhosts&quot;kopiert, sondern verwenden Symlinks zu einem relativen Pfad zur Datei &quot;available_vhosts/\*.vhost&quot;.</div></u><br><br> | \*.vhost-Dateien (virtueller Host) sind `<VirtualHosts>`  -Einträge, um Hostnamen zu entsprechen und es Apache zu ermöglichen, den jeweiligen Domänen-Traffic mit verschiedenen Regeln zu behandeln. Aus dem `.vhost` Datei, andere Dateien wie `rewrites`, `whitelisting`, `etc` eingeschlossen. |
| DATEINAME`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` Dateispeicher `mod_rewrite` Regeln, die explizit von einer `vhost` file |
| DATEINAME`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` -Dateien aus der `*.vhost` Dateien. Es enthält IP-Regex oder erlaubt Ablehnungsregeln, IP-Whitelisting zuzulassen. Wenn Sie die Anzeige eines virtuellen Hosts auf der Basis von IP-Adressen einschränken möchten, generieren Sie eine dieser Dateien und schließen sie aus Ihrem `*.vhost` file |

## In &quot;conf.modules.d/&quot;enthaltene Dateien

| File | Dateiziel | Beschreibung |
| --- | --- | --- |
| DATEINAME`.any` | `/etc/httpd/conf.dispatcher.d/` | Das AEM Dispatcher-Apache-Modul stellt die Einstellungen aus `*.any` Dateien. Die standardmäßige übergeordnete Include-Datei lautet `conf.dispatcher.d/dispatcher.any` |
| DATEINAME`_farm.any` | Staging: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Aktiv: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b> Diese Farm-Dateien sollen nicht in die `enabled_farms` Ordner, aber verwenden Sie `symlinks` zu einem relativen Pfad zum `available_farms/*_farm.any` file </div> <br/>`*_farm.any` -Dateien in `conf.dispatcher.d/dispatcher.any` -Datei. Diese übergeordneten Farm-Dateien dienen zur Steuerung des Modulverhaltens für jeden Renderer oder Website-Typ. Die Dateien werden im `available_farms` Verzeichnis und aktiviert mit einer `symlink` in `enabled_farms` Verzeichnis.  <br/>Sie werden automatisch anhand des Namens aus dem `dispatcher.any` -Datei.<br/><b>Grundlinie</b> Farm-Dateien beginnen mit `000_` , um sicherzustellen, dass sie zuerst geladen werden.<br><b>Benutzerdefiniert</b> Farm-Dateien sollten nach geladen werden, indem sie ihr Zahlenschema unter `100_` , um das richtige Einschlussverhalten sicherzustellen. |
| DATEINAME`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` -Dateien aus der `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Jede Farm verfügt über eine Reihe von Regeln, die ändern, welcher Traffic herausgefiltert werden soll, und nicht zu den Renderern. |
| DATEINAME`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` -Dateien aus der `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Diese Dateien sind eine Liste von Hostnamen oder URI-Pfaden, die durch eine Blob-Übereinstimmung abgeglichen werden, um zu bestimmen, welcher Renderer für die Bereitstellung dieser Anfrage verwendet werden soll |
| DATEINAME`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` -Dateien aus der `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Diese Dateien geben an, welche Elemente zwischengespeichert werden und welche nicht |
| DATEINAME`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` -Dateien in `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Sie geben an, welche IP-Adressen Flush- und Invalidierungsanfragen senden dürfen. |
| DATEINAME`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` -Dateien in `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Sie geben an, welche Client-Header an jeden Renderer übergeben werden sollen. |
| DATEINAME`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` -Dateien in `conf.dispatcher.d/enabled_farms/*_farm.any` Dateien. Sie geben IP-, Port- und Timeout-Einstellungen für jeden Renderer an. Ein ordnungsgemäßer Renderer kann ein LiveCycle-Server oder ein beliebiges AEM sein, von dem der Dispatcher die Anforderungen abrufen/Proxy bereitstellen kann |

## Vermeiden von Problemen

Wenn Sie die Namenskonvention befolgen, können Sie einige ziemlich einfache Fehler vermeiden, die katastrophale Folgen haben können.  Wir werden einige Beispiele anführen.

### Problembeispiel

Als Site-Beispiel für ExampleCo wurden zwei Konfigurationsdateien von den Entwicklern der Dispatcher-Konfigurationen erstellt.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Wenn die Variable `vhost` -Datei versehentlich in die `rewrites` und `rewrites file` in `vhosts` Ordner.  Sie wird anscheinend ordnungsgemäß vom Dateinamen bereitgestellt, Apache gibt jedoch eine *FEHLER* und das Problem wird nicht sofort sichtbar sein.

<b>So wird dies normalerweise zu einem Problem</b>

Wenn die Variable `two files` heruntergeladen werden. `same` Standort, den sie `overwrite themselves` oder machen es ununterscheidbar, den Implementierungsprozess zu einem Albtraum zu machen.

<b>Die Dateierweiterungen sind identisch und können automatisch eingefügt werden</b>

Die Dateierweiterungen sind identisch und verwenden die automatisch enthaltene Erweiterung, die Apache verwendet `auto include` any `.conf` -Dateien in vielen der Standardordner enthalten.

<b>So wird dies normalerweise zu einem Problem</b>

Wenn die vhost-Datei mit der Erweiterung von `.conf` in `/etc/httpd/conf.d/` -Ordner wird versucht, ihn in den Speicher auf Apache zu laden, was normalerweise in Ordnung ist, aber wenn die Datei mit den Umschreibungsregeln mit der Erweiterung `.conf` wird in `/etc/httpd/conf.d/` -Ordner, wird er automatisch eingefügt und global angewendet, was zu verwirrenden und unerwünschten Ergebnissen führt.

## Auflösung

Benennen Sie die Dateien basierend auf dem, was sie tun, und verlassen Sie sie sicher aus dem Namespace für automatische Einschlussregeln.

Wenn es sich um einen virtuellen Host-Dateinamen handelt, wird er mit `.vhost` als Erweiterung.

Wenn es sich um eine Rewrite-Regeldatei handelt, benennen Sie sie mit Site`_rewrite.rules` als Suffix und Erweiterung. Diese Namenskonvention macht deutlich, für welche Site es sich handelt und dass es sich um einen Satz von Neuschreibungsregeln handelt.

Wenn es sich um eine IP-Whitelist-Regeldatei handelt, geben Sie ihr eine Beschreibung`_whitelist.rules` als Suffix und Erweiterung. Diese Namenskonvention gibt eine Beschreibung dessen, wofür sie verwendet wird und dass es sich um einen Satz von IP-Übereinstimmungsregeln handelt.

Durch die Verwendung dieser Namenskonventionen werden Probleme vermieden, wenn eine Datei in ein Verzeichnis mit automatischem Include verschoben wird, zu dem sie nicht gehört.

Beispiel: Einfügen einer Datei mit dem Namen `.rules`, `.any`oder `.vhost` im Ordner &quot;auto-include&quot;von `/etc/httpd/conf.d/` keinen Einfluss haben.

Wenn in einer Anfrage zur Änderung der Implementierung steht: &quot;Bitte stellen Sie exampleco_rewrite.rules auf Produktions-Dispatcher bereit&quot;, kann der Benutzer, der die Änderungen bereitstellt, bereits wissen, dass er keine neue Site hinzufügt, sondern lediglich die Neuschreibungsregeln aktualisiert, wie durch den Dateinamen angegeben.

### Reihenfolge einschließen

Bei der Erweiterung der Funktionalität und Konfigurationen in Apache Webserver auf Enterprise Linux haben Sie einige wichtige Include-Bestellungen, die Sie verstehen möchten

### Grundlegende Apache-Includes

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Wie im Diagramm oben gezeigt, sucht die HTTPD-Binärdatei nur nach der Datei &quot;httpd.conf&quot;als Konfigurationsdatei.  Diese Datei enthält die folgenden Anweisungen:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS-oberste Ebene umfasst

Als wir unseren Standard angewendet haben, haben wir einige zusätzliche Dateitypen und eigene Includes hinzugefügt.

Im Folgenden finden Sie die Grundlinien-Verzeichnisse von AMS und die wichtigsten Includes
![Die AMS-Grundlinie umfasst den Start mit einer Datei &quot;dispatcher_vhost.conf&quot;, die alle Dateien mit dem *.vhost aus dem Verzeichnis /etc/httpd/conf.d/enabled_vhosts/ enthält.  Elemente im Ordner /etc/httpd/conf.d/enabled_vhosts/ sind Symlinks zur tatsächlichen Konfigurationsdatei, die sich in /etc/httpd/conf.d/available_vhosts/ befindet.](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "apache-webserver-AMS-baseline-include")

Aufbauend auf der Apache-Grundlinie zeigen wir, wie AMS einige zusätzliche Ordner und Top-Level-Includes für `conf.d` Ordner sowie modulspezifische Verzeichnisse, die unter `/etc/httpd/conf.dispatcher.d/`

Wenn Apache geladen wird, wird das `/etc/httpd/conf.modules.d/02-dispatcher.conf` und diese Datei die Binärdatei enthält `/etc/httpd/modules/mod_dispatcher.so` in den Ausführungsstatus.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

So verwenden Sie das Modul in `<VirtualHost />` legen wir eine Konfigurationsdatei in `/etc/httpd/conf.d/` benannt `dispatcher_vhost.conf` und in dieser Datei sehen Sie, wie Sie die grundlegenden Parameter einrichten, die für das Modul erforderlich sind, um zu funktionieren:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Wie Sie oben sehen können, umfasst dies die oberste Ebene `dispatcher.any` -Datei für unser Dispatcher-Modul verwenden, um die Konfigurationsdateien aus dem `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Beachten Sie den Inhalt dieser Datei:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Die oberste Ebene `dispatcher.any` enthält alle aktivierten Farm-Dateien, die in `/etc/httpd/conf.dispatcher.d/enabled_farms/` mit dem Dateinamen von `FILENAME_farm.any` die unserer standardmäßigen Namenskonvention folgt.

Später im `dispatcher_vhost.conf` -Datei, die zuvor erwähnt wurde, führen wir auch eine Include-Anweisung aus, um jede aktivierte virtuelle Host-Datei zu aktivieren, die in `/etc/httpd/conf.d/enabled_vhosts/` mit dem Dateinamen von `FILENAME.vhost` die unserer standardmäßigen Namenskonvention folgt.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

In jeder unserer .vhost-Dateien werden Sie feststellen, dass das Dispatcher-Modul als Standard-Datei-Handler für ein Verzeichnis initialisiert wird.  Im Folgenden finden Sie eine Beispieldatei mit der Syntax:

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Nachdem die oberste Ebene aufgelöst wurde, verfügen sie über weitere Untereinschlüsse, die erwähnt werden sollten.  Hier finden Sie ein Diagramm auf hoher Ebene darüber, wie Farb- und Vhost-Dateien andere Unterelemente enthalten

### AMS Virtual Host enthält

![Dieses Bild zeigt, wie eine Vhost-Datei Dateien aus Variablen, Whitelists und Neuschreibungen von Ordnern enthält](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "apache-webserver-AMS-vhost-include")

Wenn `.vhost` Dateien aus `/etc/httpd/conf.d/availabled_vhosts/` -Verzeichnis verknüpfen mit `/etc/httpd/conf.d/enabled_vhosts/` -Verzeichnis, das in der laufenden Konfiguration verwendet wird.

Die `.vhost` -Dateien enthalten Untereinschlüsse basierend auf gemeinsamen Elementen, die wir gefunden haben.  Dinge wie Variablen, Whitelists und Neuschreibungsregeln.

Die `.vhost` enthält Include-Anweisungen für jede Datei, je nachdem, wo sie in die Datei `.vhost` -Datei.  Im Folgenden finden Sie eine Beispielsyntax eines `.vhost` als Referenz:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Wie Sie im obigen Beispiel sehen können, gibt es einen Einschluss für die Variablen, die in dieser Konfigurationsdatei benötigt werden und später verwendet werden.

Innerhalb der Datei `/etc/httpd/conf.d/variables/weretail.vars` können wir sehen, welche Variablen definiert sind:

```
Define MAIN_DOMAIN dev.weretail.com
```

Sie können auch eine Zeile sehen, die eine Liste von `_whitelist.rules` -Dateien, die einschränken, wer diesen Inhalt basierend auf verschiedenen Whitelist-Kriterien anzeigen kann.  Betrachten wir den Inhalt einer der Whitelist-Dateien `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Sie können auch eine Zeile sehen, die einen Satz von Neuschreibungsregeln enthält.  Sehen wir uns den Inhalt der `weretail_rewrite.rules` Datei:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS Farm Includes

![<FILENAME>_farms.any enthält untergeordnete .any-Dateien, um eine Farm-Konfiguration abzuschließen.  In diesem Bild können Sie sehen, dass eine Farm alle Dateien der obersten Ebene enthält, die zwischengespeichert sind, Clientheaders, Filter, Renderer und vhosts .any-Dateien](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-webserver-AMS-Farm-Includes")

Wenn beliebige DATEINAME_farm.any-Dateien aus `/etc/httpd/conf.dispatcher.d/available_farms/` -Verzeichnis verknüpfen mit `/etc/httpd/conf.dispatcher.d/enabled_farms/` -Verzeichnis, das in der laufenden Konfiguration verwendet wird.

Die Farm-Dateien enthalten Untereinschlüsse, die auf [Abschnitte der obersten Ebene der Farm](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) wie Cache, Clientheaders, Filter, Renderer und Hosts.

Die `FILENAME_farm.any` -Dateien enthalten -Anweisungen für jede Datei, je nachdem, wo sie in die Farm-Datei aufgenommen werden müssen.  Im Folgenden finden Sie eine Beispielsyntax eines `FILENAME_farm.any` als Referenz:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Wie Sie sehen können, verwenden Sie stattdessen eine Include-Anweisung, anstatt die gesamte benötigte Syntax für die Weretail-Farm zu verwenden.

Sehen wir uns die Syntax einiger dieser Includes an, um zu erfahren, wie die einzelnen Unter-Includes aussehen würden.

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Wie Sie sehen, ist es eine neue, durch Zeilen getrennte Liste von Domänennamen, die von dieser Farm über die anderen gerendert werden sollen.

Als Nächstes sehen wir uns die `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Weiter -> Grundlegendes zum Cache](./understanding-cache.md)