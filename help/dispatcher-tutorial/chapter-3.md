---
title: Kapitel 3 - Themen zum erweiterten Dispatcher-Caching
description: Dies ist Teil 3 einer dreiteiligen Reihe zum Thema Caching in AEM. Die ersten beiden Teile konzentrierten sich dabei auf das einfache HTTP-Caching im Dispatcher und auf bestehende Einschränkungen. In diesem Teil werden einige Ideen zur Überwindung dieser Einschränkungen erläutert.
feature: Dispatcher
topic: Architecture
role: Architect
level: Intermediate
doc-type: Tutorial
exl-id: 7c7df08d-02a7-4548-96c0-98e27bcbc49b
duration: 1653
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '6172'
ht-degree: 99%

---

# Kapitel 3 – Fortgeschrittene Caching-Themen

*„In der Informatik sind nur zwei Sachen wirklich schwierig: die Cache-Invalidierung und die Benennung von Dingen.“*

– PHIL KARLTON

## Übersicht

Dies ist Teil 3 einer dreiteiligen Reihe zum Thema Caching in AEM. Die ersten beiden Teile konzentrierten sich dabei auf das einfache HTTP-Caching im Dispatcher und auf bestehende Einschränkungen. In diesem Teil werden einige Ideen zur Überwindung dieser Einschränkungen erläutert.

## Caching im Allgemeinen

[Kapitel 1](chapter-1.md) und [Kapitel 2](chapter-2.md) dieser Reihe konzentrierten sich hauptsächlich auf den Dispatcher. Wir haben die Grundlagen, die Einschränkungen und die Voraussetzungen für bestimmte Kompromisse erläutert.

Die Komplexität und Kompliziertheit des Cachings betreffen nicht nur den Dispatcher. Caching ist allgemein schwierig.

Auf den Dispatcher als einziges Tool in Ihrer Toolbox zu vertrauen, würde tatsächlich eine echte Einschränkung bedeuten.

In diesem Kapitel möchten wir unsere Sicht auf das Caching weiter ausdehnen und Ideen entwickeln, wie sich bestimmte Defizite des Dispatchers überwinden lassen. Es gibt keine Wunderwaffe – Sie müssen bei Ihrem Projekt zu Kompromissen bereit sein. Denken Sie daran, dass durch Genauigkeit bei Caching und Invalidierung immer Komplexität entsteht und durch Komplexität die Möglichkeit von Fehlern.

Sie müssen in den folgenden Bereichen Kompromisse eingehen:

* Leistung und Latenz
* Ressourcenverbrauch/CPU-Auslastung/Festplattenauslastung
* Genauigkeit/Zeitnähe/Veraltung/Sicherheit
* Einfachheit/Komplexität/Kosten/Wartbarkeit/Fehleranfälligkeit

Diese Dimensionen sind in einem ziemlich komplexen System miteinander verknüpft. Es gibt kein einfaches „wenn dies, dann das“. Eine einfachere Gestaltung des Systems kann es schneller oder langsamer machen. Dies kann Ihre Entwicklungskosten senken, aber die Hepdesk-Kosten erhöhen, z. B. wenn Kundinnen und Kunden veraltete Inhalte sehen oder sich über eine langsame Website beschweren. All diese Faktoren müssen berücksichtigt und gegeneinander abgewogen werden. Aber zum jetzigen Zeitpunkt sollte Ihnen schon relativ klar sein, dass es weder eine Wunderwaffe noch eine einzige Best Practice gibt. Nur jede Menge schlechter Praktiken – und ein paar gute.

## Caching in Kette

### Übersicht

#### Datenfluss

Bei der Bereitstellung einer Seite von einem Server auf einen Client-Browser ist eine Vielzahl von Systemen und Subsystemen involviert. Wer genau hinsieht, erkennt, dass die Daten von der Quelle bis zum Ziel verschiedene Stationen durchlaufen, von denen jede ein potenzieller Caching-Kandidat ist.

![Datenfluss einer typischen CMS-Anwendung](assets/chapter-3/data-flow-typical-cms-app.png)

*Datenfluss einer typischen CMS-Anwendung*

<br> 

Starten wir unsere Reise mit einem Datenelement, das auf einer Festplatte gespeichert ist und in einem Browser angezeigt werden muss.

#### Hardware und Betriebssystem

Zunächst verfügt die Festplatte (Hard Disk Drive, HDD) selbst über einen in der Hardware eingebauten Cache. Zweitens verwendet das Betriebssystem, das die Festplatte bereitstellt, freien Speicher, um häufig aufgerufene Blöcke zwischenzuspeichern und so den Zugriff zu beschleunigen.

#### Content-Repository

Auf der nächsten Ebene folgt CRX oder Oak – die von AEM verwendete Dokumentdatenbank. CRX und Oak teilen die Daten in Segmente, die im Speicher zwischengespeichert werden können, sodass ein langsamerer Zugriff auf die Festplatte vermieden wird.

#### Drittanbieterdaten

Die meisten größeren Web-Installationen verfügen auch über Drittanbieterdaten: Daten aus einem Produktinformationssystem, einem Kundenbeziehungs-Management-System, einer älteren Datenbank oder einem beliebigen anderen Web-Dienst. Diese Daten müssen, wenn sie benötigt werden, nicht jedes Mal aus der Quelle abgerufen werden – vor allem dann nicht, wenn klar ist, dass sie sich nicht allzu häufig ändern. Sie können also zwischengespeichert werden, wenn sie nicht mit der CRX-Datenbank synchronisiert werden.

#### Geschäftsebene – App/Modell

Normalerweise rendern Ihre Vorlagenskripte den aus CRX stammenden Rohinhalt nicht über die JCR-API. Wahrscheinlich gibt es eine Geschäftsebene dazwischen, die Daten in einem Business-Domain-Objekt zusammenführt, berechnet und/oder transformiert. RIchtig – wenn diese Vorgänge kostspielig sind, sollten Sie sie zwischenspeichern.

#### Markup-Fragmente

Das Modell ist jetzt die Grundlage für das Rendering des Markups für eine Komponente. Warum nicht auch das gerenderte Modell zwischenspeichern?

#### Dispatcher, CDN und andere Proxys

Die gerenderte HTML-Seite wird an den Dispatcher weitergeleitet. Wir haben bereits besprochen, dass der Hauptzweck des Dispatchers (trotz seines Namens) darin besteht, HTML-Seiten und andere Web-Ressourcen zwischenzuspeichern. Bevor die Ressourcen den Browser erreichen, können sie einen Reverse-Proxy, der zwischenspeichern kann, und ein CDN, das auch zum Caching verwendet wird, durchlaufen. Der Client kann sich in einem Büro befinden, das nur über einen Proxy Web-Zugriff gewährt. Und der Proxy entscheidet sich möglicherweise zum Caching und Speichern von Traffic.

