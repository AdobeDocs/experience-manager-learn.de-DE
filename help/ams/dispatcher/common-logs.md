---
title: AEM Dispatcher - Allgemeine Protokolle
description: Sehen Sie sich die gängigen Protokolleinträge des Dispatchers an und lernen Sie, was sie bedeuten und wie sie angegangen werden können.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# Allgemeine Protokolle

[Inhalt](./overview.md)

[&lt;- Zurück: Vanity-URLs](./disp-vanity-url.md)

## Übersicht

In diesem Dokument werden gängige Protokolleinträge beschrieben, was sie bedeuten und wie sie behandelt werden.

## GLOB-Warnung

Beispielprotokolleintrag:

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

Wenn Sie eine der vorgeschlagenen Methoden durchführen, wird diese Fehlermeldung aus den Protokollen entfernt.

## Zurückweisungen filtern


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Diese Einträge werden nicht immer angezeigt, selbst wenn Zurückweisungen auftreten, wenn die Protokollebene zu niedrig eingestellt ist. Setzen Sie sie auf Info oder debug , um sicherzustellen, dass Sie sehen, ob die Filter die Anforderungen ablehnen.
</div>

Beispielprotokolleintrag:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

oder:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Vorsicht:</b>

Beachten Sie, dass die Dispatcher-Regeln so eingerichtet wurden, dass sie diese Anforderung herausfiltern. In diesem Fall wurde die Seite, die besucht werden wollte, absichtlich abgelehnt, und wir möchten nichts daran ändern.
</div>

Wenn Ihr Protokoll wie folgt aussieht:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Das lässt uns wissen, dass unsere Designdatei `.eot` wird blockiert, und wir werden das beheben wollen.
Daher sollten wir uns unsere Filterdatei ansehen und die folgende Zeile hinzufügen, um `.eot` Dateien über

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Dadurch kann die Datei durchlaufen und die Protokollierung verhindert werden.
Wenn Sie sehen möchten, was herausgefiltert wird, können Sie diesen Befehl in Ihrer Protokolldatei ausführen:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Timeouts von Rendering

Beispiel für einen Protokolleintrag für Socket-Timeout:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Dies tritt auf, wenn Sie die falsche IP-Adresse im Renderer-Abschnitt Ihrer Farm konfiguriert haben. Diese oder die AEM Instanz reagierte nicht mehr oder hörte nicht zu und der Dispatcher kann sie nicht erreichen.

Überprüfen Sie Ihre Firewall-Regeln und stellen Sie sicher, dass die AEM-Instanz ausgeführt und fehlerfrei ist.

Beispiel-Protokolleinträge für Gateway-Timeout:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Das bedeutet, dass die AEM-Instanz über einen offenen Socket verfügte, den sie erreichen konnte, und mit der Antwort eine Zeitüberschreitung aufwies. Das bedeutet, dass Ihre AEM Instanz zu langsam oder nicht gesund war und der Dispatcher die konfigurierten Timeout-Einstellungen im Renderer-Abschnitt der Farm erreicht hat. Erhöhen Sie entweder die Timeout-Einstellung oder stellen Sie sicher, dass Ihre AEM ordnungsgemäß ausgeführt wird.

## Caching-Ebene

Beispielprotokolleintrag:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Das bedeutet, dass Ihr Abruf von der Renderebene im Vergleich zum Cache gemessen wird. Sie möchten mehr als 80 % aus dem Cache erreichen und sollten die Hilfe befolgen [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

Um diese Zahl so hoch wie möglich zu bekommen.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Selbst wenn Sie Ihre Cache-Einstellungen in der Farm-Datei haben, um alles zwischenzuspeichern, das Sie möglicherweise zu häufig oder zu aggressiv löschen, kann dies zu einem geringeren Prozentsatz des Cache-Trefferverhältnisses führen
</div>

## Fehlendes Verzeichnis

Beispiel für einen Protokolleintrag:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Dies wird normalerweise angezeigt, wenn ein Element abgerufen wird, während gleichzeitig eine Cache-Löschung erfolgt.

Diese oder das Basisverzeichnis hat schlechte Berechtigungen dafür oder den falschen SELinux-Dateikontext für den Ordner, der Apache daran hindert, die neuen erforderlichen Unterverzeichnisse zu erstellen.

Bei Berechtigungsproblemen sehen Sie sich die Berechtigungen des Dokumentenstamms an und stellen Sie sicher, dass sie in etwa wie folgt aussehen:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## Vanity-URL nicht gefunden

Beispiel für einen Protokolleintrag:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Dieser Fehler tritt auf, wenn Sie Ihren Dispatcher so konfiguriert haben, dass der dynamische automatische Filter Vanity-URLs zulässt, die Einrichtung jedoch nicht abgeschlossen haben, indem Sie das Paket auf dem AEM Renderer installieren.

Um dies zu beheben, installieren Sie bitte das Feature Pack für die Vanity-URL auf der AEM-Instanz und lassen Sie zu, dass es vom anonymen Benutzer bereit ist. Details [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

Eine funktionierende Vanity-URL-Einrichtung sieht wie folgt aus:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Fehlende Farm

Beispiel für einen Protokolleintrag:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Dieser Fehler zeigt an, dass aus allen in `/etc/httpd/conf.dispatcher.d/enabled_farms/` sie konnten keinen passenden Eintrag aus dem `/virtualhost` Abschnitt.

Die Farm-Dateien entsprechen dem Traffic basierend auf dem Domänennamen oder Pfad, in dem die Anfrage einging. Es verwendet glob-Abgleich, und wenn es nicht übereinstimmt, haben Sie entweder Ihre Farm nicht richtig konfiguriert, tippen Sie den Eintrag in die Farm ein oder haben den Eintrag komplett fehlt. Wenn die Farm keinen Einträgen entspricht, wird standardmäßig nur die letzte Farm verwendet, die im Stapel der enthaltenen Farm-Dateien enthalten ist. In diesem Beispiel war es `999_ams_publish_farm.any` , der den allgemeinen Namen von publishfarm trägt.

Im Folgenden finden Sie eine Beispiel-Farm-Datei `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` wurde reduziert, um die relevanten Teile hervorzuheben.

## Element bereitgestellt von

Beispiel für einen Protokolleintrag:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

Die Seite wurde über die GET HTTP-Methode für den Inhalt abgerufen `/content/we-retail/us/en.html` und es dauerte 24034 Millisekunden. Der Teil, auf den wir achten wollten, ist am Ende `publishfarm/0`. Sie werden feststellen, dass die Zielgruppe mit der `publishfarm`. Die Anfrage wurde vom Renderer 0 abgerufen. Das bedeutet, dass diese Seite von AEM angefordert und dann zwischengespeichert werden musste. Jetzt fordern wir diese Seite erneut an und sehen, was mit dem Protokoll passiert.

[Weiter -> schreibgeschützte Dateien](./immutable-files.md)