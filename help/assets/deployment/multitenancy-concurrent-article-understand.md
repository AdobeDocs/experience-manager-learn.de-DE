---
title: Grundlegendes zu Multitenancy und gleichzeitiger Entwicklung
description: Erfahren Sie mehr über die Vorteile, Herausforderungen und Techniken zur Verwaltung einer Implementierung mit mehreren Mandanten mit Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2022'
ht-degree: 1%

---


# Grundlegendes zu Multitenancy und gleichzeitiger Entwicklung {#understanding-multitenancy-and-concurrent-development}

## Einführung {#introduction}

Wenn mehrere Teams ihren Code in denselben AEM bereitstellen, sollten sie bestimmte Vorgehensweisen befolgen, um sicherzustellen, dass Teams so unabhängig wie möglich arbeiten können, ohne auf die Zehen anderer Teams zu springen. Obwohl sie nie vollständig eliminiert werden können, minimieren diese Techniken die Abhängigkeiten zwischen verschiedenen Teams. Damit ein gleichzeitiges Entwicklungsmodell erfolgreich sein kann, ist eine gute Kommunikation zwischen den Entwicklungsteams von entscheidender Bedeutung.

Wenn mehrere Entwicklungsteams in derselben AEM-Umgebung arbeiten, wird wahrscheinlich ein gewisses Maß an Multi-Tenancy im Spiel sein. Es wurde viel über die praktischen Aspekte geschrieben, die darin bestehen, zu versuchen, mehrere Mandanten in einer AEM zu unterstützen, insbesondere über die Herausforderungen bei der Verwaltung von Governance, Operationen und Entwicklung. In diesem Dokument werden einige der technischen Herausforderungen im Zusammenhang mit der Implementierung von AEM in einer Umgebung mit mehreren Mandanten untersucht, aber viele dieser Empfehlungen gelten für jede Organisation mit mehreren Entwicklungsteams.

Es ist wichtig, zunächst zu beachten, dass AEM zwar mehrere Sites und sogar mehrere Marken unterstützen können, die in einer Umgebung bereitgestellt werden, jedoch keine echte Mandantenfähigkeit bietet. Einige Umgebungskonfigurationen und Systemressourcen werden immer für alle Sites freigegeben, die in einer Umgebung bereitgestellt werden. Dieses Papier enthält Leitlinien für die Minimierung der Auswirkungen dieser gemeinsamen Ressourcen und enthält Vorschläge zur Straffung der Kommunikation und Zusammenarbeit in diesen Bereichen.

## Vorteile und Herausforderungen {#benefits-and-challenges}

Die Implementierung einer Umgebung mit mehreren Mandanten birgt viele Herausforderungen.

Dazu gehören:

* Zusätzliche technische Komplexität
* Erhöhter Entwicklungsaufwand
* Organisationsübergreifende Abhängigkeiten von freigegebenen Ressourcen
* Erhöhte betriebliche Komplexität

Trotz der Herausforderungen bietet die Ausführung einer Anwendung mit mehreren Mandanten Vorteile, wie zum Beispiel:

* Geringere Hardware-Kosten
* Verkürzte Time-to-Market für zukünftige Websites
* Geringere Implementierungskosten für zukünftige Mandanten
* Standardarchitektur und -entwicklungen im gesamten Unternehmen
* Eine gemeinsame Codebase

Wenn das Unternehmen echte Mandantenfähigkeit erfordert, ohne Wissen anderer Mandanten und ohne freigegebenen Code, Inhalt oder gängige Autoren, sind separate Autoreninstanzen die einzige praktikable Option. Der Gesamtanstieg der Entwicklungsbemühungen sollte mit den Einsparungen bei Infrastruktur- und Lizenzkosten verglichen werden, um festzustellen, ob dieser Ansatz am besten geeignet ist.

## Entwicklungstechniken {#development-techniques}

### Verwalten von Abhängigkeiten {#managing-dependencies}

Bei der Verwaltung von Maven-Projektabhängigkeiten ist es wichtig, dass alle Teams dieselbe Version eines bestimmten OSGi-Bundles auf dem Server verwenden. Um zu veranschaulichen, was bei einer fehlerhaften Verwaltung von Maven-Projekten schiefgehen kann, stellen wir ein Beispiel dar:

Projekt A hängt von Version 1.0 der Bibliothek ab, foo; foo Version 1.0 ist in ihre Bereitstellung auf dem Server eingebettet. Projekt B hängt von Version 1.1 der Bibliothek ab, foo; foo Version 1.1 ist in ihre Implementierung eingebettet.

