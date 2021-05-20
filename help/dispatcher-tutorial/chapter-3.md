---
title: '"Kapitel 3 - Themen zum erweiterten Dispatcher-Caching"'
description: Dies ist Teil 3 einer dreiteiligen Serie zum Zwischenspeichern in AEM. Wo sich die ersten beiden Teile auf das einfache HTTP-Caching im Dispatcher konzentrierten und welche Einschränkungen es gibt. In diesem Teil werden einige Ideen zur Überwindung dieser Einschränkungen erläutert.
feature: Dispatcher
topic: Architektur
role: Architect
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '6189'
ht-degree: 0%

---


# Kapitel 3 - Fortgeschrittene Zwischenspeicherungsthemen

*&quot;In der Informatik gibt es nur zwei schwierige Dinge: Cache-Invalidierung und Benennung von Elementen.&quot;*

- PHIL KARLTON

## Überblick

Dies ist Teil 3 einer dreiteiligen Serie zum Zwischenspeichern in AEM. Wo sich die ersten beiden Teile auf das einfache HTTP-Caching im Dispatcher konzentrierten und welche Einschränkungen es gibt. In diesem Teil werden einige Ideen zur Überwindung dieser Einschränkungen erläutert.

## Zwischenspeicherung im Allgemeinen

[Kapitel 1](chapter-1.md) und  [Kapitel 2](chapter-2.md)  dieser Reihe konzentrierten sich hauptsächlich auf den Dispatcher. Wir haben die Grundlagen, die Einschränkungen und die Voraussetzungen für bestimmte Kompromisse erläutert.

Die Komplexität und Komplexität der Zwischenspeicherung sind keine Probleme, die nur für den Dispatcher gelten. Das Zwischenspeichern ist im Allgemeinen schwierig.

Der Dispatcher als einziges Tool in Ihrer Toolbox zu haben, wäre eigentlich eine echte Einschränkung.

In diesem Kapitel möchten wir unsere Sicht auf das Caching weiter erweitern und Ideen entwickeln, wie Sie einige der Mängel des Dispatchers beheben können. Es gibt keine Silberkugel - Sie müssen Kompromisse in Ihrem Projekt schließen. Denken Sie daran, dass mit der Genauigkeit von Zwischenspeicherung und Invalidierung immer Komplexität entsteht und mit Komplexität die Möglichkeit von Fehlern besteht.

Sie müssen in diesen Bereichen Kompromisse eingehen,

* Leistung und Latenz
* Ressourcenverbrauch/CPU-Auslastung/Festplattenauslastung
* Genauigkeit/Währung/Stille/Sicherheit
* Einfachheit/Komplexität/Kosten/Wartbarkeit/Fehleraussagekraft

Diese Dimensionen sind in einem ziemlich komplexen System miteinander verknüpft. Es gibt kein einfaches &quot;if-this-then-that&quot;. Ein System einfacher zu gestalten, kann es schneller oder langsamer machen. Es kann Ihre Entwicklungskosten senken, aber die Kosten am Helpdesk erhöhen, z. B. wenn Kunden veraltete Inhalte sehen oder sich über eine langsame Website beschweren. All diese Faktoren müssen berücksichtigt und gegeneinander abgewogen werden. Aber jetzt sollten Sie schon eine gute Idee haben, dass es keine Wunderwaffe oder eine einzige &quot;Best Practice&quot; gibt - nur eine Menge schlechter Praktiken und ein paar gute.

## Verkettete Zwischenspeicherung

### Überblick

#### Datenfluss

Die Bereitstellung einer Seite von einem Server an den Browser eines Kunden umfasst eine Vielzahl von Systemen und Subsystemen. Wenn Sie genau hinschauen, muss eine Reihe von Hopfendaten von der Quelle bis zum Abfluss mitgenommen werden, von denen jeder ein potenzieller Kandidat für die Zwischenspeicherung ist.

![Datenfluss einer typischen CMS-Anwendung](assets/chapter-3/data-flow-typical-cms-app.png)

*Datenfluss einer typischen CMS-Anwendung*

<br> 

Starten wir unsere Journey mit einem Datenelement, das auf einer Festplatte gespeichert ist und in einem Browser angezeigt werden muss.

#### Hardware und Betriebssystem

Zunächst hat die Festplatte (HDD) selbst einen eingebauten Cache in der Hardware. Zweitens verwendet das Betriebssystem, das die Festplatte bereitstellt, freien Speicher, um häufig aufgerufene Bausteine zwischenzuspeichern, um den Zugriff zu beschleunigen.

#### Content-Repository

Die nächste Ebene ist CRX oder Oak - die von AEM verwendete Dokumentdatenbank. CRX und Oak teilen die Daten in Segmente, die im Speicher zwischengespeichert werden können, und vermeiden so einen langsameren Zugriff auf die Festplatte.

#### Drittanbieterdaten

Die meisten größeren Web-Installationen verfügen auch über Daten von Drittanbietern. Daten aus einem Produktinformationssystem, einem Customer Relationship Management System, einer älteren Datenbank oder einem beliebigen Web Service. Diese Daten müssen nicht bei Bedarf aus der Quelle abgerufen werden - insbesondere nicht, wenn bekannt ist, dass sie sich nicht allzu häufig ändern. Es kann also zwischengespeichert werden, wenn es nicht in der CRX-Datenbank synchronisiert wird.

#### Business Layer - App/Modell

Normalerweise rendern Ihre Vorlagenskripte nicht den Rohinhalt, der von CRX stammt, über die JCR-API. Wahrscheinlich haben Sie eine geschäftliche Schicht dazwischen, die Daten in einem Business Domain-Objekt zusammenführt, berechnet und/oder transformiert. Rate mal, was - wenn diese Vorgänge teuer sind, sollten Sie sie zwischenspeichern.

#### Markup-Fragmente

Das Modell ist jetzt die Grundlage für das Rendering des Markups für eine Komponente. Warum wird das gerenderte Modell nicht auch zwischengespeichert?

#### Dispatcher, CDN und andere Proxys

Aus leitet die gerenderte HTML-Seite an den Dispatcher weiter. Wir haben bereits besprochen, dass der Hauptzweck des Dispatchers darin besteht, HTML-Seiten und andere Web-Ressourcen zwischenzuspeichern (trotz seines Namens). Bevor die Ressourcen den Browser erreichen, kann ein Reverse-Proxy übergeben werden, der zwischengespeichert werden kann, und ein CDN, das auch für die Zwischenspeicherung verwendet wird. Der Client kann sich in einem Büro befinden, das nur über einen Proxy Webzugriff gewährt - und der Proxy entscheidet sich möglicherweise dafür, den Cache zu speichern, um Traffic zu sparen.

