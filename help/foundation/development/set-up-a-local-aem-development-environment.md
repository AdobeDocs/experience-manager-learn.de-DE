---
title: Lokale AEM-Entwicklungsumgebung einrichten
description: Anleitung zum Einrichten einer lokalen Entwicklung für Adobe Experience Manager, AEM. Behandelt wichtige Themen wie lokale Installation, Apache Maven, integrierte Entwicklungsumgebungen und Debugging/Fehlerbehebung. Die Entwicklung mit Eclipse IDE, CRXDE-Lite, Visual Studio Code und IntelliJ werden besprochen.
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 2%

---

# Lokale AEM-Entwicklungsumgebung einrichten

Anleitung zum Einrichten einer lokalen Entwicklung für Adobe Experience Manager, AEM. Behandelt wichtige Themen wie lokale Installation, Apache Maven, integrierte Entwicklungsumgebungen und Debugging/Fehlerbehebung. Entwicklung mit **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] und[!DNL IntelliJ]** werden diskutiert.

## Übersicht

Die Einrichtung einer lokalen Entwicklungsumgebung ist der erste Schritt bei der Entwicklung für Adobe Experience Manager oder AEM. Nehmen Sie sich Zeit für die Einrichtung einer qualitätsorientierten Entwicklungsumgebung, um Ihre Produktivität zu steigern und schnelleren Code zu schreiben. Wir können eine AEM lokale Entwicklungsumgebung in vier Bereiche unterteilen:

* Lokale AEM
* [!DNL Apache Maven] Projekt
* Integrierte Entwicklungsumgebungen (IDE)
* Fehlerbehebung

## Installieren lokaler AEM-Instanzen

Wenn es um eine lokale AEM geht, handelt es sich um eine Kopie von Adobe Experience Manager, die auf dem persönlichen Computer eines Entwicklers ausgeführt wird. ***Alle*** AEM Entwicklung sollte beginnen, indem Code für eine lokale AEM-Instanz geschrieben und ausgeführt wird.

Wenn Sie neu AEM sind, können zwei grundlegende Ausführungsmodi installiert werden: ***Autor*** und ***Veröffentlichen***. Die ***Autor*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  ist die Umgebung, die digitale Marketingexperten zum Erstellen und Verwalten von Inhalten verwenden werden. In der Entwicklung **most** der Zeit, zu der Sie Code in einer Autoreninstanz bereitstellen. Auf diese Weise können Sie neue Seiten erstellen sowie Komponenten hinzufügen und konfigurieren. AEM Sites ist ein WYSIWYG-Authoring-CMS und daher können die meisten CSS- und JavaScript-Dateien mit einer Authoring-Instanz getestet werden.

Es ist auch *kritisch* Testcode gegen eine lokale ***Veröffentlichen*** -Instanz. Die ***Veröffentlichen*** -Instanz ist die AEM Umgebung, mit der Besucher Ihrer Website interagieren. Während ***Veröffentlichen*** instance ist derselbe Technologiestapel wie ***Autor*** Beispielsweise gibt es einige wesentliche Unterschiede bei Konfigurationen und Berechtigungen. Code sollte *always* gegen eine lokale ***Veröffentlichen*** -Instanz, bevor sie in Umgebungen mit höherer Ebene weitergeleitet wird.

### Schritte

1. Sichern [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) installiert ist.
   * Voreinstellen [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=14) für AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) für AEM Versionen vor AEM 6.5
