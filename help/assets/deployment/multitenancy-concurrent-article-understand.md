---
title: Verstehen der Multitasking- und Gleichzeitigkeitsentwicklung
seo-title: Verstehen der Multitasking- und Gleichzeitigkeitsentwicklung
description: 'null'
seo-description: 'null'
uuid: 682093fe-ce55-4ef8-af10-99f7062f8b1b
discoiquuid: 0dfcdf39-7423-459f-8f35-ee5b4b829f2c
feature: connected-assets
topics: authoring, operations, sharing, publishing
audience: all
doc-type: article
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 99f2a8cdfe0b4f5f6f1a149d96affd2a9e8bcf75
workflow-type: tm+mt
source-wordcount: '2009'
ht-degree: 1%

---


# Verstehen der Multitasking- und Gleichzeitigkeitsentwicklung {#understanding-multitenancy-and-concurrent-development}

## Einführung {#introduction}

Wenn mehrere Teams ihren Code auf die gleiche AEM Umgebung einsetzen, sollten sie sich an die entsprechenden Verfahrensweisen halten, um sicherzustellen, dass die Teams so unabhängig wie möglich arbeiten können, ohne dabei die Zehen anderer Teams zu verlassen. Obwohl sie niemals vollständig eliminiert werden können, werden durch diese Techniken Teamübergreifende Abhängigkeiten minimiert. Damit ein paralleles Entwicklungsmodell erfolgreich sein kann, ist eine gute Kommunikation zwischen den Entwicklungsteams von entscheidender Bedeutung.

Wenn mehrere Entwicklungsteams an der gleichen AEM Umgebung arbeiten, wird wahrscheinlich auch ein gewisses Maß an Mehrspieligkeit im Spiel sein. Es wurde viel über praktische Überlegungen geschrieben, wie man versucht, mehrere Mieter in einer AEM Umgebung zu unterstützen, insbesondere über die Herausforderungen, vor denen die Verwaltung von Verwaltung, Betrieb und Entwicklung steht. In diesem Dokument werden einige der technischen Herausforderungen im Zusammenhang mit der Implementierung von AEM in einer Umgebung mit mehreren Mandanten untersucht, aber viele dieser Empfehlungen werden für jede Organisation mit mehreren Entwicklungsteams gelten.

Es ist wichtig, vorab darauf hinzuweisen, dass AEM zwar mehrere Sites und sogar mehrere Marken unterstützen können, die auf einer einzigen Umgebung bereitgestellt werden, jedoch keine echte Multi-Mandate-Funktion Angebot bietet. Einige Systemkonfigurationen und Systemressourcen werden immer für alle Sites freigegeben, die auf einer Umgebung bereitgestellt werden. Dieses Dokument enthält Leitlinien zur Minimierung der Auswirkungen dieser gemeinsamen Ressourcen und Vorschläge für Angebote, um die Kommunikation und Zusammenarbeit in diesen Bereichen zu straffen.

## Vorteile und Herausforderungen {#benefits-and-challenges}

Die Implementierung einer Umgebung mit mehreren Mandanten stellt viele Herausforderungen dar.

Dazu gehören:

* Zusätzliche technische Komplexität
* Verbesserter Entwicklungsaufwand
* Organisationsübergreifende Abhängigkeiten von freigegebenen Ressourcen
* Erhöhte betriebliche Komplexität

Ungeachtet der Herausforderungen, vor denen eine Anwendung mit mehreren Mandanten steht, bietet die Ausführung einer Anwendung Vorteile wie:

* Geringere Hardwarekosten
* Verkürzte Time-to-Market für zukünftige Websites
* Geringere Implementierungskosten für künftige Mieter
* Standardarchitektur- und Entwicklungspraktiken im gesamten Unternehmen
* Eine gemeinsame Codebasis

