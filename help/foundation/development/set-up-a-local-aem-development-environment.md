---
title: Einrichten einer lokalen AEM-Entwicklungsumgebung
description: Erfahren Sie, wie Sie eine lokale Entwicklungsumgebung für Experience Manager einrichten. Machen Sie sich mit der lokalen Installation, Apache Maven, integrierten Entwicklungsumgebungen sowie dem Debuggen und der Fehlerbehebung vertraut. Verwenden Sie Eclipse IDE, CRXDE-Lite, Visual Studio Code und IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4693
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 100%

---

# Einrichten einer lokalen AEM-Entwicklungsumgebung

Anleitung zum Einrichten einer lokalen Entwicklung für Adobe Experience Manager, AEM. Behandelt wichtige Themen wie lokale Installation, Apache Maven, integrierte Entwicklungsumgebungen und Debuggen/Fehlerbehebung. Die Entwicklung mit **Eclipse IDE, CRXDE Lite, Visual Studio Code und IntelliJ** wird diskutiert.

## Übersicht

Die Einrichtung einer lokalen Entwicklungsumgebung ist der erste Schritt bei der Entwicklung für Adobe Experience Manager oder AEM. Nehmen Sie sich Zeit für die Einrichtung einer qualitätsorientierten Entwicklungsumgebung, um Ihre Produktivität zu steigern und schneller besseren Code zu schreiben. Wir können eine lokale AEM-Entwicklungsumgebung in vier Bereiche unterteilen:

* Lokale AEM-Instanzen
* [!DNL Apache Maven]-Projekt
* Integrierte Entwicklungsumgebungen (IDE)
* Fehlerbehebung

## Installieren lokaler AEM-Instanzen

Bei einer lokalen AEM-Instanz handelt es sich um eine Kopie von Adobe Experience Manager, die auf dem persönlichen Computer einer Entwicklungsperson ausgeführt wird. ***Jede*** AEM-Entwicklung sollte damit beginnen, Code für eine lokale AEM-Instanz zu schreiben und auszuführen.

Wenn Sie neu bei AEM sind, können zwei grundlegende Ausführungsmodi installiert werden: ***Author*** und ***Publish***. Der [Ausführungsmodus](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=de) ***Author*** ist die Umgebung, die digitale Marketing-Fachleute zum Erstellen und Verwalten von Inhalten verwenden. Wenn Sie die meiste Zeit entwickeln, stellen Sie Code in einer Autoreninstanz bereit. Auf diese Weise können Sie Seiten erstellen sowie Komponenten hinzufügen und konfigurieren. Da AEM Sites ein WYSIWYG-Authoring-CMS ist, können die meisten CSS- und JavaScript-Dateien mit einer Authoring-Instanz getestet werden.

Es ist auch *ausschlaggebend*, Code in einer lokalen ***Veröffentlichungsinstanz*** zu testen. Die ***Veröffentlichungsinstanz*** ist die AEM-Umgebung, mit der Besuchende Ihrer Website interagieren. Während die ***Veröffentlichungsinstanz*** derselbe Technologiestapel wie die ***Autoreninstanz*** ist, gibt es einige wesentliche Unterschiede bei Konfigurationen und Berechtigungen. Der Code muss mit einer lokalen ***Veröffentlichungsinstanz*** getestet werden, bevor er in Umgebungen auf höherer Ebene genutzt wird.

### Schritte

1. Stellen Sie sicher, dass Java™ installiert ist.
   * Bevorzugen Sie [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) für AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) für AEM-Versionen vor AEM 6.5
