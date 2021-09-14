---
title: Entwicklerkonsole
description: AEM as a Cloud Service stellt eine Entwicklerkonsole für jede Umgebung bereit, die verschiedene Details des ausgeführten AEM-Dienstes anzeigt, die beim Debugging hilfreich sind.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1348'
ht-degree: 2%

---

# Debugging von AEM als Cloud Service mit der Developer Console

AEM as a Cloud Service stellt eine Entwicklerkonsole für jede Umgebung bereit, die verschiedene Details des ausgeführten AEM-Dienstes anzeigt, die beim Debugging hilfreich sind.

Jede AEM als Cloud Service-Umgebung verfügt über eine eigene Entwicklerkonsole.

## Zugriff auf die Developer Console

Um auf die Entwicklerkonsole zuzugreifen und sie zu verwenden, müssen die folgenden Berechtigungen über die Admin Console [der Adobe](https://adminconsole.adobe.com) der Adobe ID des Entwicklers gewährt werden.

1. Stellen Sie sicher, dass die Adobe-Organisation, die Cloud Manager und AEM as a Cloud Service-Produkte ausgeführt hat, im Adobe-Org-Umschalter aktiv ist.
1. Der Entwickler muss Mitglied des Cloud Manager-Produktprofils __Entwickler - Cloud Service__ sein.
   + Wenn diese Mitgliedschaft nicht vorhanden ist, kann sich der Entwickler nicht bei der Developer Console anmelden.
1. Der Entwickler muss Mitglied des Produktprofils __AEM Benutzer__ oder __AEM Administratoren__ in der AEM-Autoreninstanz und/oder -Veröffentlichungsinstanz sein.
   + Wenn diese Mitgliedschaft nicht vorhanden ist, werden die [status](#status)-Dumps mit einem 401 Unauthorized-Fehler als Zeitüberschreitung gekennzeichnet.

### Fehlerbehebung für den Zugriff auf die Developer Console

#### 401 Unzulässiger Fehler beim Dumpingstatus

![Entwicklerkonsole - 401 Nicht autorisiert](./assets/developer-console/troubleshooting__401-unauthorized.png)

Wenn ein Status-Fehler 401 &quot;Nicht autorisiert&quot;gemeldet wird, bedeutet dies, dass Ihr Benutzer noch nicht über die erforderlichen Berechtigungen in AEM als Cloud Service verfügt oder die verwendeten Anmelde-Token ungültig sind oder abgelaufen sind.

So beheben Sie das Problem 401 Unerlaubt :

1. Stellen Sie sicher, dass Ihr Benutzer Mitglied des entsprechenden Adobe IMS-Produktprofils (AEM Administratoren oder AEM Benutzer) für die zugehörige AEM der Developer Console als Cloud Service-Produktinstanz ist.
   + Denken Sie daran, dass die Developer Console auf 2 Adobe IMS-Produktinstanzen zugreifen kann. Stellen Sie die AEM als Cloud Service-Autoren- und -Veröffentlichungsinstanzen sicher, dass die richtigen Produktprofile verwendet werden, je nachdem, welche Dienststufe Zugriff über die Developer Console erfordert.
1. Melden Sie sich bei der AEM als Cloud Service an (Autor oder Veröffentlichung) und stellen Sie sicher, dass Ihre Benutzer und Gruppen ordnungsgemäß mit AEM synchronisiert wurden.
   + Entwicklerkonsole erfordert, dass Ihr Benutzerdatensatz in der entsprechenden AEM Dienststufe erstellt wird, damit er sich bei dieser Dienststufe authentifizieren kann.
1. Löschen Sie Ihre Browser-Cookies sowie den Anwendungsstatus (lokaler Speicher) und melden Sie sich erneut bei der Developer Console an, um sicherzustellen, dass das Zugriffstoken Developer Console richtig und nicht abgelaufen ist.

## Pod

AEM als Cloud Service-Autoren- und -Veröffentlichungsdienste bestehen aus mehreren Instanzen, um Traffic-Variablen und rollierende Aktualisierungen ohne Ausfallzeiten zu verarbeiten. Diese Instanzen werden als Pods bezeichnet. Die Auswahl von Werbeunterbrechungen in der Developer Console definiert den Umfang der Daten, die über die anderen Steuerelemente verfügbar gemacht werden.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ Ein Pod ist eine separate Instanz, die Teil eines AEM-Dienstes (Autor oder Veröffentlichung) ist.
+ Pods sind transient, d. h. AEM als Cloud Service sie nach Bedarf erstellt und zerstört
+ Nur Pods, die Teil der zugehörigen AEM als Cloud Service-Umgebung sind, werden im Pod-Umschalter der Entwicklerkonsole dieser Umgebung aufgelistet.
+ Unten im Pod-Umschalter können Sie mithilfe der bequemen Optionen Pods nach Diensttyp auswählen:
   + Alle Autoren
   + Alle Herausgeber
   + Alle Instanzen

## Status

Status bietet Optionen zum Ausgeben eines bestimmten AEM Laufzeitstatus in der Text- oder JSON-Ausgabe. Die Developer Console bietet ähnliche Informationen wie die lokale Schnellstart-OSGi-Web-Konsole des AEM SDK, mit dem deutlichen Unterschied, dass die Developer Console schreibgeschützt ist.

![Entwicklerkonsole - Status](./assets/developer-console/status.png)

### Bundles

Bundles listen alle OSGi-Bundles in AEM auf. Diese Funktion ähnelt den lokalen Schnellstart-OSGi-Bundles des AEM-SDK[unter `/system/console/bundles`.](http://localhost:4502/system/console/bundles)

Bundles helfen beim Debugging durch:

+ Auflisten aller OSGi-Pakete, die in AEM as a Service bereitgestellt werden
+ Auflisten des Status jedes OSGi-Bundles; , auch wenn sie aktiv sind oder nicht
+ Bereitstellen von Details zu nicht aufgelösten Abhängigkeiten, die dazu führen, dass OSGi-Bundles aktiv werden

### Komponenten

Komponenten listen alle OSGi-Komponenten in AEM auf. Diese Funktion ähnelt den lokalen Schnellstart-OSGi-Komponenten des AEM-SDK[unter `/system/console/components`.](http://localhost:4502/system/console/components)

Komponenten helfen beim Debugging durch:

+ Auflisten aller OSGi-Komponenten, die AEM als Cloud Service bereitgestellt wurden
+ Bereitstellen des Status jeder OSGi-Komponente; , auch wenn sie aktiv oder unzufrieden sind
+ Die Bereitstellung von Details zu nicht zufrieden stellenden Service-Referenzen kann dazu führen, dass OSGi-Komponenten aktiv werden
+ Auflisten von OSGi-Eigenschaften und deren Werten, die an die OSGi-Komponente gebunden sind

### Konfigurationen

Konfigurationen führen alle Konfigurationen der OSGi-Komponente (OSGi-Eigenschaften und -Werte) auf. Diese Funktion ähnelt dem lokalen Schnellstart-OSGi Configuration Manager des [AEM SDK unter `/system/console/configMgr`.](http://localhost:4502/system/console/configMgr)

Hilfe beim Debugging von Konfigurationen durch:

+ Auflisten von OSGi-Eigenschaften und deren Werten nach OSGi-Komponente
+ Fehlkonfigurierte Eigenschaften suchen und identifizieren

### Oak-Indizes

Oak-Indizes bieten einen Dump der Knoten, die unter `/oak:index` definiert sind. Beachten Sie, dass dies keine zusammengeführten Indizes anzeigt, die auftreten, wenn ein AEM geändert wird.

Oak-Indizes helfen beim Debugging durch:

+ Auflisten aller Oak-Index-Definitionen, die Einblicke in die Ausführung von Suchanfragen in AEM bieten. Beachten Sie, dass Änderungen an AEM Indizes hier nicht übernommen werden. Diese Ansicht ist nur für Indizes hilfreich, die ausschließlich von AEM bereitgestellt werden oder ausschließlich vom benutzerspezifischen Code bereitgestellt werden.

### OSGi-Dienste

Komponenten listen alle OSGi-Dienste auf. Diese Funktion ähnelt den lokalen Schnellstart-OSGi-Services des AEM-SDK](http://localhost:4502/system/console/services) unter `/system/console/services`.[

Hilfe zu OSGi-Diensten beim Debugging durch:

+ Auflisten aller OSGi-Dienste in AEM zusammen mit dem bereitgestellten OSGi-Bundle und allen OSGi-Bundles, die es nutzen

### Sling Jobs

In Sling-Aufträgen werden alle Warteschlangen für Sling-Aufträge aufgelistet. Diese Funktion ähnelt den lokalen Schnellstart-Aufträgen des AEM SDK](http://localhost:4502/system/console/slingevent) unter `/system/console/slingevent`.[

Hilfe zu Sling-Aufträgen beim Debugging durch:

+ Auflistung der Sling-Auftragswarteschlangen und deren Konfigurationen
+ Bereitstellung von Einblicken in die Anzahl der aktiven, in der Warteschlange befindlichen und verarbeiteten Sling-Aufträge, was beim Debugging von Problemen mit Workflow, Übergangs-Workflow und anderen von Sling-Aufträgen in AEM durchgeführten Arbeiten hilfreich ist.

## Java-Pakete

Java-Pakete ermöglichen die Überprüfung, ob ein Java-Paket und eine -Version zur Verwendung in AEM as a Cloud Service verfügbar sind. Diese Funktion entspricht der von [AEM SDKs verwendeten Dependency Finder](http://localhost:4502/system/console/depfinder) unter `/system/console/depfinder`.

![Entwicklerkonsole - Java-Pakete](./assets/developer-console/java-packages.png)

Java-Pakete werden verwendet, um Probleme beim Erstellen von Bundles zu vermeiden, die aufgrund nicht aufgelöster Importe oder nicht aufgelöster Klassen in Skripten (HTL, JSP usw.) nicht gestartet werden. Wenn Java Packages keine Pakete exportiert, exportiert dies ein Java-Paket (oder die Version stimmt nicht mit der von einem OSGi-Bundle importierten Version überein):

+ Stellen Sie sicher, dass die Version der AEM API-Maven-Abhängigkeit Ihres Projekts mit der AEM Release-Version der Umgebung übereinstimmt (und aktualisieren Sie nach Möglichkeit alles auf die neueste Version).
+ Wenn zusätzliche Maven-Abhängigkeiten im Maven-Projekt verwendet werden
   + Bestimmen Sie, ob stattdessen eine alternative API verwendet werden kann, die von der AEM SDK-API-Abhängigkeit bereitgestellt wird.
   + Wenn die zusätzliche Abhängigkeit erforderlich ist, stellen Sie sicher, dass sie als OSGi-Bundle (anstatt als einfache JAR-Datei) bereitgestellt und in das Code-Paket Ihres Projekts (`ui.apps`) eingebettet ist, ähnlich wie das Core-OSGi-Bundle in das `ui.apps`-Paket eingebettet ist.

## Servlets

Servlets werden verwendet, um Einblicke darüber zu erhalten, wie AEM eine URL zu einem Java-Servlet oder -Skript (HTL, JSP) auflöst, das die Anfrage letztendlich verarbeitet. Diese Funktion entspricht der des lokalen Schnellstarts für den Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) des AEM SDK unter `/system/console/servletresolver`.[

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Servlets helfen beim Debugging der Bestimmung:

+ Wie eine URL in adressierbare Teile (Ressource, Selektor, Erweiterung) zerlegt wird.
+ Welches Servlet oder Skript eine URL auflöst, hilft bei der Identifizierung von falsch formatierten URLs oder falsch registrierten Servlets/Skripten.

## Abfragen

Abfragen bieten Einblicke in was und wie Suchanfragen auf AEM ausgeführt werden. Diese Funktion entspricht der Konsole &quot;Tools > Abfrageleistung ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)&quot;AEM SDK für den lokalen Schnellstart.[

Abfragen funktionieren nur, wenn ein bestimmter Pod ausgewählt ist, da die Webkonsole &quot;Abfrageleistung&quot;dieses Pods geöffnet wird. Entwickler müssen Zugriff auf den AEM haben.

![Entwicklerkonsole - Abfragen - Abfrage erläutern](./assets/developer-console/queries__explain-query.png)

Abfragen erleichtern das Debugging durch:

+ Erläuterung der Interpretation, Analyse und Ausführung von Abfragen durch Oak. Dies ist sehr wichtig, wenn Sie nachverfolgen möchten, warum eine Abfrage langsam ist, und wissen, wie sie beschleunigt werden kann.
+ Auflisten der beliebtesten Abfragen, die in AEM ausgeführt werden, mit der Möglichkeit, sie zu erläutern.
+ Auflisten der langsamsten Abfragen, die in AEM ausgeführt werden, mit der Möglichkeit, sie zu erläutern.
