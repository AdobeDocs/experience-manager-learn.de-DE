---
title: '"Kapitel 2 - Dispatcher-Infrastruktur"'
description: Machen Sie sich mit der Veröffentlichungs- und Dispatcher-Topologie vertraut. Erfahren Sie mehr über die gängigsten Topologien und Setups.
feature: Dispatcher
topic: Architektur
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1866'
ht-degree: 0%

---


# Kapitel 2 - Infrastruktur

## Einrichten einer Caching-Infrastruktur

In Kapitel 1 dieser Reihe haben wir die grundlegende Topologie eines Veröffentlichungssystems und eines Dispatchers eingeführt. Eine Reihe von Veröffentlichungs- und Dispatcher-Servern kann in vielen Varianten konfiguriert werden - je nach erwarteter Auslastung, der Topologie Ihrer Rechenzentren und den gewünschten Failover-Eigenschaften.

Wir werden die gängigsten Topologien skizzieren und die Vorteile beschreiben und beschreiben, wo sie fehlen. Die Liste - natürlich - kann nie umfassend sein. Die einzige Grenze ist deine Fantasie.

### Die &quot;alte&quot;Einrichtung

In den ersten Tagen war die Anzahl der potenziellen Besucher gering, Hardware war teuer und Webserver wurden nicht als geschäftskritisch angesehen wie heute. Eine gängige Einrichtung bestand darin, einen Dispatcher als Lastenausgleich und Cache vor zwei oder mehr Veröffentlichungssystemen zu verwenden. Der Apache-Server im Kern des Dispatchers war sehr stabil und - in den meisten Einstellungen - in der Lage, eine angemessene Anzahl von Anforderungen zu erfüllen.

![&quot;Legacy&quot;-Dispatcher-Einrichtung - Nicht sehr häufig nach heutigen Standards](assets/chapter-2/legacy-dispatcher-setup.png)

*&quot;Legacy&quot;-Dispatcher-Einrichtung - Nicht sehr häufig nach heutigen Standards*

<br> 

Hier erhielt der Dispatcher seinen Namen von: Es war im Grunde, Anfragen zu senden. Diese Einrichtung ist nicht mehr allzu häufig, da sie die heute erforderlichen höheren Leistungs- und Stabilitätsanforderungen nicht mehr erfüllen kann.

### Mehrseitige Einrichtung

Heutzutage ist eine etwas andere Topologie häufiger. Eine Topologie mit mehreren Seiten würde einen Dispatcher pro Veröffentlichungsserver haben. Ein dedizierter (Hardware-)Lastenausgleich befindet sich vor der AEM Infrastruktur, die die Anforderungen an diese beiden (oder mehr) Beine sendet:

![Modernes &quot;Standard&quot;-Dispatcher-Setup - einfach zu handhaben und zu warten](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Modernes &quot;Standard&quot;-Dispatcher-Setup - einfach zu handhaben und zu warten*

<br> 

Dies sind die Gründe für diese Art von Einrichtung,

1. Im Durchschnitt liefern Websites viel mehr Traffic als bisher. Daher muss die &quot;Apache-Infrastruktur&quot;ausgebaut werden.

2. Die &quot;Legacy&quot;-Einrichtung führte nicht zu Redundanz auf Dispatcher-Ebene. Wenn der Apache-Server ausfällt, war die gesamte Website nicht erreichbar.

3. Apache-Server sind billig. Sie basieren auf Open Source und können, da Sie über ein virtuelles Rechenzentrum verfügen, sehr schnell bereitgestellt werden.

4. Dieses Setup bietet eine einfache Möglichkeit für ein &quot;rollierendes&quot;oder &quot;gestaffeltes&quot;Aktualisierungsszenario. Sie müssen den Dispatcher 1 einfach herunterfahren, während Sie ein neues Softwarepaket auf Publish 1 installieren. Wenn die Installation abgeschlossen ist und Sie ausreichend rauchgetestete Veröffentlichung 1 aus dem internen Netzwerk durchgeführt haben, löschen Sie den Cache auf Dispatcher 1 und starten ihn neu, während Sie Dispatcher 2 zur Wartung von Publish 2 herunterfahren.

5. Die Cache-Invalidierung wird in diesem Setup sehr einfach und deterministisch. Da nur ein Veröffentlichungssystem mit einem Dispatcher verbunden ist, kann nur ein Dispatcher ungültig gemacht werden. Die Reihenfolge und der Zeitpunkt der Invalidierung sind trivial.

### Die Einrichtung &quot;Ausskalieren&quot;

Apache-Server sind billig und leicht bereitzustellen, warum nicht die Skalierung dieser Ebene ein wenig mehr. Warum stehen nicht zwei oder mehr Dispatcher vor jedem Veröffentlichungsserver?

![Einrichtung &quot;Ausskalieren&quot;- Hat einige Anwendungsbereiche, aber auch Einschränkungen und Einschränkungen](assets/chapter-2/scale-out-setup.png)

*Einrichtung &quot;Ausskalieren&quot;- Hat einige Anwendungsbereiche, aber auch Einschränkungen und Einschränkungen*

<br> 

Das kannst du unbedingt tun! Und es gibt viele gültige Anwendungsszenarien für dieses Setup. Es gibt aber auch einige Einschränkungen und Komplexititäten, die Sie beachten sollten.

#### Invalidierung

Jedes Veröffentlichungssystem ist mit einer Vielzahl von Dispatchern verbunden. Jeder Dispatcher muss invalidiert werden, wenn der Inhalt geändert wurde.

#### Wartung

Es versteht sich von selbst, dass die anfängliche Konfiguration der Dispatcher- und Veröffentlichungssysteme etwas komplexer ist. Beachten Sie aber auch, dass die Anstrengung einer &quot;rollierenden&quot; Version ebenfalls etwas höher ist. AEM Systeme können und müssen während der Ausführung aktualisiert werden. Es ist jedoch klug, dies nicht zu tun, während sie Anfragen aktiv bearbeiten. Normalerweise möchten Sie nur einen Teil der Veröffentlichungssysteme aktualisieren, während die anderen weiterhin aktiv Traffic bereitstellen und dann - nach dem Testen - zum anderen Teil wechseln. Wenn Sie Glück haben und auf den Lastenausgleich in Ihrem Bereitstellungsprozess zugreifen können, können Sie hier das Routing zu den Servern, die gewartet werden, deaktivieren. Wenn Sie einen freigegebenen Lastenausgleich ohne direkten Zugriff verwenden, sollten Sie die Dispatcher der Veröffentlichung, die Sie aktualisieren möchten, lieber herunterfahren. Je mehr es gibt, desto mehr muss man schließen. Wenn es eine große Anzahl von Updates gibt und Sie häufige Aktualisierungen planen, wird eine gewisse Automatisierung empfohlen. Wenn Sie keine Automatisierungstools haben, ist die Ausskalierung ohnehin eine schlechte Idee.

In einem früheren Projekt haben wir einen anderen Trick verwendet, um ein Veröffentlichungssystem aus dem Lastenausgleich zu entfernen, ohne direkten Zugriff auf den Lastenausgleich selbst zu haben.

Der Lastenausgleich &quot;pingt&quot; normalerweise, eine bestimmte Seite, um zu sehen, ob der Server betriebsbereit ist. Eine triviale Wahl besteht normalerweise darin, die Homepage zu pingen. Wenn Sie jedoch den Ping verwenden möchten, um dem Lastenausgleich zu signalisieren, dass er Traffic nicht ausgleicht, würden Sie etwas Anderes wählen. Sie erstellen eine dedizierte Vorlage oder ein Servlet, die bzw. das so konfiguriert werden kann, dass sie mit `"up"` oder `"down"` antwortet (im Hauptteil oder als HTTP-Antwortcode). Die Antwort dieser Seite darf natürlich nicht im Dispatcher zwischengespeichert werden. Daher wird sie immer neu aus dem Veröffentlichungssystem abgerufen. Wenn Sie nun den Lastenausgleich so konfigurieren, dass diese Vorlage oder dieses Servlet überprüft wird, können Sie die Veröffentlichungsinstanz einfach so einstellen, dass sie ausfällt. Sie wäre nicht Teil des Lastenausgleichs und kann aktualisiert werden.

#### Weltweite Distribution

&quot;Worldwide Distribution&quot;ist ein &quot;Scale-out&quot;-Setup, bei dem mehrere Dispatcher vor jedem Publish-System vorhanden sind - das jetzt auf der ganzen Welt verteilt ist, um näher an den Kunden zu sein und eine bessere Leistung zu erzielen. Natürlich gibt es in diesem Szenario keinen zentralen Lastenausgleich, sondern ein DNS- und Geo-IP-basiertes Lastenausgleichssystem.

>[!NOTE]
>
>Tatsächlich erstellen Sie mit diesem Ansatz eine Art Content Distribution Network (CDN). Daher sollten Sie erwägen, eine standardmäßige CDN-Lösung zu kaufen, anstatt eine selbst zu erstellen. Das Erstellen und Verwalten eines benutzerdefinierten CDN ist keine triviale Aufgabe.

#### Horizontale Skalierung

Selbst in einem lokalen Rechenzentrum bietet eine Topologie vom Typ &quot;Ausskalieren&quot;, die mehrere Dispatcher vor jedem Veröffentlichungssystem einige Vorteile bietet. Wenn Sie Leistungsengpässe auf den Apache-Servern aufgrund hohen Traffics (und einer guten Cache-Trefferrate) feststellen und die Hardware nicht mehr skalieren können (durch Hinzufügen von CPUs, RAM und schnelleren Festplatten), können Sie die Leistung steigern, indem Sie Dispatcher hinzufügen. Dies wird als &quot;horizontale Skalierung&quot;bezeichnet. Dies hat jedoch Einschränkungen - insbesondere wenn Sie Traffic häufig invalidieren. Wir werden die Wirkung im nächsten Abschnitt beschreiben.

#### Beschränkungen der Topologie &quot;Ausskalieren&quot;

Das Hinzufügen von Proxy-Servern sollte normalerweise die Leistung steigern. Es gibt jedoch Szenarien, in denen das Hinzufügen von Servern die Leistung tatsächlich verringern kann. Wie? Angenommen, Sie haben ein Nachrichtenportal, in dem Sie jede Minute neue Artikel und Seiten vorstellen. Ein Dispatcher invalidiert durch &quot;automatische Invalidierung&quot;: Bei jeder Veröffentlichung einer Seite werden alle Seiten im Cache auf derselben Site invalidiert. Dies ist eine nützliche Funktion - wir haben dies in [Kapitel 1](chapter-1.md) dieser Reihe behandelt - aber es bedeutet auch, dass Sie, wenn Sie häufige Änderungen auf Ihrer Website haben, den Cache ziemlich oft invalidieren. Wenn pro Veröffentlichungsinstanz nur ein Dispatcher vorhanden ist und der erste Besucher eine Seite anfordert, wird eine erneute Zwischenspeicherung dieser Seite Trigger. Der zweite Besucher erhält bereits die zwischengespeicherte Version.

Wenn Sie über zwei Dispatcher verfügen, besteht die Wahrscheinlichkeit, dass die Seite nicht zwischengespeichert wird, bei 50 % des zweiten Besuchers, sodass bei der erneuten Wiedergabe dieser Seite eine höhere Latenz auftritt. Noch mehr Dispatcher pro Veröffentlichung machen die Dinge noch schlimmer. Der Veröffentlichungsserver erhält mehr Last, da er die Seite für jeden Dispatcher separat neu rendern muss.

![Verringerte Leistung bei einem Szenario mit Ausskalierung mit häufigen Cache-Leerungen.](assets/chapter-2/decreased-performance.png)

*Verringerte Leistung bei einem Szenario mit Ausskalierung mit häufigen Cache-Leerungen.*

<br> 

#### Beheben von Problemen mit zu großer Skalierung

Sie können erwägen, einen zentralen gemeinsamen Speicher für alle Dispatcher zu verwenden oder die Dateisysteme der Apache-Server zu synchronisieren, um Probleme zu beheben. Wir können nur begrenzte Erfahrungen aus erster Hand bereitstellen, sind aber darauf vorbereitet, dass dies die Komplexität des Systems erhöht und eine neue Fehlerklasse einführen kann.

Wir haben einige Experimente mit NFS durchgeführt - aber NFS führt aufgrund der Inhaltssperrung enorme Leistungsprobleme ein. Dadurch wurde die Gesamtleistung tatsächlich reduziert.

**Fazit** : Die Freigabe eines gemeinsamen Dateisystems unter mehreren Dispatchern ist NICHT empfehlenswert.

Wenn Leistungsprobleme auftreten, skalieren Sie Publish und Dispatcher gleichermaßen an, um eine Spitzenlast der Publisher-Instanzen zu vermeiden. Es gibt keine goldene Regel zum Verhältnis Veröffentlichen/Dispatcher - es hängt in hohem Maße von der Verteilung der Anforderungen und der Häufigkeit von Veröffentlichungen und Cache-Invalidierungen ab.

Wenn Sie auch Bedenken hinsichtlich der Latenz haben, mit der ein Besucher konfrontiert ist, sollten Sie ein Netzwerk zur Inhaltsbereitstellung, eine erneute Zwischenspeicherung und präventive Cache-Erwärmung verwenden, eine Übergangsphase festlegen, wie in [Kapitel 1](chapter-1.md) dieser Serie beschrieben, oder auf einige erweiterte Ideen von [Teil 3](chapter-3.md) verweisen.

### Die Einrichtung &quot;Cross Connected&quot;

Ein weiteres Setup, das wir jetzt und da gesehen haben, ist das &quot;vernetzte&quot; Setup: Die Veröffentlichungsinstanzen verfügen nicht über dedizierte Dispatcher, aber alle Dispatcher sind mit allen Veröffentlichungssystemen verbunden.

![Vernetzte Topologie: Erhöhte Redundanz und Komplexität](assets/chapter-2/cross-connected-setup.png)

<br> 

*Vernetzte Topologie: Höhere Redundanz und mehr Komplexität.*

Auf den ersten Blick bietet dies etwas mehr Redundanz für einen relativ kleinen Haushalt. Wenn einer der Apache-Server ausfällt, können Sie immer noch zwei Veröffentlichungssysteme haben, die die Wiedergabe durchführen. Wenn eines der Veröffentlichungssysteme abstürzt, verfügen Sie weiterhin über zwei Dispatcher, die die zwischengespeicherte Last abdecken.

Dies ist jedoch mit einem Preis verbunden.

Erstens ist es recht schwerfällig, ein Bein für die Instandhaltung zu nehmen. Genau darauf wurde diese Regelung ausgerichtet. um robuster zu sein und mit allen Mitteln am Laufen zu bleiben. Wir haben komplizierte Wartungspläne gesehen, wie wir damit umgehen. Konfigurieren Sie zuerst den Dispatcher 2 neu und entfernen Sie die Querverbindung. Neustart des Dispatchers 2. Herunterfahren von Dispatcher 1, Aktualisieren von Publish 1, ... usw. Du solltest sorgfältig überlegen, ob das auf mehr als zwei Beine skaliert. Sie werden zu dem Schluss kommen, dass es tatsächlich die Komplexität und die Kosten erhöht und eine gewaltige Quelle menschlichen Fehlers darstellt. Es wäre am besten, dies zu automatisieren. Überprüfen Sie also besser, ob Sie tatsächlich über die nötigen Humanressourcen verfügen, um diese Automatisierungsaufgabe in Ihren Projektplan aufzunehmen. So sparen Sie möglicherweise Hardware-Kosten, aber Sie können damit doppelt so viel für IT-Mitarbeiter ausgeben.

Außerdem kann es vorkommen, dass auf dem AEM einige Benutzeranwendungen ausgeführt werden, für die eine Anmeldung erforderlich ist. Sie verwenden fixierbare Sitzungen, um sicherzustellen, dass ein Benutzer immer von derselben AEM bereitgestellt wird, damit Sie den Sitzungsstatus in dieser Instanz beibehalten können. Wenn diese vernetzte Einrichtung vorhanden ist, müssen Sie sicherstellen, dass Sticky-Sitzungen auf dem Load-Balancer und den Dispatchern ordnungsgemäß funktionieren. Nicht unmöglich - aber Sie müssen sich dessen bewusst sein und zusätzliche Konfigurations- und Testzeiten hinzufügen, die - wieder - die Einsparungen, die Sie geplant hatten, durch die Einsparung von Hardware erhöhen könnten.

### Zusammenfassung

Wir empfehlen nicht, dieses Verbindungs-Schema als Standardoption zu verwenden. Wenn Sie sich jedoch für die Verwendung entscheiden, sollten Sie Risiken und versteckte Kosten sorgfältig bewerten und planen, die Konfigurationsautomatisierung in Ihr Projekt einzubeziehen.

## Nächster Schritt

* [3 - Fortgeschrittene Zwischenspeicherungsthemen](chapter-3.md)