#### Browser-Cache

Nicht zuletzt - der Browser speichert auch Zwischenspeicher. Dies ist ein leicht übersehenes Asset. Aber es ist der nächstgelegene und schnellste Cache, den Sie in der Caching-Kette haben. Leider - es wird nicht von Benutzern geteilt - aber immer noch zwischen verschiedenen Anforderungen eines Benutzers.

### Zwischenspeichern und Warum

Das ist eine lange Kette potenzieller Caches. Und wir alle sind mit Problemen konfrontiert, bei denen wir veraltete Inhalte gesehen haben. Aber wenn man berücksichtigt, wie viele Phasen es gibt, ist es ein Wunder, dass es meistens überhaupt funktioniert.

Aber wo in dieser Kette ist es überhaupt sinnvoll, Zwischenspeicher zu speichern? Am Anfang? Am Ende? Überall? Es kommt darauf an.. und es hängt von einer großen Anzahl von Faktoren ab. Selbst zwei Ressourcen auf derselben Website könnten eine andere Antwort auf diese Frage wünschen.

Um Ihnen eine grobe Vorstellung davon zu geben, welche Faktoren Sie in Betracht ziehen könnten,

**Lebensdauer**  - Wenn Objekte eine kurze inhärente Live-Zeit haben (Traffic-Daten können kürzer als Wetterdaten sein), lohnt es sich möglicherweise nicht, sie zwischenzuspeichern.

**Produktionskosten -** Wie teuer (in Bezug auf CPU-Zyklen und I/O) die Wiederproduktion und Bereitstellung eines Objekts ist. Wenn es billig ist, kann es nicht nötig sein, das Caching durchzuführen.

**Größe**  - Für große Objekte müssen mehr Ressourcen zwischengespeichert werden. Das könnte ein begrenzender Faktor sein und muss gegen den Nutzen abgewogen werden.

**Zugriffshäufigkeit**  - Wenn selten auf Objekte zugegriffen wird, ist das Zwischenspeichern möglicherweise nicht effektiv. Sie würden einfach veraltet sein oder invalidiert werden, bevor sie zum zweiten Mal aus dem Cache aufgerufen werden. Solche Elemente würden einfach Speicherressourcen blockieren.

**Freigegebener Zugriff**  - Daten, die von mehr als einer Entität verwendet werden, sollten weiter oben in der Kette zwischengespeichert werden. Tatsächlich ist die Caching-Kette keine Kette, sondern ein Baum. Ein Datenelement im Repository kann von mehr als einem Modell verwendet werden. Diese Modelle können wiederum von mehreren Render-Skripten verwendet werden, um HTML-Fragmente zu generieren. Diese Fragmente sind auf mehreren Seiten enthalten, die mit ihren privaten Caches im Browser an mehrere Benutzer verteilt werden. &quot;Teilen&quot; bedeutet also nicht nur, dass man zwischen Leuten teilt, sondern zwischen Softwareteilen. Wenn Sie einen potenziellen &quot;freigegebenen&quot;Cache finden möchten, verfolgen Sie einfach den Baum zum Stammverzeichnis zurück und suchen Sie nach einem gemeinsamen Vorgänger - dort sollten Sie zwischenspeichern.

**Georaumverteilung**  - Wenn Ihre Benutzer auf der ganzen Welt verteilt sind, kann die Verwendung eines verteilten Cache-Netzes dazu beitragen, die Latenz zu verringern.

**Netzwerkbandbreite und -latenz**  - Vorstellung von Latenzzeiten, wer sind Ihre Kunden und welche Netzwerktypen nutzen sie? Vielleicht sind Ihre Kunden mobile Kunden in einem unterentwickelten Land, das eine 3G-Verbindung mit Smartphones der älteren Generation nutzt? Erwägen Sie, kleinere Objekte zu erstellen und sie in den Browser-Caches zwischenzuspeichern.

Diese Liste ist bei weitem nicht vollständig, aber wir glauben, dass Sie jetzt die Idee haben.

### Grundlegende Regeln für das Caching mit einer Unterteilung

Wieder - Zwischenspeicherung ist schwierig. Lassen Sie uns einige Grundregeln teilen, die wir aus früheren Projekten extrahiert haben und mit denen Sie Probleme in Ihrem Projekt vermeiden können.

#### Vermeiden Sie die doppelte Zwischenspeicherung

Jede im letzten Kapitel eingeführte Ebene bietet einen Wert in der Caching-Kette. Entweder durch Einsparung von Rechenzyklen oder durch Annäherung der Daten an den Verbraucher. Es ist nicht falsch, ein Datenelement in mehreren Phasen der Kette zwischenzuspeichern - aber Sie sollten immer berücksichtigen, was der Nutzen und die Kosten der nächsten Phase sind. Das Zwischenspeichern einer vollständigen Seite im Veröffentlichungssystem bietet in der Regel keinen Vorteil - wie dies bereits im Dispatcher der Fall ist.

#### Mischen von Invalidierungsstrategien

Es gibt drei grundlegende Invalidierungsstrategien:

* **TTL, Time to Live:**  Ein Objekt läuft nach einer bestimmten Zeitdauer ab (z. B. &quot;2 Stunden von jetzt&quot;).
* **Ablaufdatum:** Das Objekt läuft zu einem bestimmten Zeitpunkt in der Zukunft ab (z. B. &quot;10.00 Uhr am 10. Juni 2019&quot;).
* **Ereignisbasiert:** Das Objekt wird explizit durch ein Ereignis invalidiert, das auf der Plattform aufgetreten ist (z. B. wenn eine Seite geändert und aktiviert wird).

Jetzt können Sie verschiedene Strategien auf verschiedenen Cache-Ebenen verwenden, aber es gibt einige &quot;toxische&quot;.

#### Ereignisbasierte Invalidierung

![Reine ereignisbasierte Invalidierung](assets/chapter-3/event-based-invalidation.png)

*Reine ereignisbasierte Invalidierung: Invalidierung vom inneren Cache zur äußeren Ebene*

<br> 

Eine reine ereignisbasierte Invalidierung ist am einfachsten zu verstehen, am einfachsten zu erhalten, theoretisch richtig und am präzisesten zu sein.

Einfach gesagt werden die Caches nacheinander invalidiert, nachdem sich das Objekt geändert hat.

Beachten Sie nur eine Regel:

