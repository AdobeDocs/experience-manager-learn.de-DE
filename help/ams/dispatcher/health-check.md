---
title: AMS Dispatcher-Konsistenzprüfung
description: AMS stellt ein CGI-Bin-Skript für die Konsistenzprüfung bereit, das von den Cloud-Load-Balancern ausgeführt wird, um zu überprüfen, ob AEM fehlerfrei funktioniert und für den öffentlichen Traffic weiter betrieben werden sollte.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 69b4e469-52cc-441b-b6e5-2fe7ef18da90
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '1139'
ht-degree: 100%

---

# AMS Dispatcher-Konsistenzprüfung

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Schreibgeschützte Dateien](./immutable-files.md)

Eine AMS Dispatcher-Basisinstallation umfasst verschiedene Gratisfunktionen.  Zu diesen Funktionen gehört eine Reihe von Skripten zur Konsistenzprüfung.
Anhand dieser Skripte erfährt der dem AEM-Stack vorgeschaltete Load-Balancer, welche Abzweigungen fehlerfrei sind und weiter betrieben werden sollten.

![Animierte GIF-Datei mit Traffic-Fluss](assets/load-balancer-healthcheck/health-check.gif "Schritte zur Konsistenzprüfung")

## Grundlegende Load-Balancer-Konsistenzprüfung

Wenn Kunden-Traffic über das Internet zu Ihrer AEM-Instanz gelangt, durchlaufen sie einen Load-Balancer.

![Bild mit Darstellung des Traffic-Flusses vom Internet zu AEM über einen Load-Balancer](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "Load-Balancer-Traffic-Fluss")

Alle Anfragen, die den Load-Balancer passieren, werden per Round-Robin-Verfahren an die jeweilige Instanz gesendet.  Der Load-Balancer verfügt über einen Konsistenzprüfmechanismus, um sicherzustellen, dass er Traffic an einen Host in einem einwandfreien Zustand sendet.

Standardmäßig erfolgt normalerweise eine Port-Prüfung, um festzustellen, ob die beim Lastenausgleich anvisierten Server den Port-Traffic überwachen (d. h. TCP 80 und 443).

> `Note:` Dies funktioniert, sagt jedoch nicht wirklich etwas darüber aus, ob sich AEM in einem einwandfreien Zustand befindet.  Es wird nur getestet, ob der Dispatcher (Apache-Webserver) aktiv ist.

## AMS-Konsistenzprüfung

Um zu vermeiden, dass Traffic an einen fehlerfreien Dispatcher gesendet wird, der einer fehlerhaften AEM-Instanz vorgeschaltet ist, verfügt AMS über einige zusätzlich entwickelte Funktionen, die die Integrität der Abzweigung und nicht nur die des Dispatchers bewerten.

![Bild mit Darstellung der verschiedenen Teile für eine funktionierende Konsistenzprüfung](assets/load-balancer-healthcheck/health-check-pieces.png "Bestandteile der Konsistenzprüfung")

Die Konsistenzprüfung umfasst folgende Bestandteile:
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Wir werden uns mit der Aufgabe und Bedeutung all dieser Bestandteile beschäftigen.

### AEM-Paket

Um festzustellen, ob AEM funktioniert, ist es erforderlich, eine einfache Seitenkompilierung durchzuführen und die Seite bereitzustellen.  Adobe Managed Services hat ein Basispaket mit der Testseite erstellt.  Die Seite testet, ob das Repository eingerichtet ist und ob die Ressourcen und die Seitenvorlage gerendert werden können.

![Bild mit Darstellung des AMS-Pakets in CRX Package Manager](assets/load-balancer-healthcheck/health-check-package.png "Konsistenzprüfung – Paket")

Hier ist die Seite. Sie zeigt die Repository-ID der Installation.

![Bild mit Darstellung der AMS Regent-Seite](assets/load-balancer-healthcheck/health-check-page.png "Konsistenzprüfung – Seite")

> `Note:` Wir stellen sicher, dass die Seite nicht zwischengespeichert werden kann.  Sie würde den tatsächlichen Zustand nicht überprüfen, wenn jedes Mal eine zwischengespeicherte Seite zurückgegeben wird.

Diesen einfachen Endpunkt können wir testen, um festzustellen, ob AEM ordnungsgemäß ausgeführt wird.

### Load-Balancer-Konfiguration

Wir konfigurieren die Load-Balancer so, dass sie auf einen CGI-Bin-Endpunkt verweisen, anstatt eine Port-Prüfung durchzuführen.

![Bild mit Darstellung der Konfiguration der AWS-Load-Balancer-Konsistenzprüfung](assets/load-balancer-healthcheck/aws-settings.png "AWS-Load-Balancer-Einstellungen")

![Bild mit Darstellung der Konfiguration der Azure-Load-Balancer-Konsistenzprüfung](assets/load-balancer-healthcheck/azure-settings.png "Azure-Load-Balancer-Einstellungen")

### Apache-Konsistenzprüfung virtueller Hosts

#### CGI-Bin – Virtueller Host `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Dies ist die `<VirtualHost>`-Apache-Konfigurationsdatei, die die Ausführung der CGI-Bin-Dateien ermöglicht.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` CGI-Bin-Dateien sind Skripte, die ausgeführt werden können.  Dies kann ein anfälliger Angriffsvektor sein, und diese von AMS verwendeten Skripte sind nicht öffentlich zugänglich, sondern nur für den Load-Balancer für Testzwecke verfügbar.


