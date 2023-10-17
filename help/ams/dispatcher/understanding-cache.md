---
title: Dispatcher – Grundlegendes zum Caching
description: Erfahren Sie, wie das Dispatcher-Modul den Cache verwendet.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '1747'
ht-degree: 100%

---

# Grundlegendes zum Caching

[Inhaltsverzeichnis](./overview.md)

[&lt;- Zurück: Erläuterung der Konfigurationsdateien](./explanation-config-files.md)

In diesem Dokument wird erläutert, wie das Dispatcher-Caching erfolgt und wie es konfiguriert werden kann.

## Caching von Verzeichnissen

Wir verwenden die folgenden standardmäßigen Cache-Verzeichnisse in unseren Basisinstallationen.

- Author
   - `/mnt/var/www/author`
- Publisher
   - `/mnt/var/www/html`

Jede Anfrage, die den Dispatcher durchläuft, folgt den konfigurierten Regeln, um eine lokal zwischengespeicherte Version beizubehalten und so auf geeignete Elemente reagieren zu können.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Wir halten die veröffentlichte Workload absichtlich von der Autoren-Workload getrennt, da Apache beim Suchen nach einer Datei im DocumentRoot nicht weiß, von welcher AEM-Instanz sie stammt. Selbst wenn also der Cache in der Autoren-Farm deaktiviert ist, werden etwaige vorhandene Dateien aus dem Cache bereitgestellt, wenn der autorenseitige DocumentRoot mit dem Publisher übereinstimmt. Das bedeutet, dass Sie Autorendateien aus dem veröffentlichten Cache bereitstellen. Ihre Besucherinnen und Besucher werden dadurch leider mit einem wirklich unschönen Mix-Match-Erlebnis konfrontiert.

Separate DocumentRoot-Verzeichnisse für verschiedene veröffentlichte Inhalte zu haben, ist ebenfalls keine gute Idee. Sie müssen dann mehrere erneut zwischengespeicherte Elemente erstellen, die sich zwischen Sites wie Clientlibs nicht unterscheiden, und für jeden von Ihnen eingerichteten DocumentRoot einen Replikations-Flush-Agenten konfigurieren. Mit jeder Seitenaktivierung erhöht sich so der Flush-Aufwand. Verlassen Sie sich auf den Namespace von Dateien und deren vollständig zwischengespeicherte Pfade, und vermeiden Sie mehrere DocumentRoots für veröffentlichte Sites.
</div>

## Konfigurationsdateien

Der Dispatcher steuert, was im Abschnitt `/cache {` einer beliebigen Farm-Datei als zwischenspeicherbar gilt. 
In den grundlegenden AMS-Konfigurationsfarmen finden Sie unsere include-Anweisungen, wie unten gezeigt:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Informationen zum Erstellen der Regeln für das Zwischenspeichern von Elementen finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#configuring-the-dispatcher-cache-cache) in der Dokumentation. 


## Zwischenspeicherung von Autoreninhalten

Wir haben viele Implementierungen gesehen, in denen auf eine Zwischenspeicherung von Autoreninhalten verzichtet wurde. 
Den Autorinnen und Autoren entgeht so eine enorme Steigerung der Leistung und Reaktionsfähigkeit.

Sprechen wir nun über die Strategie, die wir gewählt haben, um unsere Autoren-Farm so zu konfigurieren, dass eine ordnungsgemäße Zwischenspeicherung sichergestellt ist.

Das ist ein Basis-Autorenabschnitt `/cache {` unserer Autoren-Farm-Datei:


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
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
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

Wichtig ist hier, dass `/docroot` auf das autorenbezogene Cache-Verzeichnis festgelegt ist.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Stellen Sie sicher, dass `DocumentRoot` in der Autorendatei `.vhost` mit dem Farm-Parameter `/docroot` übereinstimmt.
</div>

Die include-Anweisung der Cache-Regeln enthält die Datei `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` mit den folgenden Regeln:

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