Immer von innen in den externen Cache invalidieren. Wenn Sie zuerst einen äußeren Cache invalidiert haben, wird möglicherweise veralteter Inhalt von einem inneren erneut zwischengespeichert. Gehen Sie nicht davon aus, wann ein Cache erneut aktualisiert wird - stellen Sie sicher, dass es sicher ist. Am besten durch Auslösen der Invalidierung des äußeren Caches _nach_ , wodurch der innere Cache invalidiert wird.

Das ist die Theorie. Aber in der Praxis gibt es eine Reihe von Fallstricken. Die Ereignisse müssen verteilt werden - potenziell über ein Netzwerk. In der Praxis ist dies das schwierigste System zur Invalidierung.

#### Auto - Heilung

Bei ereignisbasierter Invalidierung sollten Sie über einen Notfallplan verfügen. Was passiert, wenn ein Invalidierungs-Ereignis verpasst wird? Eine einfache Strategie könnte darin bestehen, nach einer bestimmten Zeit ungültig zu machen oder zu bereinigen. Sie haben dieses Ereignis möglicherweise verpasst und veralteten Inhalt bereitgestellt. Ihre Objekte haben jedoch auch eine implizite TTL von mehreren Stunden (Tage). Schließlich heilt sich das System selbst.

#### Reine TTL-basierte Invalidierung

![Nicht synchronisierte TTL-basierte Invalidierung](assets/chapter-3/ttl-based-invalidation.png)

*Nicht synchronisierte TTL-basierte Invalidierung*

<br> 

Das ist auch ein ganz gemeinsames System. Sie stapeln mehrere Cache-Ebenen, von denen jede für eine bestimmte Zeit für ein Objekt geeignet ist.

Es ist leicht zu implementieren. Leider ist es schwierig, die effektive Lebensdauer eines Datenelements vorherzusagen.

![Äußerer Winkel zur Verlängerung der Lebensdauer eines inneren Objekts](assets/chapter-3/outer-cache.png)

*Externer Cache verlängert die Lebensdauer eines inneren Objekts*

<br> 

Siehe Abbildung oben. Jede Zwischenspeicherschicht führt eine TTL von 2 Minuten ein. Jetzt - die TTL insgesamt muss auch 2 Min., nicht wahr? Nicht ganz. Wenn die äußere Schicht das Objekt unmittelbar vor dem Ablauf abruft, verlängert die äußere Schicht die effektive Live-Zeit des Objekts. Die effektive Live-Zeit kann in diesem Fall zwischen 2 und 4 Minuten betragen. Nehmen wir an, Sie haben sich mit Ihrer Geschäftsabteilung einverstanden erklärt, eines Tages ist tolerierbar - und Sie haben vier Zwischenspeicherschichten. Die tatsächliche TTL auf jeder Ebene darf nicht länger als sechs Stunden dauern... die Cache-Vermisstrate erhöhen ...

Wir sagen nicht, dass es ein schlechtes System ist. Du solltest nur seine Grenzen kennen. Es ist eine nette und einfache Strategie, mit der man beginnen kann. Nur wenn der Traffic Ihrer Site zunimmt, können Sie eine präzisere Strategie in Betracht ziehen.

*Synchronisieren der Invalidierungszeit durch Festlegen eines bestimmten Datums*

#### Invalidierung aufgrund des Ablaufdatums

Sie erhalten eine besser vorhersehbare effektive Lebensdauer, wenn Sie ein bestimmtes Datum für das innere Objekt festlegen und es an die externen Caches übertragen.

![Ablaufdaten synchronisieren](assets/chapter-3/synchronize-expiration-dates.png)

*Ablaufdaten synchronisieren*

<br> 

Allerdings können nicht alle Caches die Daten propagieren. Und es kann schlimm werden, wenn der äußere Zwischenspeicher zwei innere Objekte mit unterschiedlichen Ablaufdaten aggregiert.

#### Mischen ereignisbasierter und TTL-basierter Invalidierung

![Mischen ereignisbasierter und TTL-basierter Strategien](assets/chapter-3/mixing-event-ttl-strategies.png)

*Mischen ereignisbasierter und TTL-basierter Strategien*

<br> 

Ein gängiges Schema in der AEM ist die Verwendung ereignisbasierter Invalidierung bei inneren Caches (z. B. In-Memory-Caches, in denen Ereignisse nahezu in Echtzeit verarbeitet werden können) und TTL-basierter Caches von außen - in denen Sie möglicherweise keinen Zugriff auf explizite Invalidierung haben.

In der AEM hätten Sie einen Arbeitsspeichercache für Geschäftsobjekte und HTML-Fragmente in den Veröffentlichungssystemen, der ungültig gemacht wird, wenn sich die zugrunde liegenden Ressourcen ändern und Sie dieses Änderungsereignis an den Dispatcher weiterleiten, der auch ereignisbasiert funktioniert. Davor hätten Sie beispielsweise ein TTL-basiertes CDN.

Eine Ebene mit (kurzer) TTL-basiertem Caching vor einem Dispatcher konnte eine Spitze, die normalerweise nach einer automatischen Invalidierung auftreten würde, effektiv mildern.

#### TTL - und ereignisbasierte Invalidierung mischen

![Mischen der TTL - und ereignisbasierten Invalidierung](assets/chapter-3/toxic.png)

*Toxisch: Mischen der TTL - und ereignisbasierten Invalidierung*

<br> 

Diese Kombination ist toxisch. Platzieren und speichern Sie den ereignisbasierten Cache nie nach einer TTL- oder Ablaufdaten-basierten Zwischenspeicherung. Erinnern Sie sich an den Spillover-Effekt, den wir in der &quot;reinen TTL&quot;-Strategie hatten? Dieselbe Wirkung kann hier beobachtet werden. Nur dass das Invalidierungs-Ereignis des äußeren Caches bereits eingetreten ist, kann nicht erneut auftreten - jemals kann dies die Lebensdauer des zwischengespeicherten Objekts auf Unendlichkeit erweitern.

![TTL-basierte und ereignisbasierte Kombination: Spill-over in Unendlichkeit](assets/chapter-3/infinity.png)

*TTL-basierte und ereignisbasierte Kombination: Spill-over in Unendlichkeit*

<br> 

## Partielles Caching und Zwischenspeicherung im Arbeitsspeicher

Sie können in die Phase des Rendervorgangs einbinden, um Zwischenspeicherungsschichten hinzuzufügen. Vom Abrufen von Remote-Datenübertragungsobjekten oder Erstellen lokaler Geschäftsobjekte bis zum Zwischenspeichern des gerenderten Markups einer einzelnen Komponente. Wir überlassen konkrete Implementierungen einem späteren Tutorial. Aber vielleicht haben Sie schon vor, einige dieser Zwischenspeicherschichten selbst implementiert zu haben. Das Mindeste, was wir hier tun können, ist die Einführung der Grundprinzipien - und Fallstricke.