#### Browsercache

Nicht zuletzt speichert auch der Browser zwischen. Dies wird häufig übersehen, obwohl es sich um den nächstgelegenen und schnellsten Cache in der Caching-Kette handelt. Leider wird er zwar nicht zwischen Benutzenden, aber immerhin zwischen verschiedenen Anfragen einer Benutzerin oder eines Benutzers geteilt.

### Wo und wann zwischenspeichern

Hierbei geht es um eine lange Kette potenzieller Caches. Und wir alle haben es schon erlebt, Probleme durch veraltete Inhalte. Aber angesichts der großen Anzahl an Phasen ist es ein Wunder, dass das Ganze meistens überhaupt funktioniert.

Aber wo in dieser Kette ist es überhaupt sinnvoll, zwischenzuspeichern? Am Anfang? Am Ende? Überall? Es kommt darauf an … und hängt von einer Vielzahl von Faktoren ab. Schon bei zwei Ressourcen auf derselben Website könnte es jeweils eine andere gewünschte Antwort auf diese Frage geben.

Um Ihnen eine grobe Vorstellung davon zu geben, welche Faktoren Sie in Betracht ziehen könnten …

**Gültigkeitsdauer (Time to Live, TTL)**: Wenn Objekte eine kurze inhärente Gültigkeitsdauer haben (bei Traffic-Daten ggf. kürzer als bei Wetterdaten), ist Caching möglicherweise nicht sinnvoll.

**Produktionskosten**: Wie teuer (in Bezug auf CPU-Zyklen und I/O) sind die Reproduktion und Bereitstellung eines Objekts? Sind diese Vorgänge kostengünstig, kann auf das Caching ggf. verzichtet werden.

**Größe**: Große Objekte erfordern, dass mehr Ressourcen zwischengespeichert werden. Dies könnte ein begrenzender Faktor sein und muss gegen den Nutzen abgewogen werden.

**Zugriffshäufigkeit**: Wenn selten auf Objekte zugegriffen wird, ist das Caching möglicherweise nicht effektiv. Sie würden bloß alt oder invalidiert werden, bevor sie zum zweiten Mal aus dem Cache aufgerufen werden. Solche Elemente würden einfach Speicherressourcen blockieren.

**Gemeinsamer Zugriff**: Daten, die von mehr als einer Entität verwendet werden, sollten weiter oben in der Kette zwischengespeichert werden. Tatsächlich ist die Caching-Kette keine Kette, sondern eine Baumstruktur. Ein Datenelement im Repository kann von mehr als einem Modell verwendet werden. Diese Modelle können wiederum von mehr als einem Render-Skript verwendet werden, um HTML-Fragmente zu generieren. Diese Fragmente sind auf mehreren Seiten enthalten, die an mehrere Benutzende mit ihren privaten Caches im Browser verteilt werden. „Gemeinsam“ bezieht sich hier also nicht nur auf die gemeinsame Verwendung zwischen Personen, sondern auch zwischen Software-Teilen. Wenn Sie einen potenziellen „freigegebenen“ Cache finden möchten, verfolgen Sie einfach die Baumstruktur zum Stammverzeichnis zurück und suchen Sie nach einem gemeinsamen Vorgänger. Dort sollten Sie zwischenspeichern.

**Räumliche Verteilung**: Wenn Sie Benutzende auf der ganzen Welt haben, kann die Verwendung eines verteilten Cache-Netzwerks dazu beitragen, die Latenz zu verringern.

**Netzwerkbandbreite und -latenz**: Apropos Latenz – wer sind Ihre Kundinnen und Kunden und welche Netzwerktypen nutzen diese? Vielleicht sind Ihre Kundinnen und Kunden aus einem weniger entwickelten Land und nutzen Mobilgeräte und eine 3G-Verbindung mit Smartphones der älteren Generation. Denken Sie dann darüber nach, kleinere Objekte zu erstellen und sie in den Browser-Caches zwischenzuspeichern.

Diese Aufstellung ist bei weitem nicht vollständig, aber Sie sollten inzwischen eine gute Vorstellung davon haben, worum es geht.

### Grundlegende Regeln für das Caching in Kette

Wie schon erwähnt, Caching ist schwierig. Im Folgenden finden Sie einige Grundregeln, die wir aus früheren Projekten abgeleitet haben und mit denen Sie Probleme in Ihrem Projekt vermeiden können.

#### Vermeiden von doppeltem Caching

Jede im letzten Kapitel eingeführte Ebene liefert einen gewissen Wert in der Caching-Kette. Entweder durch Einsparung von Rechenzyklen oder durch Annäherung der Daten an die Verbrauchenden. Es ist nicht falsch, ein Datenelement in mehreren Phasen der Kette zwischenzuspeichern. Aber Sie sollten immer Vorteile und Belastungen der nächsten Phase berücksichtigen. Das Caching einer vollständigen Seite im Veröffentlichungssystem bietet in der Regel keinen Vorteil, weil dies bereits im Dispatcher erfolgt ist.

#### Mischen von Invalidierungsstrategien

Es gibt drei grundlegende Invalidierungsstrategien:

* **TTL:** Ein Objekt läuft nach einer festen Zeitdauer ab (z. B. „2 Stunden ab jetzt“).
* **Ablaufdatum:** Ein Objekt läuft zu einem bestimmten Zeitpunkt in der Zukunft ab (z. B. „10:00 Uhr am 10. Juni 2019“).
* **Ereignisbasiert:** Ein Objekt wird explizit durch ein Ereignis invalidiert, das auf der Plattform aufgetreten ist (z. B. bei Änderung oder Aktivierung einer Seite).

Nun können Sie verschiedene Strategien auf verschiedenen Cache-Ebenen verwenden. Einige davon sind allerdings „toxisch“.

#### Ereignisbasierte Invalidierung

![Rein ereignisbasierte Invalidierung](assets/chapter-3/event-based-invalidation.png)

*Rein ereignisbasierte Invalidierung: Invalidierung vom inneren Cache zur äußeren Ebene*

<br> 

Eine rein ereignisbasierte Invalidierung ist am einfachsten zu verstehen und in der Theorie umzusetzen und bietet die höchste Genauigkeit.

Einfach gesagt, werden die Caches nacheinander invalidiert, nachdem sich ein Objekt geändert hat.

Sie müssen nur an eine Regel denken:

Immer von inneren zum äußeren Cache invalidieren. Wenn Sie zuerst einen äußeren Cache invalidiert haben, wird möglicherweise veralteter Inhalt von einem inneren Cache erneut zwischengespeichert. Gehen Sie nicht davon aus, wann ein Cache wieder sauber ist – Sie müssen es wissen. Am besten durch Auslösen der Invalidierung des äußeren Caches _nach_ der Invalidierung des inneren Caches.

Soweit in der Theorie. Aber in der Praxis gibt es eine Reihe von Fallstricken. Die Ereignisse müssen verteilt werden – potenziell über ein Netzwerk. In der Praxis ist dies das schwierigste Vorhaben bei der Invalidierung.

#### Automatische Reparatur

Im Falle einer ereignisbasierten Invalidierung sollten Sie über einen Notfallplan verfügen. Was geschieht, wenn ein Invalidierungsereignis verpasst wird? Eine einfache Strategie könnte darin bestehen, eine Invalidierung oder Bereinigung nach einer bestimmten Zeit durchzuführen. Also angenommen, Sie haben das Ereignis verpasst und es wird nun veralteter Inhalt bereitgestellt. Ihre Objekte haben jedoch auch eine implizite TTL von lediglich mehreren Stunden (Tagen). Schließlich führt das System also eine automatische Selbstreparatur durch.

#### Rein TTL-basierte Invalidierung

![Nicht synchronisierte TTL-basierte Invalidierung](assets/chapter-3/ttl-based-invalidation.png)

*Nicht synchronisierte TTL-basierte Invalidierung*

<br> 

Dies ist ebenfalls ein ziemlich gängiges System. Sie stapeln mehrere Cache-Ebenen übereinander. Jede dieser Ebene dient einem Objekt dann für eine bestimmte Zeit.

Dies lässt sich leicht implementieren. Leider ist es aber schwierig, die effektive Lebensdauer eines Datenelements vorherzusagen.

![Äußerer Cache zur Verlängerung der Lebensdauer eines inneren Objekts](assets/chapter-3/outer-cache.png)

*Äußerer Cache zur Verlängerung der Lebensdauer eines inneren Objekts*

<br> 

Sehen Sie sich die obige Abbildung an. Jede Caching-Ebene führt eine TTL von 2 Minuten ein. Die TTL müsste dann insgesamt auch 2 Minuten betragen, oder? Nicht ganz. Wenn die äußere Ebene das Objekt unmittelbar vor dem Ablauf abruft, verlängert die äußere Ebene die effektive Gültigkeitsdauer des Objekts. Die effektive Gültigkeitsdauer kann in diesem Fall zwischen 2 und 4 Minuten betragen. Angenommen, Sie haben mit Ihrer Geschäftsabteilung vereinbart, dass ein Tag tolerierbar ist, und Sie haben vier Cache-Ebenen. Die tatsächliche TTL jeder Ebene darf in diesem Fall nicht länger als sechs Stunden sein, wodurch sich die Cache-Fehlerrate erhöht.

Das heißt nicht, dass es ein schlechtes System ist. Sie sollten nur seine Grenzen kennen. Es ist eine sehr einfache Strategie, mit der man beginnen kann. Aber wenn der Traffic Ihrer Site zunimmt, sollten Sie eine präzisere Strategie in Betracht ziehen.

*Synchronisieren der Invalidierungszeit durch Festlegen eines bestimmten Datums*

#### Auf dem Ablaufdatum basierende Invalidierung

Die effektive Lebensdauer lässt sich besser vorhersehen, wenn Sie ein bestimmtes Datum für das innere Objekt festlegen und es an die äußeren Caches weitergeben.

![Synchronisieren von Ablaufdaten](assets/chapter-3/synchronize-expiration-dates.png)

*Synchronisieren von Ablaufdaten*

<br> 

Allerdings können nicht alle Caches die Datumsangaben weitergeben. Und das kann schwierig werden, wenn der äußere Cache zwei innere Objekte mit unterschiedlichen Ablaufdaten aggregiert.

#### Mischen von ereignis- und TTL-basierter Invalidierung

![Mischen von ereignis- und TTL-basierten Strategien](assets/chapter-3/mixing-event-ttl-strategies.png)

*Mischen von ereignis- und TTL-basierten Strategien*

<br> 

Ebenfalls geläufig im AEM-Kontext ist die Verwendung der ereignisbasierten Invalidierung bei inneren Caches (z. B. In-Memory-Caches, in denen Ereignisse nahezu in Echtzeit verarbeitet werden können) und TTL-basierte Caches auf äußerer Ebene – möglicherweise ohne Zugang zu einer expliziten Invalidierung.

In AEM hätten Sie dann einen In-Memory-Cache für Geschäftsobjekte und HTML-Fragmente in den Veröffentlichungssystemen, der invalidiert wird, wenn sich die zugrunde liegenden Ressourcen ändern, und Sie geben dieses Änderungsereignis an den Dispatcher weiter, der ebenfalls ereignisbasiert funktioniert. Vorgeschaltet könnte hier beispielsweise ein TTL-basiertes CDN sein.

Eine Ebene mit Caching auf Basis einer kurzen TTL vor einem Dispatcher könnte eine Spitze, die normalerweise nach einer automatischen Invalidierung auftreten würde, effektiv mildern.

#### Mischen von TTL- und ereignisbasierter Invalidierung

![Mischen von TTL- und ereignisbasierter Invalidierung](assets/chapter-3/toxic.png)

*Toxisch: Vermischen von TTL- und ereignisbasierter Invalidierung*

<br> 

Diese Kombination ist toxisch. Platzieren Sie einen ereignisbasierten Cache nie nach einem TTL- oder ablaufbasierten Cache. Erinnern Sie sich noch an den Übertragungseffekt der reinen TTL-Strategie? Dieselbe Wirkung kann auch hier beobachtet werden. Nur kann es möglicherweise nicht erneut passieren, dass das Invalidierungsereignis des äußeren Caches bereits eingetreten ist, – vielleicht niemals. Dadurch kann die Lebensdauer des zwischengespeicherten Objekts unendlich werden.

![TTL- und ereignisbasierte Kombination: Übertragung bis zur Unendlichkeit](assets/chapter-3/infinity.png)

*TTL- und ereignisbasierte Kombination: Übertragung bis zur Unendlichkeit*

<br> 

## Partielles Caching und In-Memory-Caching