#### Wartung fehlerhafter virtueller Hosts

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Diese Dateien haben mit Absicht `000_` als Präfix.  Sie ist extra so konfiguriert, dass sie denselben Domain-Name wie die Livesite verwendet.  Diese Datei soll aktiviert werden, wenn die Konsistenzprüfung erkennt, dass ein Problem mit einem der AEM-Backends vorliegt.  Geben Sie dann eine Fehlerseite an, anstatt nur einen 503-HTTP-Antwort-Code ohne Seite anzubieten.  Sie stiehlt den Traffic von der normalen Datei `.vhost`, da sie vor dieser Datei `.vhost` geladen wurde, während derselbe `ServerName` oder `ServerAlias` freigegeben wurde.  Dies führt dazu, dass Seiten, die für eine bestimmte Domain bestimmt sind, zum nicht ordnungsgemäß ausgeführten statt zum standardmäßigen Host gelangen, durch den der normale Traffic fließt.

Wenn die Konsistenzprüfungsskripte ausgeführt werden, melden sie ihren aktuellen Zustand ab.  Einmal pro Minute wird ein Cronjob auf dem Server ausgeführt, der nach nicht ordnungsgemäßen Einträgen im Protokoll sucht.  Wenn festgestellt wird, dass die AEM-Authoring-Instanz nicht ordnungsgemäß ausgeführt wird, wird der Symlink aktiviert:

Protokolleintrag:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Den Fehler per Cron sichern und reagieren:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Sie können steuern, ob Authoring- oder Publishing-Sites diese Fehlerseite laden können, indem Sie die Neulademoduseinstellung in `/var/www/cgi-bin/health_check.conf` konfigurieren

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Gültige Optionen:
- author
   - Dies ist die Standardoption.
   - Dadurch wird bei nicht ordnungsgemäßer Ausführung eine Authoring-Wartungsseite eingerichtet
- publish
   - Mit dieser Option wird bei nicht ordnungsgemäßer Ausführung eine Publishing-Wartungsseite eingerichtet
- alle
   - Mit dieser Option wird bei nicht ordnungsgemäßer Ausführung eine Authoring- oder Publishing-Wartungsseite oder beides eingerichtet
- keine
   - Durch diese Option wird diese Funktion der Konsistenzprüfung übersprungen

Wenn Sie sich jeweils die `VirtualHost`-Einstellung anschauen, werden Sie sehen, dass dasselbe Dokument als Fehlerseite für jede Anfrage geladen wird, die auf die Aktivierung folgt:

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

Der Antwort-Code ist weiterhin ein `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

Anstelle einer leeren Seite wird diese Seite angezeigt.

![ Das Bild zeigt die standardmäßige Wartungsseite ](assets/load-balancer-healthcheck/unhealthy-page.png "unhealthy-page") an

### CGI-Bin-Skripte

Es gibt fünf verschiedene Skripte, die von Ihrem CSE in den Lastenausgleichseinstellungen konfiguriert werden können und die das Verhalten bzw. die Kriterien dafür ändern, wann ein Dispatcher aus dem Lastenausgleich gezogen werden soll.

#### /bin/checkauthor

Dieses Skript überprüft und protokolliert alle Instanzen, bei denen es frontiert ist, gibt jedoch nur einen Fehler zurück, wenn die `author`-AEM-Instanz nicht ordnungsgemäß funktioniert

> `Note:` Beachten Sie, dass der Dispatcher im Dienst verbleibt, damit der Datenverkehr zur AEM-Authoring-Instanz geleitet werden kann, wenn die AEM-Publishing-Instanz nicht ordnungsgemäß funktioniert

#### /bin/checkpublish (Standard)

Dieses Skript überprüft und protokolliert alle Instanzen, bei denen es frontiert ist, gibt jedoch nur einen Fehler zurück, wenn die `publish`-AEM-Instanz nicht ordnungsgemäß funktioniert

> `Note:` Beachten Sie, dass der Dispatcher im Dienst verbleibt, damit der Datenverkehr zur AEM-Publishing-Instanz geleitet werden kann, wenn die AEM-Authoring-Instanz nicht ordnungsgemäß ausgeführt wird

#### /bin/checkeither

Dieses Skript überprüft und protokolliert alle Instanzen, bei denen es frontiert ist, gibt jedoch nur einen Fehler zurück, wenn die `author`- oder `publisher`-AEM Instanz nicht ordnungsgemäß ausgeführt wird

> `Note:` Beachten Sie, dass der Dispatcher den Dienst beendet, wenn entweder die AEM-Publishing- oder die AEM-Authoring-Instanz nicht ordnungsgemäß ausgeführt wird.  Das heißt, auch wenn eine von ihnen ordnungsgemäß ausgeführt wird, erhält sie keinen Verkehr

#### /bin/checkboth

Dieses Skript überprüft und protokolliert alle Instanzen, bei denen es frontiert ist, gibt jedoch nur einen Fehler zurück, wenn die AEM-`author`- und -`publisher`-Instanz nicht ordnungsgemäß ausgeführt werden

> `Note:` Beachten Sie, dass der Dispatcher den Dienst nicht beendet, wenn entweder die AEM-Publishing- oder die AEM-Authoring-Instanz nicht ordnungsgemäß ausgeführt wird.  Das heißt, wenn eine von ihnen nicht ordnungsgemäß ausgeführt wird, erhält sie weiterhin Verkehr und verursacht Fehler bei Personen, die Ressourcen anfordern.

#### /bin/healthy

Dieses Skript überprüft und protokolliert alle Instanzen, bei denen es frontiert ist, gibt jedoch alles als ordnungsgemäß ausgeführt zurück, unabhängig davon, ob AEM einen Fehler zurückgibt oder nicht.

> `Note:` Dieses Skript wird verwendet, wenn die Konsistenzprüfung nicht wie gewünscht funktioniert und eine Überschreibung ermöglicht wird, um AEM-Instanzen im Lastenausgleich zu belassen.

[Weiter -> GIT-Symlinks](./git-symlinks.md)
