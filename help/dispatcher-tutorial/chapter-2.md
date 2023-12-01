---
title: „Kapitel 2 – Dispatcher-Infrastruktur“
description: Machen Sie sich mit der Veröffentlichungs- und Dispatcher-Topologie vertraut. Erfahren Sie mehr über die häufigsten Topologien und Setups.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
doc-type: Tutorial
exl-id: a25b6f74-3686-40a9-a148-4dcafeda032f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1864'
ht-degree: 100%

---

# Kapitel 2 – Infrastruktur

## Einrichten einer Caching-Infrastruktur

In Kapitel 1 dieser Reihe haben wir die grundlegende Topologie eines Veröffentlichungssystems und Dispatchers vorgestellt. Eine Gruppe von Veröffentlichungs- und Dispatcher-Servern kann abhängig von der erwarteten Auslastung, der Topologie Ihrer Rechenzentren und den gewünschten Failover-Eigenschaften auf vielfältige Weise konfiguriert werden.

Wir werden die gängigsten Topologien skizzieren und ihre Vor- und Nachteile beschreiben. Diese Aufstellung kann – natürlich – niemals allumfassend sein. Der Fantasie sind keine Grenzen gesetzt.

### „Legacy“-Setup

Zu Anfang war die Anzahl der potenziellen Besuchenden gering, Hardware war teuer und Webserver wurden nicht als so geschäftskritisch angesehen wie heute. Häufig wurde sich für ein Setup mit einem Dispatcher als Load-Balancer und einem Cache vor zwei oder mehr Veröffentlichungssystemen entschieden. Der Apache-Server im Zentrum des Dispatchers war sehr stabil und konnte – in den meisten Fällen – eine angemessene Anzahl von Anfragen bewältigen.

![„Legacy“-Setup eines Dispatchers – nach heutigen Standards eher unüblich](assets/chapter-2/legacy-dispatcher-setup.png)

*„Legacy“-Setup eines Dispatchers – nach heutigen Standards eher unüblich*

<br> 

Hierauf ist übrigens der Begriff „Dispatcher“ zurückzuführen. Denn „dispatch“ bedeutet in diesem Zusammenhang, dass Anfragen versendet, abgefertigt, disponiert werden. Dieses Setup ist nicht mehr allzu häufig anzutreffen, da es die heute gestellten höheren Leistungs- und Stabilitätsanforderungen nicht mehr erfüllt.

### Setup mit mehreren Abzweigungen

Heutzutage wird häufiger eine etwas andere Topologie eingesetzt. Eine Topologie mit mehreren Abzweigungen („Legs“) weist einen Dispatcher pro Veröffentlichungs-Server auf. Ein dedizierter (Hardware-)Load-Balancer befindet sich vor der AEM-Infrastruktur und sendet die Anfragen an diese beiden Abzweigungen (es können auch mehr sein):