Sie können in die Phase des Render-Vorgangs eingreifen, um Caching-Ebenen hinzuzufügen. Angefangen beim Abrufen von Remote-Datenübertragungsobjekten über das Erstellen lokaler Geschäftsobjekte bis hin zum Caching des gerenderten Markups einer einzelnen Komponente. Auf konkrete Implementierungsmöglichkeiten kommen wir in einem späteren Tutorial zurück. Aber vielleicht haben Sie die Implementierung einige dieser Caching-Ebenen bereits fest eingeplant. Das Mindeste, was wir daher an dieser Stelle tun können, ist auf die Grundprinzipien und Fallstricke hinzuweisen.

### Einige Worte zur Warnung

#### Zugriffssteuerung respektieren

Die hier beschriebenen Techniken sind überaus leistungsstark und gehören _unbedingt_ in die Toolbox jeder AEM-Entwicklungsperson. Aber lassen Sie sich von Ihrer Begeisterung nicht zu sehr mitreißen, sondern setzen Sie sie mit Bedacht ein. Wenn ein Objekt in einem Cache gespeichert und in nachfolgenden Anfragen für andere Benutzende freigegeben wird, bedeutet dies tatsächlich, die Zugriffssteuerung zu umgehen. Bei öffentlich zugänglichen Websites ist dies in der Regel kein Problem, kann aber zu einem werden, wenn sich eine Benutzerin oder ein Benutzer anmelden muss, um Zugriff zu erhalten.

Angenommen, Sie speichern das HTML-Markup eines Sites-Hauptmenüs in einem In-Memory-Cache, um es auf verschiedenen Seiten zu verwenden. Dies ist ein ideales Beispiel für die Speicherung von teilweise gerendertem HTML, da die Navigationserstellung aufgrund der Vielzahl der zu durchlaufenden Seiten normalerweise kostspielig ist.

Eine identische Menüstruktur wird nicht nur auf allen Seiten eingesetzt, sondern auch für alle Benutzenden, was für noch mehr Effizienz sorgt. Vielleicht gibt es aber auch einige Elemente im Menü, die ausschließlich für eine bestimmte Gruppe von Benutzenden reserviert sind. In diesem Fall kann das Caching etwas komplexer werden.

#### Nur benutzerdefinierte Geschäftsobjekte zwischenspeichern

Wenn überhaupt, dann ist das der wichtigste Ratschlag, den wir Ihnen geben können:

>[!WARNING]
>
>Speichern Sie nur Objekte zwischen, die Ihnen gehören, die unveränderlich sind, die Sie selbst erstellt haben, die flach sind und die keine ausgehenden Verweise haben.

Was bedeutet das?

1. Sie kennen nicht den vorgesehenen Lebenszyklus von Objekten anderer Personen. Angenommen, Sie stoßen auf einen Verweis zu einem Anfrageobjekt und beschließen, es zwischenzuspeichern. Nach Ende der Anfrage möchte der Servlet-Container dieses Objekt für die nächste eingehende Anfrage wiederverwenden. In diesem Fall ändert eine andere Person den Inhalt, von dem Sie dachten, dass Sie die exklusive Kontrolle darüber haben. Wischen Sie dies nicht einfach beiseite. So etwas ist tatsächlich schon in Projekten vorgekommen. Kundinnen und Kunden bekamen anstelle der eigenen Kundendaten die von anderen zu sehen.

2. Solange ein Objekt durch eine Kette anderer Verweise referenziert wird, kann es nicht aus dem Heap entfernt werden. Wenn Sie ein vermeintlich kleines Objekt in Ihrem Cache behalten, das beispielsweise auf eine 4 MB große Darstellung eines Bildes verweist, sind Probleme durch Arbeitsspeicherverlust durchaus wahrscheinlich. Caches sollen normalerweise auf schwachen Verweisen basieren. Aber schwache Verweise funktionieren nicht so, wie man es erwarten würde. Es gibt keine bessere Methode zum Erzeugen von Arbeitsspeicherverlust und eines Fehlers vom Typ „Nicht genügend Arbeitsspeicher“. Und Sie wissen nun einmal nicht, wie groß der zurückbehaltene Speicher der fremden Objekte ist.

3. Insbesondere in Sling können Sie (fast) alle Objekte einander anpassen. Angenommen, Sie speichern eine Ressource im Cache. In der nächsten Anfrage (mit anderen Zugriffsrechten) wird diese Ressource abgerufen und zu einem ResourceResolver oder einer Sitzung angepasst, um auf andere Ressourcen zuzugreifen, auf die sie aber nicht zugreifen kann.

4. Selbst wenn Sie einen dünnen „Wrapper“ um eine Ressource aus AEM erstellen, dürfen Sie diese nicht zwischenspeichern – auch dann nicht, wenn es sich um eine eigene und unveränderliche Ressource handelt. Das umschlossene Objekt wäre ein Verweis (was wir zuvor verboten haben), und wenn wir genau hinsehen, verursacht dies im Grunde dieselben Probleme wie im letzten Punkt beschrieben.

5. Erstellen Sie zum Caching Ihre eigenen Projekte, indem Sie primitive Daten in Ihre eigenen flachen Objekte kopieren. Vielleicht möchten Sie eine Verknüpfung zwischen Ihren eigenen Objekten mithilfe von Verweisen herstellen, z. B. eine Baumstruktur von Objekten zwischenspeichern. Das ist in Ordnung. Aber speichern Sie nur Objekte zwischen, die Sie gerade in derselben Anfrage erstellt haben, und keine Objekte, die von anderer Stelle angefordert wurden (auch wenn es sich um den Namespace „Ihres“ Objekts handelt). Das _Kopieren von Objekten_ ist der Schlüssel. Achten Sie darauf, die gesamte Struktur der verknüpften Objekte gleichzeitig zu bereinigen und eingehende und ausgehende Verweise auf Ihre Struktur zu vermeiden.

6. Lassen Sie außerdem Ihre Objekte unveränderlich. Nur private Eigenschaften, keine Setter.

Das sind viele Regeln, aber es lohnt sich, diese zu befolgen. Selbst wenn Sie erfahren und sehr intelligent sind und alles unter Kontrolle haben. Der junge Kollege, der an Ihrem Projekt mitarbeitet, hat gerade seinen Universitätsabschluss gemacht. Er kennt nicht all diese Fallstricke. Gäbe es keine Fallstricke, dann gäbe es nichts zu vermeiden. Halten Sie es einfach und verständlich.

### Tools und Bibliotheken

In dieser Serie geht es darum, Konzepte zu verstehen und Ihnen die Möglichkeit zu geben, eine Architektur aufzubauen, die am besten zu Ihrem Anwendungsfall passt.

