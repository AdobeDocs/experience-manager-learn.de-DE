---
title: Dispatcher - Grundlagen zur Zwischenspeicherung
description: Erfahren Sie, wie das Dispatcher-Modul den Cache verwendet.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# Grundlagen zum Zwischenspeichern

[Inhalt](./overview.md)

[&lt;- Zurück: Erläuterung der Konfigurationsdateien](./explanation-config-files.md)

In diesem Dokument wird erläutert, wie das Dispatcher-Caching erfolgt und wie es konfiguriert werden kann

## Zwischenspeichern von Ordnern

Wir verwenden die folgenden standardmäßigen Cache-Ordner in unseren Basisinstallationen

- Autor
   - `/mnt/var/www/author`
- Herausgeber
   - `/mnt/var/www/html`

Wenn jede Anfrage den Dispatcher durchläuft, befolgen die Anforderungen die konfigurierten Regeln, um eine lokal zwischengespeicherte Version auf die Antwort der geeigneten Elemente beizubehalten

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Die veröffentlichte Arbeitslast wird absichtlich vom Autoren-Arbeitsaufwand getrennt gehalten, da Apache beim Suchen nach einer Datei im DocumentRoot nicht weiß, von welcher AEM Instanz sie stammt. Selbst wenn Sie also den Cache in der Autorenfarm deaktiviert haben, werden Dateien aus dem Cache bereitgestellt, wenn der DocumentRoot des Autors mit dem Publisher übereinstimmt. Das bedeutet, dass Sie Autorendateien für aus dem veröffentlichten Cache bereitstellen und für ein wirklich schreckliches Mix-Match-Erlebnis für Ihre Besucher sorgen.

Die Beibehaltung separater DocumentRoot-Ordner für verschiedene veröffentlichte Inhalte ist ebenfalls eine schlechte Idee. Sie müssen mehrere erneut zwischengespeicherte Elemente erstellen, die sich nicht zwischen Sites wie Clientlibs unterscheiden, und für jeden von Ihnen eingerichteten DocumentRoot einen Replikations-Flush-Agenten einrichten. Erhöhen Sie mit jeder Seitenaktivierung den Verwaltungsaufwand. Verlassen Sie sich auf den Namespace von Dateien und deren vollständig zwischengespeicherten Pfaden und vermeiden Sie mehrere DocumentRoot-Pfade für veröffentlichte Sites.
</div>

## Konfigurationsdateien