Wenn für das Unternehmen eine echte Multi-Pachtzeit erforderlich ist, die kein Wissen über andere Mieter und keinen gemeinsamen Code, Inhalt oder allgemeine Autoren enthält, sind separate Autoreninstanzen die einzige praktikable Option. Der Gesamtanstieg der Entwicklungsbemühungen sollte mit den Einsparungen bei den Infrastruktur- und Lizenzkosten verglichen werden, um festzustellen, ob dieser Ansatz am besten geeignet ist.

## Entwicklungstechniken {#development-techniques}

### Verwalten von Abhängigkeiten {#managing-dependencies}

Bei der Verwaltung von Maven-Projektabhängigkeiten ist es wichtig, dass alle Teams dieselbe Version eines gegebenen OSGi-Bundles auf dem Server verwenden. Um zu veranschaulichen, was schief gehen kann, wenn Maven-Projekte falsch verwaltet werden, stellen wir ein Beispiel dar:

Projekt A hängt von Version 1.0 der Bibliothek ab, foo; foo Version 1.0 ist in ihre Bereitstellung auf dem Server eingebettet. Projekt B ist von Version 1.1 der Bibliothek abhängig, foo; foo Version 1.1 ist in ihre Bereitstellung eingebettet.

Nehmen wir außerdem an, dass sich in dieser Bibliothek eine API zwischen den Versionen 1.0 und 1.1 geändert hat. Zu diesem Zeitpunkt wird eines dieser beiden Projekte nicht mehr ordnungsgemäß funktionieren.

Um diese Bedenken auszuräumen, empfehlen wir, alle Maven-Projekte für Kinder eines Reaktorprojekts zu gestalten. Dieses Reaktorprojekt dient zwei Zwecken: Es ermöglicht die Erstellung und den Einsatz aller Projekte zusammen, falls gewünscht, und es enthält die Abhängigkeitserklärungen für alle untergeordneten Projekte. Das übergeordnete Projekt definiert die Abhängigkeiten und ihre Versionen, während die untergeordneten Projekte nur die Abhängigkeiten deklarieren, die sie benötigen, und die Version vom übergeordneten Projekt übernehmen.

Wenn in diesem Fall das Team, das an Projekt B arbeitet, Funktionen in Version 1.1 von foo benötigt, wird in der Umgebung der Entwicklung schnell deutlich, dass diese Änderung Projekt A beschädigt. An dieser Stelle können die Teams diese Änderung diskutieren und entweder Projekt A mit der neuen Version kompatibel machen oder nach einer alternativen Lösung für Projekt B suchen.

Beachten Sie, dass dies nicht die Notwendigkeit ausschließt, dass diese Teams diese Abhängigkeit teilen - es werden nur Probleme schnell und früh aufgezeigt, sodass Teams alle Risiken diskutieren und eine Lösung vereinbaren können.

### Codeduplizierung verhindern {#preventing-code-duplication-nbsp-br}

Bei der Arbeit an mehreren Projekten ist sicherzustellen, dass Code nicht dupliziert wird. Die Codeduplizierung erhöht die Wahrscheinlichkeit von Fehlern, die Kosten von Systemänderungen und die allgemeine Steifigkeit der Code-Basis. Um Doppelungen zu vermeiden, müssen Sie gängige Logik in wiederverwendbare Bibliotheken umwandeln, die über mehrere Projekte hinweg verwendet werden können.

Zur Unterstützung dieses Bedarfs empfehlen wir die Entwicklung und Pflege eines Kernprojekts, auf das alle Teams angewiesen sind und zu dem sie beitragen können. Dabei ist es wichtig sicherzustellen, dass dieses Kernprojekt nicht wiederum von den Projekten der einzelnen Teams abhängig ist. Dies ermöglicht eine unabhängige Bereitstellbarkeit bei gleichzeitiger Förderung der Codewiederverwendung.

Einige Beispiele für Code, der normalerweise in einem Kernmodul enthalten ist:

* Systemweite Konfigurationen wie:
   * OSGi-Konfigurationen
   * Servlet-Filter
   * ResourceResolver-Zuordnungen
   * Sling Transformer-Pipeline
   * Fehlerhandler (oder verwenden Sie den ACS AEM Commons Error Page Handler1)
   * Autorisierungsservlets für die Zwischenspeicherung mit Berechtigung
* Dienstprogrammklassen
* Kerngeschäftslogik
* Logik der Drittanbieter-Integration
* Authoring-UI-Überlagerungen
* Andere zum Authoring erforderliche Anpassungen, z. B. benutzerdefinierte Widgets
* Workflow-Starter
* Allgemeine Designelemente, die über Sites hinweg verwendet werden

*Modulare Projektarchitektur*

Dadurch entfällt nicht die Notwendigkeit, dass mehrere Teams von demselben Code abhängig sind und möglicherweise denselben Code aktualisieren. Durch die Erstellung eines Kernprojekts haben wir die Größe der Codebase verringert, die von Teams gemeinsam genutzt wird. Dadurch wird der Bedarf an freigegebenen Ressourcen verringert.

Um sicherzustellen, dass die an diesem Kernpaket vorgenommenen Änderungen die Funktionalität des Systems nicht stören, empfehlen wir, dass ein leitender Entwickler oder Entwicklerteam die Aufsicht behält. Eine Möglichkeit besteht darin, ein Team zu haben, das alle Änderungen an diesem Paket verwaltet; eine andere besteht darin, dass Teams Pull-Anforderungen senden, die von diesen Ressourcen überprüft und zusammengeführt werden. Es ist wichtig, dass ein Governance-Modell von den Teams konzipiert und akzeptiert wird und dass die Entwickler diesem Modell folgen.

## Verwalten von Deployment Scope&amp;nbsp {#managing-deployment-scope}

Da verschiedene Teams ihren Code in demselben Repository bereitstellen, ist es wichtig, dass sie die Änderungen der anderen nicht überschreiben. AEM verfügt über einen Mechanismus, um dies bei der Bereitstellung von Inhaltspaketen, dem Filter, zu steuern. XML-Datei. Es ist wichtig, dass es keine Überschneidung zwischen Filtern gibt.  XML-Dateien, andernfalls könnte die Bereitstellung eines Teams die vorherige Bereitstellung eines anderen Teams löschen. Um dies zu illustrieren, sehen Sie sich die folgenden Beispiele gut erstellter oder problematischer Filterdateien an:

/apps/my-Firma vs. /apps/my-Firma/my-site

/etc/clientlibs/my-Firma vs. /etc/clientlibs/my-Firma/my-site

/etc/designs/my-Firma vs. /etc/designs/my-Firma/my-site

Wenn jedes Team seine Filterdatei explizit auf die Site(s) konfiguriert, an der/denen es arbeitet, kann jedes Team seine Komponenten, Client-Bibliotheken und Site-Entwürfe unabhängig voneinander bereitstellen, ohne die Änderungen der anderen zu löschen.

Da es sich um einen globalen Systempfad handelt und nicht nur für eine Site spezifisch ist, sollte das folgende Servlet in das Kernprojekt einbezogen werden, da die hier vorgenommenen Änderungen möglicherweise Auswirkungen auf jedes Team haben könnten:

/apps/sling/servlet/errorhandler

### Überlagerungen {#overlays}

Überlagerungen werden häufig verwendet, um die Funktionalität von vordefinierten AEM zu erweitern oder zu ersetzen. Die Verwendung einer Überlagerung wirkt sich jedoch auf die gesamte AEM aus (d. h., alle Funktionsänderungen sind für alle Mieter verfügbar). Dies wäre noch komplizierter, wenn Mieter unterschiedliche Anforderungen an die Überlagerung hätten. Idealerweise sollten Unternehmensgruppen zusammenarbeiten, um sich über die Funktionalität und das Erscheinungsbild der Verwaltungskonsolen AEM zu einigen.

