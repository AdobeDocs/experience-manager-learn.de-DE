---
title: Grundlegendes zu Mehrmandantenfähigkeit und gleichzeitiger Entwicklung
description: Erfahren Sie mehr über die Vorteile, Herausforderungen und Techniken zur Verwaltung einer mehrmandantenfähigen Implementierung mit Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
doc-type: Article
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
duration: 524
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2022'
ht-degree: 100%

---

# Grundlegendes zu Mehrmandantenfähigkeit und gleichzeitiger Entwicklung {#understanding-multitenancy-and-concurrent-development}

## Einführung {#introduction}

Wenn mehrere Teams ihren Code in denselben AEM-Umgebungen bereitstellen, sollten sie bestimmte Vorgehensweisen befolgen, damit die Teams so unabhängig wie möglich arbeiten können, ohne anderen Teams in die Quere zu kommen. Auch wenn eine vollständige Eliminierung niemals möglich ist, minimieren diese Techniken dennoch die Abhängigkeiten zwischen verschiedenen Teams. Damit ein gleichzeitiges Entwicklungsmodell erfolgreich sein kann, ist eine gute Kommunikation zwischen den Entwicklungs-Teams von entscheidender Bedeutung.

Wenn mehrere Entwicklungs-Teams in derselben AEM-Umgebung arbeiten, wird wahrscheinlich ein gewisses Maß an Mehrmandantenfähigkeit im Spiel sein. Es wurde viel über die praktischen Aspekte geschrieben, wenn mehrere Mandanten in einer AEM-Umgebung unterstützt werden sollen, insbesondere über die Herausforderungen beim Management von Governance, Vorgängen und Entwicklung. In diesem Dokument werden verschiedene technische Herausforderungen im Zusammenhang mit der AEM-Implementierung in einer mandantenfähigen Umgebung beschrieben, aber viele dieser Empfehlungen gelten im Grunde für jede Organisation mit mehreren Entwicklungs-Teams.

Vorweg ist darauf hinzuweisen, dass AEM zwar mehrere Sites und sogar mehrere Marken unterstützen kann, die in einer Umgebung bereitgestellt werden, jedoch keine echte Mehrmandantenfähigkeit bietet. Einige Umgebungskonfigurationen und Systemressourcen werden immer gemeinsam für alle in einer Umgebung bereitgestellten Sites genutzt. Dieses Dokument enthält Leitlinien für die Minimierung der Auswirkungen dieser gemeinsam genutzten Ressourcen und macht Vorschläge zur Optimierung der Kommunikation und Zusammenarbeit in diesen Bereichen.

## Vorteile und Herausforderungen {#benefits-and-challenges}

Die Implementierung einer mehrmandantenfähigen Umgebung ist mit vielen Herausforderungen verbunden.

Dazu gehören:

* zusätzliche technische Komplexität
* höherer Entwicklungsaufwand
* organisationsübergreifende Abhängigkeiten von gemeinsam genutzten Ressourcen
* höhere betriebliche Komplexität

Trotz der Herausforderungen bietet die Ausführung einer mehrmandantenfähigen Anwendung Vorteile, z. B.:

* geringere Hardware-Kosten
* kürzere Time-to-Market für zukünftige Sites
* geringere Implementierungskosten für zukünftige Mandanten
* standardmäßige Architektur- und Entwicklungsverfahren im gesamten Unternehmen
* eine gemeinsame Code-Basis

Wenn das Unternehmen eine echte Mehrmandantenfähigkeit erfordert (ohne Kenntnis anderer Mandanten und ohne gemeinsamen Code, gemeinsamen Inhalt oder gemeinsame Autorinnen und Autoren), sind separate Autoreninstanzen die einzige praktikable Option. Der Gesamtanstieg der Entwicklungsbemühungen sollte mit den Einsparungen bei Infrastruktur- und Lizenzkosten verglichen werden, um festzustellen, ob dieser Ansatz am besten geeignet ist.

## Entwicklungstechniken {#development-techniques}

### Verwalten von Abhängigkeiten {#managing-dependencies}

Bei der Verwaltung von Maven-Projektabhängigkeiten ist es wichtig, dass alle Teams dieselbe Version eines bestimmten OSGi-Bundles auf dem Server verwenden. Folgendes Beispiel zeigt, was bei einer unsachgemäßen Verwaltung von Maven-Projekten schiefgehen kann:

Projekt A hängt von Version 1.0 der Bibliothek „foo“ ab. Die foo-Version 1.0 ist in die entsprechende Bereitstellung auf dem Server eingebettet. Projekt B hängt von Version 1.1 der foo-Bibliothek ab. Die foo-Version 1.1 ist in die entsprechende Implementierung eingebettet.

