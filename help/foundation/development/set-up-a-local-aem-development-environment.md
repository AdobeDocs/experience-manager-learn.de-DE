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
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 3%

---


# Lokale AEM-Entwicklungsumgebung einrichten

Anleitung zum Einrichten einer lokalen Entwicklung für Adobe Experience Manager, AEM. Behandelt wichtige Themen wie lokale Installation, Apache Maven, integrierte Entwicklungsumgebungen und Debugging/Fehlerbehebung. Die Entwicklung mit **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] und[!DNL IntelliJ]** wird besprochen.

## Übersicht

Die Einrichtung einer lokalen Entwicklungsumgebung ist der erste Schritt bei der Entwicklung für Adobe Experience Manager oder AEM. Nehmen Sie sich Zeit für die Einrichtung einer qualitätsorientierten Entwicklungsumgebung, um Ihre Produktivität zu steigern und schnelleren Code zu schreiben. Wir können eine AEM lokale Entwicklungsumgebung in vier Bereiche unterteilen:

* Lokale AEM
* [!DNL Apache Maven] Projekt
* Integrierte Entwicklungsumgebungen (IDE)
* Fehlerbehebung

## Installieren lokaler AEM-Instanzen

Wenn es um eine lokale AEM geht, handelt es sich um eine Kopie von Adobe Experience Manager, die auf dem persönlichen Computer eines Entwicklers ausgeführt wird. ****** Die gesamte AEM-Entwicklung sollte mit dem Schreiben und Ausführen von Code für eine lokale AEM-Instanz beginnen.

Wenn Sie neu AEM sind, können zwei grundlegende Ausführungsmodi installiert werden: ***Autor*** und ***Veröffentlichen***. Der ***Autor*** [Runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) ist die Umgebung, die digitale Marketingexperten zum Erstellen und Verwalten von Inhalten verwenden. Bei der Entwicklung von **meistens** der Bereitstellung von Code für eine Autoreninstanz. Auf diese Weise können Sie neue Seiten erstellen sowie Komponenten hinzufügen und konfigurieren. AEM Sites ist ein WYSIWYG-Authoring-CMS und daher können die meisten CSS- und JavaScript-Dateien mit einer Authoring-Instanz getestet werden.

Es handelt sich auch um den Test-Code *critical* für eine lokale ***Publish***-Instanz. Die Instanz ***Publish*** ist die AEM Umgebung, mit der Besucher Ihrer Website interagieren. Die Instanz ***Publish*** ist zwar derselbe Technologie-Stack wie die Instanz ***Autor*** , es gibt jedoch einige wesentliche Unterschiede bei Konfigurationen und Berechtigungen. Der Code sollte *always* mit einer lokalen ***Publish***-Instanz getestet werden, bevor er in Umgebungen mit höherer Ebene weitergeleitet wird.

### Schritte