In einem Autorenszenario ändert sich der Inhalt ständig, und dies ist gewollt. Sie sollten nur die Elemente zwischenspeichern, die sich nicht häufig ändern.
Es gibt Regeln zum Zwischenspeichern von `/libs`, weil diese zur AEM-Basisinstallation gehören und so lange geändert werden, bis Sie ein Service Pack, ein Cumulative Fix Pack, ein Upgrade oder einen Hotfix installiert haben. Das Zwischenspeichern dieser Elemente ist also sehr sinnvoll und bietet wirklich große Vorteile für das Autorenerlebnis von Endbenutzenden, die die Site nutzen.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Denken Sie daran, dass diese Regeln auch zum Zwischenspeichern von <b>`/apps`</b>, dem Speicherort von benutzerdefiniertem Anwendungs-Code, führen. Falls Sie Ihren Code in dieser Instanz entwickeln, wird sich dies als überaus verwirrend erweisen, wenn Sie Ihre Datei speichern und nicht sehen, ob der Code sich in der Benutzeroberfläche widerspiegelt, da eine zwischengespeicherte Kopie bereitgestellt wird. Die Absicht hier: Wenn Sie Ihren Code in AEM implementieren, würde auch dies nur selten geschehen, und es sollte zu Ihren Bereitstellungsschritten gehören, den Autoren-Cache zu löschen. Auch hier ist der Vorteil enorm, sodass Ihr zwischenspeicherbarer Code für die Endbenutzenden schneller ausgeführt werden kann.
</div>


## ServeOnStale (auch bekannt als Serve on Stale/SOS)

Dies ist eine der herausragenden Funktionen des Dispatchers. Wenn der Publisher unter Belastung steht oder nicht mehr reagiert, wird normalerweise der HTTP-Antwort-Code 502 oder 503 ausgegeben. Kommt es dazu und ist diese Funktion aktiviert, wird der Dispatcher angewiesen, nach Kräften weiterhin alle noch zwischengespeicherten Inhalte bereitzustellen, selbst wenn es sich nicht um eine neue Kopie handelt. Es ist besser, etwas bereitzustellen, sofern vorhanden, statt nur eine Fehlermeldung ohne Funktionalität anzuzeigen.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Beachten Sie, dass diese Funktion nicht ausgelöst wird, wenn der Renderer des Publishers eine Socket-Zeitüberschreitung oder eine 500-Fehlermeldung ausgibt. Wenn AEM einfach unerreichbar ist, bewirkt diese Funktion nichts.
</div>

Diese Einstellung kann in jeder Farm festgelegt werden, es ist jedoch nur sinnvoll, sie auf die Farm-Dateien im Veröffentlichungsmodus anzuwenden. Im Folgenden finden Sie ein Syntaxbeispiel der Funktion, die in einer Farm-Datei aktiviert ist:

```
/cache { 
    /serveStaleOnError "1"
```

## Zwischenspeichern von Seiten mit Abfrageparametern/Argumenten

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Eine der normalen Verhaltensweisen des Dispatcher-Moduls besteht darin, dass bei einer Anfrage mit einem Abfrageparameter im URI (in der Regel `/content/page.html?myquery=value`) das Zwischenspeichern der Datei übersprungen und direkt zur AEM-Instanz gewechselt wird. Es betrachtet diese Anfrage als eine dynamische Seite, die nicht zwischengespeichert werden sollte. Dies kann sich negativ auf die Cache-Effizienz auswirken.
</div>
<br/>

In diesem [Artikel](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) wird gezeigt, wie wichtige Abfrageparameter die Leistung Ihrer Website beeinflussen können.

Standardmäßig sollten Sie die `ignoreUrlParams`-Regeln so einstellen, dass sie `*` zulassen. Das bedeutet, dass alle Abfrageparameter ignoriert werden und alle Seiten zwischengespeichert werden können, unabhängig von den verwendeten Parametern.

Hier ist ein Beispiel, bei dem jemand einen Deep-Link-Referenzmechanismus für soziale Medien erstellt hat, der die Argumentreferenz im URI verwendet, um zu erfahren, woher die Person stammt.

<b>Ignorierbares Beispiel:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