Außerdem nehmen wir an, dass sich in dieser Bibliothek eine API zwischen Version 1.0 und Version 1.1 geändert hat. Zu diesem Zeitpunkt funktioniert eines dieser beiden Projekte nicht mehr ordnungsgemäß.

Um diese Bedenken auszuräumen, empfehlen wir, alle Maven-Projekte zu untergeordneten Elementen eines übergeordneten Reaktorprojekts zu machen. Dieses Reaktorprojekt dient zwei Zwecken: Sie ermöglicht die Erstellung und Bereitstellung aller Projekte, falls gewünscht, und enthält die Abhängigkeitsdeklarationen für alle untergeordneten Projekte. Das übergeordnete Projekt definiert die Abhängigkeiten und ihre Versionen, während die untergeordneten Projekte nur die Abhängigkeiten deklarieren, die sie benötigen, und die Version vom übergeordneten Projekt übernehmen.

Wenn in diesem Szenario das Team, das an Projekt B arbeitet, Funktionen in Version 1.1 von foo erfordert, wird in der Entwicklungsumgebung schnell deutlich, dass diese Änderung Projekt A beschädigt. An dieser Stelle können die Teams diese Änderung besprechen und entweder Projekt A mit der neuen Version kompatibel machen oder nach einer alternativen Lösung für Projekt B suchen.

Beachten Sie, dass dies nicht die Notwendigkeit ausschließt, dass diese Teams diese Abhängigkeit teilen. Es werden nur Probleme schnell und frühzeitig hervorgehoben, damit Teams alle Risiken diskutieren und sich auf eine Lösung einigen können.

### Verhindern der Codeduplizierung {#preventing-code-duplication-nbsp-br}

Bei der Arbeit an mehreren Projekten ist es wichtig sicherzustellen, dass Code nicht dupliziert wird. Eine Codeduplizierung erhöht die Wahrscheinlichkeit von Fehlerereignissen, die Kosten von Systemänderungen und die allgemeine Steifigkeit der Codebasis. Um Duplizierungen zu vermeiden, müssen Sie die allgemeine Logik in wiederverwendbare Bibliotheken umgestalten, die über mehrere Projekte hinweg verwendet werden können.

Um diese Notwendigkeit zu unterstützen, empfehlen wir die Entwicklung und Wartung eines Kernprojekts, auf das alle Teams angewiesen sind und zu dem sie beitragen können. Dabei muss sichergestellt werden, dass dieses Kernprojekt nicht von den Projekten der einzelnen Teams abhängig ist. Dies ermöglicht eine unabhängige Bereitstellung und fördert weiterhin die Wiederverwendung von Code.

Beispiele für Code, der normalerweise in einem Kernmodul enthalten ist:

* Systemweite Konfigurationen wie:
   * OSGi-Konfigurationen
   * Servlet-Filter
   * ResourceResolver-Zuordnungen
   * Sling Transformer-Pipelines
   * Fehler-Handler (oder verwenden Sie den ACS AEM Commons Error Page Handler1)
   * Autorisierungsservlets für berechtigungssensitive Zwischenspeicherung
* Dienstprogrammklassen
* Kerngeschäftslogik
* Drittanbieterintegrationslogik
* Überlagerungen in der Authoring-Benutzeroberfläche
* Andere für das Authoring erforderliche Anpassungen, z. B. benutzerdefinierte Widgets
* Workflow-Starter
* Allgemeine Designelemente, die über Sites hinweg verwendet werden

*Modulare Projektarchitektur*

Dadurch entfällt nicht die Notwendigkeit, dass mehrere Teams von demselben Code abhängig sind und möglicherweise diesen aktualisieren. Durch die Erstellung eines Kernprojekts haben wir die Größe der Codebase verringert, die von Teams gemeinsam genutzt wird. Auf diese Weise wird der Bedarf an freigegebenen Ressourcen verringert.

Um sicherzustellen, dass die Änderungen an diesem Kernpaket die Funktionalität des Systems nicht beeinträchtigen, empfehlen wir, dass ein leitender Entwickler oder Entwicklerteam die Aufsicht behält. Eine Option besteht darin, ein einzelnes Team zu haben, das alle Änderungen an diesem Paket verwaltet. Eine andere Möglichkeit besteht darin, Teams Pull-Anforderungen senden zu lassen, die von diesen Ressourcen geprüft und zusammengeführt werden. Es ist wichtig, dass ein Governance-Modell von den Teams entwickelt und akzeptiert wird und dass die Entwickler diesem folgen.

## Verwalten des Bereitstellungsumfangs&amp;nbsp {#managing-deployment-scope}