2. Eine Kopie der [AEM QuickStart Jar und eine [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Erstellen Sie eine Ordnerstruktur auf Ihrem Computer wie folgt:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Benennen Sie die [!DNL QuickStart] JAR bis ***aem-author-p4502.jar*** und platzieren Sie sie unter dem `/author` Verzeichnis. Fügen Sie die ***[!DNL license.properties]*** Datei unter `/author` Verzeichnis.
5. Erstellen Sie eine Kopie der [!DNL QuickStart] JAR, benennen Sie es in um ***aem-publish-p4503.jar*** und platzieren Sie sie unter dem `/publish` Verzeichnis. Fügen Sie eine Kopie der ***[!DNL license.properties]*** Datei unter `/publish` Verzeichnis.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Doppelklicken Sie auf die ***aem-author-p4502.jar*** Datei zum Installieren der **Autor** -Instanz. Dadurch wird die Autoreninstanz gestartet, die auf dem Port ausgeführt wird. **4502** auf dem lokalen Computer.

   Doppelklicken Sie auf die ***aem-publish-p4503.jar*** Datei zum Installieren der **Veröffentlichen** -Instanz. Dadurch wird die Veröffentlichungsinstanz gestartet, die auf dem Port ausgeführt wird. **4503** auf dem lokalen Computer.

   >[!NOTE]
   >
   >Je nach Hardware Ihres Entwicklungsgeräts kann es schwierig sein, beide **Autoren- und Veröffentlichungsinstanz** -Instanz, die gleichzeitig ausgeführt wird. In seltenen Fällen müssen Sie beide gleichzeitig auf einem lokalen Setup ausführen.

   Weitere Informationen finden Sie unter [Bereitstellen und Verwalten einer AEM-Instanz](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Installieren von Apache Maven

***[!DNL Apache Maven]*** ist ein Tool zum Verwalten des Build- und Bereitstellungsverfahrens für Java-basierte Projekte. AEM ist eine Java-basierte Plattform und [!DNL Maven] ist die Standardmethode zum Verwalten von Code für ein AEM Projekt. Wenn ***AEM Maven-Projekt*** oder einfach ***AEM***, verweisen wir auf ein Maven-Projekt, das alle *custom* Code für Ihre Site.

Alle AEM sollten auf der neuesten Version der **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). Die [!DNL AEM Project Archetype] erstellt einen Bootstrap eines AEM Projekts mit Beispielcode und -inhalt. Die [!DNL AEM Project Archetype] auch **[!DNL AEM WCM Core Components]** für die Verwendung in Ihrem Projekt konfiguriert wurde.

>[!CAUTION]
>
>Beim Starten eines neuen Projekts empfiehlt es sich, die neueste Version des Archetyps zu verwenden. Beachten Sie, dass es mehrere Versionen des Archetyps gibt und nicht alle Versionen mit früheren Versionen von AEM kompatibel sind.

### Schritte

1. Download [Apache Maven](https://maven.apache.org/download.cgi)
2. Installieren [Apache Maven](https://maven.apache.org/install.html) und stellen Sie sicher, dass die Installation Ihrer Befehlszeile hinzugefügt wurde. `PATH`.
   * [!DNL macOS] Benutzer können Maven mithilfe von [Homebrew](https://brew.sh/)
3. Stellen Sie sicher, dass **[!DNL Maven]** wird installiert, indem ein neues Befehlszeilenterminal geöffnet und Folgendes ausgeführt wird:

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
   > In der Vergangenheit wurden `adobe-public` Maven-Profil war erforderlich, um einen Punkt zu erreichen `nexus.adobe.com` , um AEM Artefakte herunterzuladen. Alle AEM Artefakte sind jetzt über Maven Central und die `adobe-public` Profil nicht benötigt.

## Einrichten einer integrierten Entwicklungsumgebung

Eine integrierte Entwicklungsumgebung oder IDE ist eine Anwendung, die einen Texteditor, Syntaxunterstützung und Build-Tools kombiniert. Je nach der Art der Entwicklung, die Sie durchführen, ist möglicherweise eine IDE besser als eine andere. Unabhängig von der IDE ist es wichtig, regelmäßig ***push*** -Code in eine lokale AEM-Instanz zu verweisen, um sie zu testen. Außerdem wird es wichtig sein, gelegentlich ***abrufen*** Konfigurationen von einer lokalen AEM-Instanz in Ihr AEM-Projekt, um zu einem Quellcodeverwaltungssystem wie Git beizubehalten.

Im Folgenden finden Sie einige der beliebtesten IDEs, die mit AEM Entwicklung mit entsprechenden Videos verwendet werden, die die Integration mit einer lokalen AEM-Instanz zeigen.

>[!NOTE]
>
> Das WKND-Projekt wurde so aktualisiert, dass es standardmäßig auf AEM as a Cloud Service funktioniert. Es wurde aktualisiert, um [Abwärtskompatibel mit 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie die `classic` Profile zu beliebigen Maven-Befehlen hinzufügen.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wenn Sie eine IDE verwenden, überprüfen Sie `classic` auf der Registerkarte &quot;Maven-Profil&quot;angezeigt.

![Maven-Profil-Registerkarte](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven-Profil*

### [!DNL Eclipse] IDE

Die **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** ist eine der beliebtesten IDEs für die Java-Entwicklung, da es sich größtenteils um Open Source handelt und ***kostenlos***! Adobe bietet ein Plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, für [!DNL Eclipse] um eine einfachere Entwicklung mit einer netten grafischen Benutzeroberfläche zu ermöglichen, um Code mit einer lokalen AEM-Instanz zu synchronisieren. Die [!DNL Eclipse] IDE wird für Entwickler empfohlen, die zum großen Teil neu AEM, da die GUI-Unterstützung von [!DNL AEM Developer Tools].

#### Installation und Einrichtung

1. Laden Sie die [!DNL Eclipse] IDE für [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Befolgen Sie die Anweisungen zum Installieren der [!DNL AEM Developer Tools] Plug-in: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importieren eines Maven-Projekts
* 01:24 - Erstellen und Bereitstellen von Quellcode mit Maven
* 04:33 - Push-Code-Änderungen mit AEM Developer Tool
* 10:55 - Codeänderungen mit AEM Developer Tool abrufen
* 13:12 - Verwenden der integrierten Debugging-Tools von Eclipse

### IntelliJ IDEA

Die **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** ist eine leistungsstarke IDE für die professionelle Java-Entwicklung. [!DNL IntelliJ IDEA] in zwei Geschmacksrichtungen verwendet wird, ***kostenlos*** [!DNL Community] Edition und einer kommerziellen (gebührenpflichtigen) [!DNL Ultimate] -Version. Die kostenlose [!DNL Community] Version von [!DNL IntellIJ IDEA] für eine AEM Entwicklung ausreicht, jedoch ist die [!DNL Ultimate] [Erweitert seinen Funktionssatz](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Laden Sie die [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installieren [!DNL Repo] (Befehlszeilen-Tool): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importieren eines Maven-Projekts
* 05:47 - Erstellen und Bereitstellen von Quellcode mit Maven
* 08:17 - Push-Änderungen mit Repo
* 14:39 - Pull changes with Repo
* 17:25 - Verwenden der integrierten Debugging-Tools von IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio-Code](https://code.visualstudio.com/)** hat sich schnell zu einem bevorzugten Tool für ***Frontend-Entwickler*** mit verbesserter JavaScript-Unterstützung, [!DNL Intellisense], und Browserdebugging-Unterstützung. **[!DNL Visual Studio Code]** ist Open Source, kostenlos, mit vielen leistungsstarken Erweiterungen. [!DNL Visual Studio Code] kann mithilfe eines Adobe-Tools für die Integration mit AEM eingerichtet werden; **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Es gibt auch mehrere von der Community unterstützte Erweiterungen, die installiert werden können, um sie in AEM zu integrieren.

[!DNL Visual Studio Code] ist eine hervorragende Wahl für Frontend-Entwickler, die hauptsächlich CSS/LESS- und JavaScript-Code schreiben, um AEM Client-Bibliotheken zu erstellen. Dieses Tool ist möglicherweise nicht die beste Wahl für neue AEM-Entwickler, da Knotendefinitionen (Dialogfelder, Komponenten) alle im rohen XML bearbeitet werden müssen. Es sind mehrere Java-Erweiterungen verfügbar für [!DNL Visual Studio Code], jedoch in erster Linie mit Java-Entwicklung [!DNL Eclipse IDE] oder [!DNL IntelliJ] vorgezogen werden.

#### Wichtige Links

* [**Download**](https://code.visualstudio.com/Download) **Visual Studio-Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - FTP-ähnliches Tool für JCR-Inhalte
* **[aemfed](https://aemfed.io/)** - Beschleunigen Sie Ihren AEM Frontend-Workflow.
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Von der Community unterstützte* Erweiterung für Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importieren eines Maven-Projekts
* 00:53 - Erstellen und Bereitstellen von Quellcode mit Maven
* 04:03 - Push-Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 08:29 - Pull-Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 10:40 - Push-Code-Änderungen mit einem emfed Tool
* 14:24 - Fehlerbehebung, Neuerstellung von Client-Bibliotheken

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) ist eine browserbasierte Ansicht des AEM-Repositorys. [!DNL CRXDE Lite] ist in AEM eingebettet und ermöglicht Entwicklern die Durchführung standardmäßiger Entwicklungsaufgaben wie das Bearbeiten von Dateien, das Definieren von Komponenten, Dialogfeldern und Vorlagen. [!DNL CRXDE Lite] is ***not*** soll eine vollständige Entwicklungsumgebung sein, ist aber als Debugging-Tool sehr effektiv. [!DNL CRXDE Lite] ist nützlich, wenn Sie Produktcode außerhalb Ihrer Codebasis erweitern oder einfach verstehen. [!DNL CRXDE Lite] bietet eine leistungsstarke Ansicht des Repositorys und eine Möglichkeit, Berechtigungen effektiv zu testen und zu verwalten.

[!DNL CRXDE Lite] sollte immer in Verbindung mit anderen IDEs zum Testen und Debuggen von Code verwendet werden, jedoch nie als primäres Entwicklungstool. Es bietet eingeschränkte Syntaxunterstützung, keine automatische Vervollständigungsfunktionen und eine eingeschränkte Integration mit Quellcodeverwaltungssystemen.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Fehlerbehebung

***Hilfe!*** Mein Code funktioniert nicht! Wie bei jeder Entwicklung wird es Zeiten (wahrscheinlich viele) geben, in denen Ihr Code einfach nicht wie erwartet funktioniert. AEM ist eine mächtige Plattform, aber mit großer Macht.. ist eine große Komplexität. Im Folgenden finden Sie einige allgemeine Ausgangspunkte zur Fehlerbehebung und zur Problemverfolgung (bei weitem nicht aus einer vollständigen Liste mit möglichen Problemen):

### Codebereitstellung überprüfen

Ein guter erster Schritt bei Auftreten eines Problems besteht darin, zu überprüfen, ob der Code bereitgestellt und erfolgreich in AEM installiert wurde.

1. **Überprüfen [!UICONTROL Package Manager]** um sicherzustellen, dass das Code-Paket hochgeladen und installiert wurde: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Überprüfen Sie den Zeitstempel, um sicherzustellen, dass das Paket kürzlich installiert wurde.
1. Wenn Sie inkrementelle Dateiaktualisierungen mit einem Tool wie [!DNL Repo] oder [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** dass die Datei an die lokale AEM-Instanz gesendet wurde und der Dateiinhalt aktualisiert wird: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Überprüfen, ob das Bundle hochgeladen wurde** wenn Probleme im Zusammenhang mit Java-Code in einem OSGi-Bundle angezeigt werden. Öffnen Sie die [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) und suchen Sie nach Ihrem Bundle. Stellen Sie sicher, dass das Bundle über eine **[!UICONTROL Aktiv]** Status. Weitere Informationen zur Fehlerbehebung bei einem Bundle in einem **[!UICONTROL Installiert]** state.

#### Überprüfen Sie die Protokolle

AEM ist eine Chatty-Plattform und protokolliert viele nützliche Informationen in der **error.log**. Die **error.log** kann dort gefunden werden, wo AEM installiert wurde: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Eine nützliche Methode zum Verfolgen von Problemen besteht darin, Protokolleinträge in Ihren Java-Code einzufügen:

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

Standardmäßig wird die **error.log** ist für die Protokollierung konfiguriert *[!DNL INFO]* -Anweisungen. Wenn Sie die Protokollebene ändern möchten, können Sie dies tun, indem Sie [!UICONTROL Protokollunterstützung]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Sie können auch feststellen, dass die Variable **error.log** ist zu chatty. Sie können die [!UICONTROL Protokollunterstützung] um Protokollanweisungen für nur ein bestimmtes Java-Paket zu konfigurieren. Dies ist eine Best Practice für Projekte, um benutzerdefinierte Code-Probleme einfach von OOTB AEM Plattformproblemen zu trennen.

![Protokollierungskonfiguration in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundle befindet sich im Status &quot;Installiert&quot; {#bundle-active}

Alle Pakete (außer Fragmente) sollten sich in einer **[!UICONTROL Aktiv]** state. Wenn Ihr Code-Bundle in einem [!UICONTROL Installiert] zuweisen, muss ein Problem behoben werden. Meistens handelt es sich um ein Abhängigkeitsproblem:

![Bundle-Fehler in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Im obigen Screenshot wird die [!DNL WKND Core bundle] ist [!UICONTROL Installiert] state. Dies liegt daran, dass das Bundle eine andere Version von erwartet. `com.adobe.cq.wcm.core.components.models` als in der AEM-Instanz verfügbar ist.

Ein nützliches Tool, das verwendet werden kann, ist die [!UICONTROL Abhängigkeitssuche]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Fügen Sie den Namen des Java-Pakets hinzu, um zu überprüfen, welche Version in der AEM-Instanz verfügbar ist:

![Kernkomponenten](assets/set-up-a-local-aem-development-environment/core-components.png)

Im weiteren Verlauf des obigen Beispiels können wir sehen, dass die auf der AEM-Instanz installierte Version **Artikel 12 Absatz 2** vs **Artikel 12 Absatz 6** dass das Bundle erwartet hat. Dort können Sie rückwärts arbeiten und sehen, ob die [!DNL Maven] Abhängigkeiten von AEM entsprechen der [!DNL Maven] Abhängigkeiten im AEM Projekt. Im obigen Beispiel [!DNL Core Components] **v2.2.0** wird auf der AEM-Instanz installiert, aber das Code-Bundle wurde mit einer Abhängigkeit von **v2.2.2** und somit der Grund für das Abhängigkeitsproblem.

#### Überprüfung der Registrierung von Sling-Modellen {#osgi-component-sling-models}

AEM Komponenten sollten immer durch eine [!DNL Sling Model] , um eine beliebige Geschäftslogik einzuschließen und sicherzustellen, dass das HTL-Rendering-Skript sauber bleibt. Wenn Probleme auftreten, bei denen das Sling-Modell nicht gefunden werden kann, ist es möglicherweise hilfreich, die [!DNL Sling Models] aus der Konsole: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Auf diese Weise erfahren Sie, ob Ihr Sling-Modell registriert wurde und mit welchem Ressourcentyp (dem Komponentenpfad) es verknüpft ist.

![Status des Sling-Modells](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Zeigt die Registrierung eines [!DNL Sling Model], `BylineImpl` die mit einem Komponenten-Ressourcentyp von `wknd/components/content/byline`.

#### CSS- oder JavaScript-Probleme

Bei den meisten CSS- und JavaScript-Problemen ist die Verwendung der Entwicklungstools des Browsers die effektivste Möglichkeit zur Fehlerbehebung. Um das Problem bei der Entwicklung für eine AEM Autoreninstanz einzuschränken, ist es hilfreich, die Seite &quot;als veröffentlicht&quot;anzuzeigen.

![CSS- oder JS-Probleme](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Öffnen Sie die [!UICONTROL Seiteneigenschaften] Menü und klicken Sie auf [!UICONTROL Als veröffentlicht anzeigen]. Dadurch wird die Seite ohne den AEM-Editor und mit einem Abfrageparameter geöffnet, der auf **wcmmode=disabled**. Dadurch wird die AEM-Authoring-Benutzeroberfläche effektiv deaktiviert und die Fehlerbehebung/das Debugging von Frontend-Problemen deutlich vereinfacht.

Ein weiteres häufig auftretendes Problem bei der Entwicklung von Frontend-Code ist veraltetes oder veraltetes CSS/JS. Als ersten Schritt müssen Sie sicherstellen, dass der Browser-Verlauf gelöscht wurde und bei Bedarf einen Inkognito-Browser oder eine neue Sitzung starten.

#### Debugging von Client-Bibliotheken

Bei verschiedenen Methoden von Kategorien und Einbettungen zum Einschließen mehrerer Client-Bibliotheken kann es umständlich sein, eine Fehlerbehebung durchzuführen. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Instrumente ist [!UICONTROL Client-Bibliotheken neu erstellen] was AEM zwingt, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [Sprunglippen](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Listet alle in der AEM-Instanz registrierten Client-Bibliotheken auf. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Testausgabe](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - ermöglicht es einem Benutzer, die erwartete HTML-Ausgabe von clientlib-Includes basierend auf der Kategorie anzuzeigen. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Überprüfung von Bibliotheksabhängigkeiten](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - markiert alle Abhängigkeiten oder eingebetteten Kategorien, die nicht gefunden werden können. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Client-Bibliotheken neu erstellen](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - ermöglicht es einem Benutzer, AEM zu erzwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken ungültig zu machen. Dieses Tool ist besonders effektiv bei der Entwicklung mit LESS, da dies AEM zwingen kann, die generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu invalidieren und dann eine Seitenaktualisierung vorzunehmen, anstatt alle Bibliotheken neu zu erstellen. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debuggen von Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Wenn Sie den Cache ständig mit dem [!UICONTROL Client-Bibliotheken neu erstellen] -Tool es sich möglicherweise lohnt, eine einmalige Neuerstellung aller Client-Bibliotheken durchzuführen. Dies kann etwa 15 Minuten dauern, in der Regel werden jedoch alle zukünftigen Caching-Probleme behoben.