Die Seite ist zu 100 % zwischenspeicherfähig, wird aber nicht zwischengespeichert, weil die Argumente vorhanden sind. 
Wenn Sie Ihre `ignoreUrlParams` als Zulassungsliste konfigurieren, können Sie dieses Problem beheben:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Wenn der Dispatcher die Anfrage sieht, ignoriert er die Tatsache, dass die Anfrage den `query`-Parameter der `?`-Referenz hat, und speichert die Seite trotzdem zwischen.

<b>Dynamisches Beispiel:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Denken Sie daran, dass Sie bei Abfrageparametern, die dazu führen, dass eine Seite ihre gerenderte Ausgabe ändert, diese aus Ihrer Ignorierliste ausschließen und die Seite wieder nicht zwischenspeicherfähig machen müssen. Beispielsweise ändert eine Suchseite, die einen Abfrageparameter verwendet, den rohen HTML-Code, der gerendert wird.

Hier ist also die HTML-Quelle jeder Suche:

`/search.html?q=fruit`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Wenn Sie `/search.html?q=fruit` zum ersten Mal besuchen, wird die HTML-Datei mit den Ergebnissen, die Früchte zeigen, zwischengespeichert.

Wenn Sie dann ein zweites Mal `/search.html?q=vegetables` besuchen, werden immer noch Ergebnisse mit Früchten angezeigt.
Dies liegt daran, dass der Abfrageparameter `q` im Hinblick auf die Zwischenspeicherung ignoriert wird. Um dieses Problem zu vermeiden, müssen Sie auf Seiten achten, die aufgrund von Abfrageparametern unterschiedliches HTML darstellen, und für diese Seiten die Zwischenspeicherung verweigern.

Beispiel:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Seiten, die Abfrageparameter über JavaScript verwenden, funktionieren weiterhin vollständig, wobei die Parameter in dieser Einstellung ignoriert werden. Dies passiert, weil sie die HTML-Datei im Ruhezustand nicht ändern. Sie verwenden Javascript, um die Domain des Browsers in Echtzeit auf dem lokalen Browser zu aktualisieren. Wenn Sie also die Abfrageparameter mit JavaScript verwenden, ist es sehr wahrscheinlich, dass Sie diesen Parameter für die Seitenzwischenspeicherung ignorieren können. Lassen Sie diese Seite zwischenspeichern und genießen Sie die Leistungssteigerung!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Diese Seiten im Auge zu behalten, erfordert zwar einen gewissen Aufwand, doch er lohnt sich im Hinblick auf den Leistungsgewinn. Sie können Ihr CSE bitten, einen Bericht über den Traffic Ihrer Website zu erstellen, um eine Liste aller Seiten zu erhalten, die in den letzten 90 Tagen Abfrageparameter verwendet haben. So können Sie analysieren und sicherstellen, dass Sie wissen, welche Seiten Sie sich ansehen und welche Abfrageparameter Sie nicht ignorieren sollten.
</div>
<br/>

## Zwischenspeichern von Antwort-Headern

Es ist ziemlich offensichtlich, dass der Dispatcher `.html`-Seiten und Clientlibs (d. h. `.js`, `.css`) zwischenspeichert, aber wussten Sie, dass er auch bestimmte Antwort-Header zusammen mit dem Inhalt in einer Datei mit dem gleichen Namen, aber einer `.h`-Dateierweiterung zwischenspeichern kann? Dadurch kann die nächste Antwort nicht nur auf den Inhalt, sondern auch auf die Antwort-Header angewendet werden, die aus dem Cache gesendet werden sollen.

AEM kann mehr als nur UTF-8-Codierung verarbeiten

Manchmal verfügen Elemente über spezielle Header, mit denen die Codierungsdetails der TTL-Cache-Datei und die zuletzt geänderten Zeitstempel gesteuert werden können.

Diese Werte werden beim Zwischenspeichern standardmäßig entfernt, und der Apache httpd-Webserver übernimmt die Verarbeitung des Assets mit seinen normalen Dateibearbeitungsmethoden, die sich normalerweise auf die Erkennung der MIME-Typen anhand der Dateierweiterungen beschränken.