![Modernes „Standard“-Setup eines Dispatchers – einfach zu handhaben und zu warten](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Modernes „Standard“-Setup eines Dispatchers – einfach zu handhaben und zu warten*

<br> 

Für diese Art des Setups sprechen folgende Gründe:

1. Im Durchschnitt sorgen Websites heute für viel mehr Traffic als früher. Daher muss die „Apache-Infrastruktur“ ausgebaut werden.

2. Das „Legacy“-Setup sah keine Redundanz auf Dispatcher-Ebene vor. Bei einem Ausfall des Apache-Servers war daher die gesamte Website unerreichbar.

3. Apache-Server sind kostengünstig. Sie haben eine Open-Source-Basis und können, sofern ein virtuelles Rechenzentrum vorhanden ist, sehr schnell bereitgestellt werden.

4. Dieses Setup bietet eine einfache Möglichkeit für ein „rollierendes“ oder „gestaffeltes“ Aktualisierungsszenario. Sie müssen Dispatcher 1 einfach herunterfahren, während Sie ein neues Software-Paket auf Veröffentlichungssystem 1 installieren. Wenn die Installation abgeschlossen ist und Sie Veröffentlichungssystem 1 einer ausreichenden Feuerprobe über das interne Netzwerk unterzogen haben, bereinigen Sie den Cache von Dispatcher 1 und starten Sie ihn neu, während Sie Dispatcher 2 zur Wartung von Veröffentlichungssystem 2 herunterfahren.

5. Die Cache-Invalidierung ist in diesem Setup sehr einfach und deterministisch. Da nur ein Veröffentlichungssystem mit einem Dispatcher verbunden ist, muss nur ein Dispatcher invalidiert werden. Reihenfolge und Zeitpunkt der Invalidierung sind dabei unbedeutend.

### „Scale-Out“-Setup

Apache-Server bereitzustellen, ist kostengünstig und einfach. Warum diese Ebene also nicht ein wenig mehr ausbauen? Warum jedem Veröffentlichungs-Server nicht zwei oder mehr Dispatcher vorschalten?

![„Scale-Out“-Setup – mit einigen Anwendungsbereichen, aber auch Einschränkungen und Komplexitäten](assets/chapter-2/scale-out-setup.png)

*„Scale-Out“-Setup – mit einigen Anwendungsbereichen, aber auch Einschränkungen und Komplexitäten*

<br> 

Das ist auch für Sie absolut möglich. Und es gibt viele gültige Anwendungsszenarien für dieses Setup. Es müssen aber auch gewisse Einschränkungen und Komplexititäten beachtet werden.

#### Invalidierung

Jedes Veröffentlichungssystem ist mit einer Vielzahl von Dispatchern verbunden. Jeder dieser Dispatcher muss bei einer Inhaltsänderung invalidiert werden.

#### Wartung

Es versteht sich von selbst, dass die anfängliche Konfiguration der Dispatcher- und Veröffentlichungssysteme etwas komplexer ist. Beachten Sie aber auch, dass die Anstrengung einer „rollierenden“ Version ebenfalls etwas höher ist. AEM-Systeme können und müssen während der Ausführung aktualisiert werden. Es ist jedoch ratsam, dies nicht zu tun, während sie aktiv Anfragen bearbeiten. Normalerweise möchten Sie nur einen Teil der Veröffentlichungssysteme aktualisieren, während die anderen weiterhin aktiv Traffic bereitstellen und dann – nach dem Testen – zum anderen Teil wechseln. Wenn Sie Glück haben und auf den Lastenausgleich in Ihrem Bereitstellungsprozess zugreifen können, können Sie hier das Routing zu den Servern, die gewartet werden, deaktivieren. Wenn Sie sich auf einem freigegebenen Lastenausgleich ohne direkten Zugriff befinden, sollten Sie die Dispatcher der Veröffentlichung, die Sie aktualisieren möchten, lieber herunterfahren. Je mehr es sind, desto mehr müssen Sie herunterfahren. Wenn es sich um eine große Anzahl handelt und Sie häufige Updates planen, ist eine gewisse Automatisierung ratsam. Wenn Sie keine Automatisierungs-Tools haben, ist das Skalieren sowieso eine schlechte Idee.

In einem früheren Projekt haben wir einen anderen Trick verwendet, um ein Veröffentlichungssystem aus dem Lastenausgleich zu entfernen, ohne direkten Zugriff auf den Lastenausgleich selbst zu haben.

Der Lastenausgleich „pingt“ normalerweise eine bestimmte Seite an, um weitere Informationen zu finden, ob der Server betriebsbereit ist. Eine triviale Wahl besteht normalerweise darin, die Homepage zu pingen. Wenn Sie jedoch den Ping verwenden wollen, um dem Lastausgleich zu signalisieren, dass er den Datenverkehr nicht ausgleichen soll, sollten Sie etwas anderes wählen. Sie erstellen eine dedizierte Vorlage oder ein Servlet, das so konfiguriert werden kann, dass es mit `"up"` oder `"down"` (im Body oder als HTTP-Antwort-Code) antwortet. Die Antwort dieser Seite darf natürlich nicht im Dispatcher zwischengespeichert werden – sie wird also immer frisch aus dem Veröffentlichungssystem geholt. Wenn Sie nun den Lastenausgleicher so konfigurieren, dass er diese Vorlage oder dieses Servlet überprüft, können Sie den Publish einfach so aussehen lassen, als sei er ausgefallen. Er wäre nicht Teil des Lastenausgleichs und kann aktualisiert werden.

#### Worldwide Distribution

„Worldwide Distribution“ ist eine „Scale-out“-Einrichtung, bei dem mehrere Dispatcher vor jedem Veröffentlichungssystem vorhanden sind, das jetzt auf der ganzen Welt verteilt ist, um näher bei der Kundschaft zu sein und eine bessere Leistung zu erzielen. Natürlich gibt es in diesem Szenario keinen zentralen Lastenausgleich, sondern ein DNS- und Geo-IP-basiertes Lastenausgleichssystem.

>[!NOTE]
>
>Tatsächlich erstellen Sie mit diesem Ansatz eine Art Content Distribution Network (CDN). Daher sollten Sie erwägen, eine standardmäßige CDN-Lösung zu kaufen, anstatt selbst eine zu erstellen. Das Erstellen und Verwalten eines benutzerdefinierten CDN ist keine triviale Aufgabe.

#### Horizontale Skalierung

Selbst in einem lokalen Rechenzentrum hat eine Topologie vom Typ „Scale Out“ mit mehreren Dispatchern vor jedem Veröffentlichungssystem einige Vorteile. Wenn Sie Leistungsengpässe auf den Apache-Servern aufgrund hohen Traffics (und einer guten Cache-Trefferrate) feststellen und die Hardware nicht mehr skalieren können (durch Hinzufügen von CPUs, RAM und schnelleren Festplatten), können Sie die Leistung steigern, indem Sie Dispatcher hinzufügen. Dies wird als „horizontale Skalierung“ bezeichnet. Dies hat jedoch seine Grenzen – vor allem, wenn der Traffic häufig ungültig gemacht wird. Wir werden die Auswirkungen im nächsten Abschnitt beschreiben.

#### Beschränkungen der Topologie „Scale Out“

Das Hinzufügen von Proxy-Servern sollte normalerweise die Leistung steigern. Es gibt jedoch Szenarien, in denen das Hinzufügen von Servern die Leistung tatsächlich verringern kann. Wie? Angenommen, Sie haben ein Nachrichtenportal, in dem Sie jede Minute neue Artikel und Seiten vorstellen. Ein Dispatcher wird durch „Auto-Invalidierung“ ungültig gemacht: Jedes Mal, wenn eine Seite veröffentlicht wird, werden alle Seiten im Cache auf derselben Site für ungültig erklärt. Dies ist eine nützliche Funktion – wir haben dies in [Kapitel 1](chapter-1.md) dieser Serie behandelt – aber es bedeutet auch, dass Sie bei häufigen Änderungen an Ihrer Website den Cache ziemlich oft ungültig machen. Wenn pro Veröffentlichungsinstanz nur ein Dispatcher vorhanden ist und beim ersten Besuch eine Seite angefordert wird, wird eine erneute Zwischenspeicherung dieser Seite ausgelöst. Die zweite Person, die die Seite besucht, erhält bereits die zwischengespeicherte Version.

Wenn Sie über zwei Dispatcher verfügen, besteht die Wahrscheinlichkeit, dass die Seite nicht zwischengespeichert wird, bei 50 % der zweiten Besucherin bzw. des zweiten Besuchers, sodass bei der erneuten Wiedergabe dieser Seite eine höhere Latenz auftritt. Mit noch mehr Dispatchern pro Veröffentlichung wird die Situation noch schlimmer. Der Veröffentlichungs-Server erhält mehr Last, da er die Seite für jeden Dispatcher separat neu rendern muss.

![Verringerte Leistung bei einem Szenario mit „scale-out“ und häufigen Cache-Leerungen.](assets/chapter-2/decreased-performance.png)

*Verringerte Leistung bei einem Szenario mit „scale-out“ und häufigen Cache-Leerungen.*

<br> 

#### Umgehen von Problemen mit zu großer Skalierung

Sie können erwägen, einen zentralen gemeinsamen Speicher für alle Dispatcher zu verwenden oder die Dateisysteme der Apache-Server zu synchronisieren, um Probleme zu umgehen. Wir können nur begrenzte Erfahrungen aus erster Hand bieten, aber seien Sie darauf vorbereitet, dass dies die Komplexität des Systems erhöht und eine ganz neue Klasse von Fehlern einführen kann.

Wir haben einige Experimente mit NFS durchgeführt, aber NFS führt aufgrund der Inhaltssperrung enorme Leistungsprobleme ein. Dadurch wurde die Gesamtleistung tatsächlich reduziert.

**Schlussfolgerung**: Die Freigabe eines gemeinsamen Dateisystems unter mehreren Dispatchern ist KEIN empfohlener Ansatz.

Wenn Leistungsprobleme auftreten, skalieren Sie Publish und Dispatcher gleichermaßen, um eine Spitzenlast der Publisher-Instanzen zu vermeiden. Es gibt keine goldene Regel zum Verhältnis Publish/Dispatcher, es hängt in hohem Maße von der Verteilung der Anfragen und der Häufigkeit von Veröffentlichungen und Cache-Invalidierungen ab.

Wenn Sie auch Bedenken hinsichtlich der Latenz haben, mit der Besuchende konfrontiert sind, sollten Sie die Verwendung eines Content Delivery Network, das erneute Abrufen des Caches, die präventive Erwärmung des Caches, die Festlegung einer Schonzeit, wie in [Kapitel 1](chapter-1.md) dieser Serie beschrieben, oder einige fortgeschrittene Ideen aus [Teil 3](chapter-3.md) in Betracht ziehen.

### Die Einrichtung „Cross Connected“

Eine weitere Einrichtung, die wir hin und wieder gefunden haben, ist die Einrichtung „cross connected“: Die Veröffentlichungsinstanzen haben keine eigenen Dispatcher, aber alle Dispatcher sind mit allen Veröffentlichungssystemen verbunden.

![Vernetzte Topologie: Erhöhte Redundanz und mehr Komplexität](assets/chapter-2/cross-connected-setup.png)

<br> 

*Vernetzte Topologie: Erhöhte Redundanz und mehr Komplexität.*

Auf den ersten Blick bietet dies etwas mehr Redundanz für ein relativ kleines Budget. Wenn einer der Apache-Server ausfällt, können Sie immer noch zwei Veröffentlichungssysteme haben, die das Rendern durchführen. Wenn eines der Veröffentlichungssysteme abstürzt, verfügen Sie weiterhin über zwei Dispatcher, die die zwischengespeicherte Menge bereitstellen.

Dies ist jedoch mit einem Preis verbunden.

Erstens ist es recht umständlich, für die Wartung ein Bein zu entfernen. Genau dafür wurde dieses System entwickelt: um widerstandsfähiger zu sein und um jeden Preis in Betrieb zu bleiben. Wir haben komplizierte Wartungspläne gesehen, wie man damit umgeht: Neukonfigurierung des Dispatcher 2 und Entfernung der Querverbindung. Neustarten des Dispatcher 2. Herunterfahren von Dispatcher 1, Aktualisieren von Publish 1, ... und so weiter. Sie sollten sorgfältig prüfen, ob sich das auf mehr als zwei Beine skaliert. Sie werden zu dem Entschluss kommen, dass es tatsächlich die Komplexität und die Kosten erhöht und eine beachtliche Quelle für menschliche Fehler darstellt. Es wäre am besten, dies zu automatisieren. Überprüfen Sie also besser, ob Sie tatsächlich über die menschlichen Ressourcen verfügen, um diese Automatisierungsaufgabe in Ihren Projektplan aufzunehmen. Während Sie auf diese Weise vielleicht einige Hardware-Kosten einsparen, geben Sie möglicherweise das Doppelte für IT-Personal aus.

Außerdem kann es vorkommen, dass auf AEM einige Benutzerapplikationen ausgeführt werden, für die eine Anmeldung erforderlich ist. Sie verwenden Sticky-Sitzungen, um sicherzustellen, dass eine Benutzerin bzw. ein Benutzer immer von derselben AEM bereitgestellt wird, damit Sie den Sitzungsstatus in dieser Instanz beibehalten können. Wenn diese vernetzte Einrichtung vorhanden ist, müssen Sie sicherstellen, dass Sticky-Sitzungen auf dem Lastenausgleich und den Dispatchern ordnungsgemäß funktionieren. Das ist nicht unmöglich – aber Sie müssen sich dessen bewusst sein und zusätzliche Konfigurations- und Teststunden einkalkulieren, die wiederum die geplanten Einsparungen durch die eingesparte Hardware ausgleichen könnten.

### Zusammenfassung

Wir empfehlen nicht, dieses vernetzte Schema als Standardoption zu verwenden. Wenn Sie sich jedoch für die Verwendung entscheiden, sollten Sie Risiken und versteckte Kosten sorgfältig bewerten und planen, die Konfigurationsautomatisierung in Ihr Projekt einzubeziehen.

## Nächster Schritt

* [3. Fortgeschrittene Zwischenspeicherungsthemen](chapter-3.md)