Außerdem nehmen wir an, dass sich in dieser Bibliothek eine API zwischen Version 1.0 und Version 1.1 geändert hat. An diesem Punkt wird eines dieser beiden Projekte nicht mehr ordnungsgemäß funktionieren.

Um dieses Problem zu lösen, empfehlen wir, alle Maven-Projekte zu untergeordneten Elementen eines übergeordneten Reaktorprojekts zu machen. Dieses Reaktorprojekt dient zwei Zwecken: Es ermöglicht den Aufbau und die Bereitstellung aller Projekte, falls gewünscht, und enthält die Abhängigkeitsdeklarationen für alle untergeordneten Projekte. Das übergeordnete Projekt definiert die Abhängigkeiten und ihre Versionen, während die untergeordneten Projekte nur die von ihnen benötigten Abhängigkeiten deklarieren und die Version vom übergeordneten Projekt übernehmen.

Wenn in diesem Szenario das Team, das an Projekt B arbeitet, Funktionen in der foo-Version 1.1 benötigt, wird in der Entwicklungsumgebung schnell deutlich, dass diese Änderung zu einer Beschädigung von Projekt A führt. An dieser Stelle können die Teams diese Änderung besprechen und entweder Projekt A mit der neuen Version kompatibel machen oder nach einer alternativen Lösung für Projekt B suchen.

Dies bedeutet nicht, dass die Teams diese Abhängigkeit nicht mehr teilen müssen. Probleme werden lediglich schnell und frühzeitig hervorgehoben, damit die Teams alle Risiken diskutieren und sich auf eine Lösung einigen können.

### Verhindern von Code-Duplizierung {#preventing-code-duplication-nbsp-br}

Bei der Arbeit an mehreren Projekten ist es wichtig sicherzustellen, dass Code nicht dupliziert wird. Eine Code-Duplizierung erhöht die Wahrscheinlichkeit von Fehlerereignissen, die Kosten von Systemänderungen und die allgemeine Starrheit der Code-Basis. Um eine Duplizierungen zu vermeiden, müssen Sie die allgemeine Logik zu wiederverwendbaren Bibliotheken umgestalten, die über mehrere Projekte hinweg verwendet werden können.

Um diese Notwendigkeit zu unterstützen, empfehlen wir die Entwicklung und Pflege eines Kernprojekts, auf das alle Teams angewiesen sind und zu dem sie beitragen können. Dabei ist es wichtig sicherzustellen, dass dieses Kernprojekt nicht wiederum von den Projekten der einzelnen Teams abhängig ist. Dies ermöglicht eine unabhängige Bereitstellung und fördert weiterhin die Wiederverwendung von Code.

Beispiele für Code, der normalerweise in einem Kernmodul enthalten ist:

* systemweite Konfigurationen wie:
   * OSGi-Konfigurationen
   * Servlet-Filter
   * ResourceResolver-Zuordnungen
   * Sling Transformer-Pipelines
   * Fehler-Handler (oder verwenden Sie ACS AEM Commons Error Page Handler1)
   * Autorisierungs-Servlets für berechtigungssensitive Zwischenspeicherung
* Dienstprogrammklassen
* Kern-Business-Logik
* Drittanbieterintegrationslogik
* Überlagerungen in der Authoring-Benutzeroberfläche
* Andere für das Authoring erforderliche Anpassungen, z. B. benutzerdefinierte Widgets
* Workflow-Starter
* Allgemeine Design-Elemente, die über Sites hinweg verwendet werden

*Modulare Projektarchitektur*

Dadurch entfällt nicht die Notwendigkeit, dass mehrere Teams denselben Code-Satz brauchen und ihn möglicherweise aktualisieren. Durch die Erstellung eines Kernprojekts haben wir die Größe der Code-Basis verringert, die von Teams gemeinsam genutzt wird. Auf diese Weise wird der Bedarf an freigegebenen Ressourcen verringert.

Um sicherzustellen, dass die Änderungen an diesem Kernpaket die Funktionalität des Systems nicht beeinträchtigen, empfehlen wir, dass eine leitende Entwicklungsperson oder ein Entwicklungs-Team die Aufsicht behält. Eine Option besteht darin, dass ein einzelnes Team alle Änderungen an diesem Paket verwaltet. Eine andere wäre, Teams Pull-Anfragen übermitteln zu lassen, die von diesen Ressourcen geprüft und zusammengeführt werden. Es ist wichtig, dass ein Governance-Modell von den Teams entwickelt und akzeptiert wird und dass die Entwicklungspersonen diesem folgen.

## Verwalten des Bereitstellungsumfangs  {#managing-deployment-scope}