### Wörter der Warnung

#### Zugriffskontrolle respektieren

Die hier beschriebenen Techniken sind ziemlich leistungsstark und _must-have_ in den Toolboxes jedes AEM Entwicklers. Aber lassen Sie sich nicht zu begeistern, benutzen Sie sie klug. Wenn ein Objekt in einem Cache gespeichert und in Folgeanfragen für andere Benutzer freigegeben wird, bedeutet dies tatsächlich, die Zugriffskontrolle zu umgehen. Dies ist in der Regel kein Problem auf öffentlich zugänglichen Websites, kann jedoch auftreten, wenn sich ein Benutzer anmelden muss, bevor er Zugriff erhält.

Beachten Sie, dass Sie das HTML-Markup eines Sites-Hauptmenüs in einem Arbeitsspeichercache speichern, um es zwischen verschiedenen Seiten freizugeben. Dies ist ein perfektes Beispiel für das Speichern teilweise gerenderter HTML-Inhalte, da die Erstellung einer Navigation normalerweise teuer ist, da sie viele Seiten durchlaufen muss.

Sie teilen nicht die gleiche Menüstruktur zwischen allen Seiten, sondern auch für alle Benutzer, wodurch sie noch effizienter wird. Aber warten ... aber vielleicht gibt es einige Elemente im Menü, die nur für eine bestimmte Gruppe von Benutzern reserviert sind. In diesem Fall kann die Zwischenspeicherung etwas komplexer werden.

#### Benutzerdefinierte Geschäftsobjekte nur zwischenspeichern

Wenn überhaupt - das ist der wichtigste Ratschlag, können wir Ihnen Folgendes geben:

>[!WARNING]
>
>Nur Objekte zwischenspeichern, die Ihnen gehören, die unveränderlich sind, die Sie selbst erstellt haben, die flach sind und keine ausgehende Referenz haben.

Was bedeutet das?

1. Man weiß nicht, welchen Lebenszyklus die Objekte anderer Menschen haben sollen. Beachten Sie, dass Sie einen Verweis auf ein Anforderungsobjekt vermerken und beschließen, es zwischenzuspeichern. Jetzt ist die Anfrage beendet und der Servlet-Container möchte dieses Objekt für die nächste eingehende Anfrage wiederverwenden. In diesem Fall ändert jemand anders den Inhalt, von dem Sie dachten, dass Sie über eine exklusive Kontrolle verfügen. Enttäuschen Sie das nicht - So etwas haben wir in einem Projekt gesehen. Der Kunde sah anstelle seiner eigenen Kundendaten.

2. Solange ein Objekt durch eine Kette anderer Verweise referenziert wird, kann es nicht aus dem Heap entfernt werden. Wenn Sie ein angeblich kleines Objekt in Ihrem Cache behalten, das referenziert, nehmen wir an, dass Sie eine 4 MB große Darstellung eines Bildes haben, können Sie mit dem Löschen von Speicher Probleme bekommen. Caches sollen auf schwachen Referenzen basieren. Aber - schwache Referenzen funktionieren nicht wie erwartet. Dies ist die absolut beste Methode, einen Speicherleck zu erzeugen und einen Out-of-Memory-Fehler zu erzeugen. Und - Sie wissen nicht, wie groß die zurückbehaltene Erinnerung an fremde Objekte ist, oder?

3. Insbesondere in Sling können Sie (fast) jedes Objekt einander anpassen. Nehmen Sie an, Sie legen eine Ressource in den Cache. Die nächste Anfrage (mit unterschiedlichen Zugriffsrechten) ruft diese Ressource ab und passt sie in einen resourceResolver oder eine Sitzung an, um auf andere Ressourcen zuzugreifen, auf die er keinen Zugriff hätte.

4. Selbst wenn Sie einen dünnen &quot;Wrapper&quot;um eine Ressource aus AEM erstellen, dürfen Sie diese nicht zwischenspeichern - auch wenn es sich um eine eigene und unveränderliche Ressource handelt. Das umschlossene Objekt wäre eine Referenz (die wir vorher verbieten) und wenn wir scharf aussehen, verursacht dies im Grunde dieselben Probleme wie im letzten Element beschrieben.

5. Wenn Sie Ihre eigenen Objekte zwischenspeichern möchten, erstellen Sie sie, indem Sie Primitive-Daten in Ihre eigenen SHALO-Objekte kopieren. Sie können eine Verknüpfung zwischen Ihren eigenen Objekten mithilfe von Verweisen herstellen. So können Sie beispielsweise eine Struktur von Objekten zwischenspeichern. Das ist in Ordnung - aber nur zwischengespeicherte Objekte, die Sie gerade in derselben Anfrage erstellt haben - und keine Objekte, die von einer anderen Stelle angefordert wurden (auch wenn es sich um die Namensräume Ihres Objekts handelt). _Das Kopieren_ von Objekten ist der Schlüssel. Achten Sie darauf, die gesamte Struktur der verknüpften Objekte gleichzeitig zu bereinigen und eingehende und ausgehende Verweise auf Ihre Struktur zu vermeiden.

6. Ja - und behalten Sie Ihre Objekte unverändert bei. Private Eigenschaften, nur und keine Setter.

Das sind viele Regeln, aber es lohnt sich, ihnen zu folgen. Selbst wenn Sie erfahren und super intelligent sind und alles unter Kontrolle haben. Der junge Kollege in Ihrem Projekt hat gerade seinen Universitätsabschluss gemacht. Er kennt nicht all diese Fallstricke. Wenn es keine Fallstricke gibt, gibt es nichts zu vermeiden. Halten Sie es einfach und verständlich.

### Tools und Bibliotheken

In dieser Reihe geht es darum, Konzepte zu verstehen und Ihnen die Möglichkeit zu geben, eine Architektur zu erstellen, die Ihrem Anwendungsfall am besten entspricht.

Insbesondere fördern wir kein Instrument. Aber geben Sie Ihnen Hinweise, wie Sie sie bewerten können. Zum Beispiel AEM hat einen einfachen integrierten Cache mit einer festen TTL seit Version 6.0. Sollen Sie ihn verwenden? Wahrscheinlich nicht bei der Veröffentlichung, wenn ein ereignisbasierter Cache in der Kette folgt (Hinweis: Dispatcher). Aber es könnte eine anständige Wahl für einen Autor sein. Es gibt auch einen HTTP-Cache nach Adobe ACS Commons, der sich möglicherweise lohnt.