Wenn Sie den Dispatcher das Asset und die gewünschten Header zwischenspeichern lassen, können Sie das richtige Erlebnis bieten und sicherstellen, dass alle Details im Browser der Kundin bzw. des Kunden angezeigt werden.

Im Folgenden finden Sie ein Beispiel für eine Farm mit Angabe der Header, die zwischengespeichert werden sollen:

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


Im Beispiel wurde AEM so konfiguriert, dass Header bereitgestellt werden, nach denen das CDN sucht, um zu wissen, wann der Cache invalidiert werden muss. Das bedeutet nun, AEM kann ordnungsgemäß bestimmen, welche Dateien basierend auf Headern invalidiert werden.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Denken Sie daran, dass Sie keine regulären Ausdrücke bzw. keinen glob-Abgleich verwenden können. Es ist eine Liste der zwischenzuspeichernden literal-Header. Fügen Sie nur eine Liste der literal-Header ein, die zwischengespeichert werden sollen.
</div>


## Nachfrist für die automatische Invalidierung

Wenn es autorenseitig auf AEM-Systemen zahlreiche Aktivitäten durch viele Seitenaktivierungen gibt, kann es zu Überschneidungen mit sich wiederholenden Invalidierungen kommen. Sich ständig wiederholende Flush-Anfragen sind nicht erforderlich und Sie können eine gewisse Toleranz aufbauen, damit ein Flush-Vorgang erst nach Ablauf der Nachfrist wiederholt wird.

### Beispiel für die Funktionsweise:

Wenn 5 Anfragen zur Invalidierung von `/content/exampleco/en/` vorliegen, finden sie alle innerhalb eines Zeitraums von 3 Sekunden statt.

Ist diese Funktion deaktiviert, wird das Cache-Verzeichnis `/content/exampleco/en/` 5-mal invalidiert.

Ist diese Funktion aktiviert und auf 5 Sekunden eingestellt, wird das Cache-Verzeichnis `/content/exampleco/en/` <b>einmal</b> invalidiert.

Im Folgenden finden Sie eine Beispielsyntax dieser Funktion, die für eine Nachfrist von 5 Sekunden konfiguriert ist:

```
/cache { 
    /gracePeriod "5"
```

## TTL-basierte Invalidierung

Zu den neueren Funktionen des Dispatcher-Moduls gehören die auf `Time To Live (TTL)` basierenden Invalidierungsoptionen für zwischengespeicherte Elemente. Wenn ein Element zwischengespeichert wird, prüft es auf vorhandene Cache-Kontroll-Header und generiert eine Datei im Cache-Verzeichnis mit demselben Namen und der Erweiterung `.ttl`.

Im Folgenden finden Sie ein Beispiel der Funktion, die in der Farm-Konfigurationsdatei konfiguriert ist:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Denken Sie daran, dass AEM trotzdem zum Senden von TTL-Headern konfiguriert werden muss, damit sie vom Dispatcher berücksichtigt werden. Durch Umschalten dieser Funktion weiß der Dispatcher lediglich, dass Dateien entfernt werden sollen, für die AEM Cache-Kontroll-Header gesendet hat. Wenn keine TTL-Header von AEM gesendet werden, führt der Dispatcher hier keine besonderen Aktionen durch.
</div>

## Cache-Filterregeln

Im Folgenden finden Sie ein Beispiel für eine Basiskonfiguration, die festlegt, welche Elemente in einem Publisher zwischengespeichert werden sollen:

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

Unsere veröffentlichte Site soll so „gierig“ wie möglich sein und alles zwischenspeichern.

Wenn das Zwischenspeichern eines Elements das Erlebnis beeinträchtigt, können Sie Regeln hinzufügen, um die Option zum Zwischenspeichern dieses Elements zu entfernen. Wie Sie im obigen Beispiel sehen, sollten die CSRF-Token nie zwischengespeichert, sondern davon ausgeschlossen werden. Weitere Informationen zum Schreiben dieser Regeln finden Sie [hier](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#configuring-the-dispatcher-cache-cache).

[Weiter -> Verwenden und Verstehen von Variablen](./variables.md)