Da verschiedene Teams ihren Code in demselben Repository bereitstellen, ist es wichtig, dass sie die Änderungen der anderen nicht überschreiben. AEM verfügt über einen Mechanismus, um dies bei der Bereitstellung von Inhaltspaketen zu steuern: der Filter. XML-Datei. Es ist wichtig, dass es keine Überschneidungen zwischen Filtern gibt.  XML-Dateien, da andernfalls die Bereitstellung eines Teams die vorherige Bereitstellung eines anderen Teams löschen könnte. Dieser Punkt wird in den folgenden Beispielen für gut erstellte vs. problematische Filterdateien verdeutlicht:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Wenn jedes Team seine Filterdatei explizit für die Site bzw. die Sites konfiguriert, an der bzw. denen es arbeitet, kann jedes Team seine Komponenten, Client-Bibliotheken und Site-Designs unabhängig voneinander bereitstellen, ohne die Änderungen des jeweils anderen zu löschen.

Da es sich um einen globalen Systempfad handelt, der nicht für eine Site spezifisch ist, sollte das folgende Servlet in das Kernprojekt aufgenommen werden, da hier vorgenommene Änderungen Auswirkungen auf jedes Team haben können:

/apps/sling/servlet/errorhandler

### Überlagerungen {#overlays}

Überlagerungen werden häufig verwendet, um vordefinierte AEM-Funktionen zu erweitern oder zu ersetzen. Die Verwendung einer Überlagerung wirkt sich jedoch auf die gesamte AEM-Anwendung aus (d. h. alle überlagerten Funktionsänderungen werden für alle Mandanten verfügbar gemacht). Dies wäre noch komplizierter, wenn die Mandanten unterschiedliche Anforderungen an die Überlagerung hätten. Idealerweise sollten Unternehmensgruppen zusammenarbeiten, um eine einheitliche Funktionalität und ein einheitliches Erscheinungsbild von AEM-Verwaltungskonsolen zu erzielen.

Wenn zwischen den verschiedenen Unternehmensgruppen kein Konsens erzielt werden kann, könnte eine Lösung darin bestehen, einfach keine Überlagerungen zu verwenden. Erstellen Sie stattdessen eine benutzerdefinierte Kopie der Funktion und stellen Sie sie für jeden Mandanten über einen unterschiedlichen Pfad bereit. Dies ermöglicht verschiedene Benutzererlebnisse für jeden Mandanten. Dieser Ansatz erhöht jedoch auch die Implementierungskosten und den nachfolgenden Aktualisierungsaufwand.

### Workflow-Starter {#workflow-launchers}

AEM verwendet Workflow-Starter, damit der Trigger-Workflow automatisch durchgeführt wird, wenn bestimmte Änderungen im Repository vorgenommen werden. AEM bietet mehrere vorkonfigurierte Starter, z. B. für die Generierung von Ausgabedarstellungen und Metadatenextraktionsprozesse für neue und aktualisierte Assets. Es ist zwar möglich, diese Starter unverändert zu lassen, doch in einer Umgebung mit mehreren Mandanten müssen einzelne Starter wahrscheinlich für jeden Mandanten erstellt und gepflegt werden, wenn Mandanten unterschiedliche Anforderungen an Starter und/oder Workflow-Modelle haben. Diese Starter müssen so konfiguriert sein, dass sie auf den Aktualisierungen ihres Mandanten ausgeführt werden, während Inhalte anderer Mandanten unverändert bleiben. Dies lässt sich einfach erreichen, indem Starter auf bestimmte Repository-Pfade angewendet werden, die mandantenspezifisch sind.

### Vanity-URLs {#vanity-urls}

AEM bietet Vanity-URL-Funktionen, die pro Seite festgelegt werden können. Das Problem bei diesem Ansatz in einem Szenario mit mehreren Mandanten besteht darin, dass AEM keine Eindeutigkeit bezüglich der auf diese Weise konfigurierten Vanity-URLs gewährleistet. Wenn zwei Benutzende denselben Vanity-Pfad für verschiedene Seiten konfigurieren, kann es zu unerwartetem Verhalten kommen. Aus diesem Grund empfehlen wir die Verwendung von mod_rewrite-Regeln in den Apache-Dispatcher-Instanzen, die einen zentralen Konfigurationspunkt in Abstimmung mit den Regeln des ausgehenden Resource Resolver ermöglichen.

### Komponentengruppen {#component-groups}

Bei der Entwicklung von Komponenten und Vorlagen für mehrere Authoring-Gruppen ist es wichtig, die Eigenschaften „componentGroup“ und „allowedPaths“ effektiv zu nutzen. Indem wir diese effektiv mit Site-Designs nutzen, können wir sicherstellen, dass Autorinnen und Autoren von Marke A nur Komponenten und Vorlagen sehen, die für ihre Site erstellt wurden, während Autorinnen und Autoren von Marke B nur ihre sehen.

