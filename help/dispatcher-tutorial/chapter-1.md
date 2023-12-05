---
title: Kapitel 1 – Konzepte, Muster und Antimuster des Dispatchers
description: Dieses Kapitel bietet eine kurze Einführung in die Geschichte und Mechanik des Dispatchers und erläutert, wie sich dies auf die Gestaltung seiner Komponenten durch AEM-Entwicklerinnen und -Entwickler auswirkt.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
doc-type: Tutorial
exl-id: 3bdb6e36-4174-44b5-ba05-efbc870c3520
duration: 4820
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '17384'
ht-degree: 100%

---

# Kapitel 1 – Konzepte, Muster und Antimuster des Dispatchers

## Übersicht

Dieses Kapitel bietet eine kurze Einführung in die Geschichte und Mechanik des Dispatchers und erläutert, wie sich dies auf die Gestaltung seiner Komponenten durch AEM-Entwicklerinnen und -Entwickler auswirkt.

## Warum sich Entwickelnde um die Infrastruktur kümmern sollten

Der Dispatcher ist ein wesentlicher Teil der meisten, wenn nicht gar aller AEM-Installationen. Es gibt eine ganze Reihe von Online-Artikeln, in denen die Konfiguration des Dispatchers sowie Tipps und Tricks beschrieben werden.

Dieses Mosaik an Informationen beginnt jedoch immer auf einer sehr technischen Ebene – vorausgesetzt, Sie wissen bereits, was Sie vorhaben, und geben deshalb Details nur dazu an, wie Sie das Gewünschte erreichen können. Wir haben noch keine konzeptionellen Beiträge gefunden, die das _Was und Warum_ beschrieben, wenn es um die Frage geht, was Sie mit dem Dispatcher tun können und was nicht.

### Antimuster: Dispatcher als eine Art Anhängsel

Dieser Mangel an grundlegenden Informationen führt zu einer Reihe von Antimustern (auch als Anti-Pattern bezeichnet), die wir in verschiedenen AEM-Projekten beobachten konnten:

1. Da der Dispatcher auf dem Apache-Webserver installiert ist, muss er von den „Unix-Gottheiten“ im Projekt konfiguriert werden. „Sterbliche Java-Entwickelnde“ müssen sich nicht damit befassen.

2. Java-Entwickelnde müssen sicherstellen, dass ihr Code funktioniert … Der Dispatcher sorgt dann später wie von Zauberhand für das nötige Tempo. An den Dispatcher wird immer zuletzt gedacht. Das funktioniert jedoch nicht. Entwickelnde müssen ihren Code unter Berücksichtigung des Dispatchers gestalten. Und dazu müssen sie seine Grundkonzepte kennen.

### „First make it work, then make it fast“ ist nicht immer richtig

Vielleicht haben Sie schon von diesem Ratschlag für Programmierende gehört: _„First make it work, then make it fast“ (Erst muss es funktionieren, dann schnell werden)._. Dies ist nicht ganz falsch. Ohne den richtigen Kontext wird es jedoch oft falsch interpretiert und nicht korrekt angewendet.

Der Rat sollte Entwicklungspersonen davon abhalten, Code vorzeitig zu optimieren, der möglicherweise nie ausgeführt wird – oder so selten ausgeführt wird, dass eine Optimierung keine ausreichenden Auswirkungen hätte, um den Aufwand für die Optimierung zu rechtfertigen. Darüber hinaus könnte eine Optimierung zu komplexerem Code führen und damit Fehler verursachen. Wenn Sie Entwicklerin oder Entwickler sind, verbringen Sie also nicht zu viel Zeit damit, jede einzelne Codezeile zu mikrooptimieren. Stellen Sie einfach sicher, dass Sie die richtigen Datenstrukturen, Algorithmen und Bibliotheken auswählen und warten Sie, bis die Hotspot-Analyse eines Profilers zeigt, wo eine gründlichere Optimierung die Gesamtleistung steigern könnte.

### Architektonische Entscheidungen und Artefakte

Der Rat „Erst muss es funktionieren, dann schnell werden“ ist jedoch völlig falsch, wenn es um „architektonische“ Entscheidungen geht. Was sind architektonische Entscheidungen? Einfach gesagt, sind das die Entscheidungen, die später nur teuer, schwierig und/oder unmöglich zu ändern sind. Denken Sie daran, dass „teuer“ manchmal dasselbe ist wie „unmöglich“. Wenn beispielsweise Ihr Projekt über kein Budget mehr verfügt, können teure Änderungen nicht mehr implementiert werden. Infrastrukturveränderungen sind die allerersten Veränderungen in dieser Kategorie, die den meisten in den Sinn kommen. Aber es gibt auch eine andere Art von „architektonischen“ Artefakten, die sich sehr schwierig ändern lassen:

1. Code-Abschnitte im „Zentrum“ einer Anwendung, auf die viele andere Teile angewiesen sind. Wenn Sie diese ändern, müssen alle Abhängigkeiten auf einmal geändert und neu getestet werden.

2. Artefakte, die in einem asynchronen, zeitabhängigen Szenario involviert sind, in dem die Eingabe – und damit das Verhalten des Systems – sehr zufällig variieren kann. Änderungen können hier unvorhersehbare Auswirkungen haben und schwer zu testen sein.

3. Software-Muster, die in allen Teilen des Systems immer wieder verwendet und wiederverwendet werden. Wenn sich das Software-Muster als suboptimal erweist, müssen alle Artefakte, die das Muster verwenden, neu codiert werden.

Erinnern Sie sich? Am Anfang dieser Seite haben wir gesagt, dass der Dispatcher ein wesentlicher Bestandteil einer AEM-Anwendung ist. Der Zugriff auf eine Web-Anwendung ist sehr zufällig – Benutzende kommen und gehen zu unvorhersehbaren Zeiten. Letztendlich werden alle Inhalte im Dispatcher zwischengespeichert (oder sollten es zumindest). Wenn Sie also genau aufgepasst haben, haben Sie vielleicht gemerkt, dass das Caching als „architektonisches“ Artefakt angesehen werden kann und daher von allen Mitgliedern des Teams, Entwicklungspersonen und Admins gleichermaßen verstanden werden sollte.

Wir sagen nicht, dass Entwicklungspersonen den Dispatcher tatsächlich konfigurieren sollten. Sie müssen jedoch die Konzepte – insbesondere die Grenzen – kennen, um sicherzustellen, dass ihr Code auch vom Dispatcher genutzt werden kann.

Der Dispatcher verbessert die Geschwindigkeit des Codes nicht auf magische Weise. Entwicklungspersonen müssen ihre Komponenten daher unter Berücksichtigung des Dispatchers erstellen. Dafür müssen sie wissen, wie er funktioniert.

## Dispatcher-Caching – Grundprinzipien

### Dispatcher als Caching-HTTP – Lastenausgleich

Was ist der Dispatcher und warum wird er überhaupt als „Dispatcher“ bezeichnet?

Der Dispatcher ist:

* zuallererst ein Cache

* ein Reverse-Proxy

* ein Modul für den Apache-httpd-Webserver, das die Vielseitigkeit von Apache um AEM-bezogene Funktionen erweitert und reibungslos mit allen anderen Apache-Modulen zusammenarbeitet (wie SSL oder sogar SSI-Einbindungen, wie wir später sehen werden)

In den Anfängen des Internets rechnete man mit ein paar hundert Besuchenden auf einer Website. Ein Setup mit einem Dispatcher, der die Last der Anfragen an eine Reihe von AEM-Veröffentlichungs-Servern „dispatchen“ (ausgewogen verteilen) konnte, war in der Regel ausreichend – daher der Name „Dispatcher“. Heutzutage wird dieses Setup jedoch nicht mehr sehr häufig verwendet.

Später in diesem Artikel werden wir verschiedene Möglichkeiten zum Einrichten von Dispatchern und Veröffentlichungssystemen sehen. Beginnen wir zunächst mit einigen Grundlagen der HTTP-Zwischenspeicherung.

![Grundlegende Funktionalität eines Dispatcher-Caches](assets/chapter-1/basic-functionality-dispatcher.png)

*Grundlegende Funktionalität eines Dispatcher-Caches*

<br> 

Die fundamentalen Grundlagen des Dispatchers werden hier erläutert. Der Dispatcher ist ein einfacher Reverse-Proxy zum Zwischenspeichern mit der Möglichkeit, HTTP-Anfragen zu empfangen und zu erstellen. Ein normaler Anfrage-/Antwortzyklus sieht wie folgt aus:

1. Benutzende fordern eine Seite an
2. Der Dispatcher prüft, ob bereits eine gerenderte Version dieser Seite vorhanden ist. Angenommen, es handelt sich um die allererste Anfrage für diese Seite und der Dispatcher kann keine lokale zwischengespeicherte Kopie finden.
3. Der Dispatcher fordert die Seite vom Veröffentlichungssystem an
4. Im Veröffentlichungssystem wird die Seite durch eine JSP- oder eine HTL-Vorlage gerendert
5. Die Seite wird an den Dispatcher zurückgegeben
6. Der Dispatcher speichert die Seite zwischen
7. Der Dispatcher gibt die Seite an den Browser zurück
8. Wenn dieselbe Seite ein zweites Mal angefordert wird, kann sie direkt aus dem Dispatcher-Cache bereitgestellt werden, ohne dass sie erneut in der Veröffentlichungsinstanz gerendert werden muss. Dadurch spart man Wartezeiten für Benutzende und CPU-Zyklen auf der Veröffentlichungsinstanz.

Wir haben im letzten Abschnitt über „Seiten“ gesprochen. Dasselbe Schema gilt jedoch auch für andere Ressourcen wie Bilder, CSS-Dateien, PDF-Downloads usw.

#### Zwischenspeicherung von Daten

Das Dispatcher-Modul nutzt die Funktionen, die der hostende Apache-Server bietet. Ressourcen wie HTML-Seiten, Downloads und Bilder werden als einfache Dateien im Apache-Dateisystem gespeichert. So einfach ist es.

Der Dateiname wird von der URL der angeforderten Ressource abgeleitet. Wenn Sie die Datei `/foo/bar.html` anfordern, wird sie beispielsweise unter /`var/cache/docroot/foo/bar.html` gespeichert.

Grundsätzlich können Sie, wenn alle Dateien zwischengespeichert und daher statisch im Dispatcher gelagert werden, das Kabel des Veröffentlichungssystems ziehen und der Dispatcher würde als einfacher Webserver fungieren. Aber dies dient nur der Veranschaulichung des Grundsatzes. Das wahre Leben ist komplizierter. Man kann nicht alles zwischenspeichern, und der Cache ist nie vollständig „voll“, da die Anzahl der Ressourcen aufgrund der dynamischen Natur des Rendervorgangs unbegrenzt sein kann. Das Modell eines statischen Dateisystems hilft, ein grobes Bild der Funktionen des Dispatchers zu erzeugen. Und es hilft, die Einschränkungen des Dispatchers zu erklären.

#### AEM-URL-Struktur und Dateisystemzuordnung

Um den Dispatcher detaillierter zu verstehen, schauen wir uns die Struktur einer einfachen Beispiel-URL erneut an. Sehen wir uns das folgende Beispiel an:

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` bezeichnet das Protokoll

* `domain.com` ist der Domain-Name

* `path/to/resource` ist der Pfad, unter dem die Ressource in CRX und anschließend im Dateisystem des Apache-Servers gespeichert wird.

Hier unterscheiden sich die Dinge ein wenig zwischen dem AEM-Dateisystem und dem Apache-Dateisystem.

In AEM sieht es so aus:

* `pagename` ist der Titel der Ressourcen

* `selectors` steht für eine Reihe von Selektoren, die in Sling verwendet werden, um zu bestimmen, wie die Ressource gerendert wird. Eine URL kann eine beliebige Anzahl von Selektoren aufweisen. Sie werden durch einen Punkt getrennt. Ein Selektorabschnitt könnte z. B. „deutsch.handy.schick“ sein. Selektoren dürfen nur Buchstaben, Ziffern und Gedankenstriche enthalten.

* `html` als letzter der „Selektoren“ wird als Erweiterung bezeichnet. In AEM/Sling bestimmt er auch teilweise das Rendering-Skript.

* `path/suffix.ext` ist ein pfadähnlicher Ausdruck, der ein Suffix der URL sein kann. Er kann in AEM-Skripten verwendet werden, um die Darstellung einer Ressource weiter zu steuern. Es gibt später einen ganzen Abschnitt über diesen Teil. Zunächst sollte es ausreichen, zu wissen, dass man ihn als zusätzlichen Parameter verwenden kann. Suffixe benötigen eine Erweiterung.

* `?parameter=value&otherparameter=value` ist der Abfrageabschnitt der URL. Er wird verwendet, um beliebige Parameter an AEM zu übergeben. URLs mit Parametern können nicht zwischengespeichert werden. Daher sollten Parameter auf Fälle beschränkt werden, in denen sie absolut erforderlich sind.

* `#fragment`, der Fragmentteil einer URL, wird nicht an AEM übergeben, sondern nur im Browser benutzt; entweder in JavaScript-Frameworks als „Routing-Parameter“ oder zum Springen zu einem bestimmten Teil der Seite.

In Apache (*bezieht sich auf das folgende Diagramm*)

* wird `pagename.selectors.html` als Dateiname im Dateisystem des Caches verwendet.

Wenn die URL ein Suffix `path/suffix.ext` hat, dann

* wird `pagename.selectors.html` als Ordner erstellt,

* `path` ist ein Ordner im Ordner `pagename.selectors.html`,

* `suffix.ext` ist eine Datei im Ordner `path`. Hinweis: Wenn das Suffix keine Erweiterung aufweist, wird die Datei nicht zwischengespeichert.