1. Stellen Sie sicher, dass [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) installiert ist.
   * Wählen Sie [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=14) für AEM 6.5+ aus.
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8)  für AEM Versionen vor AEM 6.5
2. Rufen Sie eine Kopie von [AEM QuickStart Jar und a [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware) ab.
3. Erstellen Sie eine Ordnerstruktur auf Ihrem Computer wie folgt:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Benennen Sie die JAR [!DNL QuickStart] in ***aem-author-p4502.jar*** um und platzieren Sie sie unter dem Verzeichnis `/author`. Fügen Sie die Datei ***[!DNL license.properties]*** unter dem Verzeichnis `/author` hinzu.
5. Erstellen Sie eine Kopie der [!DNL QuickStart]-JAR, benennen Sie sie in ***aem-publish-p4503.jar*** um und platzieren Sie sie unter dem Verzeichnis `/publish`. Fügen Sie eine Kopie der Datei ***[!DNL license.properties]*** unter dem Verzeichnis `/publish` hinzu.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Doppelklicken Sie auf die Datei ***aem-author-p4502.jar*** , um die **Autoreninstanz** zu installieren. Dadurch wird die Autoreninstanz gestartet, die auf dem Port **4502** auf dem lokalen Computer ausgeführt wird.

   Doppelklicken Sie auf die Datei ***aem-publish-p4503.jar*** , um die Instanz **Publish** zu installieren. Dadurch wird die Veröffentlichungsinstanz gestartet, die auf dem Port **4503** auf dem lokalen Computer ausgeführt wird.

   >[!NOTE]
   >
   >Je nach Hardware Ihres Entwicklungscomputers kann es schwierig sein, eine **Autoren- und Veröffentlichungsinstanz** gleichzeitig auszuführen. In seltenen Fällen müssen Sie beide gleichzeitig auf einem lokalen Setup ausführen.

   Weitere Informationen finden Sie unter [Bereitstellen und Verwalten einer AEM-Instanz](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Installieren von Apache Maven

***[!DNL Apache Maven]*** ist ein Tool zum Verwalten des Build- und Bereitstellungsverfahrens für Java-basierte Projekte. AEM ist eine Java-basierte Plattform und [!DNL Maven] ist die Standardmethode zum Verwalten von Code für ein AEM Projekt. Wenn wir ***AEM Maven-Projekt*** oder nur Ihr ***AEM Projekt*** nennen, verweisen wir auf ein Maven-Projekt, das den gesamten *benutzerspezifischen*-Code für Ihre Site enthält.

Alle AEM sollten von der neuesten Version von **[!DNL AEM Project Archetype]** erstellt werden: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). Der [!DNL AEM Project Archetype] erstellt ein Bootstrap eines AEM Projekts mit Beispielcode und -inhalt. Das [!DNL AEM Project Archetype] enthält auch **[!DNL AEM WCM Core Components]** , das für die Verwendung in Ihrem Projekt konfiguriert ist.

>[!CAUTION]
>
>Beim Starten eines neuen Projekts empfiehlt es sich, die neueste Version des Archetyps zu verwenden. Beachten Sie, dass es mehrere Versionen des Archetyps gibt und nicht alle Versionen mit früheren Versionen von AEM kompatibel sind.

### Schritte

1. [Apache Maven](https://maven.apache.org/download.cgi) herunterladen
2. Installieren Sie [Apache Maven](https://maven.apache.org/install.html) und stellen Sie sicher, dass die Installation zu Ihrer Befehlszeile `PATH` hinzugefügt wurde.
   * [!DNL macOS] Benutzer können Maven mit  [Homebrew installieren](https://brew.sh/)
3. Stellen Sie sicher, dass **[!DNL Maven]** installiert ist, indem Sie ein neues Befehlszeilen-Terminal öffnen und Folgendes ausführen:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. Fügen Sie das Profil **[!DNL adobe-public]** Ihrer [!DNL Maven] [settings.xml](https://maven.apache.org/settings.html)-Datei hinzu, um **[!DNL repo.adobe.com]** automatisch zum Maven-Build-Prozess hinzuzufügen.

5. Erstellen Sie eine Datei mit dem Namen `settings.xml` unter `~/.m2/settings.xml` , falls sie noch nicht vorhanden ist.

6. Fügen Sie das Profil **[!DNL adobe-public]** zur Datei `settings.xml` hinzu, basierend auf [den Anweisungen hier](https://repo.adobe.com/).

   Nachfolgend finden Sie ein Beispiel für `settings.xml`. *Beachten Sie, dass die Namenskonvention für  `settings.xml` und die Platzierung unter dem  `.m2` Ordner des Benutzers wichtig sind.*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. Stellen Sie sicher, dass das Profil **adobe-public** aktiv ist, indem Sie den folgenden Befehl ausführen:

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Wenn Sie die **[!DNL adobe-public]** nicht sehen, ist dies ein Hinweis darauf, dass in Ihrer `~/.m2/settings.xml`-Datei nicht ordnungsgemäß auf das Adobe-Repository verwiesen wird. Überprüfen Sie, ob die Datei settings.xml auf das Adobe-Repository verweist.

## Einrichten einer integrierten Entwicklungsumgebung

Eine integrierte Entwicklungsumgebung oder IDE ist eine Anwendung, die einen Texteditor, Syntaxunterstützung und Build-Tools kombiniert. Je nach der Art der Entwicklung, die Sie durchführen, ist möglicherweise eine IDE besser als eine andere. Unabhängig von der IDE ist es wichtig, regelmäßig ***Push***-Code an eine lokale AEM-Instanz senden zu können, um ihn zu testen. Außerdem ist es wichtig, gelegentlich ***Konfigurationen von einer lokalen AEM-Instanz in Ihr AEM-Projekt zu ziehen, um zu einem Quellcodeverwaltungssystem wie Git zu gelangen.***

Im Folgenden finden Sie einige der beliebtesten IDEs, die mit AEM Entwicklung mit entsprechenden Videos verwendet werden, die die Integration mit einer lokalen AEM-Instanz zeigen.

>[!NOTE]
>
> Das WKND-Projekt wurde aktualisiert und funktioniert jetzt standardmäßig auf AEM als Cloud Service. Es wurde aktualisiert und ist nun [abwärtskompatibel mit 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das Profil `classic` an beliebige Maven-Befehle an.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wenn Sie eine IDE verwenden, überprüfen Sie `classic` in Ihrer Maven-Profil-Registerkarte.

![Maven-Profil-Registerkarte](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*IntelliJ Maven-Profil*

### [!DNL Eclipse] IDE

Die **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** ist eine der beliebtesten IDEs für die Java-Entwicklung, zum großen Teil, weil sie Open Source und ***free*** ist. Adobe bietet ein Plug-in **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)** für [!DNL Eclipse], um die Entwicklung mit einer netten grafischen Benutzeroberfläche zu erleichtern, mit der Code mit einer lokalen AEM synchronisiert werden kann. Die [!DNL Eclipse] IDE wird für Entwickler empfohlen, die zum großen Teil neu AEM, da die GUI von [!DNL AEM Developer Tools] unterstützt wird.

#### Installation und Einrichtung

1. Laden Sie die [!DNL Eclipse] IDE für [!DNL Java EE Developers] herunter und installieren Sie sie: [https://www.eclipse.org](https://www.eclipse.org/)
1. Befolgen Sie die Anweisungen zum Installieren des Plug-ins [!DNL AEM Developer Tools] : [https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importieren eines Maven-Projekts
* 01:24 - Erstellen und Bereitstellen von Quellcode mit Maven
* 04:33 - Push-Code-Änderungen mit AEM Developer Tool
* 10:55 - Codeänderungen mit AEM Developer Tool abrufen
* 13:12 - Verwenden der integrierten Debugging-Tools von Eclipse

### IntelliJ IDEA

Die **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** ist eine leistungsstarke IDE für die professionelle Java-Entwicklung. [!DNL IntelliJ IDEA] in zwei Varianten, einer  ****** [!DNL Community] Freeedition und einer kommerziellen (bezahlten)  [!DNL Ultimate] Version. Die kostenlose [!DNL Community]-Version von [!DNL IntellIJ IDEA] ist ausreichend für AEM Entwicklung, die [!DNL Ultimate] [erweitert jedoch den Funktionssatz](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Laden Sie [!DNL IntelliJ IDEA] herunter und installieren Sie es: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installieren Sie [!DNL Repo] (Befehlszeilen-Tool): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importieren eines Maven-Projekts
* 05:47 - Erstellen und Bereitstellen von Quellcode mit Maven
* 08:17 - Push-Änderungen mit Repo
* 14:39 - Pull changes with Repo
* 17:25 - Verwenden der integrierten Debugging-Tools von IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio ](https://code.visualstudio.com/)** Codehas wurde schnell zu einem bevorzugten Tool für  ***Frontend-*** Entwickler mit verbesserter JavaScript-Unterstützung  [!DNL Intellisense]und Browser-Debugging-Unterstützung. **[!DNL Visual Studio Code]** ist Open Source, kostenlos, mit vielen leistungsstarken Erweiterungen. [!DNL Visual Studio Code] kann mithilfe eines Adobe-Tools,  **[Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code), zur Integration mit AEM eingerichtet werden.** Es gibt auch mehrere von der Community unterstützte Erweiterungen, die installiert werden können, um sie in AEM zu integrieren.

[!DNL Visual Studio Code] ist eine hervorragende Wahl für Frontend-Entwickler, die hauptsächlich CSS/LESS- und JavaScript-Code schreiben, um AEM Client-Bibliotheken zu erstellen. Dieses Tool ist möglicherweise nicht die beste Wahl für neue AEM-Entwickler, da Knotendefinitionen (Dialogfelder, Komponenten) alle im rohen XML bearbeitet werden müssen. Für [!DNL Visual Studio Code] sind mehrere Java-Erweiterungen verfügbar. Wenn Sie jedoch in erster Linie Java-Entwicklung betreiben, können [!DNL Eclipse IDE] oder [!DNL IntelliJ] bevorzugt werden.

#### Wichtige Links

* [****](https://code.visualstudio.com/Download) **Herunterladen von Visual Studio-Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**  - FTP-ähnliches Tool für JCR-Inhalte
* **[aemfed](https://aemfed.io/)**  - Beschleunigen Sie Ihren AEM Frontend-Workflow
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)**  - Community-unterstützte* Erweiterung für Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importieren eines Maven-Projekts
* 00:53 - Erstellen und Bereitstellen von Quellcode mit Maven
* 04:03 - Push-Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 08:29 - Pull-Code-Änderungen mit dem Repo-Befehlszeilen-Tool
* 10:40 - Push-Code-Änderungen mit einem emfed Tool
* 14:24 - Fehlerbehebung, Neuerstellung von Client-Bibliotheken

### [!DNL CRXDE Lite]

[CRXDE ](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) Liteis ist eine browserbasierte Ansicht des AEM-Repositorys. [!DNL CRXDE Lite] ist in AEM eingebettet und ermöglicht Entwicklern die Durchführung standardmäßiger Entwicklungsaufgaben wie das Bearbeiten von Dateien, das Definieren von Komponenten, Dialogfeldern und Vorlagen. [!DNL CRXDE Lite] ist  ****** nicht als vollständige Entwicklungsumgebung gedacht, ist aber als Debugging-Tool sehr effektiv. [!DNL CRXDE Lite] ist nützlich, wenn Sie Produktcode außerhalb Ihrer Codebasis erweitern oder einfach verstehen. [!DNL CRXDE Lite] bietet eine leistungsstarke Ansicht des Repositorys und eine Möglichkeit, Berechtigungen effektiv zu testen und zu verwalten.

[!DNL CRXDE Lite] sollte immer in Verbindung mit anderen IDEs zum Testen und Debuggen von Code verwendet werden, jedoch nie als primäres Entwicklungstool. Es bietet eingeschränkte Syntaxunterstützung, keine automatische Vervollständigungsfunktionen und eine eingeschränkte Integration mit Quellcodeverwaltungssystemen.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Fehlerbehebung

***Hilfe!*** Mein Code funktioniert nicht! Wie bei jeder Entwicklung wird es Zeiten (wahrscheinlich viele) geben, in denen Ihr Code einfach nicht wie erwartet funktioniert. AEM ist eine mächtige Plattform, aber mit großer Macht.. ist eine große Komplexität. Im Folgenden finden Sie einige allgemeine Ausgangspunkte zur Fehlerbehebung und zur Problemverfolgung (bei weitem nicht aus einer vollständigen Liste mit möglichen Problemen):

### Codebereitstellung überprüfen

Ein guter erster Schritt bei Auftreten eines Problems besteht darin, zu überprüfen, ob der Code bereitgestellt und erfolgreich in AEM installiert wurde.

1. **Überprüfen Sie  [!UICONTROL Package]** Manager, um sicherzustellen, dass das Codepaket hochgeladen und installiert wurde:  [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Überprüfen Sie den Zeitstempel, um sicherzustellen, dass das Paket kürzlich installiert wurde.
1. Wenn Sie inkrementelle Dateiaktualisierungen mit einem Tool wie [!DNL Repo] oder [!DNL AEM Developer Tools] durchführen, überprüfen Sie **[!DNL CRXDE Lite]**, ob die Datei an die lokale AEM-Instanz gesendet wurde und der Dateiinhalt aktualisiert wird: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Überprüfen Sie, ob das Bundle** hochgeladen wird und Probleme im Zusammenhang mit Java-Code in einem OSGi-Bundle auftreten. Öffnen Sie die [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) und suchen Sie nach Ihrem Bundle. Stellen Sie sicher, dass das Bundle den Status **[!UICONTROL Aktiv]** aufweist. Weitere Informationen zur Fehlerbehebung bei einem Bundle in einem **[!UICONTROL Installierten]**-Status finden Sie unten.

#### Überprüfen Sie die Protokolle

AEM ist eine Chatty-Plattform und protokolliert viele nützliche Informationen in **error.log**. **error.log** befindet sich dort, wo AEM installiert wurde: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

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

Standardmäßig ist **error.log** so konfiguriert, dass *[!DNL INFO]*-Anweisungen protokolliert werden. Wenn Sie die Protokollebene ändern möchten, gehen Sie zu [!UICONTROL Log Support]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Sie können auch feststellen, dass **error.log** zu chatty ist. Sie können die [!UICONTROL Protokollunterstützung] verwenden, um Protokollanweisungen für nur ein bestimmtes Java-Paket zu konfigurieren. Dies ist eine Best Practice für Projekte, um benutzerdefinierte Code-Probleme einfach von OOTB AEM Plattformproblemen zu trennen.

![Protokollierungskonfiguration in AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Bundle befindet sich im Status &quot;Installiert&quot; {#bundle-active}

Alle Bundles (außer Fragmente) sollten den Status **[!UICONTROL Aktiv]** aufweisen. Wenn Ihr Code-Bundle in einem Status [!UICONTROL Installiert] angezeigt wird, muss ein Problem behoben werden. Meistens handelt es sich um ein Abhängigkeitsproblem:

![Bundle-Fehler in AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Im obigen Screenshot ist [!DNL WKND Core bundle] ein [!UICONTROL Installierter] -Status. Dies liegt daran, dass das Bundle eine andere Version von `com.adobe.cq.wcm.core.components.models` erwartet, als in der AEM-Instanz verfügbar ist.

Ein nützliches Tool, das verwendet werden kann, ist der [!UICONTROL Dependency Finder]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Fügen Sie den Namen des Java-Pakets hinzu, um zu überprüfen, welche Version in der AEM-Instanz verfügbar ist:

![Kernkomponenten](assets/set-up-a-local-aem-development-environment/core-components.png)

Weiter mit dem obigen Beispiel können wir sehen, dass die auf der AEM-Instanz installierte Version **12.2** vs. **12.6** ist, die vom Bundle erwartet wurde. Von dort aus können Sie rückwärts arbeiten und sehen, ob die [!DNL Maven]-Abhängigkeiten AEM mit den [!DNL Maven]-Abhängigkeiten im AEM Projekt übereinstimmen. Im obigen Beispiel wird [!DNL Core Components] **v2.2.0** auf der AEM-Instanz installiert, aber das Code-Bundle wurde mit einer Abhängigkeit von **v2.2.2** erstellt. Dies ist der Grund für das Abhängigkeitsproblem.

#### Überprüfung der Registrierung von Sling-Modellen {#osgi-component-sling-models}

AEM Komponenten sollten immer durch ein [!DNL Sling Model] unterstützt werden, um eine beliebige Geschäftslogik einzukapseln und sicherzustellen, dass das HTL-Rendering-Skript sauber bleibt. Wenn Probleme auftreten, bei denen das Sling-Modell nicht gefunden werden kann, kann es hilfreich sein, den [!DNL Sling Models] in der Konsole zu überprüfen: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Auf diese Weise erfahren Sie, ob Ihr Sling-Modell registriert wurde und mit welchem Ressourcentyp (dem Komponentenpfad) es verknüpft ist.

![Status des Sling-Modells](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Zeigt die Registrierung eines [!DNL Sling Model], `BylineImpl` an, der mit einem Komponenten-Ressourcentyp von `wknd/components/content/byline` verknüpft ist.

#### CSS- oder JavaScript-Probleme

Bei den meisten CSS- und JavaScript-Problemen ist die Verwendung der Entwicklungstools des Browsers die effektivste Möglichkeit zur Fehlerbehebung. Um das Problem bei der Entwicklung für eine AEM Autoreninstanz einzuschränken, ist es hilfreich, die Seite &quot;als veröffentlicht&quot;anzuzeigen.

![CSS- oder JS-Probleme](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Öffnen Sie das Menü [!UICONTROL Seiteneigenschaften] und klicken Sie auf [!UICONTROL Als veröffentlicht anzeigen]. Dadurch wird die Seite ohne den AEM-Editor und mit einem Abfrageparameter geöffnet, der auf **wcmmode=disabled** gesetzt ist. Dadurch wird die AEM-Authoring-Benutzeroberfläche effektiv deaktiviert und die Fehlerbehebung/das Debugging von Frontend-Problemen deutlich vereinfacht.

Ein weiteres häufig auftretendes Problem bei der Entwicklung von Frontend-Code ist veraltetes oder veraltetes CSS/JS. Als ersten Schritt müssen Sie sicherstellen, dass der Browser-Verlauf gelöscht wurde und bei Bedarf einen Inkognito-Browser oder eine neue Sitzung starten.

#### Debugging von Client-Bibliotheken

Bei verschiedenen Methoden von Kategorien und Einbettungen zum Einschließen mehrerer Client-Bibliotheken kann es umständlich sein, eine Fehlerbehebung durchzuführen. AEM stellt mehrere Hilfsmittel zur Verfügung. Eines der wichtigsten Tools ist [!UICONTROL Client-Bibliotheken neu erstellen] , wodurch AEM gezwungen werden, alle LESS-Dateien neu zu kompilieren und das CSS zu generieren.

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Listet alle in der AEM-Instanz registrierten Client-Bibliotheken auf. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Testausgabe](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : Ermöglicht es einem Benutzer, die erwartete HTML-Ausgabe von clientlib-Includes basierend auf der Kategorie anzuzeigen. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Überprüfung der Abhängigkeiten von Bibliotheken](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  - hebt alle Abhängigkeiten oder eingebetteten Kategorien hervor, die nicht gefunden werden können. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Client-Bibliotheken neu erstellen](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  - ermöglicht es einem Benutzer, AEM zu erzwingen, alle Client-Bibliotheken neu zu erstellen oder den Cache von Client-Bibliotheken zu invalidieren. Dieses Tool ist besonders effektiv bei der Entwicklung mit LESS, da dies AEM zwingen kann, die generierte CSS neu zu kompilieren. Im Allgemeinen ist es effektiver, Caches zu invalidieren und dann eine Seitenaktualisierung vorzunehmen, anstatt alle Bibliotheken neu zu erstellen. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Debuggen von Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Wenn Sie den Cache mit dem Tool [!UICONTROL Client-Bibliotheken neu erstellen] ständig invalidieren müssen, kann es sich lohnen, alle Client-Bibliotheken einmal neu zu erstellen. Dies kann etwa 15 Minuten dauern, in der Regel werden jedoch alle zukünftigen Caching-Probleme behoben.
