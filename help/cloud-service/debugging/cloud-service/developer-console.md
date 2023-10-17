---
title: Developer Console
description: AEM as a Cloud Service bietet eine Developer Console für jede Umgebung, die verschiedene Details des ausgeführten AEM-Dienstes anzeigt, die beim Debugging hilfreich sind.
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
source-git-commit: 8ca9535866cc1a673a59ac3743847e68dfedd156
workflow-type: ht
source-wordcount: '1471'
ht-degree: 100%

---

# Debugging von AEM as a Cloud Service mit Developer Console

AEM as a Cloud Service bietet eine Developer Console für jede Umgebung, die verschiedene Details des ausgeführten AEM-Dienstes anzeigt, die beim Debugging hilfreich sind.

Jede AEM as a Cloud Service-Umgebung verfügt über eine eigene Developer Console.

## Navigieren zur Developer Console

Der Zugriff auf Developer Console erfolgt über eine AEM as a Cloud Service-Umgebung über Cloud Manager.

![Navigieren zur Developer Console](./assets/developer-console/navigate.png)

1. Navigieren Sie zu __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Öffnen Sie das __Programm__, das die AEM as a Cloud Service-Umgebung enthält, und öffnen Sie Developer Console.
3. Suchen Sie die __Umgebung__ und wählen Sie die `...`.
4. Wählen Sie __Developer Console__ aus der Dropdown-Liste aus.


## Zugriff auf Developer Console