Wir empfehlen kein bestimmtes Tool. Aber wir geben Ihnen Hinweise, wie Sie die Tools bewerten können. Beispielsweise verfügt AEM seit Version 6.0 über einen einfachen integrierten Cache mit einer festen TTL. Sollten Sie ihn verwenden? Wahrscheinlich nicht beim Veröffentlichen, wenn ein ereignisbasierter Cache in der Kette folgt (Stichwort: Dispatcher). Aber es könnte eine gute Wahl für Autorinnen und Autoren sein. Es gibt auch einen Adobe ACS Commons-HTTP-Cache, der interessant sein könnte.

Oder Sie erstellen einen eigenen, der auf einem ausgereiften Caching-Framework wie [Ehcache](https://www.ehcache.org) aufbaut. Dies kann zum Zwischenspeichern von Java-Objekten und gerendertem Markup (`String`-Objekte) genutzt werden.

In einfachen Fällen können Sie auch gleichzeitige Hash-Karten verwenden, wobei Sie jedoch bald an Grenzen stoßen werden, entweder bezüglich des Tools oder Ihrer Fertigkeiten. Diese Gleichzeitigkeit ist so schwer zu meistern wie Benennung und Caching.

#### Verweise

* [ACS Commons-HTTP-Cache](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Ehcache-Caching-Framework](https://www.ehcache.org)

### Grundbegriffe

Wir werden hier nicht zu tief in die Caching-Theorie einsteigen, aber wir möchten Ihnen ein paar Schlagwörter nennen, um Ihnen den Einstieg zu erleichtern.

#### Cache-Entfernung

Invalidierung und Bereinigung haben wir schon ausführlich behandelt. _Cache-Entfernung_ hängt mit diesen Begriffen zusammen: Nach einem Eintrag wird er entfernt und ist nicht mehr verfügbar. Die Entfernung erfolgt aber nicht, wenn ein Eintrag veraltet ist, sondern wenn der Cache voll ist. Durch neuere oder „wichtigere“ Elemente werden ältere oder weniger wichtige Elemente aus dem Cache entfernt. Welche Einträge Sie opfern müssen, ist eine Einzelfallentscheidung. Vielleicht entfernen Sie die ältesten oder diejenigen, die sehr selten verwendet oder lange Zeit nicht mehr aufgerufen wurden.

#### Präventives Caching

Präventives Caching bedeutet, dass der Eintrag mit neuem Inhalt erstellt wird, sobald er ungültig gemacht oder als veraltet angesehen wird. Dies sollten Sie natürlich nur mit wenigen Ressourcen tun, die häufig und sofort aufgerufen werden. Andernfalls würden Sie beim Erstellen von Cache-Einträgen Ressourcen verschwenden, die möglicherweise nie angefordert werden. Wenn Sie Cache-Einträge vorab erstellen, können Sie die Latenz der ersten Anfrage auf eine Ressource nach der Cache-Invalidierung reduzieren.

#### Cache-Wärmung

Die Cache-Wärmung ähnelt dem präventiven Caching. Dieser Begriff wird allerdings nicht für ein Live-System verwendet. Dies ist weniger zeitgebunden als das erstgenannte Prinzip. Das Caching erfolgt nicht sofort nach der Invalidierung, sondern der Cache wird nach und nach gefüllt, so wie es die Zeit erlaubt.

Sie entfernen beispielsweise den Publish-/Dispatcher-Abschnitt aus dem Lastenausgleich, um ihn zu aktualisieren. Bevor er wieder integriert wird, lassen Sie automatisch nach den am häufigsten aufgerufenen Seiten suchen, damit sie wieder in den Cache geladen werden. Wenn der Cache „warm“, also angemessen gefüllt ist, integrieren Sie den Abschnitt wieder in den Lastenausgleich.

Oder Sie integrieren Sie den Abschnitt sofort wieder, drosseln aber den Traffic zu diesem Abschnitt, damit die Zwischenspeicher durch regelmäßige Nutzung „erwärmt“ werden können.

Oder Sie möchten evtl. auch einige weniger häufig aufgerufene Seiten zwischenspeichern, wenn Ihr System inaktiv ist, um die Latenz zu verringern, wenn sie tatsächlich von echten Anfragen aufgerufen werden.

#### Identität des Cache-Objekts, Payload, Invalidierungs-Abhängigkeit und TTL

Im Allgemeinen hat ein zwischengespeichertes Objekt oder ein „Eintrag“ fünf Haupteigenschaften:

#### Schlüssel

Dieser ist die Identität oder die Eigenschaft, durch die ein Objekt identifiziert wird. Entweder, um die Payload abzurufen oder um sie aus dem Cache zu löschen. Der Dispatcher verwendet beispielsweise die URL einer Seite als Schlüssel. Beachten Sie, dass der Dispatcher die Seitenpfade nicht verwendet. Dies würde nicht ausreichen, um verschiedene Renderings voneinander zu unterscheiden. Andere Caches können unterschiedliche Schlüssel verwenden. Später folgen einige Beispiele.

#### Wert/Payload

Das ist die „Schatztruhe“ des Objekts, die Daten, die Sie abrufen möchten. Im Falle des Dispatchers sind es die Dateiinhalte. Es kann sich aber auch um eine Java-Objektstruktur handeln.

#### TTL

Wir haben die TTL bereits behandelt. Dies ist die Zeit, nach der ein Eintrag als veraltet betrachtet wird und nicht mehr bereitgestellt werden sollte.

#### Abhängigkeit

Dies bezieht sich auf die ereignisbasierte Invalidierung. Von welchen Originaldaten hängt dieses Objekt ab? In Teil I haben wir bereits erwähnt, dass ein echtes und genaues Tracking von Abhängigkeiten zu komplex ist. Aber mit unserem Wissen über das System können Sie die Abhängigkeiten mit einem einfacheren Modell abschätzen. Wir invalidieren genügend Objekte, um veraltete Inhalte zu bereinigen … und versehentlich vielleicht sogar mehr als erforderlich. Trotzdem bemühen wir uns, nicht einfach alles zu bereinigen.

Welche Objekte von anderen Objekten abhängig sind, ist in jeder einzelnen Anwendung anders. Wir werden Ihnen einige Beispiele dafür geben, wie Sie später eine Abhängigkeitsstrategie implementieren können.

### Caching von HTML-Fragmenten

![Wiederverwenden eines gerenderten Fragments auf verschiedenen Seiten](assets/chapter-3/re-using-rendered-fragment.png)

*Wiederverwenden eines gerenderten Fragments auf verschiedenen Seiten*

<br> 

Das Caching von HTML-Fragmenten ist ein mächtiges Instrument. Dabei soll das HTML-Markup, das von einer Komponente generiert wurde, in einem In-Memory-Cache gespeichert werden. Aber warum sollten Sie das tun, wenn Sie das Markup der gesamten Seite, einschließlich des Markups dieser Komponente, sowieso im Dispatcher zwischenspeichern? Ja, das tun Sie – aber einmal pro Seite. Sie verwenden dieses Markup nicht auf mehreren Seiten.

Angenommen, Sie rendern eine Navigationskomponente oben auf jeder Seite. Das Markup sieht auf jeder Seite gleich aus. Sie rendern es jedoch immer wieder für jede Seite, also nicht im Dispatcher. Denken Sie daran: Nach der automatischen Invalidierung müssen alle Seiten erneut gerendert werden. Im Grunde führen Sie also denselben Code mit denselben Ergebnissen ggf. hundertfach aus.

Nach unserer Erfahrung ist es sehr kostspielig, einen oben anzuzeigenden verschachtelten Navigationsbereich zu rendern. Normalerweise wird ein großer Teil der Dokumentstruktur durchlaufen, um die Navigationselemente zu generieren. Auch wenn Sie nur den Navigationstitel und die URL benötigen, müssen die Seiten in den Speicher geladen werden. Und hier blockieren sie wertvolle Ressourcen. Immer wieder.

Die Komponente wird jedoch auf vielen Seiten verwendet. Wenn etwas solcherart gemeinsam genutzt wird, sollte evtl. ein Cache genutzt werden. Sie sollten daher überprüfen, ob die Navigationskomponente bereits gerendert und zwischengespeichert wurde und anstelle eines erneuten Render-Vorgangs nur den Cache-Wert ausgeben.

Diese beiden wunderbaren Vorteile werden hierbei gerne übersehen:

1. Sie speichern eine Java-Zeichenfolge zwischen. Eine Zeichenfolge hat keine ausgehenden Verweise und ist unveränderlich. Angesichts der oben erwähnten Warnungen ist dies also sehr sicher.

2. Die Invalidierung ist ebenfalls sehr einfach. Wenn sich Ihre Website ändert, soll dieser Cache-Eintrag invalidiert werden. Eine Neuerstellung ist relativ günstig, da sie nur einmal durchgeführt werden muss und dann von allen (Hunderten von) Seiten verwendet wird.

Dies bedeutet eine erhebliche Entlastung Ihrer Veröffentlichungs-Server.

### Implementierung von Fragment-Caches

#### Benutzerdefinierte Tags

Früher, als JSP als Vorlagen-Engine verwendet wurde, wurde der Komponenten-Rendering-Code häufig in benutzerdefinierten JSP-Tags eingeschlossen.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

Das benutzerdefinierte Tag erfasst dann den Hauptteil und schreibt ihn in den Cache bzw. verhindert die Ausführung des Hauptteils und gibt stattdessen die Payload des Cache-Eintrags aus.

„key“ gibt dabei den Komponentenpfad an, der auf der Homepage verwendet wird. Wir verwenden den Komponentenpfad nicht auf der aktuellen Seite, da hierdurch ein Cache-Eintrag pro Seite erstellt wird und dies unserer Absicht widerspricht, diese Komponente gemeinsam zu nutzen. Wir verwenden auch nicht nur den relativen Pfad der Komponenten (`jcr:conten/mainnavigation`), da dies verhindert, dass wir verschiedene Navigationskomponenten auf verschiedenen Sites verwenden.

„cache“ gibt an, wo der Eintrag gespeichert werden soll. Normalerweise verfügen Sie über mehr als einen Cache zum Speichern von Elementen. Jeder von ihnen kann sich ein wenig anders verhalten. Es ist also gut, zu unterscheiden, was gespeichert wird – auch wenn es letztlich nur um Zeichenfolgen geht.

„dependency“ benennt die Abhängigkeit des Cache-Eintrags. Der Cache „main-navigation“ kann mit einer Regel verbunden sein, wonach bei Änderungen unter dem Knoten „dependency“ der entsprechende Eintrag bereinigt werden muss. Daher ist es erforderlich, dass sich Ihre Cache-Implementierung selbst als Ereignis-Listener im Repository registriert, um die Änderungen zu erkennen, und dann die Cache-spezifischen Regeln anwendet, um zu erfahren, was invalidiert werden muss.

Der Inhalt oben war nur ein Beispiel. Sie können sich auch für eine Baumstruktur von Caches entscheiden. Wenn mit der ersten Ebene Sites (oder Mandanten) getrennt werden und auf der zweiten Ebene dann in Inhaltsarten (z. B. „main-navigation“) verzweigt wird, können Sie ggf. darauf verzichten, den Homepage-Pfad wie im Beispiel oben hinzuzufügen.

Übrigens können Sie diesen Ansatz auch mit moderneren, HTL-basierten Komponenten anwenden. Sie verfügen dann über einen JSP-Wrapper um Ihr HTL-Skript.

#### Komponentenfilter

Bei einem reinen HTL-Ansatz würde man den Fragment-Cache eher mit einem Sling-Komponentenfilter erstellen. Das haben wir so noch nicht in der Praxis gesehen, würden hier aber auf diese Weise vorgehen.

#### Sling Dynamic Include

Der Fragment-Cache wird verwendet, wenn es im Kontext einer sich ändernden Umgebung (verschiedener Seiten) eine Konstante (die Navigation) gibt.

Das Gegenteil ist allerdings ebenfalls möglich – ein relativ konstanter Kontext (eine Seite, die sich selten ändert) und einige sich ständig ändernde Fragmente auf dieser Seite (z. B. ein Liveticker).

In diesem Fall sollten Sie ggf. den sogenannten [Dynamic Sling Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) eine Chance geben. Im Wesentlichen handelt es sich hierbei um einen Komponentenfilter, der die dynamische Komponente umschließt und, anstatt die Komponente in die Seite zu rendern, einen Verweis erstellt. Dieser Verweis kann ein Ajax-Aufruf sein, sodass die Komponente vom Browser eingeschlossen wird und die umgebende Seite daher statisch zwischengespeichert werden kann. Als Alternative kann per Sling Dynamic Include (SDI) auch eine Server Side Include(SSI)-Anweisung generiert werden. Diese Anweisung wird dann auf dem Apache-Server ausgeführt. Sie können sogar Edge Side Include(ESI)-Anweisungen verwenden, wenn Sie Varnish oder ein CDN nutzen, das ESI-Skripte unterstützt.

![Sequenzdiagramm einer Anfrage mit Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Sequenzdiagramm einer Anfrage mit Sling Dynamic Include*

<br> 

In der SDI-Dokumentation heißt es, das Caching sollte für URLs deaktiviert werden, die auf „*.nocache.html“ enden. Dies ist sinnvoll, da wir es mit dynamischen Komponenten zu tun haben.

Möglicherweise sehen Sie eine weitere Option zur SDI-Verwendung: Wenn Sie den Dispatcher-Cache _nicht_ für die Includes deaktivieren, agiert der Dispatcher wie ein Fragment-Cache, der den im letzten Kapitel beschriebenen ähnelt. Seiten und Komponentenfragmente werden gleichermaßen und unabhängig voneinander im Dispatcher zwischengespeichert und bei Anfrage der Seite vom SSI-Skript auf dem Apache-Server zusammengeführt. Auf diese Weise können Sie gemeinsam genutzte Komponenten wie die Hauptnavigation implementieren (sofern Sie immer dieselbe Komponenten-URL verwenden).

Das sollte funktionieren – jedenfalls theoretisch. Aber …

Wir raten davon ab. Sie würden nämlich die Möglichkeit verlieren, den Cache für die echten dynamischen Komponenten zu umgehen. Ein SDI wird global konfiguriert und jede Änderung, die Sie für Ihren „Fragment-Cache-für-Arme“ vornehmen, würde auch für die dynamischen Komponenten gelten.

Wir empfehlen Ihnen, die SDI-Dokumentation sorgfältig zu lesen. Es gibt einige weitere Einschränkungen, aber ein SDI kann in bestimmten Fällen ein nützliches Instrument sein.

#### Verweise

* [docs.oracle.com – Schreiben von benutzerdefinierten JSP-Tags](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß – Erstellen und Verwenden von Komponentenfiltern](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org – Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com/de – Einrichten von Sling Dynamic Includes in AEM](https://helpx.adobe.com/de/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Modell-Caching

![Modellbasiertes Caching: Geschäftsobjekt mit zwei verschiedenen Renderings](assets/chapter-3/model-based-caching.png)

*Modellbasiertes Caching: Geschäftsobjekt mit zwei verschiedenen Renderings*

<br> 

Kommen wir noch einmal auf den Fall mit der Navigation zurück. Wir sind dabei davon ausgegangen, dass für jede Seite dasselbe Navigations-Markup erforderlich ist.

Aber womöglich ist das nicht der Fall. Vielleicht möchten Sie ein anderes Markup für das Element in der Navigation rendern, das die _aktuelle Seite_ darstellt.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Dies sind zwei völlig unterschiedliche Renderings. Trotzdem ist das _Geschäftsobjekt_ – die vollständige Navigationsstruktur – identisch. Das _Geschäftsobjekt_ hier wäre ein Objektdiagramm, das die Knoten in der Baumstruktur darstellt. Dieses Diagramm kann einfach in einem Arbeitsspeicher-Cache gespeichert werden. Beachten Sie jedoch, dass dieses Diagramm kein Objekt enthalten und auf kein Objekt verweisen darf, das Sie nicht selbst erstellt haben – insbesondere jetzt JCR-Knoten.

#### Caching im Browser

Wir haben die Bedeutung des Cachings im Browser bereits angesprochen, und es gibt viele gute Tutorials dazu. Letztlich ist der Dispatcher – für den Browser – nur ein Webserver, der dem HTTP-Protokoll folgt.

Wir haben jedoch – unabhängig von der Theorie – einiges Wissen gesammelt, das wir nirgendwo anders gefunden haben und weitergeben möchten.

Im Wesentlichen kann das Browser-Caching auf zwei verschiedene Arten genutzt werden:

1. Der Browser lässt eine Ressource zwischenspeichern, von der er das genaue Ablaufdatum kennt. In diesem Fall wird die Ressource nicht erneut angefordert.

2. Der Browser verfügt über eine Ressource, weiß jedoch nicht, ob sie noch gültig ist. In diesem Fall wird der Webserver (in unserem Fall der Dispatcher) gefragt: „Bitte gib mir die Ressource, wenn sie seit deiner letzten Auslieferung geändert wurde.“ Wenn sie nicht geändert wurde, antwortet der Server mit „304 – nicht geändert“ und nur die Metadaten werden übertragen.

#### Debugging

Wenn Sie Ihre Dispatcher-Einstellungen für das Browser-Caching optimieren, ist es äußerst nützlich, einen Desktop-Proxy-Server zwischen Ihrem Browser und dem Webserver zu verwenden. Unser Favorit ist der „Charles Web Debugging Proxy“ von Karl von Randow.

Mithilfe von Charles können Sie die Anfragen und Antworten lesen, die an den und vom Server übertragen werden. Und: Sie erfahren viel über das HTTP-Protokoll. Moderne Browser bieten auch Debugging-Funktionen, aber die Funktionen eines Desktop-Proxys sind so noch nie da gewesen. Sie können die übertragenen Daten bearbeiten, die Übertragung beschränken, einzelne Anfragen wiederholen und vieles mehr. Die Benutzeroberfläche ist übersichtlich und sehr umfassend.

Der grundlegendste Test besteht darin, die Website als normale Benutzende zu verwenden – wobei der Proxy dazwischen liegt – und im Proxy zu überprüfen, ob die Anzahl der statischen Anfragen (an /etc/…) im Laufe der Zeit kleiner wird, da diese im Cache gespeichert und nicht mehr angefordert werden sollten.

Wir haben festgestellt, dass ein Proxy einen klareren Überblick geben kann, da zwischengespeicherte Anfragen nicht im Protokoll angezeigt werden; aber einige in Browser integrierte Debugger diese Anfragen immer noch mit „0 ms“ oder „from disk“ anzeigen. Dies ist in Ordnung und zutreffend, könnte aber etwas verwirrend sein.

Anschließend können Sie einen Drilldown durchführen und die Header der übertragenen Dateien prüfen, um beispielsweise zu sehen, ob die „Expires“-HTTP-Header korrekt sind. Sie können Anfragen mit „if-modified-since“-Headern wiederholen, um zu sehen, ob der Server ordnungsgemäß mit einem Antwort-Code 304 oder 200 reagiert. Sie können das Timing asynchroner Aufrufe beobachten und auch Ihre Sicherheitsannahmen in einem bestimmten Umfang testen. Wir haben schon darauf hingewiesen, dass Sie nicht jede Auswahl akzeptieren sollten, die nicht explizit erwartet wurde. Hier können Sie die URL und die Parameter testen und sehen, ob sich Ihre Anwendung richtig verhält.

Eines sollten Sie beim Debugging Ihres Caches nicht tun:

Laden Sie keine Seiten im Browser neu.

Das Neuladen eines Browsers, sowohl ein _simple-reload_ als auch ein _forced-reload_ (_shift-reload_), ist nicht dasselbe wie eine normale Seitenanfrage. Eine einfache Neuladungsanfrage setzt einen Header

```
Cache-Control: max-age=0
```

Durch „shift-reload“ (wobei man „Umschalt“ beim Klicken auf die Schaltfläche zum Neuladen gedrückt hält) wird in der Regel ein Anfrage-Header festgelegt

```
Cache-Control: no-cache
```

Beide Kopfzeilen haben etwas unterschiedliche Auswirkungen. Vor allem unterscheiden sie sich vollständig von einer normalen Anfrage, wenn Sie eine URL aus dem URL-Slot öffnen oder Links auf der Site verwenden. Das normale Browsen setzt keine Cache-Control-Header, sondern wahrscheinlich einen „if-modified-since“-Header.

Wenn Sie also das normale Browsing-Verhalten debuggen möchten, sollten Sie genau das tun: _Normal browsen_. Die Verwendung der Neulade-Schaltfläche Ihres Browsers ist die beste Möglichkeit, Cache-Konfigurationsfehler in Ihrer Konfiguration nicht zu sehen.

Verwenden Sie Ihren Charles-Proxy, um zu sehen, wovon wir sprechen. Während dieser geöffnet ist, können Sie die Anfragen direkt dort wiederholen. Es ist kein Neuladen vom Browser aus erforderlich.

## Leistungstests

Durch die Verwendung eines Proxys erhalten Sie ein Gefühl für das zeitliche Verhalten Ihrer Seiten. Aber das ist natürlich keineswegs ein Leistungstest. Für einen Leistungstest müssten mehrere Clients Ihre Seiten parallel anfordern.

Ein häufiger Irrtum besteht darin, dass ein Leistungstest nur eine sehr geringe Anzahl von Seiten enthält und diese Seiten nur aus dem Dispatcher-Cache bereitgestellt werden.

Wenn Sie Ihre Anwendung an das Live-System weiterleiten, unterscheidet sich die Last völlig von der, die Sie getestet haben.

Im Live-System ist das Zugriffsmuster nicht die geringe Anzahl gleich verteilter Seiten in Tests (Startseite und wenige Inhaltsseiten). Die Anzahl der Seiten ist wesentlich größer, und die Anfragen sind sehr ungleichmäßig verteilt. Des Weiteren können Live-Seiten können natürlich nicht zu 100 % aus dem Cache bereitgestellt werden: Es gibt Invalidierungsanfragen vom Veröffentlichungssystem, die einen großen Teil Ihrer wertvollen Ressourcen automatisch invalidieren.

Und wenn Sie Ihren Dispatcher-Cache neu erstellen, werden Sie feststellen, dass sich das Veröffentlichungssystem ebenfalls völlig anders verhält, je nachdem, ob Sie nur einige wenige oder viele Seiten anfordern. Auch wenn alle Seiten ähnlich komplex sind, spielt ihre Anzahl eine Rolle. Erinnern Sie sich, was wir über das Caching in Kette gesagt haben? Wenn Sie immer die gleiche geringe Anzahl von Seiten anfordern, ist die Wahrscheinlichkeit hoch, dass die entsprechenden Blöcke mit den Rohdaten sich im Festplatten-Cache befinden oder die Blöcke vom Betriebssystem zwischengespeichert werden. Außerdem besteht eine gute Chance, dass das Repository das entsprechende Segment in seinem Hauptspeicher zwischengespeichert hat. Das erneute Rendern ist also wesentlich schneller, als wenn andere Seiten immer mal wieder aus verschiedenen Caches entfernt werden.

Das Caching ist schwierig, ebenso auch das Testen eines Systems, das auf Caching beruht. Was können Sie also tun, um ein realistischeres Szenario zu schaffen?

Wir glauben, dass Sie mehr als einen Test durchführen und mehr als einen Leistungsindex als Messgröße für die Qualität Ihrer Lösung bereitstellen müssen.

Wenn Sie bereits eine Website haben, messen Sie die Anzahl der Anfragen und deren Verteilung. Versuchen Sie, einen Test mit einer ähnlichen Anfrageverteilung zu modellieren. Eine Prise Zufälligkeit wäre auch nicht schlecht. Sie müssen keinen Browser simulieren, der statische Ressourcen wie JS und CSS lädt, denn diese spielen kaum eine Rolle. Sie werden im Browser oder schließlich im Dispatcher zwischengespeichert und führen zu keiner zusätzlichen nennenswerten Belastung. Aber referenzierte Bilder spielen eine Rolle. Informieren Sie sich in alten Protokolldateien über deren Verteilung und modellieren Sie ein ähnliches Anfragemuster.

Führen Sie jetzt einen Test durch, bei dem Ihr Dispatcher überhaupt kein Caching durchführt. Das ist Ihr Worst-Case-Szenario. Finden Sie heraus, bei welcher Spitzenlast Ihr System unter diesen schlechtesten Bedingungen instabil wird. Das Ganze wird noch schlimmer, wenn Sie auf die Idee kommen, einige Dispatcher-/Publish-Abschnitte herauszunehmen.

Führen Sie als Nächstes denselben Test unter Aktivierung aller erforderlichen Cache-Einstellungen durch. Führen Sie die parallelen Anfragen langsam aus, um den Cache vorzufüllen („Aufwärmen des Caches“) und zu sehen, wie stark Ihr System unter diesen optimalen Bedingungen belastet werden kann.

Ein Durchschnittsszenario wäre, den Test mit aktiviertem Dispatcher, aber auch mit einigen Invalidierungen auszuführen. Sie können dies simulieren, indem Sie einen Cronjob für die Statfiles ausführen lassen oder die Invalidierungsanfragen in unregelmäßigen Abständen an den Dispatcher senden. Denken Sie daran, von Zeit zu Zeit auch einige der nicht automatisch invalidierten Ressourcen zu bereinigen.

Sie können das letzte Szenario variieren, indem Sie die Zahl der Invalidierungsanfragen und die Last erhöhen.

Das ist etwas komplexer als nur ein linearer Auslastungstest, das Vertrauen in Ihre Lösung wird aber deutlich gestärkt.

Vielleicht schrecken Sie vor diesem Aufwand zurück. Führen Sie aber wenigstens einen Worst-Case-Test auf dem Veröffentlichungssystem mit einer größeren Anzahl von Seiten (gleichmäßig verteilt) durch, um die Grenzen des Systems auszuloten. Stellen Sie sicher, dass Sie die Kennzahlen des Worst-Case-Szenarios richtig interpretieren und Ihren Systemen genügend Freiraum zur Verfügung stellen.