### Testen {#testing}

Eine gute Architektur und offene Kommunikationskanäle können zwar dazu beitragen, das Eintreten von Fehlern in unerwartete Bereiche der Site zu verhindern, doch sind diese Ansätze nicht stichhaltig. Aus diesem Grund ist es wichtig, die Bereitstellung auf der Plattform vollständig zu testen, bevor es in die Produktion gelangt. Dies erfordert eine Koordinierung zwischen den Teams in Bezug auf ihre Veröffentlichungszyklen und verstärkt die Notwendigkeit einer Reihe automatisierter Tests, die so viele Funktionen wie möglich abdecken. Da ein System von mehreren Teams gemeinsam genutzt wird, werden Leistung, Sicherheit und Belastungstests wichtiger denn je.

## Betriebliche Überlegungen {#operational-considerations}

### Freigegebene Ressourcen {#shared-resources}

AEM wird in einer einzigen JVM ausgeführt. Alle bereitgestellten AEM-Applikationen teilen sich von Natur aus Ressourcen, zusätzlich zu den Ressourcen, die bereits in der normalen Ausführung von AEM verbraucht wurden. Innerhalb des JVM-Speichers selbst gibt es keine logische Trennung von Threads und die endlichen Ressourcen, die AEM zur Verfügung stehen, wie Speicher, CPU und Datenträger-I/O werden ebenfalls freigegeben. Jeder Mandant, der Ressourcen verbraucht, wirkt sich unweigerlich auf andere Systemmandanten aus.

### Leistung {#performance}

Wenn Sie die Best Practices von AEM nicht befolgen, ist es möglich, Applikationen zu entwickeln, die Ressourcen verbrauchen, die über das für normal erachtete Maß hinausgehen. Beispiele dafür sind die Auslösung vieler umfangreicher Workflow-Vorgänge (wie DAM Update Asset), die Verwendung von MSM-Push-on-Modifizierungsvorgängen über viele Knoten oder die Verwendung teurer JCR-Abfragen zum Rendern von Inhalten in Echtzeit. Diese werden sich unweigerlich auf die Leistung anderer Mandantenapplikationen auswirken.

### Protokollierung {#logging}

AEM bietet vorkonfigurierte Schnittstellen für eine robuste Logger-Konfiguration, die in gemeinsam genutzten Entwicklungsszenarien zu unserem Vorteil genutzt werden kann. Durch die Angabe von separaten Loggern für jede Marke nach Paketnamen können wir eine gewisse Protokolltrennung erreichen. Während systemweite Vorgänge wie Replikation und Authentifizierung weiterhin an einem zentralen Speicherort protokolliert werden, kann nicht freigegebener benutzerdefinierter Code separat protokolliert werden, was die Überwachung und das Debuggen für das technische Team jeder Marke vereinfacht.

### Sichern und Wiederherstellen {#backup-and-restore}

Aufgrund der Eigenschaften des JCR-Repositorys funktionieren herkömmliche Backups über das gesamte Repository und nicht über einen einzelnen Inhaltspfad. Es ist daher nicht möglich, einfach Backups auf Mandantenbasis zu trennen. Umgekehrt werden beim Wiederherstellen aus einer Sicherung Inhalte und Repository-Knoten für alle Mandanten auf dem System zurückgesetzt. Es ist zwar möglich, mithilfe von Tools wie VLT gezielte Inhaltssicherungen durchzuführen oder Inhalte für die Wiederherstellung auszuwählen, indem ein Paket in einer separaten Umgebung erstellt wird,\
aber diese Ansätze umfassen nicht ohne Weiteres Konfigurationseinstellungen oder Anwendungslogik und können umständlich zu verwalten sein.

## Sicherheitsüberlegungen {#security-considerations}

### ACLs {#acls}

Es ist natürlich möglich, Zugriffssteuerungslisten (ACLs) zu verwenden, um zu steuern, wer Zugriff auf das Anzeigen, Erstellen und Löschen von Inhalten hat, die auf Inhaltspfaden basieren. Dies erfordert die Erstellung und Verwaltung von Benutzergruppen. Die Schwierigkeit bei der Pflege der ACLs und Gruppen hängt davon ab, wie wichtig es ist, sicherzustellen, dass jeder Mandant keine Kenntnisse über andere hat und ob die bereitgestellten Applikationen auf gemeinsame Ressourcen angewiesen sind. Um eine effiziente Verwaltung von ACL, Benutzenden und Gruppen zu gewährleisten, empfehlen wir eine zentralisierte Gruppe mit der erforderlichen Aufsicht, um sicherzustellen, dass sich diese Zugriffskontrollen und Prinzipien so überschneiden (oder sich nicht überschneiden), dass Effizienz und Sicherheit gefördert werden.