Um auf Developer Console zuzugreifen und sie zu verwenden, müssen die folgenden Berechtigungen der Adobe ID des Entwicklers bzw. der Entwicklerin über [Admin Console von Adobe](https://adminconsole.adobe.com) gegeben werden.

1. Stellen Sie sicher, dass die Adobe-Organisation, um die es bei Cloud Manager- und AEM as a Cloud Service-Produkten geht, im Adobe-Organisations-Umschalter aktiv ist.
1. Die Entwicklungspersonen müssen Mitglieder des [Produktprofils __Entwickler – Cloud Service__ des Cloud Manager-Produkts](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html?lang=de#assign-developer) sein.
   + Wenn diese Mitgliedschaft nicht vorhanden ist, können sich Entwicklungspersonen nicht bei Developer Console anmelden.
1. Die Entwickelnden müssen Mitglieder des Produktprofils [__AEM-Benutzer__ oder __AEM-Admins__ bei AEM Author und/oder Publish](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=de#aem-product-profiles) sein.
   + Wenn diese Mitgliedschaft nicht vorhanden ist, wird bei den [Statusmeldungen](#status) eine Zeitüberschreitung mit dem Fehler „401: Nicht autorisiert“ angezeigt.

### Fehlerbehebung für den Zugriff auf Developer Console

#### 401 „Nicht autorisiert“-Fehler bei der Statusmeldung

![Developer Console – 401: Nicht autorisiert](./assets/developer-console/troubleshooting__401-unauthorized.png)

Wenn der Status-Fehler 401 „Nicht autorisiert“ gemeldet wird, bedeutet dies, dass die Benutzenden noch nicht über die erforderlichen Berechtigungen in AEM as a Cloud Service verfügen oder die verwendeten Anmelde-Token ungültig sind bzw. abgelaufen sind.

Gehen Sie folgendermaßen vor, um das Problem „401: Nicht autorisiert“ zu lösen:

1. Stellen Sie sicher, dass die Benutzenden Mitglieder des entsprechenden Adobe IMS-Produktprofils (AEM-Admins oder AEM-Benutzer) für die zugehörige AEM as a Cloud Service-Produktinstanz von Developer Console sind.
   + Denken Sie daran, dass Developer Console auf zwei Adobe IMS-Produktinstanzen zugreift, nämlich die Autoren- und Veröffentlichungsinstanz von AEM as a Cloud Service. Stellen Sie daher sicher, dass die richtigen Produktprofile verwendet werden, je nachdem, welche Dienststufe Zugriff über Developer Console erfordert.
1. Melden Sie sich bei AEM as a Cloud Service (Author oder Publish) an und stellen Sie sicher, dass die Benutzenden und Gruppen ordnungsgemäß mit AEM synchronisiert wurden.
   + Developer Console erfordert, dass Ihr Benutzerdatensatz in der zugehörigen AEM-Service-Ebene erstellt wird, damit er sich bei dieser Service-Ebene authentifizieren kann.
1. Löschen Sie Ihre Browser-Cookies sowie den Anwendungsstatus (lokalen Speicher) und melden Sie sich erneut bei Developer Console an, um sicherzustellen, dass das Zugriffs-Token, das Developer Console benutzt, richtig und nicht abgelaufen ist.

## Pod

Der Author- und Publish-Service von AEM as a Cloud Service bestehen aus jeweils mehreren Instanzen, um Traffic-Varianzen und rollierende Aktualisierungen ohne Ausfallzeiten zu handhaben. Diese Instanzen werden als Pods bezeichnet. Die Auswahl von Pods in Developer Console definiert den Umfang der Daten, die über die anderen Steuerelemente verfügbar gemacht werden.

![Developer Console – Pod](./assets/developer-console/pod.png)

+ Ein Pod ist eine separate Instanz, die Teil eines AEM-Services (Author oder Publish) ist.
+ Pods sind temporär, d. h., AEM as a Cloud Service erstellt und zerstört sie nach Bedarf
+ Nur Pods, die Teil der verknüpften AEM as a Cloud Service-Umgebung sind, werden im Pod-Umschalter der Developer Console dieser Umgebung aufgelistet.
+ Unten im Pod-Umschalter können Sie Pods mithilfe bequemer Optionen nach Diensttyp auswählen:
   + Alle Autorinnen und Autoren
   + Alle Veröffentlichenden
   + Alle Instanzen

## Status

Der Status bietet Optionen zum Ausgeben eines bestimmten AEM-Laufzeitstatus als Text- oder JSON-Ausgabe. Die Developer Console bietet ähnliche Informationen wie die OSGi-Web-Konsole für den lokalen Schnellstart von AEM SDK, mit dem wichtigen Unterschied, dass die Developer Console schreibgeschützt ist.

![Developer Console – Status](./assets/developer-console/status.png)

### Bundles

Mit „Bundles“ werden alle OSGi-Bundles in AEM aufgelistet. Diese Funktion ähnelt der jenigen der [OSGi-Bundles für lokale Schnellstarts von AEM SDK](http://localhost:4502/system/console/bundles) unter `/system/console/bundles`.

„Bundles“ hilft beim Debugging durch:

+ Auflisten aller OSGi-Bundles, die in AEM as a Service bereitgestellt werden
+ Auflisten des Status jedes OSGi-Bundles, einschließlich dessen, ob es aktiv ist oder nicht
+ Bereitstellen von Details zu nicht aufgelösten Abhängigkeiten, die verhindern, dass OSGi-Bundles aktiv werden

### Komponenten

Mit „Komponenten“ werden alle OSGi-Komponenten in AEM aufgelistet. Diese Funktion ähnelt der [OSGi-Komponente für den lokalen Schnellstart von AEM SDK](http://localhost:4502/system/console/components) unter `/system/console/components`.

„Komponenten“ hilft beim Debugging durch:

+ Auflisten aller OSGi-Komponenten, die auf AEM as a Cloud Service bereitgestellt werden
+ Angeben des Status jeder OSGi-Komponente, einschließlich dessen, ob sie aktiv oder nicht erfüllt sind
+ Bereitstellen von Details zu nicht aufgelösten Dienstreferenzen, die verhindern, dass OSGi-Komponenten aktiv werden
+ Auflisten von OSGi-Eigenschaften und deren Werten, die an die OSGi-Komponente gebunden sind.
   + Dadurch werden die tatsächlichen Werte angezeigt, die über [Konfigurationsvariablen der OSGi-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#environment-specific-configuration-values) injiziert werden.

### Konfigurationen

Mit „Konfigurationen“ werden alle Konfigurationen der OSGi-Komponente (OSGi-Eigenschaften und -Werte) aufgelistet. Diese Funktion ähnelt dem [OSGi-Konfigurations-Manager für den lokalen Schnellstart des AEM SDK](http://localhost:4502/system/console/configMgr) unter `/system/console/configMgr`.

„Konfigurationen“ hilft beim Debugging durch:

+ Auflisten von OSGi-Eigenschaften und deren Werten nach OSGi-Komponente
   + Dies zeigt NICHT die tatsächlichen Werte an, die über [Konfigurationsvariablen der OSGi-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=de#environment-specific-configuration-values) injiziert werden. Siehe [Komponenten](#components) oben für die injizierten Werte.
+ Suchen und Identifizieren falsch konfigurierter Eigenschaften

### Oak-Indizes

Oak-Indizes bieten eine Kopie der Knoten, die unter `/oak:index` definiert sind. Beachten Sie, dass hier keine zusammengeführten Indizes angezeigt werden, die auftreten, wenn ein AEM-Index geändert wird.

Oak-Indizes helfen beim Debugging durch:

+ Auflisten aller Oak-Index-Definitionen, was Einblicke in die Ausführung von Suchanfragen in AEM bietet. Denken Sie daran, dass Änderungen an AEM-Indizes hier nicht widergespiegelt werden. Diese Ansicht ist nur für Indizes hilfreich, die ausschließlich von AEM oder vom benutzerspezifischen Code bereitgestellt werden.

### OSGi-Dienste

Hier werden alle OSGi-Dienste aufgelistet. Diese Funktion ähnelt den [OSGi-Diensten für den lokalen Schnellstart von AEM SDK](http://localhost:4502/system/console/services) unter `/system/console/services`.

OSGi-Dienste helfen beim Debugging durch:

+ Auflisten aller OSGi-Dienste in AEM zusammen mit dem bereitgestellten OSGi-Bundle sowie allen OSGi-Bundles, die es nutzen

### Sling-Vorgänge

Mit „Sling-Vorgänge“ werden alle Warteschlangen für Sling-Aufträge aufgelistet. Diese Funktion ähnelt den [Vorgängen für den lokalen Schnellstart von AEM SDK](http://localhost:4502/system/console/slingevent) unter `/system/console/slingevent`.

Sling-Vorgänge helfen beim Debugging durch:

+ Auflistung der Sling-Vorgangswarteschlangen und deren Konfigurationen
+ Bereitstellung von Einblicken in die Anzahl der aktiven, in der Warteschlange befindlichen und verarbeiteten Sling-Vorgänge, was beim Debugging von Problemen mit Workflows, Übergangs-Workflows und anderen durch Sling-Vorgänge in AEM durchgeführten Arbeiten hilfreich ist.

## Java-Pakete

Mit „Java-Pakete“ kann überprüft werden, ob ein Java-Paket und eine -Version zur Verwendung in AEM as a Cloud Service verfügbar ist. Diese Funktion entspricht der [Abhängigkeitssuche für den lokalen Schnellstart von AEM SDK](http://localhost:4502/system/console/depfinder) unter `/system/console/depfinder`.

![Developer Console – Java-Pakete](./assets/developer-console/java-packages.png)

„Java-Pakete“ wird zur Fehlerbehebung bei Bundles verwendet, die aufgrund nicht aufgelöster Importe oder nicht aufgelöster Klassen in Skripten (HTL, JSP usw.) nicht gestartet werden. Wenn „Java-Pakete“ meldet, dass keine Bundles ein Java-Paket exportieren (oder die Version nicht mit der von einem OSGi-Bundle importierten übereinstimmt): 

+ Stellen Sie sicher, dass die Version der AEM-API-Maven-Abhängigkeit Ihres Projekts mit der AEM-Version der Umgebung übereinstimmt (und aktualisieren Sie nach Möglichkeit alles auf die neueste Version).
+ Wenn zusätzliche Maven-Abhängigkeiten im Maven-Projekt verwendet werden
   + Bestimmen Sie, ob stattdessen eine alternative API verwendet werden kann, die von der AEM SDK-API-Abhängigkeit bereitgestellt wird.
   + Wenn die zusätzliche Abhängigkeit erforderlich ist, stellen Sie sicher, dass sie als OSGi-Bundle (anstatt als einfache JAR-Datei) bereitgestellt wird und in das Code-Paket (`ui.apps`) Ihres Projekts eingebettet ist, ähnlich wie das Kern-OSGi-Bundle im Paket `ui.apps` eingebettet ist.

## Servlets

Mit „Servlets“ lassen sich Einblicke darüber erhalten, wie AEM eine URL zu einem Java-Servlet oder -Skript (HTL, JSP) auflöst, das die Anfrage letztendlich verarbeitet. Diese Funktion entspricht dem [Sling Servlet-Resolver für den lokalen Schnellstart von AEM SDK](http://localhost:4502/system/console/servletresolver) unter `/system/console/servletresolver`.

![Developer Console – Servlets](./assets/developer-console/servlets.png)

„Servlets“ hilft beim Debugging, indem bestimmt werden kann:

+ wie eine URL in adressierbare Teile (Ressource, Selektor, Erweiterung) zerlegt wird.
+ zu welchem Servlet oder Skript sich eine URL auflöst, was bei der Identifizierung von falsch formatierten URLs oder falsch registrierten Servlets/Skripten hilft.

## Abfragen

„Abfragen“ bietet Einblicke darin, welche Suchanfragen wie auf AEM ausgeführt werden. Diese Funktion entspricht der Konsole [Lokales Schnellstart-Tool für AEM SDK > Abfrageleistung](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

„Abfragen“ funktioniert nur, wenn ein bestimmter Pod ausgewählt ist, da die Web-Konsole „Abfrageleistung“ dieses Pods geöffnet wird. Daher müssen Entwicklungspersonen Zugriff haben, um sich beim AEM-Dienst anzumelden.

![Developer Console – Abfragen – Abfrage erläutern](./assets/developer-console/queries__explain-query.png)

„Abfragen“ erleichtert das Debugging durch:

+ Erläutern der Interpretation, Analyse und Ausführung von Abfragen durch Oak. Dies ist sehr wichtig, wenn Sie nachverfolgen möchten, warum eine Abfrage langsam ist, um zu verstehen, wie sie beschleunigt werden kann.
+ Auflisten der beliebtesten Abfragen, die in AEM ausgeführt werden, mit der Möglichkeit, sie zu erläutern.
+ Auflisten der langsamsten Abfragen, die in AEM ausgeführt werden, mit der Möglichkeit, sie zu erläutern.