Da verschiedene Teams ihren Code in demselben Repository bereitstellen, ist es wichtig, dass sie die Änderungen der anderen nicht überschreiben. AEM verfügt über einen Mechanismus, um dies bei der Bereitstellung von Inhaltspaketen, dem Filter, zu steuern. XML-Datei. Es ist wichtig, dass es keine Überschneidungen zwischen Filtern gibt.  XML-Dateien, da andernfalls die Bereitstellung eines Teams die vorherige Bereitstellung eines anderen Teams löschen könnte. Um diesen Punkt zu veranschaulichen, sehen Sie sich die folgenden Beispiele für gut erstellte und problematische Filterdateien an:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Wenn jedes Team seine Filterdatei explizit auf die Site(s) konfiguriert, an denen es arbeitet, kann jedes Team seine Komponenten, Client-Bibliotheken und Site-Designs unabhängig voneinander bereitstellen, ohne die Änderungen des jeweils anderen zu löschen.

Da es sich um einen globalen Systempfad handelt und nicht nur für eine Site spezifisch ist, sollte das folgende Servlet in das Kernprojekt aufgenommen werden, da hier vorgenommene Änderungen möglicherweise Auswirkungen auf jedes Team haben können:

/apps/sling/servlet/errorhandler

### Überlagerungen {#overlays}

Überlagerungen werden häufig verwendet, um vordefinierte AEM zu erweitern oder zu ersetzen. Die Verwendung einer Überlagerung wirkt sich jedoch auf die gesamte AEM Anwendung aus (d. h. alle Funktionsänderungen sind für alle Mandanten verfügbar). Dies wäre noch komplizierter, wenn die Mandanten unterschiedliche Anforderungen an die Überlagerung hätten. Idealerweise sollten Unternehmensgruppen zusammenarbeiten, um sich auf die Funktionalität und das Erscheinungsbild der AEM Verwaltungskonsolen zu einigen.

Wenn zwischen den verschiedenen Geschäftsbereichen kein Konsens erzielt werden kann, könnte eine mögliche Lösung darin bestehen, einfach keine Überlagerungen zu verwenden. Erstellen Sie stattdessen eine benutzerdefinierte Kopie der Funktion und stellen Sie sie über einen anderen Pfad für jeden Mandanten bereit. Dadurch kann jeder Mandant über ein völlig anderes Benutzererlebnis verfügen. Dieser Ansatz erhöht jedoch auch die Implementierungskosten und die nachfolgenden Aktualisierungsbemühungen.

### Workflow-Starter {#workflow-launchers}

AEM verwendet Workflow-Starter, um die Ausführung des Triggers-Workflows automatisch durchzuführen, wenn bestimmte Änderungen im Repository vorgenommen werden. AEM bietet mehrere vorkonfigurierte Starter, um beispielsweise die Generierung von Ausgabedarstellungen und die Metadatenextraktion für neue und aktualisierte Assets auszuführen. Es ist zwar möglich, diese Starter unverändert zu lassen, doch in einer Umgebung mit mehreren Mandanten müssen einzelne Starter wahrscheinlich für jeden Mandanten erstellt und gepflegt werden, wenn Mandanten unterschiedliche Anforderungen an Starter und/oder Workflow-Modelle haben. Diese Starter müssen so konfiguriert werden, dass sie auf den Aktualisierungen ihres Mandanten ausgeführt werden, während Inhalte anderer Mandanten unverändert bleiben. Dies lässt sich einfach erreichen, indem Launcher auf bestimmte Repository-Pfade angewendet werden, die mandantenspezifisch sind.

### Vanity-URLs {#vanity-urls}

AEM bietet Vanity-URL-Funktionen, die pro Seite festgelegt werden können. Das Problem bei diesem Ansatz in einem Szenario mit mehreren Mandanten besteht darin, dass AEM keine Eindeutigkeit zwischen den auf diese Weise konfigurierten Vanity-URLs gewährleistet. Wenn zwei verschiedene Benutzer denselben Vanity-Pfad für verschiedene Seiten konfigurieren, kann es zu unerwartetem Verhalten kommen. Aus diesem Grund empfehlen wir die Verwendung von mod_rewrite-Regeln in den Apache Dispatcher-Instanzen, die einen zentralen Konfigurationspunkt in Abstimmung mit den Regeln des ausgehenden Resource Resolver ermöglichen.

### Komponentengruppen {#component-groups}

Bei der Entwicklung von Komponenten und Vorlagen für mehrere Authoring-Gruppen ist es wichtig, die Eigenschaften componentGroup und allowedPaths effektiv zu nutzen. Indem wir diese effektiv mit Site-Designs nutzen, können wir sicherstellen, dass Autoren von Marke A nur Komponenten und Vorlagen sehen, die für ihre Site erstellt wurden, während Autoren von Marke B nur ihre sehen.

### Tests {#testing}