Oder Sie erstellen Ihre eigene, basierend auf einem ausgereiften Caching-Framework wie [Ehcache](https://www.ehcache.org). Dies kann verwendet werden, um Java-Objekte und gerenderte Markup-Objekte (`String` -Objekte) zwischenzuspeichern.

In einigen einfachen Fällen können Sie auch mit der Verwendung von gleichzeitigen Hash-Karten zurechtkommen - hier werden Sie schnell Einschränkungen sehen - entweder im Tool oder in Ihren Fähigkeiten. Die Gleichzeitigkeit ist so schwer Übergeordnet wie Benennung und Zwischenspeicherung.

#### Verweise

* [ACS Commons HTTP Cache  ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Cache-Caching-Framework](https://www.ehcache.org)

### Grundbegriffe

Wir werden hier nicht zu tief in die Caching-Theorie gehen, aber wir fühlen uns verpflichtet, ein paar Schlagwörter bereitzustellen, damit Sie einen guten Sprung beginnen.

#### Cache-Entfernung

Wir sprachen viel über Invalidierung und Bereinigung. _Die Cache-_ Entfernung bezieht sich auf folgende Begriffe: Nach einem Eintrag wird er verworfen und ist nicht mehr verfügbar. Die Entfernung erfolgt jedoch nicht, wenn ein Eintrag veraltet ist, sondern wenn der Cache voll ist. Neuere oder &quot;wichtigere&quot;Elemente führen ältere oder weniger wichtige aus dem Cache. Welche Einträge Sie opfern müssen, ist eine Einzelfallentscheidung. Vielleicht möchten Sie die ältesten oder diejenigen, die sehr selten verwendet wurden oder lange Zeit zuletzt aufgerufen wurden, ausweisen.

#### Präventivzwischenspeicherung

Preemptive Caching bedeutet, den Eintrag mit neuem Inhalt zu erstellen, sobald er ungültig gemacht oder als veraltet betrachtet wird. Natürlich - Sie würden dies nur mit wenigen Ressourcen tun, die Sie sicher sind, häufig und sofort aufgerufen werden. Andernfalls würden Sie Ressourcen beim Erstellen von Cache-Einträgen verschwenden, die möglicherweise nie angefordert werden. Wenn Sie Cache-Einträge vorab erstellen, können Sie die Latenz der ersten Anforderung auf eine Ressource nach der Cache-Invalidierung reduzieren.

#### Cachewärmung

Die Cachewärmung steht in engem Zusammenhang mit der präventiven Zwischenspeicherung. Obwohl du diesen Begriff nicht für ein Live-System verwenden würdest. Und es ist weniger zeitgebunden als der erste. Sie werden nicht sofort nach der Invalidierung erneut zwischengespeichert, sondern nach und nach füllen Sie den Cache, wenn die Zeit dies zulässt.

Sie nehmen beispielsweise die Veröffentlichen-/Dispatcher-Ebene aus dem Lastenausgleich, um sie zu aktualisieren. Vor der erneuten Integration durchsuchen Sie automatisch die am häufigsten aufgerufenen Seiten, um sie erneut in den Cache zu laden. Wenn der Cache &quot;warm&quot; - ausreichend gefüllt ist, integrieren Sie das Bein wieder in den Lastverteiler.

Oder vielleicht integrieren Sie das Bein auf einmal wieder, aber Sie drosseln den Verkehr zu diesem Bein, damit es die Möglichkeit hat, die Zwischenspeicher durch regelmäßige Nutzung zu erwärmen.

Oder Sie möchten auch einige weniger häufig aufgerufene Seiten zwischenspeichern, wenn Ihr System inaktiv ist, um die Latenz zu verringern, wenn sie tatsächlich von echten Anforderungen aufgerufen werden.

#### Cache Object Identity, Payload, Invalidierung Dependency und TTL

Im Allgemeinen hat ein zwischengespeichertes Objekt oder &quot;Eintrag&quot;fünf Haupteigenschaften,

#### Schlüssel

Dies ist die Identität, die die Eigenschaft ist, mit der Sie und das Objekt identifizieren. Sie können die Payload entweder abrufen oder aus dem Cache löschen. Der Dispatcher verwendet beispielsweise die URL einer Seite als Schlüssel. Beachten Sie, dass der Dispatcher die Seitenpfade nicht verwendet. Dies reicht nicht aus, um verschiedene Renderings voneinander zu unterscheiden. Andere Caches können unterschiedliche Schlüssel verwenden. Wir werden später einige Beispiele sehen.

#### Wert/Nutzlast

Das ist die Schatztruhe des Objekts, die Daten, die Sie abrufen möchten. Im Falle des Dispatchers sind es die Dateiinhalte. Es kann sich aber auch um eine Java-Objektstruktur handeln.

#### TTL

Wir haben die TTL bereits abgedeckt. Die Zeit, nach der ein Eintrag als veraltet betrachtet wird und nicht länger bereitgestellt werden sollte.

#### Abhängigkeit

Dies bezieht sich auf die ereignisbasierte Invalidierung. Von welchen Originaldaten hängt dieses Objekt ab? In Teil I haben wir bereits gesagt, dass ein echtes und genaues Abhängigkeitstracking zu komplex ist. Aber mit unserem Wissen über das System können Sie die Abhängigkeiten mit einem einfacheren Modell nähern. Wir machen genügend Objekte ungültig, um veraltete Inhalte zu bereinigen.. und vielleicht versehentlich mehr als erforderlich. Trotzdem versuchen wir, unter &quot;alles bereinigen&quot; zu bleiben.

Welche Objekte davon abhängen, was andere in jeder einzelnen Anwendung echt sind. Wir werden Ihnen einige Beispiele dafür geben, wie Sie später eine Abhängigkeitsstrategie implementieren können.

### HTML-Fragmentzwischenspeicherung

![Wiederverwenden eines gerenderten Fragments auf verschiedenen Seiten](assets/chapter-3/re-using-rendered-fragment.png)

*Wiederverwenden eines gerenderten Fragments auf verschiedenen Seiten*

<br> 

Die Zwischenspeicherung von HTML-Fragmenten ist ein mächtiges Werkzeug. Die Idee besteht darin, das von einer Komponente generierte HTML-Markup in einem Arbeitsspeicher-Cache zu speichern. Du darfst fragen, warum ich das tun soll? Ich zwischenspeichere sowieso das Markup der gesamten Seite im Dispatcher - einschließlich des Markups dieser Komponente. Wir stimmen zu. Sie tun dies - aber einmal pro Seite. Sie teilen dieses Markup nicht zwischen den Seiten.

Angenommen, Sie rendern eine Navigation auf jeder Seite. Das Markup sieht auf jeder Seite gleich aus. Sie rendern sie jedoch immer wieder für jede Seite, das heißt nicht im Dispatcher. Und denken Sie daran: Nach der automatischen Invalidierung müssen alle Seiten erneut gerendert werden. Im Grunde führen Sie denselben Code mit denselben Ergebnissen hundertmal aus.

Aus unserer Erfahrung ist es sehr kostspielig, eine verschachtelte Top-Navigation zu rendern. Normalerweise durchlaufen Sie einen guten Teil der Dokumentstruktur, um die Navigationselemente zu generieren. Auch wenn Sie nur den Navigationstitel und die URL benötigen, müssen die Seiten in den Speicher geladen werden. Und hier verstopfen sie wertvolle Ressourcen. Immer wieder.

Die Komponente wird jedoch auf vielen Seiten verwendet. Und die Freigabe von etwas ist ein Hinweis auf die Verwendung eines Cache. Sie sollten also überprüfen, ob die Navigationskomponente bereits gerendert und zwischengespeichert wurde, und anstelle des erneuten Renderns nur den Cache-Wert ausgeben.

Zwei wunderbare Schönheiten dieses Programms sind leicht zu vermissen:

1. Sie speichern eine Java-Zeichenfolge im Cache. Ein String hat keine ausgehenden Verweise und ist unveränderlich. Unter Berücksichtigung der obigen Warnungen ist dies also supersicher.

2. Die Invalidierung ist auch sehr einfach. Wenn sich Ihre Website ändert, möchten Sie diesen Cache-Eintrag ungültig machen. Neubau ist relativ billig, da es nur einmal durchgeführt werden muss und dann von allen Hunderten von Seiten wiederverwendet wird.

Dies ist eine große Erleichterung für Ihre Veröffentlichungs-Server.

### Implementierung von Fragment-Caches

#### Benutzerdefinierte Tags

Früher, als Sie JSP als Vorlagenmodul verwendet haben, wurde häufig ein benutzerdefiniertes JSP-Tag verwendet, das um die Komponenten-Rendering-Code herum umschlug.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

Das benutzerdefinierte -Tag erfasst seinen Text und schreibt ihn in den Cache oder verhindert die Ausführung seines Hauptteils und gibt stattdessen die Payload des Cache-Eintrags aus.

Der &quot;Schlüssel&quot;ist der Komponentenpfad, den er auf der Startseite haben würde. Wir verwenden den Pfad der Komponente nicht auf der aktuellen Seite, da dies einen Cache-Eintrag pro Seite erstellen würde - was unserer Absicht widerspricht, diese Komponente freizugeben. Wir verwenden auch nicht nur den relativen Pfad der Komponenten (`jcr:conten/mainnavigation`), da dies die Verwendung verschiedener Navigationskomponenten auf verschiedenen Sites verhindern würde.

&quot;Cache&quot;ist ein Indikator, in dem der Eintrag gespeichert werden soll. Normalerweise verfügen Sie über mehr als einen Cache, in dem Sie Elemente speichern. Jeder von ihnen könnte sich etwas anders benehmen. Es ist also gut, zu unterscheiden, was gespeichert wird - auch wenn es letztlich nur Zeichenfolgen sind.

&quot;Abhängigkeit&quot;: Hiervon ist der Cache-Eintrag abhängig. Der Cache &quot;Hauptnavigation&quot;kann eine Regel enthalten, wonach der entsprechende Eintrag gelöscht werden muss, wenn unter dem Knoten &quot;dependency&quot;Änderungen vorgenommen werden. Daher muss sich Ihre Cache-Implementierung selbst als Ereignis-Listener im Repository registrieren, um sich der Änderungen bewusst zu sein, und dann die Cache-spezifischen Regeln anwenden, um herauszufinden, was ungültig gemacht werden muss.

Die obige war nur ein Beispiel. Sie können auch eine Struktur von Caches wählen. Wenn die erste Ebene verwendet wird, um Sites (oder Mandanten) zu trennen, und die zweite Ebene dann in Inhaltsarten (z. B. &quot;Hauptnavigation&quot;) verzweigt wird, können Sie das Hinzufügen des Pfads für die Homepage ersparen, wie im Beispiel oben gezeigt.

Übrigens: Sie können diesen Ansatz auch mit moderneren HTL-basierten Komponenten verwenden. Sie verfügen dann über einen JSP-Wrapper um Ihr HTL-Skript.

#### Komponentenfilter

Bei einem reinen HTL-Ansatz würden Sie jedoch den Fragmentcache lieber mit einem Sling-Komponentenfilter erstellen. Wir haben das noch nicht in der Wildnis gesehen, aber das ist der Ansatz, den wir in dieser Frage einschlagen würden.

#### Sling Dynamic Include

Der Fragmentcache wird verwendet, wenn Sie im Kontext einer sich ändernden Umgebung (verschiedener Seiten) eine Konstante (die Navigation) haben.

Sie können jedoch auch das Gegenteil aufweisen, einen relativ konstanten Kontext (eine Seite, die sich selten ändert) und einige sich ständig ändernde Fragmente auf dieser Seite (z. B. ein Live-Ticker).

In diesem Fall können Sie [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) eine Chance geben. Im Wesentlichen handelt es sich hierbei um einen Komponentenfilter, der die dynamische Komponente umbricht und anstatt die Komponente in die Seite zu rendern, wird eine Referenz erstellt. Diese Referenz kann ein Ajax-Aufruf sein, sodass die Komponente vom Browser eingeschlossen wird und die umgebende Seite statisch zwischengespeichert werden kann. Oder - alternativ - Sling Dynamic Include kann eine SSI-Direktive (Server Side Include) generieren. Diese Anweisung würde auf dem Apache-Server ausgeführt. Sie können sogar die Anweisungen ESI - Edge Side Include verwenden, wenn Sie Varnish oder ein CDN nutzen, das ESI-Skripte unterstützt.

![Sequenzdiagramm einer Anforderung mit Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Sequenzdiagramm einer Anforderung mit Sling Dynamic Include*

<br> 

In der SDI-Dokumentation heißt es, Sie sollten die Zwischenspeicherung für URLs deaktivieren, die auf &quot;*.nocache.html&quot;enden. Dies ist sinnvoll - da Sie es mit dynamischen Komponenten zu tun haben.

Möglicherweise wird eine weitere Option zur Verwendung von SDI angezeigt: Wenn Sie _nicht_ den Dispatcher-Cache für die Includes deaktivieren, fungiert der Dispatcher wie ein Fragment-Cache, der dem im letzten Kapitel beschriebenen ähnelt: Seiten und Komponentenfragmente werden gleichermaßen und unabhängig voneinander im Dispatcher zwischengespeichert und vom SSI-Skript auf dem Apache-Server zugeordnet, wenn die Seite angefordert wird. Auf diese Weise können Sie freigegebene Komponenten wie die Hauptnavigation implementieren (sofern Sie immer dieselbe Komponenten-URL verwenden).

Das sollte funktionieren - theoretisch. Aber...

Wir empfehlen, dies nicht zu tun: Sie würden die Möglichkeit verlieren, den Cache für die echten dynamischen Komponenten zu umgehen. Das SDI ist global konfiguriert und die Änderungen, die Sie für Ihren &quot;arm-mans-fragment-cache&quot;vornehmen würden, würden auch für die dynamischen Komponenten gelten.

Wir empfehlen Ihnen, die SDI-Dokumentation sorgfältig zu lesen. Es gibt einige weitere Einschränkungen, aber SDI ist in einigen Fällen ein nützliches Tool.

#### Verweise

* [docs.oracle.com - So schreiben Sie benutzerdefinierte JSP-Tags](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Erstellen und Verwenden von Komponentenfiltern](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Einrichten dynamischer Sling-Includes in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Modellzwischenspeicherung

![Modellbasierte Zwischenspeicherung: Ein Geschäftsobjekt mit zwei verschiedenen Renderings](assets/chapter-3/model-based-caching.png)

*Modellbasierte Zwischenspeicherung: Ein Geschäftsobjekt mit zwei verschiedenen Renderings*

<br> 

Lassen Sie uns den Fall mit der Navigation erneut aufrufen. Wir gingen davon aus, dass jede Seite dasselbe Markup der Navigation erfordert.

Aber vielleicht ist das nicht der Fall. Möglicherweise möchten Sie ein anderes Markup für das Element in der Navigation rendern, das die _aktuelle Seite_ darstellt.

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

Dies sind zwei völlig unterschiedliche Renderings. Dennoch ist das _Geschäftsobjekt_ - die vollständige Navigationsstruktur - identisch.  Das _Geschäftsobjekt_ hier wäre ein Objektdiagramm, das die Knoten in der Struktur darstellt. Dieses Diagramm kann einfach in einem Arbeitsspeichercache gespeichert werden. Beachten Sie jedoch, dass dieses Diagramm kein Objekt enthalten oder auf kein Objekt verweisen darf, das Sie selbst nicht erstellt haben - insbesondere jetzt JCR-Knoten.

#### Zwischenspeicherung im Browser

Wir haben die Wichtigkeit des Caching im Browser bereits angesprochen, und es gibt viele gute Tutorials da draußen. Letztlich ist der Dispatcher - für den Browser - nur ein Webserver, der dem HTTP-Protokoll folgt.

Wir haben jedoch - trotz der Theorie - einige Erkenntnisse gesammelt, die wir nirgendwo anders gefunden haben und die wir teilen wollen.

Im Wesentlichen kann die Browser-Zwischenspeicherung auf zwei verschiedene Arten genutzt werden:

1. Der Browser hat eine Ressource zwischengespeichert, von der er das genaue Ablaufdatum kennt. In diesem Fall wird die Ressource nicht erneut angefordert.

2. Der Browser verfügt über eine Ressource, ist jedoch nicht sicher, ob sie noch gültig ist. In diesem Fall wird der Webserver (in unserem Fall der Dispatcher) gefragt. Bitte geben Sie mir die Ressource an, wenn sie seit der letzten Auslieferung geändert wurde. Wenn es nicht geändert wurde, antwortet der Server mit &quot;304 - nicht geändert&quot; und nur die Metadaten wurden übertragen.

#### Debugging

Wenn Sie Ihre Dispatcher-Einstellungen für die Browserzwischenspeicherung optimieren, ist es äußerst nützlich, einen Desktop-Proxy-Server zwischen Ihrem Browser und dem Webserver zu verwenden. Wir bevorzugen &quot;Charles Web Debugging Proxy&quot; von Karl von Randow.

Mithilfe von Charles können Sie die Anforderungen und Antworten lesen, die an und vom Server übertragen werden. Und - Sie können viel über das HTTP-Protokoll lernen. Moderne Browser bieten auch Debugging-Funktionen, aber die Funktionen eines Desktop-Proxys sind noch nie da gewesene. Sie können die übertragenen Daten bearbeiten, die Übertragung beschränken, einzelne Anforderungen wiederholen und vieles mehr. Die Benutzeroberfläche ist übersichtlich und sehr umfassend.

Der grundlegendste Test besteht darin, die Website als normalen Benutzer zu verwenden - wobei der Proxy dazwischen liegt - und im Proxy zu überprüfen, ob die Anzahl der statischen Anforderungen (an /etc/...) im Laufe der Zeit kleiner wird, da diese im Cache gespeichert und nicht mehr angefordert werden sollten.

Wir haben festgestellt, dass ein Proxy einen klareren Überblick geben kann, da zwischengespeicherte Anforderungen nicht im Protokoll angezeigt werden, während einige in Browsern integrierte Debugger diese Anforderungen immer noch mit &quot;0 ms&quot;oder &quot;from disk&quot;anzeigen. Das ist in Ordnung und genau, könnte aber Ihre Ansicht etwas verdecken.

Anschließend können Sie einen Drilldown durchführen und die Header der übertragenen Dateien überprüfen, um beispielsweise zu sehen, ob die HTTP-Header &quot;Expires&quot;korrekt sind. Sie können Anfragen mit if-modified-Since-Headern wiedergeben, um zu sehen, ob der Server ordnungsgemäß mit einem Antwort-Code 304 oder 200 reagiert. Sie können den Zeitpunkt asynchroner Aufrufe beobachten und Ihre Sicherheitsannahmen auch in einem bestimmten Umfang testen. Denken Sie daran, dass wir Ihnen gesagt haben, nicht alle Selektoren zu akzeptieren, die nicht explizit erwartet werden? Hier können Sie mit der URL und den Parametern spielen und sehen, ob sich Ihre Anwendung gut verhält.

Es gibt nur eine Sache, die Sie beim Debugging Ihres Caches nicht tun sollten:

Laden Sie keine Seiten im Browser neu!

Ein &quot;Browser-Neuladung&quot;, ein _simple-reload_ sowie ein _forced-reload_ (&quot;_shift-reload_&quot;) ist nicht dasselbe wie eine normale Seitenanforderung. Eine einfache Neuladungsanforderung setzt einen Header

```
Cache-Control: max-age=0
```

Bei einer Umschalt-Umschalt-Umschalt-Umschalt-Umschalt-Umschalt-Umschalt-Umschalt-Umschalt-Taste (bei Klick auf die Schaltfläche zum Neuladen gedrückt halten) wird in der Regel ein Anforderungsheader festgelegt

```
Cache-Control: no-cache
```

Beide Kopfzeilen haben ähnliche, aber etwas unterschiedliche Auswirkungen. Vor allem unterscheiden sie sich jedoch vollständig von einer normalen Anforderung, wenn Sie eine URL aus dem URL-Slot öffnen oder Links auf der Site verwenden. Das normale Browsen setzt keine Cache-Control-Header, sondern wahrscheinlich einen if-modified-da-Header.

Wenn Sie also das normale Browsing-Verhalten debuggen möchten, sollten Sie genau das tun: _Browse normal_. Die Verwendung der Schaltfläche zum Neuladen Ihres Browsers ist die beste Möglichkeit, in Ihrer Konfiguration keine Cache-Konfigurationsfehler zu sehen.

Verwenden Sie Ihren Charles-Proxy, um zu sehen, wovon wir sprechen. Ja - und während Sie es geöffnet haben - können Sie die Anfragen direkt dort wiedergeben. Es ist nicht erforderlich, den Browser neu zu laden.

## Leistungstests

Durch die Verwendung eines Proxy erhalten Sie einen Eindruck vom Timing-Verhalten Ihrer Seiten. Das ist natürlich kein Leistungstest.  Für einen Leistungstest ist es erforderlich, dass mehrere Clients Ihre Seiten parallel anfordern.

Ein häufiger Fehler, den wir zu oft gesehen haben, besteht darin, dass der Leistungstest nur eine sehr geringe Anzahl von Seiten enthält und diese Seiten nur aus dem Dispatcher-Cache bereitgestellt werden.

Wenn Sie Ihre Anwendung auf das Live-System weiterleiten, unterscheidet sich die Last vollständig von der, die Sie getestet haben.

Auf dem Live-System ist das Zugriffsmuster nicht die geringe Anzahl gleich verteilter Seiten, die Sie in Tests haben (Startseite und wenige Inhaltsseiten). Die Anzahl der Seiten ist viel größer und die Anforderungen sind sehr ungleichmäßig verteilt. Und natürlich können Live-Seiten nicht zu 100 % aus dem Cache bereitgestellt werden: Es gibt Invalidierungsanfragen vom Veröffentlichungssystem, die einen großen Teil Ihrer wertvollen Ressourcen automatisch ungültig machen.

Ah ja - und wenn Sie Ihren Dispatcher-Cache neu erstellen, werden Sie feststellen, dass sich das Veröffentlichungssystem auch ganz anders verhält, je nachdem, ob Sie nur eine Handvoll Seiten oder eine größere Zahl anfordern. Auch wenn alle Seiten ähnlich komplex sind, spielt ihre Anzahl eine Rolle. Erinnern Sie sich, was wir über das Caching verkettet haben? Wenn Sie immer die gleiche kleine Anzahl von Seiten anfordern, ist die Wahrscheinlichkeit gut, dass die entsprechenden Blöcke mit den Rohdaten sich im Festplattencache befinden oder die Blöcke vom Betriebssystem zwischengespeichert werden. Außerdem besteht eine gute Chance, dass das Repository das entsprechende Segment im Hauptspeicher zwischengespeichert hat. Das erneute Rendern ist also wesentlich schneller als wenn andere Seiten jetzt und dann aus verschiedenen Caches entfernt werden.

Das Caching ist schwierig, und auch das Testen eines Systems, das auf dem Caching beruht. Was können Sie also tun, um ein genaueres Echtzeit-Szenario zu haben?

Wir glauben, dass Sie mehr als einen Test durchführen müssen und mehr als einen Leistungsindex als Messwert für die Qualität Ihrer Lösung bereitstellen müssen.

Wenn Sie bereits über eine bestehende Website verfügen, messen Sie die Anzahl der Anfragen und deren Verteilung. Versuchen Sie, einen Test zu modellieren, der eine ähnliche Verteilung von Anforderungen verwendet. Das Hinzufügen einer Zufälligkeit konnte nicht schaden. Sie müssen keinen Browser simulieren, der statische Ressourcen wie JS und CSS lädt - das ist eigentlich egal. Sie werden schließlich im Browser oder im Dispatcher zwischengespeichert und ergeben keine signifikanten Auswirkungen auf die Auslastung. Aber referenzierte Bilder sind wichtig. Suchen Sie auch deren Verteilung in alten Protokolldateien und modellieren Sie ein ähnliches Anforderungsmuster.

Führen Sie jetzt einen Test durch, bei dem Ihr Dispatcher überhaupt nicht zwischenspeichert. Das ist dein schlimmstes Szenario. Finden Sie heraus, welche Spitzenlast Ihr System unter diesen schlechtesten Bedingungen instabil wird. Sie können es auch verschlimmern, indem Sie bei Bedarf einige Dispatcher-/Publish-Beine herausnehmen.

Führen Sie als Nächstes denselben Test mit allen erforderlichen Cache-Einstellungen durch, um &quot;ein&quot;zu aktivieren. Führen Sie die parallelen Anfragen langsam aus, um den Cache zu warnen und zu sehen, wie viel Ihr System unter diesen optimalen Bedingungen verbrauchen kann.

Ein durchschnittliches Szenario wäre, den Test mit aktiviertem Dispatcher auszuführen, aber auch mit einigen Invalidierungen. Sie können dies simulieren, indem Sie die Statfiles durch einen Cronjob berühren oder die Invalidierungsanforderungen in unregelmäßigen Abständen an den Dispatcher senden. Vergessen Sie nicht, ab und zu auch einige der nicht automatisch invalidierten Ressourcen zu bereinigen.

Sie können das letzte Szenario variieren, indem Sie die Invalidierungsanforderungen erhöhen und die Last erhöhen.

Das ist etwas komplexer als nur ein linearen Belastungstest - aber gibt viel mehr Vertrauen in Ihre Lösung.

Du könntest vor den Bemühungen zurückschrecken. Führen Sie jedoch einen Worst-Case-Test auf dem Publish-System mit einer größeren Anzahl von Seiten (gleichmäßig verteilt) durch, um die Grenzen des Systems zu sehen. Stellen Sie sicher, dass Sie die Nummer des Best-Case-Szenarios richtig interpretieren und Ihren Systemen genügend Kopfraum zur Verfügung stellen.