Wenn zwischen den verschiedenen Geschäftsbereichen kein Konsens erzielt werden kann, könnte eine Lösung darin bestehen, einfach keine Überlagerungen zu verwenden. Erstellen Sie stattdessen eine benutzerdefinierte Kopie der Funktion und stellen Sie sie für jeden Mandanten über einen anderen Pfad bereit. Dadurch kann jeder Mandant ein völlig anderes Benutzererlebnis haben, aber dieser Ansatz erhöht auch die Kosten für die Implementierung und die anschließenden Aktualisierungsbemühungen.

### Workflow-Starter {#workflow-launchers}

AEM verwendet Workflow-Starter, um die Ausführung des Workflows automatisch auszulösen, wenn bestimmte Änderungen im Repository vorgenommen werden. AEM bietet verschiedene Starter, z. B. zum Ausführen von Prozessen zur Generierung von Darstellungen und zur Extraktion von Metadaten für neue und aktualisierte Assets. Es ist zwar möglich, diese Launcher wie bisher zu belassen, aber in einer mehrteiligen Umgebung, wenn Mieter unterschiedliche Anforderungen an das Launcher- und/oder Workflow-Modell haben, ist es wahrscheinlich, dass für jeden Mieter einzelne Launcher erstellt und gepflegt werden müssen. Diese Starter müssen so konfiguriert sein, dass sie auf die Aktualisierungen ihres Mandanten zugreifen können, während Inhalte anderer Mieter unberührt bleiben. Dies lässt sich ganz einfach erreichen, indem Launcher auf bestimmte Repository-Pfade angewendet werden, die für Pächter spezifisch sind.

### Vanity-URLs {#vanity-urls}

AEM bietet Vanity-URL-Funktionen, die pro Seite eingestellt werden können. Die Sorge bei diesem Ansatz bei einem Szenario mit mehreren Mandanten besteht darin, dass AEM keine Eindeutigkeit zwischen den auf diese Weise konfigurierten Vanity-URLs sicherstellt. Wenn zwei verschiedene Benutzer denselben Vanity-Pfad für verschiedene Seiten konfigurieren, kann unerwartetes Verhalten auftreten. Aus diesem Grund empfehlen wir die Verwendung von mod_rewrite-Regeln in den Apache-Dispatcher-Instanzen, die einen zentralen Konfigurationspunkt in Abstimmung mit den Regeln für den Nur-Ressourcen-Resolver-Ausgangs ermöglichen.

### Komponentengruppen {#component-groups}

Bei der Entwicklung von Komponenten und Vorlagen für mehrere Authoring-Gruppen ist es wichtig, die Eigenschaften componentGroup und allowedPaths effektiv zu nutzen. Indem wir diese effektiv mit Site-Entwürfen nutzen, können wir sicherstellen, dass Autoren von Marke A nur Komponenten und Vorlagen sehen, die für ihre Site erstellt wurden, während Autoren von Marke B nur ihre sehen.

### Testen {#testing}

Obwohl eine gute Architektur und offene Kanäle der Kommunikation dazu beitragen können, die Einschleppung von Fehlern in unerwartete Bereiche der Site zu verhindern, sind diese Ansätze nicht stichhaltig. Aus diesem Grund ist es wichtig, das, was auf der Plattform bereitgestellt wird, vollständig zu testen, bevor etwas in die Produktion freigegeben wird. Dies erfordert die Koordinierung zwischen den Teams bei ihren Versionszyklen und macht eine Reihe automatisierter Tests erforderlich, die so viele Funktionen wie möglich abdecken. Da ein System von mehreren Teams gemeinsam genutzt wird, werden außerdem Leistung, Sicherheit und Lastentests wichtiger denn je.

## Operative Aspekte {#operational-considerations}