Eine gute Architektur und offene Kommunikationskanäle können zwar dazu beitragen, das Eintreten von Fehlern in unerwartete Bereiche der Site zu verhindern, doch sind diese Ansätze nicht stichhaltig. Aus diesem Grund ist es wichtig, die Bereitstellung auf der Plattform vollständig zu testen, bevor etwas in die Produktion eingeführt wird. Dies erfordert die Koordinierung zwischen Teams bei ihren Release-Zyklen und macht eine Suite automatisierter Tests erforderlich, die so viele Funktionen wie möglich abdecken. Da ein System von mehreren Teams gemeinsam genutzt wird, werden Leistung, Sicherheit und Belastungstests wichtiger denn je.

## Operative Aspekte {#operational-considerations}

### Gemeinsame Ressourcen {#shared-resources}

AEM läuft innerhalb einer einzelnen JVM; Alle bereitgestellten AEM teilen sich von Natur aus Ressourcen miteinander, zusätzlich zu den Ressourcen, die bereits für die normale AEM verbraucht wurden. Innerhalb des JVM-Speichers selbst gibt es keine logische Trennung von Threads. Außerdem werden die endlichen Ressourcen, die AEM zur Verfügung stehen, wie Speicher, CPU und Datenträger-i/o ebenfalls freigegeben. Jeder Mandant, der Ressourcen verbraucht, wirkt sich unweigerlich auf andere Systemmandanten aus.

### Leistung {#performance}

Wenn Sie AEM Best Practices nicht befolgen, ist es möglich, Anwendungen zu entwickeln, die Ressourcen verbrauchen, die über das für normal erachtete Maß hinausgehen. Beispiele dafür sind die Auslösung vieler umfangreicher Workflow-Vorgänge (wie DAM Update Asset), die Verwendung von MSM-Push-on-Modifizierungsvorgängen über viele Knoten oder die Verwendung teurer JCR-Abfragen zum Rendern von Inhalten in Echtzeit. Diese werden sich unweigerlich auf die Leistung anderer Mandantenanwendungen auswirken.

### Protokollierung {#logging}

AEM bietet vorkonfigurierte Schnittstellen für eine robuste Logger-Konfiguration, die in gemeinsam genutzten Entwicklungsszenarien zu unserem Vorteil genutzt werden kann. Wenn Sie für jede Marke separate Logger nach Paketnamen angeben, können wir eine gewisse Log-Trennung erreichen. Während systemweite Vorgänge wie Replikation und Authentifizierung weiterhin an einem zentralen Speicherort protokolliert werden, kann nicht freigegebener benutzerdefinierter Code separat protokolliert werden, was die Überwachung und das Debugging für das technische Team jeder Marke vereinfacht.

### Sichern und Wiederherstellen {#backup-and-restore}

Aufgrund der Eigenschaften des JCR-Repositorys funktionieren herkömmliche Backups über das gesamte Repository und nicht über einen einzelnen Inhaltspfad. Es ist daher nicht möglich, Backups auf Mandantenbasis einfach zu trennen. Umgekehrt werden beim Wiederherstellen aus einer Sicherung Inhalte und Repository-Knoten für alle Mandanten auf dem System zurückgesetzt. Auch wenn es möglich ist, mithilfe von Tools wie VLT gezielte Sicherungen von Inhalten durchzuführen oder Inhalte zu übernehmen, um sie wiederherzustellen, indem ein Paket in einer separaten Umgebung erstellt wird, werden diese\
Ansätze umfassen nicht einfach Konfigurationseinstellungen oder Anwendungslogik und können schwerfällig zu verwalten sein.

## Sicherheitsüberlegungen {#security-considerations}

### ACLs {#acls}

Es ist natürlich möglich, Zugriffssteuerungslisten (ACLs) zu verwenden, um zu steuern, wer Zugriff auf das Anzeigen, Erstellen und Löschen von Inhalten hat, die auf Inhaltspfaden basieren. Dies erfordert die Erstellung und Verwaltung von Benutzergruppen. Die Schwierigkeit bei der Pflege der ACLs und Gruppen hängt davon ab, wie wichtig es ist, sicherzustellen, dass jeder Mandant keine Kenntnisse über andere hat und ob die bereitgestellten Anwendungen auf gemeinsame Ressourcen angewiesen sind. Um eine effiziente Verwaltung von ACL, Benutzern und Gruppen zu gewährleisten, empfehlen wir eine zentralisierte Gruppe mit der erforderlichen Aufsicht, um sicherzustellen, dass sich diese Zugriffskontrollen und Prinzipale so überschneiden (oder sich nicht überschneiden), dass Effizienz und Sicherheit gefördert werden.
