---
title: AEM Dispatcher – Allgemeine Protokolle
description: Sehen Sie sich die gängigen Protokolleinträge des Dispatchers an und lernen Sie, was sie bedeuten und wie sie behandelt werden.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 252
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '799'
ht-degree: 100%

---

# Allgemeine Protokolle

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Vanity-URLs](./disp-vanity-url.md)

## Übersicht

In diesem Dokument werden gängige Protokolleinträge beschrieben und es wird erläutert, was sie bedeuten und wie sie behandelt werden.

## GLOB-Warnmeldung

Beispiel-Protokolleintrag:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Seit dem Dispatcher-Modul 4.2.x wird davon abgeraten, den folgenden Übereinstimmungstyp in der Filterdatei zu verwenden:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

Stattdessen sollte die neue Syntax wie folgt verwendet werden:

```
/0041 { /type "allow" /url "*.css" }
```

Oder noch besser, um überhaupt keine Übereinstimmung mit einem Platzhalter-Matcher zu finden:

```
/0041 { /type "allow" /extension "css" }
```

Wenn Sie eine der vorgeschlagenen Methoden durchführen, wird die Fehlermeldung in den Protokollen nicht mehr angezeigt.

## Filtern nach Zurückweisungen


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Diese Einträge werden nicht immer angezeigt, selbst wenn Zurückweisungen auftreten, wenn die Protokollebene zu niedrig eingestellt ist. Setzen Sie sie auf „Info“ oder „debug“, um sicherzustellen, dass Sie sehen, ob die Filter die Anfragen ablehnen.
</div>

Beispiel-Protokolleintrag:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

oder:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Vorsicht:</b>

Beachten Sie, dass die Dispatcher-Regeln so eingerichtet wurden, dass sie diese Anfrage herausfiltern. In diesem Fall wurde die Seite, die besucht werden sollte, absichtlich abgelehnt, und dies sollte nicht verändert werden.
</div>

Wenn Ihr Protokoll wie dieser Eintrag aussieht:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Dies besagt, dass die Design-Datei `.eot` blockiert ist und das behoben werden sollte.
Daher sollte die Filterdatei angeschaut und die folgende Zeile hingefügt werden, um `.eot`-Dateien zuzulassen:

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

So kann die Datei durchgelassen und die Protokollierung verhindert werden.
Wenn Sie sehen möchten, was herausgefiltert wird, können Sie diesen Befehl in Ihrer Protokolldatei ausführen:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Zeitüberschreitungen beim Rendern

Beispiel-Protokolleintrag für eine Socket-Zeitüberschreitung:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Dies tritt auf, wenn Sie im Render-Abschnitt Ihrer Farm die falsche IP-Adresse konfiguriert haben. Es liegt entweder dies vor, oder die AEM-Instanz reagierte nicht mehr bzw. wartete nicht hierauf und der Dispatcher konnte sie nicht erreichen.

Überprüfen Sie Ihre Firewall-Regeln und stellen Sie sicher, dass die AEM-Instanz fehlerfrei ausgeführt wird.

Beispiel-Protokolleinträge für Gateway-Zeitüberschreitungen:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Das bedeutet, dass die AEM-Instanz über einen offenen Socket verfügte, den sie erreichen konnte, es bei der Antwort aber zu einer Zeitüberschreitung kam. Das bedeutet, dass Ihre AEM-Instanz zu langsam war bzw. nicht richtig funktionierte und für den Dispatcher die konfigurierten Zeitüberschreitungseinstellungen im Render-Abschnitt der Farm zum Tragen kamen. Erhöhen Sie die Zeitüberschreitungseinstellung bzw. stellen Sie sicher, dass Ihre AEM-Instanz ordnungsgemäß ausgeführt wird.

## Caching-Ebene

Beispiel-Protokolleintrag:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Das bedeutet, dass Ihre Abrufe von der Render-Ebene im Vergleich zu Abrufen vom Cache gemessen werden. Sie sollten mehr als 80 % aus dem Cache erreichen – Hilfe dazu finden Sie [hier](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

Diese Zahl sollte so hoch wie möglich sein.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Wenn entsprechend Ihrer Cache-Einstellungen in der Farm-Datei alles zwischengespeichert werden soll, Sie den Cache aber zu häufig oder zu umfangreich leeren, kann dies den Prozentsatz des Cache-Trefferverhältnisses verringern
</div>

## Fehlendes Verzeichnis

Beispiel-Protokolleintrag:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Dies wird meist angezeigt, wenn ein Element abgerufen wird, während gleichzeitig eine Cache-Leerung erfolgt.

Es liegt entweder dies vor, oder das Basisverzeichnis hat ungünstige Berechtigungen dafür, oder für den Ordner, durch den Apache die neuen erforderlichen Unterverzeichnisse nicht erstellen kann, liegt der falsche SELinux-Dateikontext vor.

Sehen Sie sich bei Problemen mit den Berechtigungen die DocumentRoot-Berechtigungen an und stellen Sie sicher, dass sie in etwa wie folgt aussehen:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## Vanity-URL nicht gefunden

Beispiel-Protokolleintrag:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Dieser Fehler tritt auf, wenn Sie Ihren Dispatcher so konfiguriert haben, dass der dynamische automatische Filter Vanity-URLs zulässt, die Einrichtung jedoch nicht durch Installation des Pakets im AEM-Renderer abgeschlossen wurde.

Um dies zu beheben, installieren Sie das Feature Pack für Vanity-URLs in der AEM-Instanz und lassen Sie zu, dass es für anonyme Benutzende bereit ist. Weitere Informationen dazu finden Sie [hier](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den).

Eine eingerichtete Vanity-URL, die funktioniert, sieht wie folgt aus:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Fehlende Farm

Beispiel-Protokolleintrag:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Dieser Fehler zeigt an, dass in keiner der unter `/etc/httpd/conf.dispatcher.d/enabled_farms/` verfügbaren Farm-Dateien ein passender Eintrag im Abschnitt `/virtualhost` gefunden wurde.

Die Farm-Dateien entsprechen dem Traffic basierend auf dem Domain-Namen oder Pfad, über den die Anfrage eingegangen ist. Es wird ein glob-Abgleich durchgeführt. Gibt es keine Übereinstimmung, haben Sie Ihre Farm nicht richtig konfiguriert, der Eintrag wurde falsch in die Farm eingegeben oder der Eintrag fehlt komplett. Wenn die Farm keinen Einträgen entspricht, wird letztendlich standardmäßig einfach die zuletzt im Stapel der enthaltenen Farm-Dateien enthaltene Farm verwendet. In diesem Beispiel ist dies `999_ams_publish_farm.any` (der generische publishfarm-Name).

Im Folgenden sehen Sie ein Beispiel einer Farm-Datei `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any`, die reduziert wurde, um die relevanten Teile hervorzuheben.

## Element bereitgestellt von

Beispiel-Protokolleintrag:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

Die Seite wurde über die HTTP-GET-Methode für den Inhalt `/content/we-retail/us/en.html` abgerufen (Dauer des Vorgangs: 24034 Millisekunden). Der Teil, auf den wir achten wollen, findet sich ganz am Ende: `publishfarm/0`. Wie zu sehen ist, ist `publishfarm` das Ziel und es wurde ein entsprechender Abgleich durchgeführt. Die Anfrage wurde vom Renderer 0 abgerufen. Das bedeutet, dass diese Seite von AEM angefordert und dann zwischengespeichert werden musste. Als Nächstes fordern wir diese Seite erneut an und sehen, was mit dem Protokoll passiert.

[Weiter -> Schreibgeschützte Dateien](./immutable-files.md)