1. Besorgen Sie sich eine Kopie der [AEM QuickStart-JAR und  [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=de).
1. Erstellen Sie eine Ordnerstruktur auf Ihrem Computer wie folgt:

```plain
~/aem-sdk
    /author
    /publish
```

1. Benennen Sie die [!DNL QuickStart]-JAR in ***aem-author-p4502.jar*** um und platzieren Sie sie unter dem `/author`-Verzeichnis. Fügen Sie die Datei ***[!DNL license.properties]*** unter dem Verzeichnis `/author` hinzu.

1. Erstellen Sie eine Kopie der [!DNL QuickStart]-JAR, benennen Sie sie in ***aem-publish-p4503.jar*** um und platzieren Sie sie unter dem Verzeichnis `/publish`. Fügen Sie eine Kopie der Datei ***[!DNL license.properties]*** unter dem Verzeichnis `/publish` hinzu.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Klicken Sie zweimal auf die Datei ***aem-author-p4502.jar***, um die **Authoring-Instanz** zu installieren. Dadurch wird die Autoreninstanz gestartet, die auf dem Port **4502** auf dem lokalen Computer ausgeführt wird.

Klicken Sie zweimal auf die Datei ***aem-publish-p4503.jar***, um die **Veröffentlichungsinstanz** zu installieren. Dadurch wird die Veröffentlichungsinstanz gestartet, die auf dem Port **4503** auf dem lokalen Computer ausgeführt wird.

>[!NOTE]
>
>Je nach Hardware Ihres Entwicklungsgeräts kann es schwierig sein, sowohl die **Autoren- als auch die Veröffentlichungsinstanz** gleichzeitig auszuführen. In seltenen Fällen müssen Sie beide gleichzeitig auf einem lokalen Setup ausführen.

### Verwenden der Befehlszeile

Eine Alternative zum Doppelklicken auf die JAR-Datei besteht darin, AEM über die Befehlszeile zu starten oder ein Skript zu erstellen (`.bat` oder `.sh`), abhängig von Ihrem lokalen Betriebssystem. Nachfolgend finden Sie ein Beispiel für den entsprechenden Befehl:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Hier sind die `-X` JVM-Optionen und die `-D` zusätzliche Framework-Eigenschaften. Weitere Informationen finden Sie unter [Bereitstellen und Verwalten einer AEM-Instanz](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=de) und unter [Weitere in der Schnellstartdatei verfügbare Optionen](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Installieren von Apache Maven

***[!DNL Apache Maven]*** ist ein Tool zum Verwalten des Erstellungs- und Bereitstellungsverfahrens für Java-basierte Projekte. AEM ist eine Java-basierte Plattform, und [!DNL Maven] ist die Standardmethode zum Verwalten von Code für ein AEM-Projekt. Mit dem ***AEM Maven-Projekt*** oder einfach Ihrem ***AEM-Projekt*** meinen wir ein Maven-Projekt, das allen *benutzerdefinierten* Code für Ihre Site enthält.

Alle AEM-Projekte sollten mit der neuesten Version von **[!DNL AEM Project Archetype]** aufgebaut worden sein: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). Der [!DNL AEM Project Archetype] stellt einen Bootstrap eines AEM-Projekts mit Beispiel-Code und -inhalt bereit. Der [!DNL AEM Project Archetype] enthält auch **[!DNL AEM WCM Core Components]**, die für die Verwendung in Ihrem Projekt konfiguriert wurden.

>[!CAUTION]
>
>Beim Starten eines neuen Projekts besteht die Best Practise darin, die neueste Version des Archetyps zu verwenden. Beachten Sie, dass es mehrere Versionen des Archetyps gibt und nicht alle Versionen mit früheren Versionen von AEM kompatibel sind.

### Schritte

1. Laden Sie [Apache Maven](https://maven.apache.org/download.cgi) herunter
2. Installieren Sie [Apache Maven](https://maven.apache.org/install.html) und stellen Sie sicher, dass die Installation zum `PATH` Ihrer Befehlszeile hinzugefügt wurde.
   * Benutzerinnen und Benutzer von [!DNL macOS] können Maven mithilfe von [Homebrew](https://brew.sh/) installieren
3. Überprüfen Sie, ob **[!DNL Maven]** installiert ist, indem Sie ein neues Befehlszeilen-Terminal öffnen und Folgendes ausführen:

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> In der Vergangenheit war das Hinzufügen eines Maven-Profils `adobe-public` notwendig, um `nexus.adobe.com` auf das Herunterladen von AEM-Artefakten zu verweisen. Alle AEM-Artefakte sind aber jetzt über Maven Central verfügbar, und so wird das `adobe-public`-Profil nicht benötigt.

## Einrichten einer integrierten Entwicklungsumgebung

Eine integrierte Entwicklungsumgebung oder IDE ist eine Anwendung, die einen Texteditor, Syntaxunterstützung und Build-Tools kombiniert. Je nach Art der Entwicklung, die Sie betreiben, kann eine IDE einer anderen vorzuziehen sein. Unabhängig von der IDE ist es wichtig, regelmäßig ***Push***-Code an eine lokale AEM-Instanz senden zu können, um ihn zu testen. Es ist wichtig, gelegentlich ***Pull***-Konfigurationen aus einer lokalen AEM-Instanz in Ihr AEM-Projekt zu ziehen, um sie in einem Versionsverwaltungssystem wie Git zu erhalten.

Im Folgenden finden Sie einige der gängigsten IDEs, die bei der AEM-Entwicklung verwendet werden, mit entsprechenden Videos, die die Integration mit einer lokalen AEM-Instanz zeigen.

>[!NOTE]
>
> Das WKND-Projekt wurde so aktualisiert, dass es standardmäßig auf AEM as a Cloud Service funktioniert. Es wurde aktualisiert, um [rückwärtskompatibel mit 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx) zu sein. Wenn Sie AEM 6.5 oder 6.4 verwenden, fügen Sie das `classic`-Profil an beliebige Maven-Befehle an.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wenn Sie eine IDE verwenden, stellen Sie bitte sicher, dass Sie `classic` in Ihrer Registerkarte „Maven-Profil“ aktivieren.

![Registerkarte „Maven-Profil“](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ-Maven-Profil*

### [!DNL Eclipse] IDE

Die **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** ist eine der beliebtesten IDEs für die Java™-Entwicklung, vor allem weil sie Open-Source und ***kostenlos*** ist. Adobe stellt ein Plug-in namens **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=de)** für [!DNL Eclipse] zur Verfügung, um die Entwicklung mit einer ansprechenden grafischen Benutzeroberfläche zu erleichtern und den Code mit einer lokalen AEM-Instanz zu synchronisieren. Die [!DNL Eclipse]-IDE wird für Entwicklerinnen und Entwickler empfohlen, die neu in AEM sind, vor allem wegen der GUI-Unterstützung durch [!DNL AEM Developer Tools].

#### Installation und Einrichtung

1. Laden Sie die [!DNL Eclipse]-IDE für [!DNL Java™ EE Developers] herunter und installieren Sie sie: [https://www.eclipse.org](https://www.eclipse.org/)
1. Folgen Sie den Anweisungen, um das [!DNL AEM Developer Tools]-Plug-in zu installieren: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=de)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 – Importieren eines Maven-Projekts
* 01:24 – Erstellen und Bereitstellen von Quell-Code mit Maven
* 04:33 – Pushen von Code-Änderungen mit AEM Developer Tool
* 10:55 – Abrufen von Code-Änderungen mit AEM Developer Tool
* 13:12 – Verwenden der integrierten Debugging-Tools von Eclipse

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)** ist eine leistungsstarke IDE für professionelle Java™-Entwicklung. [!DNL IntelliJ IDEA] gibt es in zwei Varianten, eine ***kostenlose*** [!DNL Community]-Edition und eine kommerzielle (kostenpflichtige) [!DNL Ultimate]-Version. Die kostenlose [!DNL Community]-Version von [!DNL IntellIJ IDEA] reicht für die weitere AEM-Entwicklungen aus, die [!DNL Ultimate]-Version [erweitert jedoch den Funktionsumfang](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Laden Sie die [!DNL IntelliJ IDEA] herunter und installieren Sie sie: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installieren Sie [!DNL Repo] (Befehlszeilen-Tool): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 – Importieren des Maven Projects
* 05:47 – Erstellen und Bereitstellen von Quell-Code mit Maven
* 08:17 –Pushen von Änderungen mit Repo
* 14:39 – Abrufen von Änderungen mit Repo
* 17:25 – Verwenden der integrierten Debugging-Tools von IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** hat sich dank der verbesserten JavaScript-Unterstützung, [!DNL Intellisense]und der Unterstützung von Browser-Debugging schnell zu einem beliebten Werkzeug für ***Frontend-Entwicklerinnen und -Entwickler*** etabliert. **[!DNL Visual Studio Code]** ist Open Source, kostenlos und besitzt viele leistungsstarke Erweiterungen. [!DNL Visual Studio Code] kann mithilfe eines Adobe-Tools namens **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code) für die Integration mit AEM eingerichtet werden.** Es gibt auch mehrere von der Community unterstützte Erweiterungen, die installiert werden können, um sie in AEM zu integrieren.

[!DNL Visual Studio Code] ist eine hervorragende Wahl für Frontend-Entwicklerinnen und -Entwickler, die hauptsächlich CSS/LESS- und JavaScript-Code schreiben, um AEM Client-Bibliotheken zu erstellen. Dieses Tool ist möglicherweise nicht die beste Wahl für neue AEM-Entwicklungspersonen, da Knotendefinitionen (Dialogfelder, Komponenten) in rohen XML-Dateien bearbeitet werden müssen. Es gibt mehrere Java™-Erweiterungen für [!DNL Visual Studio Code], aber wenn Sie hauptsächlich Java™-Entwicklung betreiben, sollten Sie [!DNL Eclipse IDE] oder [!DNL IntelliJ] vorziehen.

#### Wichtige Links

* [**Download**](https://code.visualstudio.com/Download) von **Visual Studio Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**: FTP-ähnliches Tool für JCR-Inhalte
* **[aemfed](https://aemfed.io/)**: Beschleunigt AEM-Frontend-Workflows
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**: Von der Community unterstützte&#42; Erweiterung für Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 – Importieren eines Maven-Projekts
* 00:53 – Erstellen und Bereitstellen von Quell-Code mit Maven
* 04:03 – Pushen von Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 08:29 – Abrufen von Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 10:40 – Pushen von Code-Änderungen mit dem aemfed-Tool
* 14:24 – Fehlerbehebung, Neuerstellung von Client-Bibliotheken

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html?lang=de) ist eine Browser-basierte Ansicht des AEM-Repositorys. [!DNL CRXDE Lite] ist in AEM eingebettet und ermöglicht Entwicklungspersonen die Durchführung standardmäßiger Entwicklungsaufgaben wie das Bearbeiten von Dateien oder das Definieren von Komponenten, Dialogfeldern und Vorlagen. [!DNL CRXDE Lite] dient ***nicht*** als vollständige Entwicklungsumgebung, ist aber ein effektives Debugging-Tool. [!DNL CRXDE Lite] ist nützlich, wenn Sie Produkt-Code außerhalb Ihrer Code-Basis erweitern oder einfach verstehen wollen. [!DNL CRXDE Lite] bietet eine leistungsstarke Ansicht des Repositorys und eine Möglichkeit, Berechtigungen effektiv zu testen und zu verwalten.

[!DNL CRXDE Lite] sollte zusammen mit anderen IDEs zum Testen und Debuggen von Code verwendet werden, jedoch nie als primäres Entwicklungs-Tool. Es bietet nur eine eingeschränkte Syntaxunterstützung, keine automatischen Vervollständigungsfunktionen und eine eingeschränkte Integration in Versionsverwaltungssysteme.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Fehlerbehebung

***Hilfe!*** Mein Code funktioniert nicht! Wie bei allen Entwicklungsaktivitäten gibt es auch hier Momente (wahrscheinlich viele), in denen Ihr Code nicht wie erwartet funktioniert. AEM ist eine leistungsstarke Plattform, aber große Leistung bringt auch große Komplexität mit sich. Im Folgenden finden Sie einige allgemeine Ausgangspunkte zur Fehlerbehebung und zum Erkennen von Problemen (bei weitem keine vollständige Liste von Dingen, die schiefgehen können):

### Überprüfen der Code-Bereitstellung

Ein guter erster Schritt bei Auftreten eines Problems besteht darin, zu überprüfen, ob der Code bereitgestellt und erfolgreich in AEM installiert wurde.

1. **Überprüfen Sie [!UICONTROL Package Manager]**, um sicherzustellen, dass das Code-Paket hochgeladen und installiert wurde: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Überprüfen Sie den Zeitstempel, um sicherzustellen, dass das Paket kürzlich installiert wurde.
1. Wenn Sie inkrementelle Dateiaktualisierungen mit einem Tool wie [!DNL Repo] oder [!DNL AEM Developer Tools] durchführen, **stellen Sie mit[!DNL CRXDE Lite]** sicher, dass die Datei an die lokale AEM-Instanz gesendet wurde und der Dateiinhalt aktuell ist: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Überprüfen Sie, ob das Bundle hochgeladen wurde**, wenn Probleme im Zusammenhang mit Java™-Code in einem OSGi-Bundle auftreten. Öffnen Sie die [!UICONTROL Adobe Experience Manager-Web-Konsole]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles), und suchen Sie nach Ihrem Bundle. Stellen Sie sicher, dass das Bundle über den Status **[!UICONTROL Aktiv]** verfügt. Weitere Informationen zur Fehlerbehebung bei einem Bundle in **[!UICONTROL installiertem]** Zustand finden Sie unten.

#### Überprüfen der Protokolle

AEM ist eine gesprächige Plattform und protokolliert nützliche Informationen im **error.log**. Die Datei **error.log** kann dort gefunden werden, wo AEM installiert wurde: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Eine nützliche Methode zum Erkennen von Problemen besteht darin, Protokollanweisungen in Ihren Java™-Code einzufügen:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

Standardmäßig ist **error.log** für die Protokollierung von *[!DNL INFO]*-Anweisungen konfiguriert. Wenn Sie die Protokollebene ändern möchten, gehen Sie zur [!UICONTROL Protokollunterstützung]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Vielleicht finden Sie auch, dass die **error.log** zu geschwätzig ist. Sie können die [!UICONTROL Protokollunterstützung] verwenden, um Protokolleinträge nur für ein bestimmtes Java™-Paket zu konfigurieren. Dies ist eine Best Practise für Projekte, um Probleme mit benutzerdefiniertem Code leicht von Problemen zu trennen, die von AEM Platform selbst herrühren.

![Protokollierungskonfiguration in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundle befindet sich im Status „installiert“ {#bundle-active}

Alle Bundles (außer Fragmenten) sollten sich im Status **[!UICONTROL aktiv]** befinden. Wenn sich Ihr Code-Bundle in [!UICONTROL installiertem] Status befindet, muss ein Problem behoben werden. Meistens handelt es sich um ein Abhängigkeitsproblem:

![Bundle-Fehler in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Im obigen Screenshot befindet sich das [!DNL WKND Core bundle] im [!UICONTROL installierten] Status. Dies liegt daran, dass das Bundle eine andere Version von `com.adobe.cq.wcm.core.components.models` erwartet, als in der AEM-Instanz verfügbar ist.

Ein nützliches Tool, das verwendet werden kann, ist die [!UICONTROL Abhängigkeitssuche]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Fügen Sie den Namen des Java™-Pakets hinzu, um zu überprüfen, welche Version in der AEM-Instanz verfügbar ist:

![Kernkomponenten](assets/set-up-a-local-aem-development-environment/core-components.png)

Weiter können wir am obigen Beispiel sehen, dass die auf der AEM-Instanz installierte Version **12.2** statt der vom Bundle erwarteten **12.6** ist. Von dort aus können Sie rückwärts arbeiten und sehen, ob die [!DNL Maven]-Abhängigkeiten von AEM den [!DNL Maven]-Abhängigkeiten im AEM-Projekt entsprechen. Im obigen Beispiel ist [!DNL Core Components] **v2.2.0** auf der AEM-Instanz installiert, aber das Code-Bundle wurde mit einer Abhängigkeit von **v2.2.2** gebaut, was der Grund für das Abhängigkeitsproblem ist.

#### Überprüfen der Sling-Modellregistrierung {#osgi-component-sling-models}

AEM-Komponenten müssen von einem [!DNL Sling Model] unterstützt werden, um jegliche Business-Logik zu kapseln und sicherzustellen, dass das HTL-Rendering-Skript sauber bleibt. Bei Problemen, bei denen das Sling-Modell nicht gefunden werden kann, kann es hilfreich sein, die [!DNL Sling Models] auf der Konsole zu überprüfen: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Dies teilt Ihnen mit, ob Ihr Sling-Modell registriert wurde und mit welchem Ressourcentyp (dem Komponentenpfad) es verknüpft ist.

![Sling-Modell-Status](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Zeigt die Registrierung eines [!DNL Sling Model] mit `BylineImpl`, die an einen Komponenten-Ressourcentyp von `wknd/components/content/byline` gebunden ist.

#### CSS- oder JavaScript-Probleme

Bei den meisten CSS- und JavaScript-Problemen ist die Verwendung der Entwicklungs-Tools des Browsers die effektivste Möglichkeit zur Fehlerbehebung. Um das Problem bei der Entwicklung für eine AEM-Autoreninstanz einzuschränken, ist es hilfreich, die Seite „als veröffentlicht“ anzuzeigen.

![CSS- oder JS-Probleme](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Öffnen Sie das Menü [!UICONTROL Seiteneigenschaften] und klicken Sie auf [!UICONTROL Als veröffentlicht anzeigen]. Dadurch wird die Seite ohne den AEM-Editor und mit einem auf **wcmmode=disabled** festgelegten Abfrageparameter geöffnet. Dadurch wird die AEM-Authoring-Benutzeroberfläche deaktiviert und die Fehlerbehebung/das Debuggen von Frontend-Problemen deutlich vereinfacht.

Ein weiteres Problem, das bei der Entwicklung von Frontend-Code häufig auftritt, ist das Laden von altem oder veraltetem CSS/JS. Als ersten Schritt müssen Sie sicherstellen, dass der Browser-Verlauf gelöscht wurde, und bei Bedarf einen Inkognito-Browser oder eine neue Sitzung starten.

#### Debuggen von Client-Bibliotheken

Mit den verschiedenen Methoden der Kategorien und Einbettungen, die mehrere Client-Bibliotheken umfassen, kann die Fehlersuche mühsam sein. AEM stellt dafür mehrere Tools als Hilfe zur Verfügung. Eines der wichtigsten Tools ist [!UICONTROL Rebuild Client Libraries], das AEM zwingt, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html): Listet alle in der AEM-Instanz registrierten Client-Bibliotheken auf. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Testausgabe](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) – ermöglicht es Benutzenden, die erwartete HTML-Ausgabe von clientlib-Einfügungen basierend auf der Kategorie anzuzeigen. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Gültigkeitsprüfung von Bibliotheksabhängigkeiten](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) – markiert alle Abhängigkeiten oder eingebetteten Kategorien, die nicht gefunden werden können. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Client-Bibliotheken neu erstellen](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html): ermöglicht es Benutzenden, AEM zu zwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken zu invalidieren. Dieses Tool ist bei der Entwicklung mit LESS nützlich, da dies AEM zwingen kann, das generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, die Caches zu deaktivieren und dann die Seite zu aktualisieren, als alle Bibliotheken neu zu erstellen. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debuggen von Client-Bibliotheken](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Wenn Sie den Cache ständig mit dem Tool [!UICONTROL Client-Bibliotheken neu erstellen] ungültig machen müssen, kann es sich lohnen, alle Client-Bibliotheken einmalig neu zu erstellen. Dies kann etwa 15 Minuten dauern, in der Regel werden jedoch alle zukünftigen Caching-Probleme behoben.