Der Dispatcher steuert, was im `/cache {` -Abschnitt einer beliebigen Farm-Datei. 
In den AMS-Grundlinien-Konfigurationsfarmen finden Sie unsere Includes wie unten gezeigt:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Informationen zum Erstellen der Regeln für das Zwischenspeichern finden Sie in der Dokumentation . [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Caching-Autor

Es gibt viele Implementierungen, die wir gesehen haben, in denen Benutzer keine Autoreninhalte zwischenspeichern. 
Sie vermissen ein enormes Upgrade der Leistung und Reaktionsfähigkeit ihrer Autoren.

Sprechen wir über die Strategie, die bei der Konfiguration unserer Autoren-Farm für den richtigen Cache verwendet wurde.

Hier finden Sie einen Basisautor `/cache {` Abschnitt unserer Autoren-Farm-Datei:


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

Wichtig ist hier, dass die `/docroot` auf das Cache-Verzeichnis für Autor festgelegt ist.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Stellen Sie sicher, dass `DocumentRoot` im `.vhost` Datei stimmt mit den Farmen überein `/docroot` parameter
</div>

Die Include-Anweisung der Cache-Regeln enthält die -Datei `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` , der die folgenden Regeln enthält:

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

In einem Autorenszenario ändert sich der Inhalt ständig und absichtlich. Sie möchten nur Elemente zwischenspeichern, die sich nicht häufig ändern.
Wir verfügen über Regeln zum Zwischenspeichern `/libs` weil sie Teil der grundlegenden AEM-Installation sind und sich ändern würden, bis Sie ein Service Pack, ein kumulatives Fixpack, ein Upgrade oder Hotfix installiert haben. Das Zwischenspeichern dieser Elemente ist also sehr sinnvoll und bietet wirklich große Vorteile für die Autorenerfahrung von Endbenutzern, die die Site nutzen.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Beachten Sie, dass diese Regeln auch zwischenspeichern <b>`/apps`</b> Hier lebt benutzerdefinierter Anwendungscode. Wenn Sie Ihren Code auf dieser Instanz entwickeln, wird es sich beim Speichern Ihrer Datei als sehr verwirrend erweisen und nicht sehen, ob der Code in der Benutzeroberfläche angezeigt wird, da eine zwischengespeicherte Kopie bereitgestellt wird. Wenn Sie Ihren Code in AEM implementieren, ist dies nur selten der Fall und Teil Ihrer Implementierungsschritte sollte darin bestehen, den Autoren-Cache zu leeren. Auch hier ist der Vorteil enorm, dass Ihr zwischenspeicherbarer Code für die Endbenutzer schneller ausgeführt werden kann.
</div>


## ServeOnStale (AKA-Serve auf Stale/SOS)

Dies ist einer der Stärken einer Funktion des Dispatchers. Wenn der Herausgeber unter Last ist oder nicht mehr reagiert, wird normalerweise ein HTTP-Antwortcode 502 oder 503 ausgegeben. Wenn dies eintritt und diese Funktion aktiviert ist, wird der Dispatcher angewiesen, weiterhin den Inhalt bereitzustellen, der immer noch im Cache gespeichert ist, selbst wenn es sich nicht um eine neue Kopie handelt. Es ist besser, etwas zu bedienen, wenn Sie es haben, anstatt nur eine Fehlermeldung anzuzeigen, die keine Funktionalität bietet.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Beachten Sie, dass diese Funktion nicht Trigger wird, wenn der Herausgeber-Renderer eine Socket-Zeitüberschreitung oder eine Fehlermeldung von 500 aufweist. Wenn AEM einfach nicht erreichbar ist, bewirkt diese Funktion nichts
</div>

Diese Einstellung kann in jeder Farm festgelegt werden, es ist jedoch nur sinnvoll, sie auf die Farm-Dateien im Veröffentlichungsmodus anzuwenden. Im Folgenden finden Sie ein Syntaxbeispiel der Funktion, die in einer Farm-Datei aktiviert ist:

```
/cache { 
    /serveStaleOnError "1"
```

## Zwischenspeichern von Seiten mit Abfrageparametern/Argumenten

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Eines der normalen Verhaltensweisen des Dispatcher-Moduls besteht darin, dass eine Anforderung einen Abfrageparameter im URI enthält (wird normalerweise wie folgt angezeigt: `/content/page.html?myquery=value`) wird das Zwischenspeichern der Datei übersprungen und direkt zur AEM Instanz geleitet. Sie erwägt, dass diese Anfrage eine dynamische Seite ist und nicht zwischengespeichert werden sollte. Dies kann sich negativ auf die Cache-Effizienz auswirken.
</div>
<br/>

Siehe dies [Artikel](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) zeigt an, wie wichtige Abfrageparameter die Leistung Ihrer Site beeinflussen können.

Standardmäßig möchten Sie die Variable `ignoreUrlParams` Regeln, die `*`.  Das bedeutet, dass alle Abfrageparameter ignoriert werden und alle Seiten zwischengespeichert werden können, unabhängig von den verwendeten Parametern.

Hier ist ein Beispiel, bei dem jemand einen Deep-Link-Referenzmechanismus für soziale Medien erstellt hat, der die Argumentreferenz im URI verwendet, um zu erfahren, woher die Person stammt.

<b>Ignorierbares Beispiel:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

Die Seite kann zu 100 % zwischengespeichert werden, jedoch nicht zwischengespeichert werden, da die Argumente vorhanden sind. 
Konfiguration `ignoreUrlParams` da eine Zulassungsliste dazu beiträgt, dieses Problem zu beheben:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Wenn der Dispatcher jetzt die Anfrage sieht, ignoriert er die Tatsache, dass die Anfrage über die `query` Parameter von `?` referenzieren und dennoch die Seite zwischenspeichern

<b>Dynamisches Beispiel:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Beachten Sie, dass Sie, wenn Sie Abfrageparameter haben, die eine Seitenänderung vornehmen, die gerenderte Ausgabe ausgeben, diese von Ihrer ignorierten Liste ausschließen und die Seite wieder unzwischenspeicherbar machen müssen.  Beispielsweise ändert eine Suchseite, die einen Abfrageparameter verwendet, den rohen HTML-Code, der gerendert wird.

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

Wenn Sie `/search.html?q=fruit` zuerst würde es den HTML-Code mit Ergebnissen zwischenspeichern, die Früchte zeigen.

Dann besuchen Sie `/search.html?q=vegetables` zweitens würde es die Früchte zeigen.
Dies liegt daran, dass der Abfrageparameter von `q` wird im Hinblick auf die Zwischenspeicherung ignoriert.  Um dieses Problem zu vermeiden, müssen Sie Seiten beachten, die verschiedene HTML basierend auf Abfrageparametern rendern und die Zwischenspeicherung für diese verweigern.

Beispiel:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

Seiten, die Abfrageparameter über JavaScript verwenden, funktionieren weiterhin vollständig, wobei die Parameter in dieser Einstellung ignoriert werden.  Weil sie die HTML-Datei im Ruhezustand nicht ändern.  Sie verwenden JavaScript, um die Browser in Echtzeit im lokalen Browser zu aktualisieren.  Wenn Sie also die Abfrageparameter mit JavaScript verwenden, ist es sehr wahrscheinlich, dass Sie diesen Parameter für die Seitenzwischenspeicherung ignorieren können.  Lassen Sie diese Seite zwischenspeichern und genießen Sie die Leistungssteigerung!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Diese Seiten im Auge zu behalten, erfordert zwar einen gewissen Aufwand, doch lohnt sich der Leistungsgewinn.  Sie können Ihren CSE bitten, einen Bericht über den Traffic Ihrer Websites auszuführen, um Ihnen eine Liste aller Seiten mit Abfrageparametern der letzten 90 Tage zu geben, damit Sie analysieren und sicherstellen können, dass Sie wissen, welche Seiten angezeigt werden und welche Abfrageparameter nicht ignoriert werden sollen
</div>
<br/>

## Zwischenspeichern von Antwortheadern

Es ist ziemlich offensichtlich, dass der Dispatcher zwischenspeichert `.html` Seiten und Clientlibs (d. h. `.js`, `.css`), wussten Sie jedoch, dass es auch bestimmte Antwortheader neben dem Inhalt in einer Datei mit demselben Namen, aber einer `.h` Dateierweiterung. Dadurch kann die nächste Antwort nicht nur auf den Inhalt, sondern auch auf die Antwortheader angewendet werden, die aus dem Cache gesendet werden sollen.

AEM kann mehr als nur UTF-8-Kodierung verarbeiten

Manchmal verfügen Elemente über spezielle Header, mit denen die Codierungsdetails der TTL-Cache-Datei und die zuletzt geänderten Zeitstempel gesteuert werden können.

Diese Werte werden beim Zwischenspeichern standardmäßig entfernt und der Apache httpd-Webserver übernimmt die Verarbeitung des Assets mit seinen normalen Dateibearbeitungsmethoden, die normalerweise auf MIME-Typen beschränkt sind, die auf Dateierweiterungen basieren.

Wenn der Dispatcher das Asset und die gewünschten Header zwischenspeichert, können Sie das richtige Erlebnis bereitstellen und sicherstellen, dass alle Details es dem Client-Browser bereitstellen.

Im Folgenden finden Sie ein Beispiel für eine Farm mit den Headern, die zwischengespeichert werden sollen:

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


In dem Beispiel, für das sie AEM zum Bereitstellen von Headern konfiguriert haben, prüft das CDN, wann der Cache invalidiert werden soll. Das bedeutet nun, AEM kann ordnungsgemäß bestimmen, welche Dateien basierend auf Headern invalidiert werden.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Beachten Sie, dass Sie keine regulären Ausdrücke oder glob-Übereinstimmungen verwenden können. Es handelt sich um eine wörtliche Liste der Header, die zwischengespeichert werden sollen. Fügen Sie nur eine Liste der literalen Header ein, die zwischengespeichert werden sollen.
</div>


## Übergangsphase für automatische Invalidierung

Auf AEM Systemen mit viel Aktivität von Autoren, die viele Seitenaktivierungen durchführen, kann es zu einer Race-Bedingung kommen, bei der wiederholte Invalidierungen auftreten. Erheblich wiederholte Leerungsanfragen sind nicht erforderlich und Sie können eine Toleranz aufbauen, um einen Leerlauf erst nach Ablauf der Übergangsphase zu wiederholen.

### Beispiel:

Wenn Sie 5 Anforderungen zur Invalidierung haben `/content/exampleco/en/` alle treten innerhalb eines Zeitraums von 3 Sekunden auf.

Mit dieser Funktion würden Sie das Cache-Verzeichnis invalidieren `/content/exampleco/en/` 5-mal

Wenn diese Funktion aktiviert ist und auf 5 Sekunden festgelegt ist, wird das Cache-Verzeichnis invalidiert `/content/exampleco/en/` <b>once</b>

Hier finden Sie eine Beispielsyntax dieser Funktion, die für eine Übergangsphase von 5 Sekunden konfiguriert wird:

```
/cache { 
    /gracePeriod "5"
```

## TTL-basierte Invalidierung

Eine neuere Funktion des Dispatcher-Moduls war `Time To Live (TTL)` Optionen für die Invalidierung auf Basis der im Cache gespeicherten Elemente. Wenn ein Element zwischengespeichert wird, sucht es nach Cache-Steuerelement-Headern und generiert eine Datei im Cache-Verzeichnis mit demselben Namen und einer `.ttl` -Erweiterung.

Im Folgenden finden Sie ein Beispiel der Funktion, die in der Farm-Konfigurationsdatei konfiguriert wird:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Beachten Sie, dass AEM weiterhin so konfiguriert werden muss, dass TTL-Header gesendet werden, damit der Dispatcher sie berücksichtigt. Durch das Umschalten dieser Funktion kann der Dispatcher nur wissen, wann die Dateien entfernt werden sollen, für die AEM Cache-Steuerelement-Header gesendet hat. Wenn AEM nicht beginnt, TTL-Header zu senden, führt der Dispatcher hier keine besonderen Aktionen durch.
</div>

## Cache-Filterregeln

Im Folgenden finden Sie ein Beispiel für eine Basiskonfiguration, für die Elemente in einem Publisher zwischengespeichert werden sollen:

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

Wir möchten unsere veröffentlichte Site so gierig wie möglich machen und alles zwischenspeichern.

Wenn es Elemente gibt, die das Erlebnis beim Zwischenspeichern beschädigen, können Sie Regeln hinzufügen, um die Option zum Zwischenspeichern dieses Elements zu entfernen. Wie Sie im obigen Beispiel sehen, sollten die CSRF-Token nie zwischengespeichert und ausgeschlossen werden. Weitere Informationen zum Schreiben dieser Regeln finden Sie unter [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Weiter -> Verwenden und Verstehen von Variablen](./variables.md)