### Freigegebene Ressourcen {#shared-resources}

AEM wird innerhalb einer einzigen JVM ausgeführt; alle bereitgestellten AEM von Natur aus gemeinsam genutzten Ressourcen, zusätzlich zu den Ressourcen, die bereits während der normalen AEM genutzt werden. Innerhalb des JVM-Raums selbst wird es keine logische Trennung von Threads geben, und die begrenzten Ressourcen, die AEM zur Verfügung stehen, wie z. B. Speicher, CPU und Festplatten-i/o, werden ebenfalls freigegeben. Jeder Mieter, der Ressourcen verbraucht, wirkt sich unweigerlich auf andere Systemmieter aus.

### Leistung {#performance}

Wenn AEM Best Practices nicht befolgt werden, ist es möglich, Anwendungen zu entwickeln, die Ressourcen verbrauchen, die über das hinausgehen, was als normal angesehen wird. Beispiele dafür sind die Auslösung vieler Arbeitsablaufvorgänge (wie DAM Update Asset), der Einsatz von MSM-Push-on-Modifizierungs-Vorgängen über viele Knoten oder der Einsatz teurer JCR-Abfragen zum Rendern von Inhalten in Echtzeit. Diese werden sich unweigerlich auf die Leistung anderer Mietanträge auswirken.

### Protokollierung {#logging}

AEM bietet sofort einsetzbare Schnittstellen für eine stabile Logger-Konfiguration, die in freigegebenen Entwicklungsszenarien zu unserem Vorteil genutzt werden kann. Durch die Angabe separater Logger für jede Marke, nach Paketnamen, können wir eine gewisse Blocktrennung erreichen. Während systemweite Vorgänge wie Replikation und Authentifizierung weiterhin an einem zentralen Speicherort protokolliert werden, kann nicht freigegebener benutzerdefinierter Code separat protokolliert werden, was die Überwachung und das Debugging für das technische Team der einzelnen Marken erleichtert.

### Sichern und Wiederherstellen {#backup-and-restore}

Aufgrund der Beschaffenheit des JCR-Repositorys funktionieren herkömmliche Backups über das gesamte Repository und nicht über einen einzelnen Inhaltspfad. Es ist daher nicht möglich, Backups auf Pachtbasis einfach zu trennen. Umgekehrt werden beim Wiederherstellen von einer Sicherung Inhalt und Repository-Knoten für alle Mandanten auf dem System zurückgenommen. Obwohl es möglich ist, gezielte Inhaltssicherungen mithilfe von Werkzeugen wie VLT durchzuführen oder Inhalte zu sammeln, um durch Erstellen eines Pakets in einer separaten Umgebung wiederherzustellen, werden diese\
Ansätze umfassen nicht einfach Konfigurationseinstellungen oder Anwendungslogik und können schwerfällig zu verwalten sein.

## Sicherheitsüberlegungen {#security-considerations}

### ACLs {#acls}

Es ist natürlich möglich, mithilfe von Zugriffskontrolle Listen (ACLs) zu steuern, wer Zugriff auf Ansichten hat, Inhalte auf der Grundlage von Inhaltspfaden zu erstellen und zu löschen, was die Erstellung und Verwaltung von Benutzergruppen erfordert. Die Schwierigkeit, die ACLs und Gruppen zu erhalten, hängt davon ab, wie sichergestellt werden soll, dass jeder Mieter keine Kenntnisse über andere hat und ob die bereitgestellten Anwendungen auf gemeinsamen Ressourcen beruhen. Um eine effiziente Verwaltung von ACL, Benutzern und Gruppen zu gewährleisten, empfehlen wir eine zentralisierte Gruppe mit der erforderlichen Aufsicht, um sicherzustellen, dass sich diese Zugriffskontrollen und Prinzipale überschneiden (oder nicht überschneiden), sodass Effizienz und Sicherheit gefördert werden.
