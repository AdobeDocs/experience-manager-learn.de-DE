---
title: AEM Dispatcher-Leerung
description: Erfahren Sie, wie AEM alte Cache-Dateien vom Dispatcher ungültig macht.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 7815b1a78949c433f2c53ff752bf39dd55f9ac94
workflow-type: tm+mt
source-wordcount: '2223'
ht-degree: 0%

---


# Dispatcher Vanity-URLs

[Inhalt](./overview.md)

[&lt;- Zurück: Verwenden und Verstehen von Variablen](./variables.md)

In diesem Dokument wird erläutert, wie das Leeren erfolgt, und der Mechanismus erläutert, der das Leeren und Invalidieren des Cache ausführt.


## Funktionsweise

### Reihenfolge der Vorgänge

Der typische Workflow wird am besten beschrieben, wenn Inhaltsautoren eine Seite aktivieren. Wenn der Herausgeber den neuen Inhalt erhält, wird eine Leerungsanfrage an den Dispatcher Trigger, wie im folgenden Diagramm dargestellt:
![Autor aktiviert Inhalte, die vom Trigger-Publisher an den Dispatcher gesendet werden](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
Diese Verkettung von Ereignissen macht deutlich, dass wir Elemente nur dann löschen, wenn sie neu sind oder sich geändert haben.  Dadurch wird sichergestellt, dass der Inhalt vor dem Leeren des Caches beim Herausgeber eingegangen ist, um Race-Bedingungen zu vermeiden, bei denen das Leeren auftreten könnte, bevor die Änderungen vom Herausgeber übernommen werden können.

## Replikationsagenten

Beim Autor gibt es einen Replikationsagenten, der so konfiguriert ist, dass auf den Herausgeber verwiesen wird, dass der Trigger die Datei und alle Abhängigkeiten an den Herausgeber sendet, wenn etwas aktiviert wird.

Wenn der Herausgeber die Datei erhält, hat er einen Replikationsagenten, der so konfiguriert ist, dass er auf den Dispatcher verweist, der auf das On-Receiver-Ereignis Trigger.  Anschließend wird eine Leerungsanfrage serialisiert und an den Dispatcher gesendet.

### AUTORREPLIKATIONSAGENTEN

Im Folgenden finden Sie einige Screenshots eines konfigurierten standardmäßigen Replikationsagenten
![Screenshot des standardmäßigen Replikationsagenten von der AEM Webseite /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

Es gibt in der Regel 1 oder 2 Replikationsagenten, die auf dem Autor für jeden Herausgeber konfiguriert sind, an den sie Inhalte replizieren.

Zunächst ist der standardmäßige Replikationsagent, der Inhaltsaktivierungen an sendet.

Zweitens ist der Rückwärtsagent.  Dies ist optional und ist so eingerichtet, dass bei jedem Herausgeber-Postausgang geprüft wird, ob neuer Inhalt zum Aufrufen an den Autor als Aktivität zur Rückwärtsreplikation vorhanden ist.

### VERÖFFENTLICHUNGS-REPLIKATIONSAGENTEN

Im Folgenden finden Sie ein Beispiel für Screenshots eines konfigurierten standardmäßigen Flush-Replikationsagenten
![Screenshot des standardmäßigen Flush-Replikationsagenten von der AEM Webseite /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### DISPATCHER-FLUSH-REPLIKATION, DIE VIRTUELLEN HOST ERHÄLT

Das Dispatcher-Modul sucht nach bestimmten Headern, um zu erfahren, wann eine POST-Anforderung an AEM Renderer übergeben werden soll oder ob es sich um eine serialisierte Leerungsanfrage handelt und vom Dispatcher-Handler selbst verarbeitet werden muss.

Im Folgenden finden Sie einen Screenshot der Konfigurationsseite mit den folgenden Werten:
![Abbildung der Registerkarte mit den Einstellungen des Hauptkonfigurationsbildschirms mit dem Serialisierungstyp, der als Dispatcher Flush angezeigt wird](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

Auf der Seite mit den Standardeinstellungen wird die `Serialization Type` as `Dispatcher Flush` und legt die Fehlerstufe fest

![Screenshot des Transport-Tabs des Replikationsagenten.  Dies zeigt den URI, an den die Leerungsanfrage gepostet werden soll.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

Im `Transport` -Registerkarte angezeigt wird, können Sie `URI` gesetzt wird, um auf die IP-Adresse des Dispatchers zu verweisen, der die Flush-Anfragen erhält.  Der Pfad `/dispatcher/invalidate.cache` ist nicht die Art, wie das Modul bestimmt, ob es sich um eine Leerung handelt. Es ist nur ein offensichtlicher Endpunkt, den Sie im Zugriffsprotokoll sehen können, um zu erfahren, dass es sich um eine Leerungsanfrage handelt.  Im `Extended` -Tab werden wir die vorhandenen Elemente durchgehen, um zu qualifizieren, dass es sich um eine Leerungsanfrage an das Dispatcher-Modul handelt.

![Screenshot der Registerkarte &quot;Erweitert&quot;des Replikationsagenten.  Notieren Sie die Header, die mit der POST-Anfrage gesendet werden, um den Dispatcher zum Leeren aufzufordern.](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

Die `HTTP Method` für Flush-Anfragen ist nur eine `GET` Anfrage mit einigen speziellen Anfrage-Headern:
- CQ-Action
   - Hierbei wird eine AEM -Variable verwendet, die auf der Anforderung basiert. Der Wert ist normalerweise *Aktivieren oder Löschen*
- CQ-Handle
   - Hierbei wird eine AEM -Variable verwendet, die auf der Anfrage basiert. Der Wert ist normalerweise der vollständige Pfad zum geleerten Element, z. B. `/content/dam/logo.jpg`
- CQ-Path
   - Hierbei wird eine AEM -Variable verwendet, die auf der Anfrage basiert. Der Wert ist normalerweise der vollständige Pfad zum geleerten Element, z. B. `/content/dam`
- Host
   - Hier ist die `Host` Kopfzeile wird zur Zielgruppenbestimmung für eine bestimmte `VirtualHost` , der auf dem Dispatcher-Apache-Webserver konfiguriert ist (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  Der hartcodierte Wert entspricht einem Eintrag im `aem_flush.vhost` -Datei `ServerName` oder `ServerAlias`

![Bildschirm eines standardmäßigen Replikationsagenten, der anzeigt, dass der Replikationsagent reagieren kann und Trigger, wenn neue Elemente von einem Replikationsereignis vom Autor empfangen wurden, der Inhalte veröffentlicht](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

Im `Triggers` registrieren, werden wir die verwendeten umschalteten Trigger und deren

- `Ignore default`
   - Dies ist aktiviert, sodass der Replikationsagent bei einer Seitenaktivierung nicht ausgelöst wird.  Wenn eine Autoreninstanz eine Änderung an einer Seite vornehmen sollte, würde dies zu einer Leerung führen.  Da es sich um einen Herausgeber handelt, möchten wir keinen Trigger von dieser Art von Ereignis machen.
- `On Receive`
   - Wenn eine neue Datei empfangen wird, soll eine Leerung Trigger werden.  Wenn der Autor also eine aktualisierte Datei sendet, wird eine Leerungsanfrage an den Dispatcher Trigger und gesendet.
- `No Versioning`
   - Wir überprüfen dies, um zu verhindern, dass der Herausgeber neue Versionen generiert, da eine neue Datei empfangen wurde.  Wir werden nur die Datei ersetzen, die wir haben, und uns darauf verlassen, dass der Autor die Versionen statt des Herausgebers verfolgt.

Wenn wir uns nun ansehen, wie eine typische Flush-Anfrage in Form einer `curl` command

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

Dieses Leerungsbeispiel würde die `/content/dam` Pfad durch Aktualisieren der `.stat` -Datei in diesem Verzeichnis.

## Die `.stat` file

Der Flushing-Mechanismus ist einfach und wir möchten die Bedeutung der `.stat` -Dateien, die im Basisverzeichnis generiert werden, in dem die Cachedateien erstellt werden.

Innerhalb des `.vhost` und `_farm.any` -Dateien konfigurieren wir eine Dokument-Stamm-Direktive, um anzugeben, wo sich der Cache befindet und wo Dateien gespeichert/bereitgestellt werden sollen, wenn eine Anfrage von einem Endbenutzer eingeht.

Wenn Sie den folgenden Befehl auf Ihrem Dispatcher-Server ausführen würden, würden Sie mit der Suche beginnen `.stat` files

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Hier sehen Sie ein Diagramm, wie diese Dateistruktur aussieht, wenn Sie Elemente im Cache haben und eine Leerungsanfrage vom Dispatcher-Modul gesendet und verarbeitet wurde.

![Statfiles gemischt mit Inhalt und Daten, angezeigt mit STAT-Levels](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### STAT-DATEIEBENE

Beachten Sie, dass in jedem Verzeichnis ein `.stat` vorhanden ist.  Dies ist ein Hinweis darauf, dass eine Leerung aufgetreten ist.  Im obigen Beispiel wird die `statfilelevel` festgelegt wurde auf `3` in der entsprechenden Farm-Konfigurationsdatei.

Die `statfilelevel` -Einstellung gibt an, wie viele Ordner tief das Modul durchlaufen und eine `.stat` -Datei.  Die STAT-Datei ist leer, es handelt sich lediglich um einen Dateinamen mit einem Datenstempel. Sie kann sogar manuell erstellt werden, aber den Touch-Befehl in der Befehlszeile des Dispatcher-Servers ausführen.

Wenn die Einstellung auf stat-Dateiebene zu hoch eingestellt ist, durchläuft jede Leerungsanfrage die Verzeichnisstruktur und ändert stat-Dateien.  Dies kann zu einem großen Leistungseinbruch bei großen Cache-Bäumen werden und sich auf die Gesamtleistung Ihres Dispatchers auswirken.

Wenn Sie diese Dateiebene zu niedrig festlegen, kann eine Leerungsanfrage dazu führen, dass mehr gelöscht wird, als beabsichtigt war.  Dies würde wiederum dazu führen, dass der Cache häufiger abwandert und weniger Anforderungen aus dem Cache bereitgestellt werden, und kann Leistungsprobleme verursachen.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Legen Sie die `statfilelevel` auf einer vernünftigen Ebene.  Schauen Sie sich Ihre Ordnerstruktur an und stellen Sie sicher, dass sie so eingerichtet ist, dass knappe Leeren möglich sind, ohne zu viele Verzeichnisse durchlaufen zu müssen.   Testen Sie es und stellen Sie während eines Leistungstests des Systems sicher, dass es Ihren Anforderungen entspricht.

Ein gutes Beispiel ist eine Site, die Sprachen unterstützt.  Die typische Inhaltsstruktur würde die folgenden Verzeichnisse aufweisen

`/content/brand1/en/us/`

Verwenden Sie in diesem Beispiel eine stat-Dateiebene-Einstellung von 4.  Dadurch wird sichergestellt, dass Inhalte geleert werden, die unter dem <b>`us`</b> -Ordner, der nicht dazu führt, dass auch die Sprachordner geleert werden.
</div>

### STAT FILE TIMESTAMP HANDSHAKE

Wenn eine Inhaltsanforderung in derselben Routine erfolgt

1. Zeitstempel der `.stat` wird mit dem Zeitstempel der angeforderten Datei verglichen
2. Wenn die Variable `.stat` -Datei aktueller als die angeforderte Datei ist, löscht sie den zwischengespeicherten Inhalt und ruft eine neue aus AEM ab und speichert sie zwischen.  Dann wird der Inhalt bereitgestellt
3. Wenn die Variable `.stat` -Datei älter als die angeforderte Datei ist, weiß sie dann, dass die Datei neu ist und den Inhalt bedienen kann.

### CACHE HANDSHAKE - BEISPIEL 1

Im obigen Beispiel eine Anfrage für den Inhalt `/content/index.html`

Die Uhrzeit der `index.html` Datei ist 2019-11-01 @ 6:21PM

Die Uhrzeit der nächsten `.stat` Datei ist 2019-11-01 @ 12:22 PM

Wenn Sie wissen, was wir oben gelesen haben, können Sie sehen, dass die Indexdatei neuer ist als die `.stat` -Datei und die Datei wird vom Cache an den Endbenutzer geliefert, der sie angefordert hat

### CACHE HANDSHAKE - BEISPIEL 2

Im obigen Beispiel eine Anfrage für den Inhalt `/content/dam/logo.jpg`

Die Uhrzeit der `logo.jpg` Datei ist 2019-10-31 @ 1:13PM

Die Uhrzeit der nächsten `.stat` Datei ist 2019-11-01 @ 12:22 PM

Wie in diesem Beispiel gezeigt, ist die Datei älter als die `.stat` und wird entfernt und eine neue wird aus AEM abgerufen, um sie im Cache zu ersetzen, bevor sie dem Endbenutzer bereitgestellt wird, der sie angefordert hat.

## Farm-Dateieinstellungen

Die Dokumentation enthält alle Konfigurationsoptionen: [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de)

Wir möchten einige von ihnen hervorheben, die sich auf die Cache-Leerung beziehen

### Flush-Farmen

Es gibt zwei Schlüssel `document root` -Verzeichnissen, die Dateien aus dem Autoren- und Herausgeber-Traffic zwischenspeichern.  Um diese Verzeichnisse mit neuem Inhalt auf dem neuesten Stand zu halten, müssen wir den Cache leeren.  Diese Flush-Anfragen möchten nicht an Ihre normalen Traffic-Farm-Konfigurationen angepasst werden, die die Anfrage ablehnen oder etwas Unerwünschtes tun könnten.  Stattdessen stellen wir zwei Flush-Farmen für diese Aufgabe bereit:

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

Diese Farm-Dateien haben nichts anderes als die Ordner des Dokumentenstamms zu leeren.

```
/publishflushfarm {  
    /virtualhosts {
        "flush"
    }
    /cache {
        /docroot "${PUBLISH_DOCROOT}"
        /statfileslevel "${DEFAULT_STAT_LEVEL}"
        /rules {
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
        }
        /invalidate {
            /0000 {
                /glob "*"
                /type "allow"
            }
        }
        /allowedClients {
            /0000 {
                /glob "*.*.*.*"
                /type "deny"
            }
            $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
        }
    }
}
```

### Dokumentenstamm

Dieser Konfigurationseintrag befindet sich im folgenden Abschnitt der Farm-Datei:

```
/myfarm { 
    /cache { 
        /docroot
```

Sie geben das Verzeichnis an, in dem der Dispatcher als Cache-Verzeichnis gefüllt und verwaltet werden soll.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Dieser Ordner sollte mit der Apache Document Root-Einstellung für die Domäne übereinstimmen, für die Ihr Webserver konfiguriert ist.

Verschachtelte Basisordner pro Farm, die Unterordner des Apache-Basisverzeichnisses sind, sind aus vielen Gründen eine schreckliche Idee.
</div>

### Ebene der statischen Dateien

Dieser Konfigurationseintrag befindet sich im folgenden Abschnitt der Farm-Datei:

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Mit dieser Einstellung wird gemessen, wie tief `.stat` -Dateien müssen generiert werden, wenn eine Leerungsanfrage eingeht.

`/statfileslevel` auf die folgende Zahl mit dem Basisverzeichnis von `/var/www/html/` würde bei der Flushing die folgenden Ergebnisse haben `/content/dam/brand1/en/us/logo.jpg`

- 0 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
- 1 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 - Die folgenden stat-Dateien werden erstellt
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Denken Sie daran, dass der Zeitstempel-Handshake nach dem nächstgelegenen `.stat` -Datei.

mit `.stat` Dateiebene 0 und stat-Datei nur auf `/var/www/html/.stat` bedeutet, dass Inhalt, der unter `/var/www/html/content/dam/brand1/en/us/` würde nach der nächsten suchen `.stat` Datei speichern und fünf Ordner durchlaufen, um die einzige `.stat` -Datei, die auf Ebene 0 existiert und Datumswerte mit dieser vergleicht.  Das bedeutet, dass eine Leerung auf dieser hohen Ebene im Wesentlichen alle zwischengespeicherten Elemente ungültig macht.
</div>

### Invalidierung zulässig

Dieser Konfigurationseintrag befindet sich im folgenden Abschnitt der Farm-Datei:

```
/myfarm { 
    /cache { 
        /allowedClients {
```

Innerhalb dieser Konfiguration legen Sie eine Liste von IP-Adressen fest, die Leerungsanfragen senden dürfen.  Wenn eine Leerungsanfrage in den Dispatcher kommt, muss sie von einer vertrauenswürdigen IP-Adresse stammen.  Wenn Sie diese Konfiguration falsch konfiguriert haben oder eine Leerungsanfrage von einer nicht vertrauenswürdigen IP-Adresse senden, wird der folgende Fehler in der Protokolldatei angezeigt:

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### Invalidierungsregeln

Dieser Konfigurationseintrag befindet sich im folgenden Abschnitt der Farm-Datei:

```
/myfarm { 
    /cache { 
        /invalidate {
```

Diese Regeln geben normalerweise an, welche Dateien bei einer Leerungsanfrage invalidiert werden dürfen.

Um zu verhindern, dass wichtige Dateien bei einer Seitenaktivierung invalidiert werden, können Sie Regeln anwenden, die angeben, welche Dateien ungültig gemacht werden dürfen und welche manuell invalidiert werden müssen.  Im Folgenden finden Sie einen Beispielsatz an Konfigurationen, die nur die Invalidierung von HTML-Dateien ermöglichen:

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## Tests/Fehlerbehebung

Wenn Sie eine Seite aktivieren und grünes Licht dafür erhalten, dass die Seitenaktivierung erfolgreich war, sollten Sie erwarten, dass der aktivierte Inhalt auch aus dem Cache geleert wird.

Sie aktualisieren Ihre Seite und sehen die alten Dinge! Was? Es gab ein grünes Licht?!

Gehen wir ein paar manuelle Schritte durch den Flushing-Prozess durch, um uns einen Einblick zu verschaffen, was falsch sein könnte.  Führen Sie in der Publisher-Shell die folgende Leerungsanforderung mit curl aus:

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

Beispieltest für eine Leerungsanfrage

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Nachdem Sie den Anforderungsbefehl an den Dispatcher ausgelöst haben, möchten Sie sehen, was in den Protokollen geschehen ist und was mit dem `.stat files`.  Verfolgen Sie die Protokolldatei und Sie sollten die folgenden Einträge sehen, um den Flush-Aufruf des Dispatcher-Moduls zu bestätigen.

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Nachdem wir nun sehen, dass das Modul die Leerungsanfrage erfasst und bestätigt hat, müssen wir sehen, wie sie sich auf die `.stat` Dateien.  Führen Sie den folgenden Befehl aus und beobachten Sie die Aktualisierung der Zeitstempel, wenn Sie eine weitere Flush-Benachrichtigung durchführen:

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Wie Sie in der Befehlsausgabe sehen können, werden die Zeitstempel der aktuellen `.stat` files

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

Wenn wir die Flush-Benachrichtigung jetzt erneut ausführen, sehen Sie sich die Zeitstempel-Aktualisierung an

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

Vergleichen wir unsere Inhalts-Zeitstempel mit unseren `.stat` Dateien Zeitstempel

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

Wenn Sie sich einen der Zeitstempel ansehen, werden Sie feststellen, dass der Inhalt eine neuere Zeit hat als die `.stat` -Datei, die das -Modul anweist, die Datei aus dem Cache bereitzustellen, da sie neuer ist als die `.stat` -Datei.

Setzen Sie einfach etwas aktualisiert die Zeitstempel dieser Datei, die es nicht als &quot;geleert&quot; oder ersetzt zu qualifizieren.

[Weiter -> Vanity-URLs](./disp-vanity-url.md)