![Dateisystem-Layout nach Abrufen von URLs vom Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Dateisystem-Layout nach Abrufen von URLs vom Dispatcher*

<br> 

#### Grundlegende Einschränkungen

Die Zuordnung zwischen einer URL, der Ressource und dem Dateinamen ist ziemlich einfach.

Sie haben jedoch möglicherweise einige Fallen bemerkt:

1. URLs können sehr lang werden. Das Hinzufügen des „Pfad“-Teils einer `/docroot` auf dem lokalen Dateisystem kann die Grenzen einiger Dateisysteme leicht überschreiten. Das Ausführen des Dispatchers in NTFS unter Windows kann eine Herausforderung sein. Bei Linux sind Sie jedoch sicher.

2. URLs können Sonderzeichen und Umlaute enthalten. Dies ist normalerweise kein Problem für den Dispatcher. Beachten Sie jedoch, dass die URL an vielen Stellen Ihrer Anwendung interpretiert wird. Schon oft haben wir merkwürdiges Verhalten einer Anwendung beobachtet – nur um herauszufinden, dass ein Teil des selten verwendeten (benutzerdefinierten) Codes nicht gründlich auf Sonderzeichen getestet wurde. Sie sollten diese also vermeiden, wenn möglich. Wenn nicht, planen Sie gründliche Tests ein.

3. In CRX verfügen Ressourcen über Unterressourcen. Eine Seite enthält beispielsweise eine Reihe von Unterseiten. Dies kann nicht in einem Dateisystem abgeglichen werden, da Dateisysteme entweder Dateien oder Ordner haben.

#### URLs ohne Erweiterung werden nicht zwischengespeichert

URLs müssen immer über eine Erweiterung verfügen. Sie können zwar URLs ohne Erweiterungen in AEM bereitstellen. Diese URLs werden aber nicht im Dispatcher zwischengespeichert.

**Beispiele**

`http://domain.com/home.html` ist **zwischenspeicherbar**

`http://domain.com/home` ist **nicht zwischenspeicherbar**

Dieselbe Regel gilt, wenn die URL ein Suffix enthält. Das Suffix muss über eine Erweiterung verfügen, um zwischenspeicherbar zu sein.

**Beispiele**

`http://domain.com/home.html/path/suffix.html` ist **zwischenspeicherbar**

`http://domain.com/home.html/path/suffix` ist **nicht zwischenspeicherbar**

Sie fragen sich vielleicht, was passiert, wenn der Ressourcenteil keine Erweiterung hat, aber das Suffix schon? Nun, in diesem Fall hat die URL überhaupt kein Suffix. Sehen Sie sich das folgende Beispiel an:

**Beispiel**

`http://domain.com/home/path/suffix.ext`

Das `/home/path/suffix` ist der Pfad zur Ressource … also ist kein Suffix in der URL.

**Zusammenfassung**

Fügen Sie sowohl dem Pfad als auch dem Suffix immer Erweiterungen hinzu. SEO-bewusste Menschen argumentieren manchmal, dass Sie dadurch in den Suchergebnissen weiter unten landen. Eine nicht zwischengespeicherte Seite wäre jedoch extrem langsam und würde sogar noch weiter nach unten geraten.

#### Konflikte bei Suffix-URLs

Angenommen, Sie haben zwei gültige URLs

`http://domain.com/home.html`

und

`http://domain.com/home.html/suffix.html`

Beide sind in AEM absolut gültig. Auf Ihrem lokalen Entwicklungs-Computer würden Sie kein Problem sehen (ohne Dispatcher). Wahrscheinlich stoßen Sie auch bei UAT- oder Belastungstests nicht auf Probleme. Das Problem, vor dem wir stehen, ist derart subtil, dass es bei den meisten Tests nicht erkennt wird.  Es wird Sie hart treffen, wenn die Belastung nicht höher sein könnte und Sie nur begrenzt Zeit haben, um dieses Problem zu beheben. Wobei Ihnen für die Lösung wahrscheinlich der Server-Zugriff und die Ressourcen fehlen. Wir haben das alles schon erlebt …

Aber was ist nun das Problem?

`home.html` in einem Dateisystem kann entweder eine Datei oder ein Ordner sein, aber nicht beides gleichzeitig wie in AEM.

Wenn Sie `home.html` erstmalig anfordern, wird eine Datei erstellt.

Bei nachfolgenden Anfragen an `home.html/suffix.html` werden zwar gültige Ergebnisse zurückgegeben, allerdings „blockiert“ die Datei `home.html` die Position im Dateisystem. `home.html` kann nicht ein zweites Mal als Ordner erstellt werden und `home.html/suffix.html` wird daher nicht zwischengespeichert.

![Blockierte Dateiposition im Dateisystem verhindert das Caching von Unterressourcen.](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Blockierte Dateiposition im Dateisystem verhindert das Caching von Unterressourcen.*

<br> 

Wenn Sie in umgekehrter Reihenfolge vorgehen, also erst `home.html/suffix.html` anfordern, wird `suffix.html` zunächst unter einem Ordner `/home.html` zwischengespeichert. Dieser Ordner wird jedoch gelöscht und durch eine Datei `home.html` ersetzt, wenn Sie später `home.html` als Ressource anfordern.

![Löschen einer Pfadstruktur, wenn ein übergeordnetes Element als Ressource abgerufen wird](assets/chapter-1/deleting-path-structure.png)

*Löschen einer Pfadstruktur, wenn ein übergeordnetes Element als Ressource abgerufen wird*

<br> 

Das Ergebnis der Zwischenspeicherung ist also völlig zufällig und hängt von der Reihenfolge der eingehenden Anfragen ab. Was die Sache noch schwieriger macht, ist die Tatsache, dass es normalerweise mehr als einen Dispatcher gibt. Und die Leistung, die Cache-Trefferrate und das Verhalten sind je nach Dispatcher unterschiedlich. Wenn Sie herausfinden möchten, warum Ihre Website nicht reagiert, müssen Sie sicherstellen, dass Sie sich den richtigen Dispatcher mit der misslichen Caching-Reihenfolge ansehen. Wenn Sie sich den Dispatcher ansehen, der – glücklicherweise – ein günstigeres Anfragemuster aufweist, werden Sie bei dem Versuch, das Problem zu finden, scheitern.

#### Vermeiden in Konflikt stehender URLs

Sie können in Konflikt stehende URLs vermeiden, bei denen ein Ordnername und ein Dateiname im Dateisystem mit demselben Pfad konkurrieren, wenn Sie bei vorhandenem Suffix eine andere Erweiterung für die Ressource verwenden.

**Beispiel**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Für beide ist eine Zwischenspeicherung problemlos möglich.

![](assets/chapter-1/cacheable.png)

Wählen Sie ein dediziertes Erweiterungsverzeichnis („dir“) für eine Ressource, wenn Sie ein Suffix anfordern, oder verzichten Sie komplett auf das Suffix. In seltenen Fällen ist dies nützlich. Und es ist einfach, diese Fälle korrekt zu implementieren.  Und genau das werden wir im nächsten Kapitel sehen, wenn es um die Themen Cache-Invalidierung und -Leerung geht.

#### Nicht zwischenspeicherbare Anfragen

Sehen wir uns eine kurze Zusammenfassung des letzten Kapitels sowie einige weitere Ausnahmen an. Der Dispatcher kann eine URL zwischenspeichern, wenn sie als zwischenspeicherbar konfiguriert ist und es sich um eine GET-Anfrage handelt. Sie kann bei einer der folgenden Ausnahmen nicht zwischengespeichert werden.

**Zwischenspeicherbare Anfragen**

* Die Anfrage ist in der Dispatcher-Konfiguration als zwischenspeicherbar konfiguriert.
* Die Anfrage ist eine einfache GET-Anfrage.

**Nicht zwischenspeicherbare Anfragen oder Antworten**

* Anfrage, die durch die Konfiguration verweigert wird (Pfad, Muster, MIME-Typ)
* Antworten, die den Header „Dispatcher: no-cache“ zurückgeben
* Antwort, die den Header „Cache-Control: no-cache|private“ zurückgibt
* Antwort, die den Header „Pragma: no-cache“ zurückgibt
* Anfrage mit Abfrageparametern
* URL ohne Erweiterung
* URL mit Suffix ohne Erweiterung
* Antwort, die einen anderen Status-Code als 200 zurückgibt
* POST-Anfrage

## Invalidieren und Leeren des Caches

### Übersicht

Im letzten Kapitel wurde eine große Anzahl von Ausnahmen aufgelistet, wenn der Dispatcher eine Anfrage nicht zwischenspeichern kann. Es gibt jedoch noch mehr zu beachten: Nur weil der Dispatcher eine Anfrage zwischenspeichern _kann_, bedeutet dies nicht unbedingt, dass er das auch _sollte_.

Der Punkt ist: In der Regel ist das Caching einfach. Der Dispatcher muss nur das Ergebnis einer Antwort speichern und es beim nächsten Eintreffen derselben Anfrage zurückgeben. Richtig? Falsch!

Die Schwierigkeit besteht in der _Invalidierung_ oder im _Leeren_ des Cache. Der Dispatcher muss herausfinden, wann sich eine Ressource geändert hat – und erneut gerendert werden muss.

Das scheint auf den ersten Blick eine banale Aufgabe zu sein, aber das ist es nicht. Lesen Sie weiter, und Sie werden einige schwierige Unterschiede zwischen einzelnen und einfachen Ressourcen und Seiten, die sich auf eine hochgradig vernetzte Struktur mehrerer Ressourcen stützen, herausfinden.

### Einfache Ressourcen und Leeren

Wir haben unser AEM-System eingerichtet, um bei Bedarf eine Miniaturansicht für jedes Bild dynamisch mit einem speziellen „Miniaturansichten“-Selektor zu erstellen:

`/content/dam/path/to/image.thumb.png`

Natürlich stellen wir eine URL bereit, um das Originalbild mit einer selektorenlosen URL bereitzustellen:

`/content/dam/path/to/image.png`

Wenn wir beides herunterladen, die Miniaturansicht und das Originalbild, erhalten wir so etwas wie

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

im Dateisystem unseres Dispatchers.

Jetzt lädt jemand eine neue Version dieser Datei hoch und aktiviert sie. Schließlich wird eine Invalidierungsanfrage von AEM an den Dispatcher gesendet.

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

Die Invalidierung ist so einfach: Eine einfache GET-Anfrage an eine spezielle „/invalidate“-URL an den Dispatcher. Ein HTTP-Text ist nicht erforderlich, die „Payload“ ist nur der Header „invalidate-path“. Beachten Sie außerdem, dass der invalidierungspfad im Header die Ressource ist, die AEM kennt – und nicht die Datei(en), die der Dispatcher zwischengespeichert hat. AEM weiß nur von Ressourcen. Erweiterungen, Selektoren und Suffixe werden zur Laufzeit verwendet, wenn eine Ressource angefordert wird. AEM führt keine Buchführung darüber durch, welche Selektoren für eine Ressource verwendet wurden. Daher ist der Ressourcenpfad alles, was es bei der Aktivierung einer Ressource sicher kennt.

Das reicht in unserem Fall aus. Wenn sich eine Ressource geändert hat, können wir sicher davon ausgehen, dass sich auch alle Ausgabedarstellungen dieser Ressource geändert haben. In unserem Beispiel wird, wenn sich das Bild geändert hat, auch eine neue Miniatur gerendert.

Der Dispatcher kann die Ressource mit allen Ausgabedarstellungen, die er zwischengespeichert hat, sicher löschen. Er tut in etwa das Folgende:

`$ rm /content/dam/path/to/image.*`

Entfernen von `image.png` und `image.thumb.png` und aller anderen Ausgabedarstellungen, die diesem Muster entsprechen.

Sehr einfach, ja … solange Sie nur eine einzige Ressource verwenden, um auf eine Anfrage zu antworten.

### Verweise und vermaschte Inhalte

#### Das Problem mit vermaschten Inhalten

Im Gegensatz zu Bildern oder anderen Binärdateien, die in AEM hochgeladen werden, sind HTML-Seiten keine Einzelgänger. Sie leben in Herden und sind durch Hyperlinks und Verweise stark miteinander verbunden. Der einfache Link ist harmlos, aber es wird schwierig, wenn es um Inhaltsverweise geht. Die allgegenwärtige obere Navigation oder Teaser auf Seiten sind Inhaltsreferenzen.

#### Inhaltsverweise und warum sie Probleme machen

Sehen wir uns ein einfaches Beispiel an. Ein Reisebüro hat eine Webseite, auf der eine Reise nach Kanada beworben wird. Diese Werbung wird im Teaser-Abschnitt auf zwei weiteren Seiten, auf der „Startseite“ und auf der Seite „Winter-Specials“ vorgestellt.

Da auf beiden Seiten derselbe Teaser angezeigt wird, wäre es unnötig, den Autor bzw. die Autorin aufzufordern, den Teaser für jede Seite, auf der er angezeigt werden soll, neu zu erstellen. Stattdessen reserviert die Zielseite „Canada“ einen Abschnitt in den Seiteneigenschaften, um die Informationen für den Teaser bereitzustellen – oder genauer, um eine URL bereitzustellen, die den Teaser insgesamt rendert:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

oder

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Auf AEM funktioniert das einwandfrei, aber wenn Sie einen Dispatcher in der Veröffentlichungsinstanz verwenden, passiert etwas Seltsames.

Stellen Sie sich vor, Sie haben Ihre Website veröffentlicht. Der Titel auf Ihrer Kanada-Seite ist „Kanada“. Wenn jemand Ihre Startseite anfordert, die einen Teaser-Verweis auf diese Seite enthält, rendert die Komponente auf der Seite „Kanada“ etwas wie das Folgende

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*in* die Startseite. Die Startseite wird vom Dispatcher als statische HTML-Datei gespeichert, einschließlich des Teasers und dessen Überschrift in der Datei.

Jetzt hat die Marketing-Fachkraft erfahren, dass Teaser-Überschriften etwas Umsetzbares sein sollten. Sie entscheidet also, den Titel von „Kanada“ in „Besuchen Sie Kanada“ zu ändern, und aktualisiert auch das Bild.

Sie veröffentlicht die bearbeitete „Kanada“-Seite und besucht die zuvor veröffentlichte Startseite erneut, um ihre Änderungen zu sehen. Aber – dort hat sich nichts verändert. Es wird weiterhin der alte Teaser angezeigt. So überprüft sie es auf der „Winter-Special“-Seite. Diese Seite wurde noch nie angefordert und wird daher nicht statisch im Dispatcher zwischengespeichert. Daher wird diese Seite neu von der Veröffentlichungsinstanz gerendert und enthält jetzt den neuen Teaser „Besuchen Sie Kanada“.

![Dispatcher speichert veralteten eingeschlossenen Inhalt auf der Startseite](assets/chapter-1/dispatcher-storing-stale-content.png)

*Dispatcher speichert veralteten eingeschlossenen Inhalt auf der Startseite*

<br> 

Was ist passiert? Der Dispatcher speichert eine statische Version einer Seite, die alle Inhalte und Markups enthält, die beim Rendern aus anderen Ressourcen gezogen wurden.

Der Dispatcher, ein reiner auf Dateisystemen basierender Webserver, ist schnell, aber auch relativ simpel. Wenn sich eine eingeschlossene Ressource ändert, wird dies nicht erkannt. Er hält weiterhin an dem Inhalt fest, der zum Zeitpunkt des Renderings der einschließenden Seite vorhanden war.

Die Seite „Winter-Special“ wurde noch nicht gerendert, sodass keine statische Version auf dem Dispatcher vorhanden ist und sie daher mit dem neuen Teaser angezeigt wird, da sie auf Anfrage neu gerendert wird.

Sie könnten denken, dass der Dispatcher beim Rendern und Leeren aller Seiten, die diese Ressource verwendet haben, jede Ressource verfolgt, die er anrührt, wenn sich diese Ressource ändert. Der Dispatcher rendert die Seiten jedoch nicht. Das Rendering wird vielmehr vom Veröffentlichungssystem durchgeführt. Der Dispatcher weiß nicht, welche Ressourcen in eine gerenderte HTML-Datei aufgenommen werden.

Noch nicht überzeugt? Vielleicht denken Sie: *„Aber es muss doch eine Möglichkeit geben, eine Art Abhängigkeits-Tracking zu implementieren.“* Nun, die gibt es, oder genauer gesagt, die *gab* es. Communiqué 3, der Ururururgroßvater von AEM, hatte einen Abhängigkeits-Tracker in der _Sitzung_ implementiert, der zum Rendern einer Seite verwendet wurde.

Während einer Anfrage wurde jede Ressource, die über diese Sitzung erfasst wurde, als Abhängigkeit der URL verfolgt, die derzeit gerendert wurde.

Aber es stellte sich heraus, dass die Überwachung der Abhängigkeiten sehr aufwendig war. Man hat bald entdeckt, dass die Website schneller ist, wenn die Funktion zum Abhängigkeits-Tracking komplett deaktiviert ist und alle HTML-Seiten erneut gerendert werden, nachdem eine HTML-Seite geändert wurde. Jedoch war diese Regelung auch nicht perfekt – es gab eine Reihe von Fallstricken und Ausnahmen. In einigen Fällen wurde nicht die Standardsitzung mit Anfragen verwendet, um eine Ressource zu erhalten, sondern eine Admin-Sitzung, um einige Hilfsressourcen zum Rendern einer Anfrage zu erhalten. Diese Abhängigkeiten wurden in der Regel nicht nachverfolgt und führten zu Kopfschmerzen und Telefonanrufen an das operative Team, das darum gebeten wurde, den Cache manuell zu leeren. Man hatte noch Glück, wenn sie dazu über ein Standardverfahren verfügten. Auf dem Weg gab es noch mehr Fallstricke, aber … hören wir auf, in Erinnerungen zu schwelgen. Dies führt bis 2005 zurück. Letztlich wurde diese Funktion in Communiqué 4 standardmäßig deaktiviert und nicht wieder in den Nachfolger CQ5 aufgenommen, der dann zu AEM wurde.

### Automatische Invalidierung

#### Wenn eine komplette Leerung günstiger ist als das Abhängigkeits-Tracking

Seit CQ5 verlassen wir uns vollständig auf die Invalidierung der gesamten Website (mehr oder weniger), wenn sich nur eine der Seiten ändert. Diese Funktion wird als „Automatische Invalidierung“ bezeichnet.

Aber nochmal – wie kann es sein, dass es billiger ist, Hunderte von Seiten wegzuwerfen und neu zu rendern, als ein ordnungsgemäßes Abhängigkeits-Tracking und ein partielles Rendering durchzuführen?

Es gibt zwei Hauptgründe:

1. Auf einer durchschnittlichen Website wird nur eine kleine Teilmenge der Seiten häufig angefordert. Selbst wenn Sie also alle gerenderten Inhalte wegwerfen, werden nur ein paar Dutzend tatsächlich sofort danach angefordert. Die Wiedergabe des Großteils der Seiten kann auf die Zeitpunkte verteilt werden, an denen sie tatsächlich angefordert werden. Tatsächlich ist die Belastung beim Rendern von Seiten also nicht so hoch wie vielleicht erwartet. Natürlich gibt es immer Ausnahmen … wir werden später einige Tricks dafür besprechen, wie man gleichmäßig verteilte Ladungen auf größeren Websites mit leeren Dispatcher-Caches verarbeitet.

2. Alle Seiten sind sowieso über die Hauptnavigation verbunden. Fast alle Seiten sind also letztlich voneinander abhängig. Das bedeutet, dass selbst der intelligenteste Abhängigkeits-Tracker herausfinden wird, was wir bereits wissen: Wenn sich eine Seite ändert, müssen Sie alle anderen invalidieren.

Das glauben Sie nicht? Lassen Sie uns den letzten Punkt veranschaulichen.

Wir verwenden dasselbe Argument wie im letzten Beispiel mit Teasern, die auf den Inhalt einer Remote-Seite verweisen. Jetzt verwenden wir aber ein extremeres Beispiel: eine automatisch gerenderte Hauptnavigation. Wie beim Teaser wird der Navigationstitel von der verknüpften oder „Remote“-Seite als Inhaltsreferenz bezogen. Die Titel der Remote-Navigation werden nicht auf der derzeit gerenderten Seite gespeichert. Beachten Sie, dass die Navigation auf jeder einzelnen Seite Ihrer Website gerendert wird. Der Titel einer Seite wird also immer wieder auf allen Seiten mit Hauptnavigation verwendet. Wenn Sie einen Navigationstitel ändern möchten, sollten Sie dies nur einmal auf der Remote-Seite tun – nicht auf jeder einzelnen Seite, die auf die Seite verweist.

In unserem Beispiel werden also alle Seiten durch die Navigation miteinander verknüpft, indem der „NavTitle“ der Zielseite zum Rendern eines Namens in der Navigation verwendet wird. Der Navigationstitel für „Island“ wird von der Seite „Island“ bezogen und auf jeder Seite mit Hauptnavigation dargestellt.

![Hauptnavigation vermascht zwangsläufig alle Seiten mit Inhalten, indem sie ihre „NavTitles“ abruft](assets/chapter-1/nav-titles.png)

*Hauptnavigation vermascht zwangsläufig alle Seiten mit Inhalten, indem sie ihre „NavTitles“ abruft*

<br> 

Wenn Sie den NavTitle auf der isländischen Seite von „Island“ zu „Wunderschönes Island“ ändern, ändert sich dieser Titel sofort auf dem Hauptmenü aller anderen Seiten. Somit sind die vor dieser Änderung gerenderten und zwischengespeicherten Seiten alle veraltet und müssen invalidiert werden.

#### Implementierung der automatischen Invalidierung: die STAT-Datei

Wenn Sie nun eine große Site mit Tausenden von Seiten haben, würde es recht lange dauern, alle Seiten zu durchlaufen und sie physisch zu löschen. Während dieses Zeitraums könnte der Dispatcher unbeabsichtigt veraltete Inhalte bereitstellen. Schlimmer noch: Beim Zugriff auf die Cache-Dateien kann es zu Konflikten kommen. Möglicherweise wird eine Seite angefordert, während sie gerade gelöscht wird, oder eine Seite wird aufgrund einer zweiten Invalidierung, die nach einer sofortigen nachfolgenden Aktivierung stattgefunden hat, erneut gelöscht. Denken Sie nur, was für ein Chaos das wäre. Glücklicherweise passiert das nicht. Der Dispatcher verwendet einen cleveren Trick, um dies zu vermeiden: Statt Hunderte und Tausende von Dateien zu löschen, wird eine einfache, leere Datei in das Stammverzeichnis Ihres Dateisystems eingefügt, wenn eine Datei veröffentlicht wird, wodurch alle abhängigen Dateien als ungültig betrachtet werden. Diese Datei wird als „Stat-Datei“ bezeichnet. Die Stat-Datei ist eine leere Datei – das Einzige, was bei der Stat-Datei wichtig ist, ist das Erstellungsdatum.

Alle Dateien im Dispatcher, deren Erstellungsdatum älter ist als das der Stat-Datei, wurden vor der letzten Aktivierung (und Invalidierung) gerendert und werden daher als „ungültig“ betrachtet. Sie sind weiterhin physisch im Dateisystem vorhanden, aber der Dispatcher ignoriert sie. Sie sind „veraltet“. Bei jeder Anfrage an eine veraltete Ressource fordert der Dispatcher das AEM-System auf, die Seite erneut zu rendern. Diese neu gerenderte Seite wird dann im Dateisystem gespeichert – jetzt mit einem neuen Erstellungsdatum, und sie ist wieder neu.

![Erstellungsdatum der STAT-Datei definiert, welcher Inhalt veraltet und welcher neu ist](assets/chapter-1/creation-date.png)

*Erstellungsdatum der STAT-Datei definiert, welcher Inhalt veraltet und welcher neu ist*

<br> 

Sie fragen vielleicht, warum die Datei „.stat“ heißt? Und nicht vielleicht „.invalidated“? Nun, Sie können sich vorstellen, dass diese Datei in Ihrem Dateisystem dem Dispatcher hilft, zu bestimmen, welche Ressourcen *statisch* bereitgestellt werden könnten – genau wie von einem statischen Webserver. Diese Dateien müssen nicht mehr dynamisch gerendert werden.

Der wahre Charakter des Namens ist jedoch weniger metaphorisch. Er geht auf den Unix-Systemaufruf `stat()` zurück, der die Änderungszeit einer Datei (und andere Eigenschaften) zurückgibt.

#### Mischen der einfachen und der automatischen Validierung

Aber Moment mal … vorher haben wir gesagt, dass einzelne Ressourcen physisch gelöscht werden. Jetzt sagen wir, dass eine neuere Stat-Datei sie in den Augen des Dispatchers praktisch ungültig macht. Warum dann zuerst die physische Löschung?

Die Antwort ist einfach. Normalerweise verwendet man beide Strategien parallel – aber für verschiedene Arten von Ressourcen. Binäre Assets, wie Bilder, sind eigenständig. Sie sind nicht mit anderen Ressourcen verbunden, d. h., sie benötigen deren Informationen nicht, um gerendert zu werden.

HTML-Seiten hingegen sind stark voneinander abhängig. Sie würden also die automatische Invalidierung darauf anwenden. Dies ist die Standardeinstellung im Dispatcher. Alle Dateien, die zu einer invalidierten Ressource gehören, werden physisch gelöscht. Darüber hinaus werden Dateien, die auf „.html“ enden, automatisch invalidiert.

Der Dispatcher entscheidet anhand der Dateierweiterung, ob das automatische Invalidierungsschema angewendet werden soll oder nicht.

Die Dateiendungen für die automatische Invalidierung können konfiguriert werden. Theoretisch können Sie alle Erweiterungen in die automatische Invalidierung einbeziehen. Aber denken Sie daran, dass dies zu sehr hohen Kosten führt. Es werden zwar nicht unbeabsichtigt veraltete Ressourcen bereitgestellt, aber die Bereitstellungsleistung verschlechtert sich stark aufgrund der übermäßigen Invalidierung.

Stellen Sie sich beispielsweise vor, Sie implementieren ein Schema, bei dem PNGs und JPG dynamisch gerendert werden und dafür von anderen Ressourcen abhängig sind. Möglicherweise möchten Sie die Skalierung von hochauflösenden Bildern auf eine kleinere, Web-kompatible Auflösung ändern. Während Sie schon dabei sind, ändern Sie auch die Komprimierungsrate. Auflösung und Komprimierungsrate sind in diesem Beispiel keine festen Konstanten, sondern konfigurierbare Parameter in der Komponente, die das Bild verwendet. Wenn dieser Parameter geändert wird, müssen Sie die Bilder invalidieren.

Kein Problem – wir haben schließlich gerade erfahren, dass wir Bilder zur automatischen Invalidierung hinzufügen können und Bilder immer neu gerendert werden, wenn sich etwas ändert.

#### Das Kind mit dem Bade ausschütten

Das ist richtig – und ein riesiges Problem. Lesen Sie den letzten Absatz erneut. „… Bilder immer neu gerendert werden, wenn sich etwas ändert.“ Wie Sie wissen, wird eine gute Website ständig geändert. Es werden hier neue Inhalte hinzugefügt, dort ein Tippfehler korrigiert, irgendwo anders ein Teaser angepasst. Das bedeutet, dass alle Ihre Bilder ständig invalidiert werden und erneut gerendert werden müssen. Unterschätzen Sie das nicht. Die dynamische Wiedergabe und Übertragung von Bilddaten erfolgt in Millisekunden auf Ihrem lokalen Entwicklungs-Computer. Ihre Produktionsumgebung muss dies hundertmal öfter tun – pro Sekunde.

Und lassen Sie uns hier klarstellen, dass Ihre JPGs erneut gerendert werden müssen, wenn sich eine HTML-Seite ändert und umgekehrt. Es gibt nur einen einzigen „Eimer“ mit Dateien, die automatisch ungültig gemacht werden sollen. Er wird vollständig geleert. Ohne die Möglichkeit, ihn in detailliertere Strukturen aufzuschlüsseln.

Es gibt einen guten Grund, warum die automatische Invalidierung standardmäßig auf „.html“ beibehalten wird. Das Ziel ist, diesen Eimer so klein wie möglich zu halten. Schütten Sie das Kind nicht mit dem Bade aus, indem Sie einfach alles invalidieren – nur um auf der sicheren Seite zu bleiben.

Auf dem Pfad dieser Ressource sollten eigenständige Ressourcen bereitgestellt werden. Das hilft sehr bei der Invalidierung. Halten Sie es einfach, erstellen Sie keine Zuordnungsschemata wie „resource /a/b/c“ wird von „/x/y/z“ bereitgestellt. Sorgen Sie dafür, dass Ihre Komponenten mit den standardmäßigen Einstellungen für die automatische Invalidierung des Dispatchers funktionieren. Versuchen Sie nicht, eine schlecht entworfene Komponente mit übermäßiger Invalidierung im Dispatcher zu reparieren.

##### Ausnahmen für die automatische Invalidierung: einzig ressourcenbezogene Invalidierung

Die Invalidierungsanfrage für den Dispatcher wird normalerweise von einem Replikationsagenten aus den Veröffentlichungssystemen ausgelöst.

Wenn Sie sich hinsichtlich Ihrer Abhängigkeiten absolut sicher fühlen, können Sie versuchen, einen eigenen Replikationsagenten für die Invalidierung zu erstellen.

Es würde den Rahmen dieses Handbuchs sprengen, auf die Details einzugehen, aber wir möchten Ihnen zumindest einige Hinweise geben.

1. Sie müssen wirklich wissen, was Sie tun. Es ist wirklich schwer, die Invalidierung richtig einzustellen. Dies ist ein Grund, warum die automatische Invalidierung so rigoros ist: um die Bereitstellung veralteter Inhalte zu vermeiden.

2. Wenn Ihr Agent einen HTTP-Header `CQ-Action-Scope: ResourceOnly` sendet, bedeutet das, dass diese einzelne Anfrage zur Invalidierung keine automatische Invalidierung triggert. Dieses Code-Stück ([https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) könnte ein guter Ausgangspunkt für Ihren eigenen Replikationsagenten sein.

3. `ResourceOnly` verhindert nur die automatische Invalidierung. Um die erforderlichen Abhängigkeiten aufzulösen und Invalidierungen durchzuführen, müssen Sie die Invalidierungsanfragen selbst triggern. Dafür könnten Sie die Regeln des Pakets für das Dispatcher-Leeren überprüfen ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)), um Anregungen zu erhalten, wie das passieren könnte.

Es wird nicht empfohlen, ein Schema zur Auflösung von Abhängigkeiten zu erstellen. Das bedeutet einfach zu viel Mühe und wenig Gewinn – und wie bereits gesagt, es gäbe zu viel, bei dem Sie Fehler machen würden.

Sie sollten stattdessen herausfinden, welche Ressourcen nicht von anderen Ressourcen abhängig sind und ohne automatische Invalidierung invalidiert werden können. Dafür müssen Sie jedoch keinen benutzerdefinierten Replikationsagenten verwenden. Erstellen Sie einfach eine benutzerdefinierte Regel in Ihrer Dispatcher-Konfiguration, die diese Ressourcen von der automatischen Invalidierung ausschließt.

Wir haben gesagt, dass die Hauptnavigation oder Teaser eine Quelle für Abhängigkeiten sind. Nun, wenn Sie die Navigation und Teaser asynchron laden oder sie mit einem SSI-Skript in Apache einschließen, müssen Sie diese Abhängigkeit nicht nachverfolgen. Später in diesem Dokument werden wir das asynchrone Laden von Komponenten vertiefen, wenn wir über „Sling Dynamic Includes“ sprechen.

Dasselbe gilt für Popup-Fenster oder Inhalte, die in eine Lightbox geladen werden. Diese Teile haben selten ebenfalls Navigationen (auch „Abhängigkeiten“ genannt) und können als einzelne Ressource invalidiert werden.

## Erstellen von Komponenten mit dem Dispatcher im Hinterkopf

### Anwenden des Dispatcher-Mechanismus auf ein reales Beispiel

Im letzten Kapitel haben wir erläutert, wie die grundlegende Mechanik des Dispatchers im Allgemeinen funktioniert und welche Einschränkungen bestehen.

Wir möchten diese Mechanik nun auf eine Art von Komponenten anwenden, die Sie wahrscheinlich in den Anforderungen Ihres Projekts finden werden. Wir wählen die Komponente bewusst aus, um Probleme zu demonstrieren, die auch früher oder später bei Ihnen auftreten werden. Nur keine Furcht – nicht alle Komponenten brauchen die Aufmerksamkeit, die wir aufwenden werden. Wenn Sie jedoch die Notwendigkeit sehen, eine solche Komponente zu erstellen, sind Sie sich der Folgen bewusst und wissen, wie sie zu bewältigen sind.

### Das (Anti-)Muster der Spooling-Komponente

#### Die responsive Bildkomponente

Lassen Sie uns ein gemeinsames Muster (oder Anti-Muster) einer Komponente anhand miteinander verbundener Binärdateien veranschaulichen. Wir erstellen die Komponente „respi“ – für „responsives Bild“. Diese Komponente sollte in der Lage sein, das angezeigte Bild an das Gerät anzupassen, auf dem es angezeigt wird. Auf Desktops und Tablets zeigt es die vollständige Auflösung des Bildes, auf Smartphones eine kleinere Version mit schmalem Zuschnitt – oder vielleicht sogar ein völlig anderes Motiv (dies wird in der responsiven Welt als „Kunstrichtung“ bezeichnet).

Die Assets werden in den DAM-Bereich von AEM hochgeladen und in der responsiven Bildkomponente nur _referenziert_.

Die respi-Komponente übernimmt sowohl das Rendering des Markups als auch die Bereitstellung der binären Bilddaten.

Die Art und Weise, wie wir sie hier implementieren, entspricht einem gängigen Muster, das wir in vielen Projekten gesehen haben, und selbst eine der AEM-Kernkomponenten basiert auf diesem Muster. Daher ist es sehr wahrscheinlich, dass Sie als Entwicklungsperson dieses Muster übernehmen. Die Kapselung bietet Vorteile, aber es erfordert viel Aufwand, es Dispatcher-fähig zu machen. Wir werden später mehrere Möglichkeiten erörtern, wie das Problem gelöst werden kann.

Wir nennen das hier verwendete Muster das „Spooler-Muster“, da das Problem in die frühen Tage von Communiqué 3 zurückreicht, wo es die Methode „spool“ gab, die für eine Ressource aufgerufen werden konnte, um ihre binären Rohdaten in die Antwort zu streamen.

Der ursprüngliche Begriff „Spooling“ bezieht sich eigentlich auf gemeinsam genutzte langsame Offline-Peripheriegeräte wie Drucker, weshalb er hier nicht ganz richtig verwendet wird. Aber wir mögen den Begriff trotzdem, weil er in der Online-Welt selten vorkommt und daher charakteristisch ist. Jedes Muster sollte sowieso einen charakteristischen Namen haben, nicht wahr? Es liegt an Ihnen zu entscheiden, ob es ein Muster oder ein Anti-Muster ist.

#### Implementierung

So wird unsere responsive Bildkomponente implementiert:

Die Komponente besteht aus zwei Teilen. Der erste Teil rendert das HTML-Markup des Bildes, der zweite Teil „spoolt“ die Binärdaten des referenzierten Bildes. Da es sich um eine moderne Website mit einem responsiven Design handelt, wird kein einfaches `<img src"…">`-Tag, sondern eine Reihe von Bildern in einem `<picture/>`-Tag gerendert. Für jedes Gerät laden wir zwei verschiedene Bilder in das DAM hoch und referenzieren sie aus unserer Bildkomponente.

Die Komponente verfügt über drei Rendering-Skripte (implementiert in JSP, HTL oder als Servlet), die jeweils mit einem dedizierten Selektor adressiert werden:

1. `/respi.jsp` – ohne Selektor zum Rendern des HTML-Markups
2. `/respi.img.java` – zum Rendern der Desktop-Version
3. `/respi.img.mobile.java` – zum Rendern der Mobile-Version.


Die Komponente wird im Paragraph-System der Startseite platziert. Die resultierende Struktur im CRX ist unten dargestellt.

![Ressourcenstruktur des responsiven Bildes im CRX](assets/chapter-1/responsive-image-crx.png)

*Ressourcenstruktur des responsiven Bildes im CRX*

<br> 

Das Komponenten-Markup wird wie folgt gerendert:

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

und … damit haben wir unsere schön verkapselte Komponente fertiggestellt.

#### Responsive Bildkomponente in Aktion

Jetzt fordert jemand die Seite und die Assets über den Dispatcher an. Dies führt zu Dateien im Dispatcher-Dateisystem, wie unten dargestellt:

![Zwischengespeicherte Struktur der verkapselten responsiven Bildkomponente](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Zwischengespeicherte Struktur der verkapselten responsiven Bildkomponente*

<br> 

Angenommen, Benutzende laden eine neue Version der beiden Blumenbilder ins DAM hoch und aktivieren sie. AEM sendet dann die entsprechende Invalidierungsanfrage für

`/content/dam/flower.jpg`

und

`/content/dam/flower-mobile.jpg`

an den Dispatcher. Diese Anfragen sind jedoch vergebens. Die Inhalte wurden als Dateien unterhalb der Unterstruktur der Komponente zwischengespeichert. Diese Dateien sind jetzt veraltet, werden aber weiterhin bei Anfragen bereitgestellt.

![Strukturfehler führt zu veraltetem Inhalt](assets/chapter-1/structure-mismatch.png)

*Strukturfehler führt zu veraltetem Inhalt*

<br> 

Bei diesem Ansatz gibt es noch einen weiteren Vorbehalt. Beachten Sie, dass Sie dieselbe flower.jpg auf mehreren Seiten verwenden. Anschließend wird dasselbe Asset unter mehreren URLs oder Dateien zwischengespeichert.

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Jedes Mal, wenn eine neue und nicht zwischengespeicherte Seite angefragt wird, werden die Assets von AEM unter verschiedenen URLs abgerufen. Keine Dispatcher-Zwischenspeicherung und keine Browser-Zwischenspeicherung können die Bereitstellung beschleunigen.

#### Wo das Spooler-Muster glänzt

Es gibt eine natürliche Ausnahme, in der dieses Muster auch in seiner einfachen Form nützlich ist: Wenn die Binärdatei in der Komponente selbst gespeichert ist – und nicht im DAM. Dies ist jedoch nur für Bilder nützlich, die einmal auf der Website verwendet werden. Wenn Assets nicht im DAM gespeichert werden, ist die Verwaltung Ihrer Assets schwierig. Stellen Sie sich vor, Ihre Nutzungslizenz für ein bestimmtes Asset läuft aus. Wie können Sie herausfinden, für welche Komponenten Sie das Asset verwendet haben?

Sehen Sie? Das „M“ in DAM steht für „Management“ – wie in Digital Asset Management. Sie wollen diese Funktion nicht verschenken.

#### Zusammenfassung

Aus der Perspektive einer AEM-Entwicklerperson sah das Muster überaus elegant aus. Aber wenn man den Dispatcher in die Gleichung mit einbezieht, könnte man zustimmen, dass der naive Ansatz nicht ausreichend ist.

Wir überlassen es Ihnen, zu entscheiden, ob es sich um ein Muster oder ein Anti-Muster handelt. Und vielleicht haben Sie bereits einige gute Ideen im Kopf, wie Sie die oben genannten Probleme umgehen können? Gut. Dann sollten Sie unbedingt wissen wollen, wie andere Projekte diese Probleme gelöst haben.

### Beheben häufiger Dispatcher-Probleme

#### Übersicht

Lassen Sie uns darüber sprechen, wie das etwas Cache-freundlicher hätte umgesetzt werden können. Es gibt verschiedene Optionen. Manchmal kann man sich nicht für die beste Lösung entscheiden. Vielleicht kommen Sie in ein bereits laufendes Projekt und haben nur ein begrenztes Budget, um das aktuelle „Cache-Problem“ zu beheben, aber nicht genug, um ein umfassendes Refactoring durchzuführen. Oder Sie haben ein Problem, das komplexer ist als die Beispielbildkomponente.

In den folgenden Abschnitten werden die Grundsätze und Vorbehalte erläutert.

Auch dies basiert auf realen Erfahrungen. Wir haben all diese Muster bereits in freier Wildbahn gefunden, es handelt sich also nicht um eine akademische Übung. Deshalb zeigen wir Ihnen einige Anti-Muster, sodass Sie die Möglichkeit haben, aus Fehlern zu lernen, die andere bereits gemacht haben.

#### Cache-Killer

>[!WARNING]
>
>Dies ist ein Anti-Muster. Verwenden Sie es nicht. Niemals.

Haben Sie schon einmal Abfrageparameter gesehen wie `?ck=398547283745`? Sie werden als Cache-Killer („ck“) bezeichnet. Wenn Sie Abfrageparameter hinzufügen, wird die Ressource nicht zwischengespeichert. Wenn Sie außerdem eine zufällige Nummer als Parameterwert hinzufügen (z. B. „398547283745“), wird die URL eindeutig und Sie stellen sicher, dass auch kein anderer Cache zwischen AEM und Ihrem Bildschirm zwischengespeichert werden kann. Übliche Verdächtige wären ein „Varnish“-Cache vor dem Dispatcher, ein CDN oder auch der Browser-Cache. Nochmal: Machen Sie das nicht! Sie möchten, dass Ihre Ressourcen so oft und so lange wie möglich zwischengespeichert werden. Der Cache ist dein Freund. Töte deine Freunde nicht.

#### Automatische Invalidierung

>[!WARNING]
>
>Dies ist ein Anti-Muster. Vermeiden Sie seine Verwendung für digitale Assets. Versuchen Sie, die Standardkonfiguration des Dispatchers beizubehalten, wonach sich die automatische Invalidierung nur auf „.html“-Dateien bezieht.

Kurzfristig können Sie der Konfiguration für die automatische Invalidierung im Dispatcher „.jpg“ und „.png“ hinzufügen. Das bedeutet, dass bei jeder Invalidierung jedes „.jpg“, „.png“ und „.html“ erneut gerendert werden muss.

Dieses Muster ist sehr einfach zu implementieren, wenn sich Geschäftsinhaberinnen und -inhaber darüber beschweren, dass ihre Änderungen nicht schnell genug auf der Live-Site gezeigt werden. Aber das verschafft Ihnen nur etwas Zeit, um eine ausgereiftere Lösung zu finden.

Vergewissern Sie sich, dass Sie die enormen Auswirkungen auf die Leistung verstehen. Dies wird Ihre Website erheblich verlangsamen und könnte sogar die Stabilität beeinträchtigen – wenn Ihre Website eine Website mit hoher Auslastung und häufigen Änderungen ist – z. B. ein Nachrichtenportal.

#### URL-Fingerabdruck

Ein URL-Fingerabdruck sieht wie ein Cache-Mörder aus. Aber das ist es nicht. Es handelt sich nicht um eine zufällige Zahl, sondern um einen Wert, der den Inhalt der Ressource charakterisiert. Dies kann ein Hash des Ressourceninhalts oder – noch einfacher – ein Zeitstempel für den Moment sein, an dem die Ressource hochgeladen, bearbeitet oder aktualisiert wurde.

Ein Unix-Zeitstempel ist ausreichend für eine reale Implementierung. Für die bessere Lesbarkeit verwenden wir in diesem Tutorial ein lesbareres Format: `2018 31.12 23:59 or fp-2018-31-12-23-59`.

Der Fingerabdruck darf nicht als Abfrageparameter verwendet werden, da URLs mit Abfrageparametern nicht zwischengespeichert werden können. Sie können einen Selektor oder das Suffix für den Fingerabdruck verwenden.

Nehmen wir an, die Datei `/content/dam/flower.jpg` hat als `jcr:lastModified`-Datum den 31. Dezember 2018, 23:59 Uhr. Die URL mit dem Fingerabdruck lautet `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Diese URL bleibt stabil, solange die referenzierte Ressourcendatei (`flower.jpg`) nicht geändert wird. Sie kann also unbegrenzt lange zwischengespeichert werden und ist kein Cache-Mörder.

Beachten Sie, dass diese URL von der responsiven Bildkomponente erstellt und bereitgestellt werden muss. Es handelt sich um keine vordefinierte AEM-Funktion.

Das ist das Grundkonzept. Es gibt jedoch einige Details, die leicht übersehen werden können.

In unserem Beispiel wurde die Komponente um 23:59 Uhr gerendert und zwischengespeichert. Jetzt wurde das Bild, sagen wir, um 00:00 Uhr geändert. Die Komponente _würde_ eine neue Fingerabdruck-URL im Markup generieren.

Vielleicht denken Sie, sie _sollte_ es … aber sie tut es nicht. Da nur die Binärdatei des Bildes geändert und die einschließende Seite nicht angerührt wurde, ist kein erneutes Rendern des HTML-Markups erforderlich. Der Dispatcher stellt also die Seite mit dem alten Fingerabdruck und damit die alte Version des Bildes bereit.

![Aktuellere Bildkomponente als das referenzierte Bild, es wird kein neuer Fingerabdruck gerendert.](assets/chapter-1/recent-image-component.png)

*Aktuellere Bildkomponente als das referenzierte Bild, es wird kein neuer Fingerabdruck gerendert.*

<br> 

Wenn Sie jetzt die Startseite (oder eine andere Seite dieser Site) erneut aktivieren, wird die Stat-Datei aktualisiert. Der Dispatcher betrachtet die Datei „home.html“ als veraltet und rendert sie erneut mit einem neuen Fingerabdruck in der Bildkomponente.

Aber wir haben die Startseite nicht aktiviert, nicht wahr? Warum sollten wir auch eine Seite aktivieren, die wir gar nicht angerührt haben? Außerdem haben wir vielleicht gar nicht genügend Rechte, um Seiten zu aktivieren, oder der Genehmigungs-Workflow ist so lang und zeitaufwendig, dass wir das einfach nicht kurzfristig tun können. Was ist also zu tun?

#### Das Tool der faulen Admins – Verringern der Stat-Dateiebenen

>[!WARNING]
>
>Dies ist ein Anti-Muster. Verwenden Sie es nur kurzfristig, um etwas Zeit zu gewinnen und eine ausgereiftere Lösung zu finden.

Faule Admis „_setzen die automatische Invalidierung auf jpgs und die Stat-Datei-Ebene meist auf null – dies hilft immer beim Caching von Problemen aller Art_“. Sie finden solche Ratschläge in technischen Foren, und sie helfen Ihnen bei Ihrem Invalidierungsproblem.

Bis jetzt haben wir nicht über die Stat-Datei-Ebene geredet. Im Grunde funktioniert die automatische Invalidierung nur für Dateien in derselben Unterstruktur. Das Problem besteht jedoch darin, dass Seiten und Assets normalerweise nicht in derselben Unterstruktur vorhanden sind. Seiten befinden sich irgendwo unter `/content/mysite` und Assets unter `/content/dam`.

Die Statfile-Ebene definiert, wo und wie verzweigt die Stammknoten der Unterstrukturen sind. Im obigen Beispiel wäre die Ebene „2“ (1=/content, 2=/mysite,dam)

Die Idee, die Statfile-Ebene auf 0 zu reduzieren, besteht im Grunde darin, die gesamte /content-Struktur als die einzige Unterstruktur zu definieren, damit Seiten und Assets in derselben Domain für die automatische Invalidierung live sind. Also hätten wir nur eine große Struktur auf der Ebene (Basisverzeichnis „/“). Dadurch werden jedoch alle Sites auf dem Server automatisch invalidiert, sobald etwas veröffentlicht wird – auch überhaupt nicht verwandte Sites. Vertrauen Sie uns: Dies ist langfristig eine schlechte Idee, da Ihre gesamte Cache-Trefferrate stark beeinträchtigt sein wird. Sie können nur hoffen, dass Ihre AEM-Server über ausreichend Firepower verfügen, um ohne Cache ausgeführt zu werden.

Sie werden die Vorteile tieferer Statfile-Ebenen bald verstehen.

#### Implementieren eines benutzerdefinierten Invalidierungs-Agenten

Der Dispatcher muss irgendwie informiert werden, dass die HTML-Seiten ungültig gemacht werden sollen, wenn ein „.jpg“ oder „.png“ geändert wurde, damit das erneute Rendern mit einer neuen URL möglich ist.

Was wir in Projekten gesehen haben, sind zum Beispiel spezielle Replikationsagenten im Publishing-System, die Invalidierungsanfragen für eine Site senden, sobald ein Bild dieser Site veröffentlicht wurde.

Hier ist es sehr hilfreich, den Pfad der Site per Namenskonvention vom Pfad des Assets abzuleiten.

Im Allgemeinen ist es empfehlenswert, die Sites und Asset-Pfade wie folgt anzugleichen:

**Beispiel**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

Auf diese Weise kann Ihr benutzerdefinierter Dispatcher-Flushing-Agent einfach eine Anfrage zur Invalidierung an /content/site-a senden, wenn eine Änderung an `/content/dam/site-a` festgestellt wird.

Es spielt keine Rolle, welchen Pfad Sie dem Dispatcher zur Invalidierung vorschreiben – solange er sich auf derselben Site und in derselben Unterstruktur befindet. Sie müssen nicht einmal einen echten Ressourcenpfad verwenden. Es kann auch „virtuell“ sein:

```
GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy
```

![](assets/chapter-1/resource-path.png)

1. Ein Listener im Publishing-System wird ausgelöst, wenn sich eine Datei im DAM ändert

2. Der Listener sendet eine Invalidierungsanfrage an den Dispatcher. Es spielt keine Rolle, welcher Pfad bei der automatischen Invalidierung gesendet wird, es sei denn, er befindet sich auf der Homepage der Site – oder genauer gesagt auf der Statfile-Ebene der Site.

3. Die Statfile wird aktualisiert.

4. Wenn die Homepage das nächste Mal angefordert wird, wird sie erneut gerendert. Der neue Fingerabdruck bzw. das neue Datum wird aus der lastModified-Eigenschaft des Bildes als zusätzliche Auswahl übernommen

5. Dadurch wird implizit ein Verweis auf ein neues Bild erstellt

6. Wenn das Bild tatsächlich angefordert wird, wird eine neue Ausgabedarstellung erstellt und im Dispatcher gespeichert


#### Notwendige Bereinigung

Puh. Beendet. Hurra!

Noch nicht ganz.

Der Pfad

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

bezieht sich nicht auf die invalidierten Ressourcen. Merken? Wir haben nur eine Platzhalterressource ungültig gemacht und uns auf die automatische Invalidierung verlassen, sodass „home“ als ungültig angesehen werden kann. Das Bild selbst wird vielleicht niemals _physisch_ gelöscht. Der Cache wächst also immer weiter. Wenn Bilder geändert und aktiviert werden, erhalten sie im Dateisystem des Dispatchers neue Dateinamen.

Es gibt drei Probleme mit dem Umgehen des physischen Löschens der zwischengespeicherten Dateien und deren unbegrenzter Beibehaltung:

1. Es wird eindeutig Speicherkapazität verschwendet. Speicherplatz ist allerdings in den letzten Jahren immer billiger geworden. Über die letzten Jahre sind aber auch Bildauflösungen höher und Dateien größer geworden – mit der Einführung von Retina-Displays, die kristallscharfe Bilder bieten.

2. Festplatten sind billiger geworden, Speicherplatz jedoch nicht unbedingt. Es ist die Tendenz zu beobachten, nicht (billigen) Metall-HDD-Speicher zu nutzen, sondern beim Rechenzentrumsanbieter gemieteten virtuellen Speicher auf einem NAS. Diese Art von Speicher ist etwas zuverlässiger und skalierbarer, aber auch etwas teurer. Vielleicht sollten Sie den Speicherplatz nicht verschwenden, indem Sie veralteten Müll speichern. Dies betrifft nicht nur den primären Speicher, sondern auch Backups. Wenn Sie über eine native Sicherungslösung verfügen, können Sie die Cache-Verzeichnisse möglicherweise nicht ausschließen. Und so speichern Sie auch völlig unnötige Daten.

3. Noch schlimmer: Sie haben vielleicht nur für eine begrenzte Zeit Nutzungslizenzen für bestimmte Bilder erworben – so lange Sie diese eben benötigten. Wenn Sie ein Bild auch noch nach Ablauf der Lizenz speichern, kann dies als Urheberrechtsverletzung betrachtet werden. Möglicherweise verwenden Sie das Bild nicht mehr auf Ihren Web-Seiten, aber Google wird es trotzdem finden.

Schließlich werden Sie einen Cronjob für die Systemverwaltung entwickeln, um alle Dateien zu bereinigen, die beispielsweise älter sind als eine Woche, und um so diese Art von Durcheinander unter Kontrolle zu halten.

#### Missbrauch von URL-Fingerabdrücken für Denial-of-Service-Angriffe

Aber Moment – diese Lösung hat einen weiteren Fehler:

Wir missbrauchen praktisch einen Selektor als Parameter: „fp-2018-31-12-23-59“ wird dynamisch als eine Art „Cache-Killer“ generiert. Aber vielleicht fängt irgendwo ein gelangweilter Teenager oder ein außer Kontrolle geratener Suchmaschinen-Crawler an, diese Seiten anzufordern:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Jede Anfrage umgeht den Dispatcher, was zu Last in einer Veröffentlichungsinstanz führt. Und es wird – noch schlimmer – eine entsprechende Datei im Dispatcher erstellt.

Anstatt also den Fingerabdruck einfach nur als Cache-Killer zu verwenden, müssten Sie das jcr:lastModified-Datum des Bildes überprüfen und einen 404-Fehler zurückgeben, wenn es sich nicht um das erwartete Datum handelt. Dies braucht eine gewisse Zeit und erfordert CPU-Zyklen auf dem Veröffentlichungssystem, was genau das ist, was Sie eigentlich verhindern wollten.

#### Nachteile von URL-Fingerabdrücken bei häufig erscheinenden neuen Versionen

Sie können das Fingerabdruckschema nicht nur für Assets aus dem DAM-System verwenden, sondern auch für JS- und CSS-Dateien und zugehörige Ressourcen.

Das Modul für [versionierte Client-Bibliotheken](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) nutzt diesen Ansatz.

Hier könnten Sie jedoch mit einem weiteren Nachteil von URL-Fingerabdrücken konfrontiert werden: Die URL wird mit dem Inhalt verknüpft. Sie können den Inhalt nicht ändern, ohne auch die URL zu ändern (also das Änderungsdatum zu aktualisieren). Darauf sind die Fingerabdrücke von vornherein ausgelegt. Aber angenommen, dass Sie eine neue Version mit neuen CSS- und JS-Dateien und damit neuen URLs mit neuen Fingerabdrücken herausgeben. Alle Ihre HTML-Seiten weisen immer noch Verweise auf die alten Fingerabdruck-URLs auf. Damit die neue Version konsistent funktioniert, müssen Sie alle HTML-Seiten gleichzeitig invalidieren, um ein erneutes Rendering mit Verweisen auf die neuen Fingerabdruckdateien durchzusetzen. Wenn Sie über mehrere Sites verfügen, die auf denselben Bibliotheken basieren, kann der Aufwand zum erneuten Rendering enorm sein – wobei Sie nicht die `statfiles` nutzen können. Stellen Sie sich nach einem Rollout also auf Lastspitzen in Ihren Veröffentlichungssystemen ein. Sie könnten eine Blau/Grün-Implementierung mit Cache-Warming oder vielleicht einen Ihrem Dispatcher vorgeschalteten TTL-basierten Cache in Erwägung ziehen … Die Möglichkeiten sind endlos.

#### Kurze Pause

Wow, das sind jede Menge Details, die beachtet werden müssen, nicht wahr? Einfach zu verstehen, zu testen und zu debuggen ist das Ganze auch nicht. Und all das für eine scheinbar elegante Lösung. Aber sie ist tatsächlich elegant – aber nur aus AEM-Perspektive. In Verbindung mit dem Dispatcher wird es schwierig.

Und trotzdem – ein grundlegender Nachteil wird nicht behoben: Wenn ein Bild mehrfach auf verschiedenen Seiten verwendet wird, wird es unter diesen Seiten zwischengespeichert. Ohne nennenswerte Caching-Synergien.

Im Allgemeinen sind URL-Fingerabdrücke ein gutes Werkzeug für Ihr Toolkit, aber Sie müssen es mit Vorsicht anwenden, da es neue Probleme verursachen kann und nur einige wenige der vorhandenen Probleme löst.

Das war nun ein langes Kapitel. Aber wir haben dieses Muster so oft gesehen, dass wir es für notwendig hielten, Ihnen einen vollständigen Überblick mit allen Vor- und Nachteilen zu geben. URL-Fingerabdrücke lösen einige inhärente Probleme im Spooler-Muster, aber der Aufwand für die Implementierung ist recht hoch und Sie müssen auch andere – einfachere – Lösungen in Betracht ziehen. Wir empfehlen, immer zu überprüfen, ob Sie Ihre URLs auf den Pfaden für bereitgestellte Ressourcen basieren und auf Zwischenkomponenten verzichten können. Wir kommen im nächsten Kapitel darauf zurück.

##### Runtime-Auflösung von Abhängigkeiten

Die Runtime-Auflösung von Abhängigkeiten ist ein Konzept, das wir in einem Projekt in Erwägung gezogen haben.  Bei näherer Betrachtung wurde es aber ziemlich komplex, sodass wir uns gegen eine Implementierung entschieden haben.

Die Grundidee lautet:

Der Dispatcher weiß nichts über die Abhängigkeiten von Ressourcen. Da ist einfach nur ein Haufen einzelner Dateien mit wenig Semantik.

AEM weiß ebenfalls wenig über Abhängigkeiten. Es fehlt eine richtige Semantik oder ein „Abhängigkeits-Tracker“.

AEM hat Kenntnis von einigen Verweisen. Diese Informationen werden verwendet, um Sie zu warnen, wenn Sie versuchen, eine referenzierte Seite oder ein referenziertes Asset zu löschen oder zu verschieben. Dies geschieht durch Abfrage der internen Suche beim Löschen eines Assets. Inhaltsreferenzen haben eine sehr spezifische Form. Hierbei handelt es sich um Pfadausdrücke, die mit „/content“ beginnen. So können sie einfach im Volltext indiziert und bei Bedarf abgefragt werden.

In unserem Fall bräuchten wir einen benutzerdefinierten Replikationsagenten auf dem Veröffentlichungssystem, der eine Suche nach einem bestimmten Pfad auslöst, wenn sich dieser Pfad geändert hat.

Sagen wir mal:

`/content/dam/flower.jpg`

Hat sich bei der Veröffentlichung geändert. Der Agent würde eine Suche nach „/content/dam/flower.jpg“ auslösen und alle Seiten finden, die auf diese Bilder verweisen.

Anschließend kann eine Reihe von Invalidierungsanfragen an den Dispatcher gestellt werden. Eine für jede Seite mit dem Asset.

Theoretisch sollte das funktionieren. Aber nur für Abhängigkeiten der ersten Ebene. Sie sollten dieses Schema nicht auf Abhängigkeiten mit mehreren Ebenen anwenden, z. B. wenn Sie das Bild in einem Erlebnisfragment verwenden, das auf einer Seite verwendet wird. Tatsächlich glauben wir, dass der Ansatz zu komplex ist. Zudem könnte es Laufzeitprobleme geben. Im Allgemeinen ist der beste Ratschlag, keine teure Berechnung in Ereignis-Handlern durchzuführen. Besonders die Suche kann sehr teuer werden.

##### Zusammenfassung

Wir hoffen, dass wir das Spooler-Muster gründlich genug besprochen haben, um Ihnen bei der Entscheidung zu helfen, wann es in Ihrer Implementierung verwendet und nicht verwendet werden soll.

## Vermeiden von Dispatcher-Problemen

### Ressourcenbasierte URLs

Eine sehr viel elegantere Möglichkeit, das Abhängigkeitsproblem zu lösen, besteht darin, überhaupt keine Abhängigkeiten zu haben. Vermeiden Sie künstliche Abhängigkeiten, die auftreten, wenn eine Ressource verwendet wird, um einfach eine andere zu projizieren (wie wir es im letzten Beispiel getan haben). Versuchen Sie, Ressourcen so oft wie möglich als „Einzelentitäten“ zu betrachten.

Unser Beispiel lässt sich leicht lösen:

![Selektieren des Bildes mit einem Servlet, das an das Bild gebunden ist, nicht mit der Komponente.](assets/chapter-1/spooling-image.png)

*Selektieren des Bildes mit einem Servlet, das an das Bild gebunden ist, nicht mit der Komponente.*

<br> 

Wir verwenden die ursprünglichen Ressourcenpfade für Assets, um die Daten zu rendern. Wenn das Originalbild unverändert wiedergegeben werden muss, können wir einfach die Standard-Renderer von AEM für Assets verwenden.

Wenn wir eine spezielle Verarbeitung für eine bestimmte Komponente durchführen müssen, registrieren wir ein dediziertes Servlet für diesen Pfad und ein Selektor, um die Umwandlung im Namen der Komponente durchzuführen. Das haben wir hier mit der „.respi.“ vorbildlich gemacht. selector. Es ist ratsam, die Selektornamen, die im globalen URL-Raum verwendet werden (z. B. `/content/dam`), im Auge zu behalten und eine gute Namenskonvention zu haben, um Namenskonflikte zu vermeiden.

Übrigens: es gibt keine Probleme mit der Code-Kohärenz. Das Servlet kann im selben Java-Paket wie das Komponenten-Sling-Modell definiert werden.

Wir können sogar zusätzliche Selektoren im globalen Raum verwenden, z. B.:

`/content/dam/flower.respi.thumbnail.jpg`

Einfach, nicht wahr? Warum kommen die Leute dann auf komplizierte Muster wie den Spooler?

Nun, wir könnten das Problem lösen, indem wir den internen Inhaltsverweis vermeiden, da die äußere Komponente nur wenig Wert oder Informationen zur Darstellung der inneren Ressource hinzufügt, sodass sie leicht in einer Reihe von statischen Selektoren codiert werden könnte, die die Darstellung einer einzelnen Ressource steuern.

Es gibt jedoch eine Klasse von Fällen, die Sie nicht einfach mit einer ressourcenbasierten URL lösen können. Wir nennen diesen Falltyp „Parameter Injection Components“ und besprechen ihn im nächsten Kapitel.

### Parameter Injection Components

#### Übersicht

Der Spooler im letzten Kapitel war nur ein dünner Wrapper um eine Ressource herum. Er verursachte mehr Schwierigkeiten, als bei der Lösung des Problems zu helfen.

Wir könnten dieses Wrapping einfach ersetzen, indem wir einen einfachen Selektor verwenden und ein entsprechendes Servlet hinzufügen, um solche Anfragen zu bedienen.

Was aber, wenn die „respi“-Komponente mehr ist als nur ein Proxy. Was passiert, wenn die Komponente tatsächlich zum Rendern der Komponente beiträgt?

Lassen Sie uns eine kleine Erweiterung unserer „respi“-Komponente einführen, die sozusagen ein kleiner Game-Changer sein kann. Auch hier werden wir zunächst einige naive Lösungen zur Bewältigung der neuen Herausforderungen vorstellen und zeigen, wo sie versagen.

#### Die Komponente „Respi2“

Die „respi2“-Komponente ist eine Komponente, die ein responsives Bild anzeigt, ebenso wie die „respi“-Komponente. Aber sie hat einen leichten Zusatznutzen,

![CRX-Struktur: respi2-Komponente, die eine Qualitätseigenschaft zur Bereitstellung hinzufügt](assets/chapter-1/respi2.png)

*CRX-Struktur: respi2-Komponente, die eine Qualitätseigenschaft zur Bereitstellung hinzufügt*

<br> 

Die Bilder sind JPEGs, und JPEGs können komprimiert werden. Wenn Sie ein JPEG-Bild komprimieren, tauschen Sie die Qualität für die Dateigröße aus. Die Komprimierung wird als numerischer Qualitätsparameter im Bereich von „1“ bis „100“ definiert. „1“ bedeutet „klein, aber schlechte Qualität“, „100“ steht für „hervorragende Qualität, aber große Dateien“. Was ist also der perfekte Wert?

Wie bei allen IT-Dingen lautet die Antwort: „Es kommt darauf an.“

Hier kommt es auf das Motiv an. Motive mit kontrastreichen Rändern wie Motive mit geschriebenem Text, Fotos von Gebäuden, Illustrationen, Skizzen oder Fotos von Produktverpackungen (mit scharfen Konturen und Text darauf) fallen normalerweise in diese Kategorie. Motive mit weicheren Farb- und Kontrastübergängen wie Landschaften oder Porträts können etwas mehr komprimiert werden, ohne dass die Qualität verloren geht. Naturfotos fallen normalerweise in diese Kategorie.

Je nachdem, wo das Bild verwendet wird, können Sie auch einen anderen Parameter verwenden. Eine kleine Miniaturansicht in einem Teaser kann eine bessere Komprimierung vertragen als das gleiche Bild in einem bildschirmbreiten Hero-Banner. Das bedeutet, dass der Qualitätsparameter nicht auf das Bild, sondern auf das Bild und den Kontext zurückzuführen ist. Und nach dem Geschmack der Autorin bzw. des Autors.

Kurz gesagt: Es gibt keine perfekte Einstellung für alle Bilder. Es gibt keine Einheitsgrößen. Am besten entscheidet die Autorin bzw. der Autor. Sie bzw. er wird den Parameter „Qualität“ als Eigenschaft in der Komponente anpassen, bis sie bzw. er mit der Qualität zufrieden ist und nicht weiter gehen wird, um die Bandbreite nicht zu opfern.

Wir haben jetzt eine Binärdatei im DAM und eine Komponente, die eine Qualitätseigenschaft bereitstellt. Wie sollte die URL aussehen? Welche Komponente ist für das Spooling verantwortlich?

#### Naiver Ansatz 1: Übergeben von Eigenschaften als Abfrageparameter

>[!WARNING]
>
>Dies ist ein Anti-Muster. Verwenden Sie es nicht.

Im letzten Kapitel sah die von der Komponente gerenderte Bild-URL wie folgt aus:

`/content/dam/flower.respi.jpg`

Alles, was fehlt, ist der Wert für die Qualität. Die Komponente weiß, welche Eigenschaft von der Autorin bzw. vom Autor eingegeben wird. Sie kann einfach als Abfrageparameter an das Bild-Rendering-Servlet übergeben werden, wenn das Markup gerendert wird, z. B. `flower.respi2.jpg?quality=60`:

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

Das ist eine schlechte Idee. Merken? Anfragen mit Abfrageparametern können nicht zwischengespeichert werden.

#### Naiver Ansatz 2: Übergeben von zusätzlichen Informationen als Selektor

>[!WARNING]
>
>Dies könnte zu einem Anti-Muster werden. Benutzen Sie es sorgfältig.

![Übergeben von Komponenteneigenschaften als Selektoren](assets/chapter-1/passing-component-properties.png)

*Übergeben von Komponenteneigenschaften als Selektoren*

<br> 

Dies ist eine leichte Variation der letzten URL. Nur dieses Mal verwenden wir einen Selektor, um die Eigenschaft an das Servlet zu übergeben, damit das Ergebnis zwischenspeicherbar ist:

`/content/dam/flower.respi.q-60.jpg`

Das ist viel besser, aber erinnern Sie sich an das fiese Skript-Kind aus dem letzten Kapitel, das nach solchen Mustern Ausschau hält? Er würde sehen, wie weit er mit dem Überspringen von Werten kommen kann:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Dadurch wird der Cache umgangen und das Veröffentlichungssystem wird geladen. Es könnte also eine schlechte Idee sein. Sie können dies umgehen, indem Sie nur eine kleine Untergruppe von Parametern filtern. Sie möchten nur `q-20, q-40, q-60, q-80, q-100` zulassen.

#### Filtern ungültiger Anfragen bei Verwendung von Selektoren

Die Verringerung der Anzahl der Selektoren war ein guter Anfang. Als Faustregel gilt, dass Sie die Anzahl der gültigen Parameter immer auf ein absolutes Minimum beschränken sollten. Wenn Sie das geschickt anstellen, können Sie sogar eine Web-Applikations-Firewall außerhalb von AEM nutzen, indem Sie einen statischen Satz von Filtern verwenden, ohne das zugrunde liegende AEM-System zu kennen, um Ihre Systeme zu schützen:

```
Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
       \.respi\.q-(20|40|60|80|100)\.jpg
```

Wenn Sie keine Web-Applikations-Firewall haben, müssen Sie im Dispatcher oder in AEM selbst filtern. Wenn Sie es in AEM tun, stellen Sie bitte sicher, dass:

1. Der Filter überaus effizient implementiert wird, ohne dass der Zugriff auf CRX zu aufwändig ist und Speicher und Zeit verschwendet werden.

2. Der Filter auf die Fehlermeldung „404 – Nicht gefunden“ antwortet.

Lassen Sie uns noch einmal den letzten Punkt betonen. Die HTTP-Konversation würde wie folgt aussehen:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Wir haben auch Implementierungen gesehen, die ungültige Parameter gefiltert, aber ein gültiges Fallback-Rendering zurückgegeben haben, wenn ein ungültiger Parameter verwendet wurde. Nehmen wir einmal an, wir erlauben nur Parameter von 20 bis 100. Die dazwischen liegenden Werte sind den gültigen Werten zugeordnet. Das bedeutet,

`q-41, q-42, q-43, …`

würde immer mit dem gleichen Bild antworten, das q-40 hätte:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Dieser Ansatz hilft überhaupt nicht. Diese Anfragen sind tatsächlich gültige Anfragen.  Sie verbrauchen Verarbeitungsleistung und nehmen Speicherplatz im Cache-Verzeichnis auf dem Dispatcher auf.

Besser ist es, `301 – Moved permanently` zurückzugeben:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Hier teilt AEM dem Browser mit. „Ich habe keine `q-41`. Aber hey – Sie können mich nach `q-40` fragen“.

Dies fügt eine zusätzliche Anfrage-Antwort-Schleife zur Konversation hinzu, was zwar einen gewissen Mehraufwand verursacht, aber billiger ist als die vollständige Verarbeitung auf `q-41`. Außerdem können Sie die Datei nutzen, die bereits unter `q-40` zwischengespeichert ist. Sie müssen jedoch verstehen, dass 302 Antworten nicht im Dispatcher zwischengespeichert werden. Wir sprechen von Logik, die im AEM ausgeführt wird. Immer wieder. Also machen Sie es besser schnell und schlank.

Uns persönlich gefällt die 404-Antwort am besten. Sie macht sehr deutlich, was passiert. Und hilft beim Erkennen von Fehlern auf Ihrer Website bei der Analyse von Protokolldateien. 301er können beabsichtigt sein, wobei 404er immer analysiert und eliminiert werden sollten.

## Sicherheit – Ausflug

### Filtern von Anfragen

#### Wo es sich am besten filtern lässt

Am Ende des letzten Kapitels haben wir darauf hingewiesen, dass der eingehende Traffic nach bekannten Selektoren gefiltert werden muss. Damit bleibt die Frage: Wo soll ich die Anfragen filtern?

Nun, es kommt darauf an. Je früher, desto besser.

#### Firewalls für Web-Applikationen

Wenn Sie über eine Web-Applikations-Firewall (WAF) verfügen, die für Web-Sicherheit entwickelt wurde, sollten Sie diese Funktionen unbedingt nutzen. Sie können jedoch feststellen, dass die WAF von Personen betrieben wird, die nur über eingeschränkte Kenntnisse Ihrer Inhaltsanwendung verfügen und entweder gültige Anfragen filtern oder zu viele schädliche Anfragen übergeben. Vielleicht stellen Sie fest, dass die Mitarbeitenden, die die WAF betreiben, einer anderen Abteilung mit anderen Schichten und Veröffentlichungsplänen zugewiesen sind, dass die Kommunikation nicht so eng ist wie mit Ihren direkten Teamkollegen und dass Sie die Änderungen nicht immer rechtzeitig erhalten, was letztendlich bedeutet, dass Ihre Entwicklung und die Geschwindigkeit der Inhalte leiden.

Am Ende haben Sie vielleicht ein paar allgemeine Regeln oder sogar eine Blockierungsliste, die nach Ihrem Bauchgefühl noch verschärft werden könnten.

#### Dispatcher- und Publish-Filter

Der nächste Schritt besteht darin, URL-Filterregeln im Apache-Kern und/oder im Dispatcher hinzuzufügen.

Hier haben Sie nur Zugriff auf URLs. Sie sind auf musterbasierte Filter beschränkt. Wenn Sie eine eher inhaltsbasierte Filterung einrichten müssen (z. B. nur Dateien mit einem korrekten Zeitstempel zulassen) oder einen Teil der Filterung auf Ihrem Author kontrolliert werden soll, müssen Sie letztendlich so etwas wie einen benutzerdefinierten Servlet-Filter schreiben.

#### Überwachung und Debugging

In der Praxis werden Sie etwas Sicherheit auf jeder Ebene haben. Stellen Sie jedoch sicher, dass Sie über Mittel verfügen, um herauszufinden, auf welcher Ebene eine Anfrage herausgefiltert wird. Stellen Sie sicher, dass Sie direkten Zugriff auf das Veröffentlichungssystem, den Dispatcher und die Protokolldateien auf der WAF haben, um herauszufinden, welcher Filter in der Kette Anfragen blockiert.

### Selektoren und Selektorverbreitung

Der Ansatz mit „selector-parameters“ im letzten Kapitel ist schnell und einfach und kann die Entwicklungszeit neuer Komponenten beschleunigen, hat jedoch Einschränkungen.

Das Festlegen einer „quality“-Eigenschaft ist nur ein einfaches Beispiel. Angenommen, das Servlet erwartet auch, dass Parameter für „width“ vielseitiger sind.

Sie können die Anzahl der gültigen URLs verringern, indem Sie die Anzahl der möglichen Selektorwerte reduzieren. Sie können dies auch mit der Breite tun:

quality = q-20, q-40, q-60, q-80, q-100

width = w-100, w-200, w-400, w-800, w-1000, w-1200

Alle Kombinationen sind nun gültige URLs:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Jetzt haben wir bereits 5 x 6 = 30 gültige URLs für eine Ressource. Jede zusätzliche Eigenschaft erhöht die Komplexität. Und es kann Eigenschaften geben, die sich nicht auf eine vernünftige Anzahl von Werten reduzieren lassen.

Dieser Ansatz hat also auch Grenzen.

#### Versehentliches Offenlegen einer API

Was passiert hier? Wenn wir genau hinschauen, sehen wir, dass wir uns allmählich von einer statisch gerenderten zu einer hochdynamischen Website bewegen. Wir stellen versehentlich eine Bild-Rendering-API dem Browser der Kundschaft vor, die eigentlich nur von Autorinnen bzw. Autoren verwendet werden sollte.

Das Festlegen der Qualität und Größe eines Bildes sollte von der Autorin bzw. vom Autor vorgenommen werden, die bzw. der die Seite bearbeitet. Die gleichen Funktionen, die von einem Servlet verfügbar gemacht werden, können als Funktion oder als Vektor für einen DoS-Angriffe (Denial of Service) angesehen werden. Was es tatsächlich ist, hängt vom Kontext ab. Wie geschäftskritisch ist die Website? Wie viel Last ist auf den Servern? Wie viel Spielraum ist noch vorhanden? Wie viel Budget haben Sie für die Implementierung? Sie müssen diese Faktoren ausgleichen. Sie sollten sich der Vor- und Nachteile bewusst sein.

## Das Spooler-Muster – überarbeitet und rehabilitiert

### So vermeidet der Spooler die Anzeige der API

Im letzten Kapitel haben wir das Spooler-Muster etwas in Verruf gebracht. Es ist Zeit, es zu rehabilitieren.

![](assets/chapter-1/spooler-pattern.png)

Das Spooler-Muster verhindert das Problem mit der Anzeige einer API, die wir im letzten Kapitel besprochen haben. Die Eigenschaften werden gespeichert und in der Komponente eingekapselt. Alles, was wir für den Zugriff auf diese Eigenschaften benötigen, ist der Pfad zur Komponente. Wir müssen die URL nicht als Vehikel verwenden, um die Parameter zwischen Markup und binärem Renderer zu übertragen:

1. Der Client rendert das HTML-Markup, wenn die Komponente in der primären Anfragenschleife angefordert wird

2. Der Komponentenpfad dient als Rückverweis vom Markup zur Komponente

3. Der Browser verwendet diesen Rückverweis, um die Binärdatei anzufragen

4. Wenn die Anfrage die Komponente erreicht, haben wir alle Eigenschaften in der Hand, um die Größe zu ändern, zu komprimieren und die Binärdaten zu spoolen

5. Das Bild wird über die Komponente an den Client-Browser übertragen

Das Spooler-Muster ist gar nicht so schlecht, deshalb ist es auch so beliebt. Wenn es nur nicht so umständlich wäre, den Cache ungültig zu machen...

### Der Inverted Spooler – das Beste aus beiden Welten?

Das bringt uns zur Frage. Warum können wir nicht einfach das Beste aus beiden Welten bekommen? Die gute Kapselung des Spooler-Musters und die guten Zwischenspeichereigenschaften einer ressourcenbasierten URL?

Wir müssen zugeben, dass wir das nicht in einem echten Live-Projekt gesehen haben. Aber lassen Sie uns doch ein kleines Gedankenexperiment wagen, als Ausgangspunkt für Ihre eigene Lösung.

Wir nennen dieses Muster den _Inverted Spooler_. Der Inverted Spooler muss auf der Bilder-Ressource basieren, um alle schönen Eigenschaften der Cache-Invalidierung zu haben.

Er darf jedoch keine Parameter offenlegen. Alle Eigenschaften sollten in der Komponente eingekapselt werden. Aber wir können den Komponentenpfad als nicht transparenten Verweis auf die Eigenschaften verfügbar machen.

Dies führt zu einer URL im Formular:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` ist der Pfad zur Bildressource

`.respi3` ist eine Auswahl für das richtige Servlet zum Bereitstellen des Bildes

`.content-mysite-home-jcrcontent-par-respi` ist eine zusätzliche Auswahl. Sie kodiert den Pfad zu der Komponente, die die für die Bildumwandlung erforderliche Eigenschaft speichert. Auswahlen sind auf einen kleineren Zeichenbereich als Pfade beschränkt. Das Kodierungsschema hier ist nur ein Beispiel. Es ersetzt „/“ durch „-“. Dabei wird nicht berücksichtigt, dass der Pfad selbst auch „-“ enthalten kann. In einem realen Beispiel würde ein ausgefeilteres Kodierungsprogramm empfohlen werden. Base64 sollte in Ordnung sein. Das Debugging wird jedoch etwas schwieriger.

`.jpg` ist das Dateisuffix

### Zusammenfassung

Oh – die Diskussion über den Spooler wurde länger und komplizierter als erwartet. Wir schulden Ihnen eine Entschuldigung. Wir hielten es für notwendig, Ihnen eine Vielzahl von Aspekten – gute und schlechte – zu präsentieren, damit Sie Anhaltspunkte bekommen, was bezüglich Dispatchern gut funktioniert und was nicht.

## Statfile und Statfile-Ebene

### Grundlagen

#### Einführung

Wir haben die _Statfile_ bereits kurz erwähnt. Sie bezieht sich auf die automatische Invalidierung:

Alle Cache-Dateien im Dateisystem des Dispatchers, die für die automatische Invalidierung konfiguriert sind, werden als ungültig betrachtet, wenn ihr Datum der letzten Änderung älter ist als das Datum der letzten `statfile's`-Änderung.

>[!NOTE]
>
>Das Datum der letzten Änderung, von dem wir sprechen, betrifft die zwischengespeicherte Datei – das Datum, an dem die Datei vom Browser des Clients angefordert und schließlich im Dateisystem erstellt wurde. Es ist nicht das `jcr:lastModified`-Datum der Ressource.

Das Datum der letzten Änderung der Statfile (`.stat`) ist das Datum, an dem die Invalidierungsanfrage von AEM beim Dispatcher eingegangen ist.

Wenn Sie mehr als einen Dispatcher haben, kann dies zu seltsamen Auswirkungen führen. Ihr Browser kann über eine neuere Version eines Dispatchers verfügen (wenn Sie mehr als einen Dispatcher haben). Oder ein Dispatcher könnte glauben, dass die vom anderen Dispatcher herausgegebene Version des Browsers veraltet ist und unnötigerweise eine neue Kopie gesendet wird. Diese Effekte wirken sich nicht wesentlich auf die Leistung oder die Funktionserfordernisse aus. Und sie werden im Laufe der Zeit auslaufen, wenn die Browser-Version vorliegt. Es kann jedoch etwas verwirrend sein, wenn Sie das Cache-Verhalten des Browsers optimieren und debuggen. Seien Sie also gewarnt.

#### Einrichten von Invalidierungs-Domains mit /statfileslevel

In der Einführung zur automatischen Invalidierung und Statfile sagten wir, dass *alle* Dateien als ungültig betrachtet werden, wenn es eine Änderung gibt und dass alle Dateien ohnehin voneinander abhängig sind.

Das ist nicht ganz präzise. Normalerweise sind alle Dateien voneinander abhängig, die einen gemeinsamen Hauptnavigationsstamm haben. Eine AEM-Instanz kann jedoch eine Reihe von Websites hosten – *unabhängige* Websites. Diese haben keine gemeinsame Navigation und teilen gar nichts.

Wäre es nicht eine Verschwendung, Site B für ungültig zu erklären, da es eine Änderung an Site A gibt? Ja, das wäre es. Und es muss nicht so sein.

Der Dispatcher bietet eine einfache Möglichkeit, die Sites voneinander zu trennen: `statfiles-level`.

Es ist eine Zahl, die definiert, ab welcher Ebene im Dateisystem zwei Unterverzeichnisse als „unabhängig“ gelten.

Sehen wir uns den Standardfall an, in dem „statfileslevel“ 0 ist.

![/statfileslevel &quot;0&quot;: Die_ _.stat_ _wird im Basisverzeichnis erstellt. Die Invalidierungs-Domain erstreckt sich über die gesamte Installation einschließlich aller Sites](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` Die `.stat`-Datei wird im Basisverzeichnis erstellt. Die Invalidierungs-Domain erstreckt sich über die gesamte Installation, einschließlich aller Sites.

Unabhängig davon, welche Datei invalidiert wird, wird die `.stat`-Datei ganz oben im Docroot des Dispatchers immer aktualisiert. Wenn Sie also `/content/site-b/home` für ungültig erklären, werden auch alle Dateien in `/content/site-a` für ungültig erklärt, da sie jetzt älter sind als die Datei `.stat` in der Docroot. Offensichtlich nicht das, was Sie brauchen, wenn Sie `site-b` für ungültig erklären.

In diesem Beispiel würden Sie die `statfileslevel` eher auf `1` setzen.

Wenn Sie nun veröffentlichen – und damit `/content/site-b/home` oder eine andere Ressource unterhalb von `/content/site-b` ungültig machen – wird die Datei `.stat` bei `/content/site-b/` erstellt.

Inhalte unterhalb von `/content/site-a/` sind davon nicht betroffen. Dieser Inhalt würde mit einer `.stat`-Datei bei `/content/site-a/` verglichen werden. Wir haben zwei separate Invalidierungs-Domains erstellt.

![Eine „statfileslevel 1“ erstellt unterschiedliche Invalidierungs-Domains](assets/chapter-1/statfiles-level-1.png)

*Eine „statfileslevel 1“ erstellt unterschiedliche Invalidierungs-Domains*

<br> 

Große Installationen sind in der Regel etwas komplexer und tiefer strukturiert. Ein gemeinsames System besteht darin, Sites nach Marke, Land und Sprache zu strukturieren. In diesem Fall können Sie den Statfileslevel noch höher festlegen. _1_ würde Invalidierungs-Domains pro Marke, _2_ pro Land und _3_ pro Sprache schaffen.

### Notwendigkeit einer homogenen Site-Struktur

Das Statfileslevel wird gleichermaßen auf alle Sites in Ihrer Einrichtung angewendet. Daher müssen alle Sites dieselbe Struktur aufweisen und auf derselben Ebene beginnen.

Nehmen wir an, Sie haben einige Marken in Ihrem Portfolio, die nur auf einigen kleinen Märkten verkauft werden, während andere weltweit verkauft werden. Die kleinen Märkte haben zufällig nur eine lokale Sprache, während es auf dem Weltmarkt Länder gibt, in denen mehr als eine Sprache gesprochen wird:

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

Ersteres würde ein `statfileslevel` von _2_ erfordern, während letzteres _3_ erfordert.

Keine ideale Situation. Wenn Sie den Wert auf _3_ setzen, funktioniert die automatische Invalidierung nicht innerhalb der kleineren Sites zwischen den Unterverzweigungen `/home`, `/products` und `/about`.

Wenn Sie den Wert auf _2_ setzen, bedeutet das, dass Sie in den größeren Sites `/canada/en` und `/canada/fr` als abhängig deklarieren, was sie nicht unbedingt sind. Somit würde jede Invalidierung in `/en` auch `/fr` invalidieren. Dies führt zu einer etwas geringeren Cache-Trefferrate, ist jedoch besser als die Bereitstellung veralteter zwischengespeicherter Inhalte.

Die beste Lösung ist natürlich, die Wurzeln aller Sites gleich tief zu schlagen:

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Website-übergreifendes Verknüpfen

Welches ist nun die richtige Ebene? Dies hängt von der Anzahl der Abhängigkeiten ab, die Sie zwischen den Sites haben. Einschlüsse, die Sie zum Rendern einer Seite auflösen, gelten als „harte Abhängigkeiten“. Wir haben eine solche _Einbindung_ demonstriert, als wir zu Beginn dieses Handbuchs die _Teaser_-Komponente vorgestellt haben.

_Hyperlinks_ sind eine weichere Form von Abhängigkeiten. Es ist sehr wahrscheinlich, dass Sie Hyperlinks innerhalb einer Website haben... und nicht unwahrscheinlich, dass Sie Links zwischen Ihren Websites haben. Einfache Hyperlinks erzeugen normalerweise keine Abhängigkeiten zwischen Websites. Denken Sie nur an einen externen Link, den Sie von Ihrer Website zu Facebook eingerichtet haben... Sie müssen Ihre Seite nicht rendern, wenn sich etwas in Facebook ändert und umgekehrt, nicht wahr?

Eine Abhängigkeit tritt auf, wenn Sie Inhalte aus der verknüpften Ressource lesen (z. B. der Navigationstitel). Solche Abhängigkeiten können vermieden werden, wenn Sie sich nur auf lokal eingegebene Navigationstitel verlassen und sie nicht von der Zielseite (wie bei externen Links) beziehen.

#### Eine unerwartete Abhängigkeit

Es kann jedoch einen Teil Ihrer Einrichtung geben, in dem – angeblich unabhängige – Sites zusammenkommen. Schauen wir uns ein reales Szenario an, auf das wir in einem unserer Projekte gestoßen sind.

Die Kundschaft hatte eine Site-Struktur, wie sie im letzten Kapitel skizziert wurde:

```
/content/brand/country/language
```

Beispiel:

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Jedes Land hatte seine eigene Domain:

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Es gab keine navigierbaren Verknüpfungen zwischen den Sprach-Sites und keine sichtbaren Einschlüsse, daher setzten wir den Statfileslevel auf 3.

Alle Sites haben im Grunde denselben Inhalt bereitgestellt. Der einzige große Unterschied war die Sprache.

Suchmaschinen wie Google betrachten den gleichen Inhalt auf verschiedenen URLs als „trügerisch“. Eine Benutzerin bzw. ein Benutzer könnte versuchen, durch die Erstellung von Farmen mit identischen Inhalten eine höhere Platzierung oder eine häufigere Auflistung zu erreichen. Suchmaschinen erkennen diese Versuche und stufen Seiten, die einfach nur Inhalte recyceln, schlechter ein.

Sie können eine Herabstufung verhindern, indem Sie transparent machen, dass Sie tatsächlich mehr als eine Seite mit demselben Inhalt haben und dass Sie nicht versuchen, das System zu „manipulieren“ (siehe [„Informieren Sie Google über lokalisierte Versionen Ihrer Seite“](https://support.google.com/webmasters/answer/189077?hl=de)), indem Sie `<link rel="alternate">`-Tags für jede verwandte Seite im Header-Bereich jeder Seite setzen:

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![Verknüpfung von allen](assets/chapter-1/inter-linking-all.png)

*Verknüpfung von allen*

<br> 

Einige SEO-Expertinnen und -Experten argumentieren sogar, dass dies die Reputation oder den „Link-Saft“ von einer hochrangigen Website in einer Sprache auf dieselbe Website in einer anderen Sprache übertragen könnte.

Diese Regelung führte nicht nur zu einer Reihe von Verbindungen, sondern auch zu Problemen. Die Anzahl der Links, die für _p_ in _n_ Sprachen erforderlich sind, beträgt _p x (n<sup>2</sup>-n)_: Jede Seite verlinkt zu jeder anderen Seite (_n x n_) außer zu sich selbst (_-n_). Dieses Schema wird auf jede Seite angewendet. Wenn wir eine kleine Website in 4 Sprachen mit 20 Seiten haben, beläuft sich jede dieser Seiten auf _240_ Links.

Erstens soll nicht eine Bearbeiterin oder ein Bearbeiter diese Links manuell pflegen müssen – sie müssen automatisch vom System generiert werden.

Zweitens sollten sie genau sein. Jedes Mal, wenn das System einen neuen „Verwandten“ erkennt, sollten Sie ihn von allen anderen Seiten mit demselben Inhalt verknüpfen (jedoch in unterschiedlichen Sprachen).

In unserem Projekt sind häufig neue verwandte Seiten aufgetaucht. Aber sie wurden nicht als „alternative“ Links materialisiert. Wenn beispielsweise die Seite `de-de/produkte` auf der deutschen Website veröffentlicht wurde, war sie nicht sofort auf den anderen Sites sichtbar.

Der Grund dafür war, dass die Sites bei unserer Einrichtung unabhängig sein sollten. Eine Änderung auf der deutschen Website hat also keine Invalidierung auf der französischen Website getriggert.

Sie kennen bereits eine Lösung für dieses Problem. Verringern Sie einfach die Stat-Dateiebene auf 2, um die Invalidierungs-Domain zu erweitern. Dies verringert natürlich auch das Cache-Trefferverhältnis – insbesondere bei Veröffentlichungen – und führt daher häufiger zu Invalidierungen.

In unserem Fall war es noch komplizierter:

Obwohl wir denselben Inhalt hatten, waren die tatsächlichen Nichtmarkenbezeichnungen in jedem Land unterschiedlich.

`shiny-brand` hieß `marque-brillant` in Frankreich und `blitzmarke` in Deutschland:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Das hätte bedeutet, die `statfiles`-Ebene auf 1 zu setzen – was zu einer zu großen Invalidierungs-Domain geführt hätte.

Eine Umstrukturierung der Site hätte dies behoben. Das Zusammenführen aller Marken unter einem gemeinsamen Stamm. Aber damals hatten wir nicht die Kapazität, und – damit hätten wir nur Ebene 2 geschafft.

Wir beschlossen, bei Ebene 3 zu bleiben und haben den Preis dafür bezahlt, nicht immer über aktuelle „alternative“ Links zu verfügen. Um dies zu vermeiden, wurde auf dem Dispatcher ein „Sensenmann“-Cronjob ausgeführt, der ohnehin Dateien bereinigt, die älter als 1 Woche sind. Irgendwann wurden alle Seiten sowieso wieder gerendert. Aber das ist ein Kompromiss, der in jedem Projekt einzeln entschieden werden muss.

## Zusammenfassung

Wir haben einige grundlegende Prinzipien behandelt, wie der Dispatcher im Allgemeinen funktioniert, und wir haben einige Beispiele vorgestellt, bei denen Sie möglicherweise etwas mehr Implementierungsanstrengungen unternehmen müssen, um alles richtig zu machen, und wo Sie möglicherweise Kompromisse eingehen müssen.

Es wurden keine Details zur zugehörigen Konfiguration im Dispatcher erläutert. Wir wollten, dass Sie zuerst die grundlegenden Konzepte und Probleme verstehen, ohne Sie zu früh an die Konsole zu verlieren. Die eigentliche Konfigurationsarbeit ist gut dokumentiert – wenn Sie die grundlegenden Konzepte verstehen, sollten Sie wissen, wofür die verschiedenen Schalter verwendet werden.

## Tipps und Tricks zum Dispatcher

Wir werden den ersten Teil dieses Buches mit einer zufälligen Sammlung von Hinweisen und Tricks abschließen, die in der einen oder anderen Situation nützlich sein könnten. Wie zuvor präsentieren wir nicht die Lösung, sondern die Idee, damit Sie die Möglichkeit erhalten, die Idee und das Konzept zu verstehen und Links zu Artikeln zu erhalten, die die Konfiguration detaillierter beschreiben.

### Korrekte Invalidierungszeit

Wenn Sie eine AEM-Autoren- und -Veröffentlichungsinstanz vorkonfiguriert installieren, ist die Topologie etwas seltsam. Die Autoreninstanz sendet gleichzeitig den Inhalt an die Veröffentlichungssysteme und die Invalidierungsanfrage an die Dispatcher. Da sowohl die Veröffentlichungssysteme als auch der Dispatcher durch Warteschlangen von der Autoreninstanz entkoppelt sind, kann das Timing suboptimal sein. Der Dispatcher kann die Invalidierungsanfrage von der Autoreninstanz erhalten, bevor der Inhalt im Veröffentlichungssystem aktualisiert wird.

Wenn ein Client diesen Inhalt zwischenzeitlich anfordert, fordert der Dispatcher veraltete Inhalte an und speichert sie.

Eine zuverlässigere Einrichtung ist das Senden der Invalidierungsanfrage von den Veröffentlichungssystemen, _nachdem_ sie den Inhalt erhalten haben. Der Artikel „[Invalidierung des Dispatcher-Caches von einer Veröffentlichungsinstanz](https://helpx.adobe.com/de/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)“ beschreibt die Details dazu.

**Verweise**

[helpx.adobe.com – Invalidierung des Dispatcher-Caches von einer Veröffentlichungsinstanz](https://helpx.adobe.com/de/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### HTTP-Header und Header-Zwischenspeicherung

Früher hat der Dispatcher nur einfache Dateien im Dateisystem gespeichert. Wenn man HTTP-Header für die Bereitstellung an den Kunden oder die Kundin benötigt hat, hat man dafür Apache anhand der wenigen Informationen konfiguriert, die man aus der Datei oder dem Speicherort hatte. Dies war besonders ärgerlich, wenn man eine Web-Anwendung in AEM implementiert hat, die stark auf HTTP-Header angewiesen war. In der reinen AEM-Instanz hat alles einwandfrei funktioniert, jedoch nicht, wenn man einen Dispatcher verwendet hat.

Normalerweise hat man begonnen, die fehlenden Header mit `mod_headers` erneut auf die Ressourcen im Apache-Server durch Verwendung von Informationen anzuwenden, die man aus dem Ressourcenpfad und -suffix ableiten kann. Aber das war nicht immer ausreichend.

Besonders ärgerlich war, dass selbst mit dem Dispatcher die erste _nicht zwischengespeicherte_ Antwort an den Browser aus dem Veröffentlichungssystem mit einer vollständigen Reihe von Headern stammte, während die nachfolgenden Antworten vom Dispatcher mit einer begrenzten Anzahl von Headern generiert wurden.

Ab Dispatcher 4.1.11 kann der Dispatcher von Veröffentlichungssystemen generierte Header speichern.

Dadurch müssen Sie die Header-Logik im Dispatcher nicht mehr duplizieren und können die volle Ausdruckskraft von HTTP und AEM ausschöpfen.

**Verweise**

* [helpx.adobe.com – Zwischenspeichern von Antwort-Headern](https://helpx.adobe.com/de/experience-manager/kb/dispatcher-cache-response-headers.html)

### Einzelne Caching-Ausnahmen

Sie sollten im Allgemeinen alle Seiten und Bilder zwischenspeichern, jedoch unter bestimmten Umständen Ausnahmen machen. Sie sollten beispielsweise PNG-Bilder zwischenspeichern, aber keine PNG-Bilder, die ein Captcha anzeigen (das sich bei jeder Anfrage ändert). Der Dispatcher erkennt ein Captcha möglicherweise nicht als Captcha … aber AEM tut es auf jeden Fall. Es kann den Dispatcher auffordern, diese Anfrage nicht zwischenzuspeichern, indem es einen entsprechenden Header mit der Antwort sendet:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Steuerung und Pragma sind offizielle HTTP-Header, die von oberen Zwischenspeicherungsschichten wie CDN übertragen und interpretiert werden. Der `Dispatcher`-Header ist nur ein Hinweis für den Dispatcher, nicht zwischenzuspeichern. Er kann verwendet werden, um den Dispatcher anzuweisen, keine Zwischenspeicherung vorzunehmen, während die oberen Zwischenspeicherungsschichten dies weiterhin tun können. Tatsächlich ist es schwierig, einen Fall zu finden, in dem das nützlich sein könnte. Aber wir sind sicher, dass es welche gibt … irgendwo.

**Verweise**

* [Dispatcher – Kein Cache](https://helpx.adobe.com/de/experience-manager/kb/DispatcherNoCache.html)

### Browser-Zwischenspeicherung

Die schnellste HTTP-Antwort ist die Antwort des Browsers selbst. Weil die Anfrage und die Antwort nicht über ein Netzwerk zu einem Webserver unter hoher Belastung reisen müssen.

Sie können dem Browser bei der Entscheidung helfen, wann der Server nach einer neuen Version der Datei gefragt werden soll, indem Sie ein Ablaufdatum für eine Ressource festlegen.

Normalerweise tun Sie dies statisch, indem Sie `mod_expires` von Apache nutzen oder indem Sie die Header für die Cache-Kontrolle und für das Ablaufdatum speichern, die von AEM stammen, wenn Sie eine individuellere Steuerung benötigen.

Ein zwischengespeichertes Dokument im Browser kann drei Stufen der Aktualität aufweisen.

1. _Garantiert neu_ – Der Browser kann das zwischengespeicherte Dokument verwenden.

2. _Potenziell veraltet_ – Der Browser sollte zuerst den Server fragen, ob das zwischengespeicherte Dokument immer noch aktuell ist.

3. _Veraltet_ – Der Browser muss den Server nach einer neuen Version fragen.

Die erste Stufe wird durch das vom Server festgelegte Ablaufdatum garantiert. Wenn eine Ressource nicht abgelaufen ist, ist es nicht nötig, den Server erneut zu fragen.

Wenn das Dokument das Ablaufdatum erreicht hat, kann es weiterhin neu sein. Das Ablaufdatum wird bei Bereitstellung des Dokuments festgelegt. Aber oft weiß man nicht im Voraus, wann neue Inhalte verfügbar sind – also ist dies nur eine konservative Schätzung.

Um festzustellen, ob das Dokument im Browser-Cache weiterhin mit dem Dokument übereinstimmt, das bei einer neuen Anfrage bereitgestellt wird, kann der Browser das `Last-Modified`-Datum des Dokuments nutzen. Der Browser fragt den Server:

„_Ich habe eine Version vom 10. Juni … brauche ich eine Aktualisierung?_“. Der Server antwortet entweder mit

„_304 – Deine Version ist noch auf dem neuesten Stand_“, ohne die Ressource erneut zu übertragen, oder der Server antwortet mit

„_200 – hier ist eine neuere Version_“ im HTTP-Header und dem aktuelleren Inhalt im HTTP-Text.

Damit dieser zweite Teil funktioniert, müssen Sie das `Last-Modified`-Datum an den Browser übertragen, damit er einen Referenzpunkt hat, um nach Aktualisierungen zu fragen.

Wir haben vorher erklärt, dass das `Last-Modified`-Datum, wenn es vom Dispatcher generiert wird, von einer Anfrage zur nächsten variieren kann, da die zwischengespeicherte Datei – und damit ihr Datum – generiert wird, wenn die Datei vom Browser angefordert wird. Eine Alternative wäre die Verwendung von „e-Tags“. Dabei handelt es sich um Zahlen, die den tatsächlichen Inhalt (z. B. durch Generieren eines Hash-Codes), anstatt eines Datums identifizieren.

„[e-Tag-Unterstützung](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)“ aus dem _ACS Commons-Paket_ verwendet diesen Ansatz. Dies ist jedoch mit Kosten verbunden: Da das e-Tag als Header gesendet werden muss, die Berechnung des Hashcodes jedoch das vollständige Lesen der Antwort erfordert, muss die Antwort vollständig im Hauptspeicher gepuffert werden, bevor sie bereitgestellt werden kann. Dies kann sich negativ auf die Latenz auswirken, wenn Ihre Website mit höherer Wahrscheinlichkeit über nicht zwischengespeicherte Ressourcen verfügt, und Sie müssen natürlich den Speicher, der von Ihrem AEM-System belegt wird, im Auge behalten.

Wenn Sie URL-Fingerabdrücke verwenden, können Sie sehr lange Ablaufdaten festlegen. Sie können Fingerabdruckressourcen dauerhaft im Browser zwischenspeichern. Eine neue Version wird mit einer neuen URL markiert und ältere Versionen müssen nie aktualisiert werden.

Wir haben URL-Fingerabdrücke verwendet, als wir das Spooler-Muster vorgestellt haben. Statische Dateien aus `/etc/design` (CSS, JS) werden selten geändert, sodass sie auch gute Kandidaten für die Verwendung von Fingerabdrücken sind.

Bei regulären Dateien richten wir normalerweise ein festes Schema ein, z. B. dass HTML alle 30 Minuten erneut überprüft wird, Bilder alle 4 Stunden usw.

Das Browser-Caching ist im Autorensystem überaus hilfreich. Sie sollten so viel wie möglich im Browser zwischenspeichern, um das Erlebnis beim Bearbeiten zu verbessern. Leider können die kostspieligsten Assets – die HTML-Seiten – nicht zwischengespeichert werden … Sie sollen sich nämlich häufig auf Autorenseite ändern.

Die Granite-Bibliotheken, aus denen die AEM-Benutzeroberfläche besteht, können eine ganze Weile zwischengespeichert werden. Sie können Ihre Sites auch als statische Dateien (Schriftarten, CSS und JavaScript) im Browser zwischenspeichern. Sogar Bilder unter `/content/dam` können in der Regel für ca. 15 Minuten zwischengespeichert werden, da sie nicht so oft wie Fließtext auf den Seiten geändert werden. Bilder werden in AEM nicht interaktiv bearbeitet. Sie werden zuerst bearbeitet und genehmigt, bevor sie nach AEM hochgeladen werden. Sie können daher davon ausgehen, dass sie sich nicht so häufig ändern wie Text.

Durch das Caching von Benutzeroberflächendateien, Bibliotheksdateien und Bildern Ihrer Sites können Seiten deutlich schneller neu geladen werden, wenn Sie sich im Bearbeitungsmodus befinden.



**Verweise**

* [developer.mozilla.org – Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org – mod_expires](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [ACS Commons – e-Tag-Unterstützung](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Kürzen von URLs

Ihre Ressourcen werden unter folgendem Pfad gespeichert:

`/content/brand/country/language/…`

Dies ist aber selbstverständlich nicht die URL, die Kundinnen und Kunden sehen sollen. Aus Gründen der Ästhetik, Lesbarkeit und Suchmaschinen-Optimierung möchten Sie die URL vielleicht um den Teil kürzen, der bereits im Domain-Namen angezeigt wird.

Beispielsweise ist es bei der Domain

`www.shiny-brand.fi`

normalerweise nicht erforderlich, dass Marke und Land im Pfad angegeben werden. Anstelle von

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

ist Folgendes vorzuziehen:

`www.shiny-brand.fi/home.html`

Sie müssen diese Zuordnung in AEM implementieren, da AEM wissen muss, wie Links entsprechend diesem gekürzten Format gerendert werden.

Aber verlassen Sie sich nicht nur auf AEM. Ansonsten hätten Sie Pfade wie `/home.html` im Stammverzeichnis Ihres Caches. Aber entspricht das der Homepage für die finnische, deutsche oder kanadische Website? Und wenn die Datei `/home.html` im Dispatcher vorhanden ist, woher weiß der Dispatcher, dass diese invalidiert werden muss, wenn eine Invalidierungsanfrage für `/content/brand/fi/fi/home` gestellt wird.

Wir hatten schon einmal ein Projekt mit verschiedenen Docroot-Verzeichnissen für jede Domain. Debugging und Wartung waren ein wahrer Albtraum – und tatsächlich wurde es nie fehlerfrei ausgeführt.

Wir konnten die Probleme durch eine Neustrukturierung des Caches lösen. Wir hatten ein einziges Docroot-Verzeichnis für alle Domains, und die Invalidierungsanfragen konnten 1:1 verarbeitet werden, da alle Dateien auf dem Server mit `/content` begannen.

Auch das Kürzen war sehr einfach.  AEM generierte gekürzte Links aufgrund einer entsprechenden Konfiguration unter `/etc/map`.

Wenn nun eine `/home.html`-Anfrage beim Dispatcher eingeht, wird zunächst eine Neuschreibungsregel angewendet, die den Pfad intern erweitert.

Diese Regel wurde in jeder vhost-Konfiguration statisch eingerichtet. Einfach gesagt, sahen die Regeln wie folgt aus:

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

Im Dateisystem gibt es jetzt einfache `/content`-basierte Pfade, die auch auf Autoren- und Veröffentlichungsseite vorhanden sind, was sich für das Debugging als überaus hilfreich erwiesen hat. Ganz zu schweigen von der korrekten Invalidierung, das war kein Problem mehr.

Auf diese Weise sind wir übrigens nur für „sichtbare“ URLs vorgegangen, also für URLs, die im URL-Slot des Browsers angezeigt werden. URLs für Bilder wurden beispielsweise weiterhin als reine „/content“-URLs behandelt. Wir glauben, dass es im Hinblick auf die Suchmaschinen-Optimierung ausreicht, die Haupt-URL zu „verschönern“.

Ein gemeinsames Docroot-Verzeichnis zu haben, war zudem mit einer anderen angenehmen Funktion verbunden. Bei einem Fehler im Dispatcher konnten wir den gesamten Cache durch Ausführen des folgenden Befehls bereinigen:

`rm -rf /cache/dispatcher/*`

(Das käme bei hohen Belastungsspitzen vielleicht auch für Sie infrage.)

**Verweise**

* [apache.org – mod_rewrite](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com – Ressourcenzuordnung](https://helpx.adobe.com/de/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Fehlerverarbeitung

In AEM-Kursen erfahren Sie, wie Sie einen Fehler-Handler in Sling programmieren. Dies unterscheidet sich nicht so sehr vom Schreiben einer gewöhnlichen Vorlage. Sie erstellen eine Vorlage einfach in JSP oder HTL, oder?

Ja, aber das ist nur der Teil, der AEM betrifft. Denken Sie daran: Der Dispatcher speichert keine `404 – not found`- oder `500 – internal server error`-Antworten zwischen.

Wenn Sie diese Seiten bei jeder (fehlgeschlagenen) Anfrage dynamisch rendern, ist die Belastung der Veröffentlichungssysteme unnötig hoch.

Nach unserer Erfahrung ist es nützlich, nicht die vollständige Fehlerseite zu rendern, wenn ein Fehler auftritt, sondern nur eine extrem vereinfachte und kleine – sogar statische – Version dieser Seite, ohne Ausschmückungen oder Logik.

Das ist natürlich nicht das, was die Kundinnen und Kunden gesehen haben. Im Dispatcher registrierten wir `ErrorDocuments` wie folgt:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Jetzt konnte das AEM-System den Dispatcher einfach darüber informieren, dass ein Problem aufgetreten ist, und der Dispatcher konnte eine „schöne“ Version des Fehlerdokuments bereitstellen.

Hier sind zwei Dinge zu beachten.

Erstens handelt es sich bei `error-404.html` immer um dieselbe Seite. Es gibt also keine individuelle Meldung wie „Ihre Suche nach ‚_Produkten_‘ hat keinen Treffer ergeben“. Damit könnten wir problemlos leben.

Zweitens … Wenn es zu einem internen Server-Fehler – oder schlimmer – einem Ausfall des AEM-Systems kommt, gibt es keine Möglichkeit, AEM aufzufordern, eine Fehlerseite zu rendern. Die erforderliche nachfolgende Anfrage, wie in der `ErrorDocument`-Anweisung definiert, würde ebenfalls fehlschlagen. Wir haben dieses Problem durch Ausführen eines Cron-Auftrags umgangen, der die Fehlerseiten regelmäßig über `wget` von ihren definierten Speicherorten abruft und diese an den in der `ErrorDocuments`-Anweisung festgelegten statischen Dateispeicherorten speichert.

**Verweise**

* [apache.org – Benutzerdefinierte Fehlerdokumente](https://httpd.apache.org/docs/2.4/custom-error.html)

### Zwischenspeichern geschützter Inhalte

Der Dispatcher überprüft keine Berechtigungen, wenn er standardmäßig Ressourcen bereitstellt. Das ist so absichtlich implementiert, um Ihre öffentliche Website zu beschleunigen. Wenn Sie einige Ressourcen durch einen Anmeldevorgang schützen möchten, haben Sie im Grunde drei Optionen:

1. Schützen der Ressource, bevor die Anfrage den Cache erreicht, d. h. durch ein Single Sign-on(SSO)-Gateway vor dem Dispatcher oder als Modul auf dem Apache-Server

2. Ausschließen sensibler Ressourcen aus der Zwischenspeicherung und damit Sicherstellen ihrer Live-Bereitstellung über das Veröffentlichungssystem

3. Zwischenspeichern unter Berücksichtigung von Berechtigungen im Dispatcher

Selbstverständlich können Sie auch Ihren eigenen Mix aus allen drei Ansätzen anwenden.

**Option 1**. Ein „SSO“-Gateway ist von Ihrem Unternehmen möglicherweise ohnehin vorgesehen. Wenn Ihr Zugriffsschema sehr grob abgestuft ist, benötigen Sie möglicherweise keine Informationen von AEM, um zu entscheiden, ob Sie den Zugriff auf eine Ressource gewähren oder verweigern.

>[!NOTE]
>
>Dieses Muster setzt ein _Gateway_ voraus, das jede Anfrage _abfängt_ und die tatsächliche _Autorisierung_ durchführt und im Zuge dessen Anfragen an den Dispatcher Anforderungen zulässt oder ablehnt. Wenn es sich bei Ihrem SSO-System um einen _Authenticator_ handelt, der nur die Identität einer Benutzerin oder eines Benutzers festlegt, müssen Sie Option 3 implementieren. Wenn Sie Begriffe wie „SAML“ oder „OAauth“ im Handbuch Ihres SSO-Systems finden, ist dies ein eindeutiges Anzeichen dafür, dass Sie Option 3 implementieren müssen.


**Option 2**. Nicht zwischenzuspeichern ist im Allgemeinen eine schlechte Idee. Wenn Sie sich dafür entscheiden, stellen Sie sicher, dass der Traffic und die Anzahl der ausgeschlossenen sensiblen Ressourcen gering sind. Oder achten Sie darauf, dass Sie im Veröffentlichungssystem einen In-Memory-Cache installiert haben, damit die Veröffentlichungssysteme die daraus resultierende Last bewältigen können. Mehr dazu erfahren Sie in Teil III dieser Reihe.

**Option 3**. „Zwischenspeicherung unter Berücksichtigung von Berechtigungen“ ist ein interessanter Ansatz. Der Dispatcher speichert eine Ressource zwischen. Vor der Bereitstellung fragt er jedoch das AEM-System, ob dies auch erlaubt ist. Dadurch wird eine zusätzliche Anfrage vom Dispatcher an das Veröffentlichungssystem erstellt – und das Veröffentlichungssystem muss in der Regel eine Seite nicht erneut rendern, wenn sie bereits zwischengespeichert ist. Dieser Ansatz erfordert jedoch eine benutzerdefinierte Implementierung. Weitere Informationen dazu finden Sie im Artikel zur [Zwischenspeicherung unter Berücksichtigung von Berechtigungen](https://helpx.adobe.com/de/experience-manager/dispatcher/using/permissions-cache.html).

**Verweise**

* [helpx.adobe.com – Zwischenspeicherung unter Berücksichtigung von Berechtigungen](https://helpx.adobe.com/de/experience-manager/dispatcher/using/permissions-cache.html)

### Festlegen der Nachfrist

Wenn Sie Invalidierungen häufig in kurzer Folge durchführen, z. B. durch Aktivierung einer Baumstruktur oder einfach aus der Notwendigkeit heraus, Inhalte auf dem neuesten Stand zu halten, kann es vorkommen, dass Sie den Cache ständig leeren und Ihre Besuchende so fast immer auf einen leeren Cache treffen.

Die folgende Grafik zeigt den möglichen zeitlichen Ablauf beim Zugriff auf eine einzelne Seite.  Das Problem verschärft sich natürlich, wenn die Anzahl der verschiedenen angeforderten Seiten größer wird.

![Häufige Aktivierungen führen meistens zu einem ungültigen Cache.](assets/chapter-1/frequent-activations.png)

*Häufige Aktivierungen, die den Großteil der Zeit zu ungültigem Cache führen.*

<br> 

Um das Problem dieses „Cache-Invalidierungs-Sturms“, wie es manchmal bezeichnet wird, zu beheben, könnten Sie eine weniger strenge `statfile`-Interpretation in Erwägung ziehen.

Sie können den Dispatcher mit einer `grace period` für die automatische Invalidierung einstellen. Dies würde intern etwas mehr Zeit für das `statfiles`-Änderungsdatum einräumen.

Angenommen, Ihre `statfile` hat eine Änderungszeit von heute 12:00 Uhr und Ihre `gracePeriod` ist auf zwei Minuten eingestellt. Dann werden alle automatisch invalidierten Dateien um 12:01 Uhr und um 12:02 Uhr als gültig angesehen. Sie werden nach 12:02 Uhr erneut gerendert.

Die Referenzkonfiguration schlägt aus gutem Grund eine `gracePeriod` von zwei Minuten vor. Sie denken jetzt vielleicht: „Zwei Minuten? Das ist so gut wie nichts. Ich warte einfach zehn Minuten, bis der Inhalt angezeigt wird …“ Sie könnten also versucht sein, einen längeren Zeitraum festzulegen, z. B. 10 Minuten, vorausgesetzt, dass Ihr Inhalt mindestens nach diesen 10 Minuten angezeigt wird.

>[!WARNING]
>
>So funktioniert der Schlüssel `gracePeriod` aber nicht. Die Nachfrist entspricht _nicht_ der Zeit, nach der ein Dokument auf jeden Fall invalidiert wird, sondern einem Zeitraum, in dem keine Invalidierung stattfindet. Jede nachfolgende Invalidierung in diesem Zeitraum _verlängert_ den Zeitrahmen, der von unbegrenzter Länge sein kann.

Veranschaulichen wir an einem Beispiel, wie `gracePeriod` tatsächlich arbeitet:

Angenommen, Sie betreiben eine Medien-Site und Ihr Bearbeitungs-Team stellt alle fünf Minuten reguläre Inhaltsaktualisierungen bereit. Beachten Sie, dass Sie die Nachfrist auf 5 Minuten festgelegt haben.

Wir beginnen mit einem kurzen Beispiel um 12:00 Uhr.

12:00 Uhr: Die Statfile ist auf 12:00 Uhr eingestellt. Alle zwischengespeicherten Dateien gelten bis 12:05 Uhr als gültig.

12:01 Uhr: Es kommt zu einer Invalidierung. Dadurch verlängert sich die Nachfrist auf 12:06 Uhr.

12:05 Uhr: Eine andere Bearbeiterin oder ein anderer Editor veröffentlicht einen Artikel. Dadurch verlängert sich die die Nachfrist auf 12:10 Uhr.

Und so weiter... der Inhalt wird nie invalidiert. Jede Invalidierung *innerhalb* der Nachfrist verlängert die Fristzeit effektiv. Die `gracePeriod` ist so konzipiert, dass sie den Sturm der Invalidierung übersteht … aber irgendwann muss man in den Regen … also halten Sie die `gracePeriod` weitgehend kurz, um zu verhindern, dass sie für immer im Unterschlupf Schutz sucht.

#### Eine deterministische Nachfrist

Wir möchten eine andere Idee vorstellen, wie man einen Invalidierungssturm überstehen kann. Es ist nur eine Idee. Wir haben es nicht in der Produktion versucht, aber wir fanden das Konzept interessant genug, um die Idee mit Ihnen zu teilen.

Die `gracePeriod` kann unvorhersehbar lang werden, wenn Ihr reguläres Replikationsintervall kürzer ist als Ihre `gracePeriod`.

Die alternative Idee lautet wie folgt: nur in festen Zeitintervallen invalidieren. In der Zeit dazwischen werden immer veraltete Inhalte bereitgestellt. Die Invalidierung wird irgendwann stattfinden, aber eine Reihe von Invalidierungen wird für eine „Massen“-Invalidierung gesammelt, sodass der Dispatcher in der Zwischenzeit einige zwischengespeicherte Inhalte bereitstellen und dem Veröffentlichungssystem etwas Luft zum Atmen geben kann.

Die Implementierung würde wie folgt aussehen:

Sie verwenden ein „benutzerdefiniertes Invalidierungsskript“ (siehe Referenz), das nach der Invalidierung ausgeführt wird. Dieses Skript liest das Datum der letzten Änderung der `statfile's` und rundet es bis zum nächsten Intervallende auf. Der Unix-Shell-Befehl `touch --time` erlaubt Ihnen, einen Zeitpunkt anzugeben.

Wenn Sie beispielsweise die Nachfrist auf 30 Sekunden festlegen, rundet der Dispatcher das Datum der letzten Änderung der Stat-Datei auf die nächsten 30 Sekunden. Invalidierungsanfragen, die dazwischen auftreten, warten einfach die nächsten vollen 30 Sekunden ab.

![Das Verschieben der Invalidierung auf die nächste volle 30-Sekunden-Periode erhöht die Trefferrate.](assets/chapter-1/postponing-the-invalidation.png)

*Das Verschieben der Invalidierung auf die nächste volle 30-Sekunden-Periode erhöht die Trefferrate.*

<br> 

Die Cache-Treffer, die zwischen der Invalidierungsanfrage und der nächsten runden 30-Sekunden-Periode auftreten, werden dann als veraltet betrachtet. Es gab eine Aktualisierung auf der Veröffentlichungsinstanz – der Dispatcher stellt jedoch weiterhin alte Inhalte bereit.

Dieser Ansatz könnte dabei helfen, längere Nachfristen zu definieren, ohne befürchten zu müssen, dass nachfolgende Anfragen den Zeitraum unbestimmt verlängern. Wie wir bereits gesagt haben, ist das nur eine Idee, und wir hatten keine Chance, sie zu testen.

**Verweise**

[helpx.adobe.com – Dispatcher-Konfiguration](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Automatisches Neuabrufen

Ihre Site hat ein sehr bestimmtes Zugriffsmuster. Der eingehende Traffic ist sehr hoch und der Großteil des Traffics ist auf einen kleinen Teil Ihrer Seiten konzentriert. 90 % des Traffics werden auf der Startseite, den Landingpages der Kampagne und den Detailseiten der am häufigsten vorgestellten Produkte erzielt. Oder wenn Sie eine neue Site betreiben, weisen die aktuelleren Artikel im Vergleich zu älteren eine höhere Traffic-Anzahl auf.

Diese Seiten werden sehr wahrscheinlich im Dispatcher zwischengespeichert, da sie so häufig angefordert werden.

Eine beliebige Invalidierungsanfrage wird an den Dispatcher gesendet, wodurch alle Seiten – einschließlich der beliebtesten – ungültig gemacht werden.

Da diese Seiten so beliebt sind, gibt es daraufhin neue eingehende Anfragen von verschiedenen Browsern. Nehmen wir als Beispiel die Startseite.

Da der Cache jetzt ungültig ist, werden alle gleichzeitig eingehenden Anfragen an die Startseite an das Veröffentlichungssystem weitergeleitet, was zu einer hohen Belastung führt.

![Parallele Anfragen an dieselbe Ressource im leeren Cache: Anfragen werden an die Veröffentlichungsinstanz weitergeleitet](assets/chapter-1/parallel-requests.png)

*Parallele Anfragen an dieselbe Ressource im leeren Cache: Anfragen werden an die Veröffentlichungsinstanz weitergeleitet*

Mit automatischen Neuabrufen können Sie dies bis zu einem gewissen Grad umgehen. Die meisten invalidierten Seiten werden nach der automatischen Invalidierung weiterhin physisch auf dem Dispatcher gespeichert. Sie werden nur als veraltet _betrachtet_. _Automatischer Neuabruf_ bedeutet, dass Sie diese veralteten Seiten noch einige Sekunden lang bereitstellen, während Sie _eine einzelne_ Anfrage an das Veröffentlichungssystem zum Neuabruf des veralteten Inhalts initiieren:

![Bereitstellen von veraltetem Inhalt bei Neuabruf im Hintergrund](assets/chapter-1/fetching-background.png)

*Bereitstellen von veraltetem Inhalt bei Neuabruf im Hintergrund*

<br> 

Um den Neuabruf zu aktivieren, müssen Sie dem Dispatcher mitteilen, welche Ressourcen nach einer automatischen Invalidierung erneut abgerufen werden sollen. Denken Sie daran, dass jede aktivierte Seite auch automatisch alle anderen Seiten invalidiert – darunter auch Ihre beliebtesten.

Neuabruf bedeutet in der Tat, dass dem Dispatcher bei jeder (!) Invalidierungsanfrage gesagt wird, dass Sie die beliebtesten Seiten erneut abrufen möchten – und was die beliebtesten Seiten sind.

Dies wird erreicht, indem eine Liste von Ressourcen-URLs (tatsächliche URLs – nicht nur Pfade) in den Text der Invalidierungsanfragen eingefügt wird:

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Wenn der Dispatcher eine solche Anfrage sieht, wird die automatische Invalidierung wie gewohnt getriggert und Anfragen werden sofort in die Warteschlange gestellt, um neue Inhalte aus dem Veröffentlichungssystem erneut abzurufen.

Da wir jetzt einen Anfragetext verwenden, müssen wir auch den Inhaltstyp und die Inhaltslänge entsprechend dem HTTP-Standard festlegen.

Der Dispatcher markiert die entsprechenden URLs auch intern, sodass er weiß, dass er diese Ressourcen direkt bereitstellen kann, auch wenn sie durch die automatische Invalidierung als invalidiert betrachtet werden.

Alle aufgelisteten URLs werden einzeln angefordert. Sie müssen sich also keine Gedanken darüber machen, eine zu hohe Belastung der Veröffentlichungssysteme zu erzeugen. Sie sollten jedoch auch nicht zu viele URLs in diese Liste aufnehmen. Schließlich muss die Warteschlange irgendwann in begrenzter Zeit verarbeitet werden, um veraltete Inhalte nicht zu lange bereitzustellen. Schließen Sie einfach Ihre 10 am häufigsten aufgerufenen Seiten ein.

Wenn Sie sich das Cache-Verzeichnis Ihres Dispatchers ansehen, sehen Sie temporäre Dateien, die mit Zeitstempeln versehen sind. Dies sind die Dateien, die derzeit im Hintergrund geladen werden.

**Verweise**

[helpx.adobe.com – Invalidierung zwischengespeicherter Seiten von AEM](https://helpx.adobe.com/de/experience-manager/dispatcher/using/page-invalidate.html)

### Abschirmen des Veröffentlichungssystems

Der Dispatcher bietet ein wenig zusätzliche Sicherheit, indem er das Veröffentlichungssystem vor Anfragen abschirmt, die nur für Wartungszwecke vorgesehen sind. Sie sollten beispielsweise Ihre URLs mit `/crx/de` oder `/system/console` nicht vor der Öffentlichkeit zeigen.

Es schadet nicht, eine Web-Anwendungs-Firewall (WAF) in Ihrem System installiert zu haben. Aber das erhöht Ihr Budget bedeutend, und nicht alle Projekte befinden sich in einer Situation, in der sie sich eine WAF leisten und – nicht zu vergessen – auch betreiben und pflegen können.

Häufig sehen wir eine Reihe von Apache-Neuschreibungsregeln in der Dispatcher-Konfiguration, die den Zugriff auf anfälligere Ressourcen verhindern.

Sie können jedoch auch einen anderen Ansatz in Betracht ziehen:

Gemäß der Dispatcher-Konfiguration ist das Dispatcher-Modul an ein bestimmtes Verzeichnis gebunden:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Aber warum den Handler an die gesamte Docroot binden, wenn Sie danach nach unten filtern müssen?

Sie können die Bindung des Handlers zunächst eingrenzen. `SetHandler` bindet nur einen Handler an ein Verzeichnis, man könnte den Handler auch an eine URL oder an ein URL-Muster binden:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Vergessen Sie dabei nicht, immer den Dispatcher-Handler an die Invalidierungs-URL des Dispatchers zu binden. Andernfalls können Sie keine Invalidierungsanfragen von AEM an den Dispatcher senden.

Eine weitere Alternative zur Verwendung des Dispatchers als Filter ist die Einrichtung von Filterrichtlinien in der `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Wir schreiben nicht die Verwendung einer Richtlinie gegenüber einer anderen vor, sondern empfehlen einen angemessenen Mix aus allen Richtlinien.

Wir schlagen jedoch vor, den URL-Bereich so früh wie möglich und so weit wie nötig einzugrenzen, und zwar auf möglichst einfache Weise. Bedenken Sie jedoch, dass diese Techniken bei hochsensiblen Websites kein Ersatz für eine WAF sind. Manche Leute nennen diese Techniken „die Firewall des armen Mannes“ – nicht ohne Grund.

**Verweise**

[apache.org – Sethandler-Richtlinie](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com – Konfigurieren des Zugriffs auf den Inhaltsfilter](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtern mit regulären Ausdrücken und Globs

Früher konnte man nur „globs“ verwenden – einfache Platzhalter, um Filter in der Dispatcher-Konfiguration zu definieren.

Glücklicherweise hat sich dies in den späteren Versionen des Dispatchers geändert. Jetzt können Sie auch reguläre POSIX-Ausdrücke verwenden und auf verschiedene Teile einer Anfrage zugreifen, um einen Filter zu definieren. Für jemanden, der gerade erst mit dem Dispatcher angefangen hat, mag das selbstverständlich sein. Aber wenn man es gewohnt ist, nur Globs zu haben, ist es irgendwie eine Überraschung und kann leicht übersehen werden. Außerdem ist die Syntax von Globs und Regexes einfach zu ähnlich. Vergleichen wir zwei Versionen, die dasselbe tun:

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

Sehen Sie den Unterschied?

Version B verwendet einfache Anführungszeichen `'` zur Kennzeichnung eines _regelmäßigen Ausdrucksmusters_. „Beliebiges Zeichen“ wird durch die Verwendung von `.*` ausgedrückt.

_Globing-Muster_, verwenden dagegen doppelte Anführungszeichen `"` und können nur einfache Platzhalter wie `*` verwenden.

Wenn Sie diesen Unterschied kennen, ist er unerheblich – aber wenn nicht, können Sie einfach die Anführungszeichen vermischen und einen sonnigen Nachmittag damit verbringen, Ihre Konfiguration zu debuggen. Jetzt sind Sie gewarnt.

„Ich erkenne `'/url'` in der Konfiguration ... Aber was ist dieses `'/glob'` in dem Filter?“ – werden Sie sich vielleicht fragen.

Diese Richtlinie stellt die gesamte Anforderungszeichenfolge dar, einschließlich Methode und Pfad. Es könnte für Folgendes stehen:

`"GET /content/foo/bar.html HTTP/1.1"`

Dies ist die Zeichenfolge, mit der Ihr Muster verglichen wird. Anfangende neigen dazu, den ersten Teil zu vergessen, die `method` (GET, POST, ...). Also ein Muster

`/0002  { /glob "/content/\*" /type "allow" }`

Würde immer fehlschlagen, da „/content“ nicht mit dem „GET …“ der Anfrage übereinstimmt.

Wenn Sie also Globs verwenden möchten, würde

`/0002  { /glob "GET /content/\*" /type "allow" }`

korrekt sein.

Für eine anfängliche Ablehnungsregel, z. B.

`/0001  { /glob "\*" /type "deny" }`

wäre das in Ordnung. Für die nachfolgenden Erlaubnisse ist es jedoch besser und deutlicher, aussagekräftiger und viel sicherer, die einzelnen Teile einer Anfrage zu verwenden:

```
/method
/url
/path
/selector
/extension
/suffix
```

Etwa so:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Beachten Sie, dass Sie Regex- und Glob-Ausdrücke in einer Regel mischen können.

Ein letztes Wort zu den „Zeilennummern“ wie `/005` vor jeder Definition.

Sie haben überhaupt keine Bedeutung! Sie können beliebige Nenner für Regeln auswählen. Die Verwendung von Zahlen erfordert nicht viel Aufwand, um über ein Schema nachzudenken, aber beachten Sie, dass die Reihenfolge wichtig ist.

Wenn Sie Hunderte von Regeln wie diese haben:

```
/001
/002
/003
…
/100
…
```

und Sie möchten eine Zahl zwischen /001 und /002 einfügen, was passiert mit den nachfolgenden Zahlen? Erhöhen Sie dadurch ihre Zahlen? Setzen Sie Zwischennummern ein?

```
/001
/001a
/002
/003
…
/100
…
```

Oder was passiert, wenn Sie /003 und /001 neu anordnen wollen? Ändern Sie die Namen und ihre Identitäten

```
/003
/002
/001
…
/100
…
```

oder nummerieren Sie, was zunächst einfach erscheint, aber auf Dauer an seine Grenzen stößt? Seien wir ehrlich, die Verwendung von Zahlen als Kennungen ist ohnehin ein schlechter Programmierstil.

Wir möchten einen anderen Ansatz vorschlagen: Wahrscheinlich fallen Ihnen keine aussagekräftigen Kennungen für jede einzelne Filterregel ein. Aber sie dienen wahrscheinlich einem größeren Zweck, sodass sie auf irgendeine Weise nach diesem Zweck gruppiert werden können. Beispiel: „grundlegendes Setup“, „anwendungsspezifische Ausnahmen“, „globale Ausnahmen“ und „Sicherheit“.

Sie können dann die Regeln entsprechend benennen und gruppieren und der Person, die die Konfiguration liest (Ihrem lieben Kollegen oder Ihrer lieben Kollegin) die Orientierung in der Datei erleichtern:

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


Wahrscheinlich werden Sie einer der Gruppen eine neue Regel hinzufügen – oder vielleicht sogar eine neue Gruppe erstellen. In diesem Fall ist die Anzahl der Elemente, die umbenannt/umnummeriert werden müssen, auf diese Gruppe beschränkt.

>[!WARNING]
>
>In ausgereifteren Setups werden Filterregeln in mehrere Dateien aufgeteilt, die in der Haupt-`dispatcher.any`-Konfigurationsdatei enthalten sind. Eine neue Datei führt jedoch keinen neuen Namespace ein. Wenn Sie also eine Regel „001“ in einer Datei und „001“ in einer anderen Datei haben, erhalten Sie einen Fehler. Noch ein Grund mehr, semantisch starke Namen zu finden.

**Verweise**

[helpx.adobe.com – Entwerfen von Mustern für GLOB-Eigenschaften](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Protokollspezifikation

Der letzte Tipp ist kein echter Tipp, aber wir dachten, dass es sich lohnt, ihn trotzdem mit Ihnen zu teilen.

AEM und der Dispatcher arbeiten in den meisten Fällen so wie vorkonfiguriert. Daher finden Sie keine umfassende Dispatcher-Protokollspezifikation zum Invalidierungsprotokoll, um darauf Ihre eigene Anwendung zu erstellen. Die Informationen sind öffentlich, aber etwas verstreut über eine Reihe von Ressourcen.

Wir versuchen, die Lücke hier zu füllen. So sieht eine Invalidierungsanfrage aus:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1` – Die erste Zeile ist die URL des Dispatcher-Steuerendpunkts, die Sie wahrscheinlich nicht ändern.

`CQ-Action: <action>` – Was geschehen sollte. `<action>` ist entweder:

* `Activate:` löscht `/path-pattern.*`
* `Deactive:` löscht `/path-pattern.*`
UND löscht `/path-pattern/*`
* `Delete:` löscht `/path-pattern.*`
UND löscht `/path-pattern/*`
* `Test:` Gib „ok“ zurück, aber tue nichts

`CQ-Handle: <path-pattern>` – Der Pfad der Inhaltsressource, der invalidiert werden soll. Hinweis: `<path-pattern>` ist eigentlich ein „Pfad“ und kein „Muster“.

`CQ-Action-Scope: ResourceOnly` – Optional: Wenn dieser Header festgelegt ist, wird die `.stat`-Datei nicht angerührt.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Legen Sie diese Header fest, wenn Sie eine Liste von URLs für den automatischen Neuabruf definieren. `<bytes in request body>` ist die Anzahl der Zeichen im HTTP-Textkörper

`<newline>` – Wenn Sie einen Anfragetext haben, muss dieser durch eine leere Zeile vom Header getrennt werden.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Listen Sie die URLs auf, die Sie nach der Invalidierung sofort erneut abrufen möchten.

## Zusätzliche Ressourcen

Eine gute Übersicht und Einführung zum Dispatcher-Caching: [https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher.html)

Die Dispatcher-Dokumentation mit allen Anweisungen wird hier erklärt: [https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/de/experience-manager/dispatcher/using/dispatcher-configuration.html)

Häufig gestellte Fragen: [https://helpx.adobe.com/de/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/de/experience-manager/using/dispatcher-faq.html)

Aufzeichnung eines Webinars zur Optimierung des Dispatchers – dringend empfohlen: [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Präsentation der „adaptTo()“-Konferenz „unterschätzte Macht der Invalidierung von Inhalten“ in Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

Invalidierung zwischengespeicherter Seiten aus AEM: [https://helpx.adobe.com/de/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/de/experience-manager/dispatcher/using/page-invalidate.html)

## Nächster Schritt

* [2. Infrastrukturmuster](chapter-2.md)
