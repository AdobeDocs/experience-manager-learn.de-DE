---
title: Erste Schritte mit AEM Sites - Projekteinrichtung
seo-title: Erste Schritte mit AEM Sites - Projekteinrichtung
description: Behandelt die Erstellung eines Maven Multi-Module-Projekts zur Verwaltung des Codes und der Konfigurationen für eine AEM Site.
sub-product: Sites
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 8%

---


# Projekt-Setup {#project-setup}

Dieses Lernprogramm umfasst die Erstellung eines Maven-Multi-Module-Projekts zur Verwaltung des Codes und der Konfigurationen für eine Adobe Experience Manager-Site.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager lokal verfügbar ist und dass keine zusätzlichen Beispiel-/Demopakete installiert wurden (außer den erforderlichen Service Packs).

## Vorgabe {#objective}

1. Erfahren Sie, wie Sie mit einem Maven-Archetyp ein neues AEM Projekt erstellen.
1. Machen Sie sich mit den verschiedenen Modulen vertraut, die vom AEM-Projektarchetyp generiert wurden und wie sie zusammenarbeiten.
1. Verstehen Sie, wie AEM Kernkomponenten in ein AEM Projekt einbezogen werden.

## Was Sie erstellen werden {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In diesem Kapitel erstellen Sie ein neues Adobe Experience Manager-Projekt mit dem AEM [Projekttyp](https://github.com/adobe/aem-project-archetype). Ihr AEM-Projekt enthält den gesamten Code, den Inhalt und die Konfigurationen, die für eine Sites-Implementierung verwendet werden. Das in diesem Kapitel erstellte Projekt wird als Grundlage für die Implementierung der WKND-Site dienen und in künftigen Kapiteln aufbauen.

## Hintergrund {#background}

**Was ist ein Maven-Projekt?** -  [Apache ](https://maven.apache.org/) Mavenis ist ein Software-Management-Tool zum Aufbau von Projekten. *Alle Adobe Experience* Manager-Implementierungen verwenden Maven-Projekte, um benutzerdefinierten Code zu erstellen, zu verwalten und über AEM bereitzustellen.

**Was ist ein Maven-Archetyp?** - Ein  [Maven-](https://maven.apache.org/archetype/index.html) Archetyp ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der AEM Projekt-Archetyp ermöglicht es uns, ein neues Projekt mit einem benutzerdefinierten Namensraum zu erstellen und eine Projektstruktur einzubinden, die sich an Best Practices orientiert und unser Projekt erheblich beschleunigt.

## Projekt {#create} erstellen

Es gibt mehrere Optionen zum Erstellen eines Maven-Multi-Modul-Projekts für AEM. Dieses Tutorial nutzt den AEM [Maven-Projekttyp **2**](https://github.com/adobe/aem-project-archetype). Cloud Manager bietet außerdem einen UI-Assistenten[, um die Erstellung eines AEM Anwendungsprojekts zu starten. ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) Das zugrunde liegende Projekt, das von der Benutzeroberfläche von Cloud Manager generiert wurde, ergibt dieselbe Struktur wie die direkte Verwendung des Archetyps.

>[!NOTE]
>
>Um diesem Tutorial zu folgen, benutzen Sie bitte die Version **22** des Archetyps. Es empfiehlt sich jedoch immer, die **letzte** Version des Archetyps zu verwenden, um ein neues Projekt zu erstellen.

Die nächste Reihe von Schritten erfolgt mit einem UNIX-basierten Befehlszeilenterminal, sollte jedoch bei Verwendung eines Windows-Terminals ähnlich sein.

1. Öffnen Sie ein Befehlszeilenterminal und überprüfen Sie, ob Maven installiert wurde und dem Befehlszeilenpfad hinzugefügt wurde:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Stellen Sie sicher, dass das Profil **adobe-public** aktiv ist, indem Sie den folgenden Befehl ausführen:

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

   Wenn Sie **not** nicht **suchen, sehen Sie in der Datei &lt;a2/>adobe-public**, dass auf die Adobe Repo in Ihrer `~/.m2/settings.xml`-Datei nicht richtig verwiesen wird. Bitte wiederholen Sie die Schritte zur Installation und Konfiguration von Apache Maven in [einer lokalen Entwicklungs-Umgebung](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Navigieren Sie zu einem Ordner, in dem Sie das AEM Projekt erstellen möchten. Dies kann ein beliebiger Ordner sein, in dem Sie den Quellcode Ihres Projekts verwalten möchten. Beispiel: Ordner `code` unter dem Basisordner des Benutzers:

   ```shell
   $ cd ~/code
   ```

1. Fügen Sie Folgendes in die Befehlszeile zu [Generieren des Projekts im Stapelmodus](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html) ein:

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >Beim Generieren eines Projekts aus dem Maven-Archetyp wird standardmäßig der interaktive Modus verwendet. Um eine schnelle Eingabe von Werten zu vermeiden, verwenden Sie den Stapelmodus. Es ist auch möglich, das Maven AEM Projekt mit dem Plugin [AEM Developer Tools für Eclipse](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/aem-eclipse.html) zu erstellen.

   >[!CAUTION]
   >
   >Wenn Sie einen Fehler wie den folgenden erhalten: *Fehler beim Ausführen von Ziel org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate (default-cli) für Projekt standalone-pom: Der gewünschte Archetyp ist nicht vorhanden.* Es ist ein Hinweis darauf, dass die Adobe Repo in Ihrer `~/.m2/settings.xml`-Datei nicht richtig referenziert wird. Bitte überprüfen Sie, ob die Datei &quot;settings.xml&quot;auf den Adoben-Repo verweist.

   In der folgenden Tabelle werden die für dieses Lernprogramm verwendeten Werte Liste:

   | Name | Werte | Beschreibung |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | Base Maven groupId |
   | artifactId | aem-guides-work | Maven-Basis ArtefaktId |
   | Version | 0.0.1-SNAPSHOT | Version |
   | package | com.adobe.aem.guides.wknd | Java-Quellpaket |
   | appsFolderName | wknd | /apps Ordnername |
   | artifactName | WKND-Sites-Projekt | Maven-Projektname |
   | componentGroupName | WKND | AEM-Komponentengruppen-Name |
   | contentFolderName | wknd | /content-Ordnername |
   | confFolderName | wknd | /conf-Ordnername |
   | cssId | wknd | Genutztes Präfix in erstelltem CSS |
   | packageGroup | wknd | Inhaltspaket-Gruppenname |
   | siteName | WKND-Site | AEM-Sitename |
   | optionAemVersion | 6.5.0 | Target AEM-Version |
   | language_country | en_us | Sprach-/Ländercode zum Erstellen der Inhaltsstruktur aus (z.B. en_us) |
   | optionIncludeExamples | y | Beispielsite Komponentenbibliothek einschließen |
   | optionIncludeErrorHandler | n | Eine benutzerdefinierte 404-Antwortseite einschließen |
   | optionIncludeFrontendModule | y | Ein dediziertes Frontend-Modul einschließen |
   | isSingleCountryWebsite | n | Erstellt eine Sprach-Master-Struktur im Beispielinhalt |
   | optionDispatcherConfig | keine | Dispatcher-Konfigurationsmodul erstellen |

1. Die folgende Ordner- und Dateistruktur wird vom Maven-Archetyp auf Ihrem lokalen Dateisystem generiert:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Erstellen Sie das Projekt {#build}

Nachdem wir ein neues Projekt erstellt haben, können wir den Projektcode auf einer lokalen Instanz von AEM bereitstellen.

1. Stellen Sie sicher, dass eine Instanz von AEM lokal am Port **4502** ausgeführt wird.
1. Navigieren Sie in der Befehlszeile zum Projektverzeichnis `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Führen Sie den folgenden Befehl aus, um das gesamte Projekt zu erstellen und für AEM bereitzustellen:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Projektmodule und stellt ein einzelnes Paket für die AEM Instanz bereit. Standardmäßig wird dieses Paket auf einer AEM Instanz bereitgestellt, die lokal am Port **4502** und mit den Anmeldeinformationen von `admin:admin` ausgeführt wird.

1. Navigieren Sie zu Package Manager auf Ihrer lokalen AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Es sollten drei Pakete für `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.content` und `aem-guides-wknd.all` angezeigt werden.

   Es sollten auch mehrere Pakete für [AEM Kernkomponenten](https://docs.adobe.com/content/help/de/experience-manager-core-components/using/introduction.html) angezeigt werden, die vom Archetyp im Projekt enthalten sind. Dies wird später im Tutorial behandelt.

1. Navigieren Sie zur Sites-Konsole: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Die WKND-Site wird eine der Sites sein. Es wird eine Site-Struktur mit einer Hierarchie der US- und Sprachmeister enthalten. Diese Site-Hierarchie basiert auf den Werten für `language_country` und `isSingleCountryWebsite`, wenn das Projekt mit dem Archetyp generiert wird.

1. Öffnen Sie die Seite **US** `>` **Englisch**, indem Sie die Seite auswählen und in der Menüleiste auf die Schaltfläche **Bearbeiten** klicken:

   ![Site-Konsole](assets/project-setup/aem-sites-console.png)

1. Einige Inhalte wurden bereits erstellt, und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Die Konfiguration dieser Seite und der Komponenten wird später im Tutorial ausführlich erläutert.

## Inspect des Projekts {#project-structure}

Der AEM Archetyp besteht aus einzelnen Maven-Modulen:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  - Java Bundle, das alle Kernfunktionen wie OSGi-Dienste, Listener oder Planungen sowie komponentenbezogenen Java-Code wie Servlets oder Anforderungs-Filter enthält.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  - enthält die /apps-Teile des Projekts, d.h. JS&amp;CSS clientlibs, Komponenten und OSGi-Konfigurationen
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  - enthält Strukturinhalte und Konfigurationen wie bearbeitbare Vorlagen, Metadaten-Schemas (/content, /conf)
* ui.tests - Java Bundle, das JUnit-Tests enthält, die serverseitig ausgeführt werden. Dieses Bundle soll nicht in der Produktion bereitgestellt werden.
* ui.launcher - enthält Kleber-Code, der das Paket ui.tests (und abhängige Pakete) auf dem Server bereitstellt und die Ausführung der JUnit-Remote-Ausführung auslöst
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) - (optional) enthält die Artefakte, die zur Verwendung des Webpack-basierten Front-End-Buildmoduls erforderlich sind.
* all - dies ist ein leeres Maven-Modul, das die oben genannten Module zu einem einzigen Paket zusammenfasst, das auf einer AEM Umgebung bereitgestellt werden kann.

![Maven-Projektdiagramm](assets/project-setup/project-pom-structure.png)

Weitere Informationen zu den Maven-Modulen finden Sie in der Dokumentation zum AEM Archetype-Projekt[.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)

## Erweiterte Maven-Befehle {#advanced-maven-commands}

Während der Entwicklung können Sie mit nur einem der Module arbeiten und vermeiden, das gesamte Projekt zu bauen, um Zeit zu sparen. Möglicherweise möchten Sie die Bereitstellung auch direkt auf einer AEM Publish-Instanz oder möglicherweise auf einer Instanz von AEM durchführen, die nicht auf Port 4502 ausgeführt wird.

Als Nächstes werden wir einige zusätzliche Maven-Profil und -Befehle betrachten, die Sie für mehr Flexibilität während der Entwicklung verwenden können.

### Kernmodul {#core-module}

Das **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)**-Modul enthält den gesamten Java-Code, der mit dem Projekt verknüpft ist. Bei der Erstellung wird ein OSGi-Bundle zur AEM bereitgestellt. So erstellen Sie nur dieses Modul:

1. Navigieren Sie zum Ordner `core` (unter `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Führen Sie folgenden Befehl aus:

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dies ist die OSGi-Webkonsole und enthält Informationen zu allen auf der AEM Instanz installierten Bundles.

1. Schalten Sie die Sortierspalte **Id** um und das WKND-Bundle sollte installiert und aktiv sein.

   ![Kernpaket](assets/project-setup/wknd-osgi-console.png)

1. Sie können die &quot;physische&quot;Position der JAR-Datei in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar) sehen:

   ![CRXDE-Speicherort von Jar](assets/project-setup/jcr-bundle-location.png)

### Ui.apps- und Ui.content-Module {#apps-content-module}

Das Maven-Modul **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** enthält den gesamten Rendercode, der für die Site unter `/apps` benötigt wird. Dazu gehört auch CSS/JS, das im AEM Format [clientlibs](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/clientlibs.html) gespeichert wird. Dazu gehören auch [HTL](https://docs.adobe.com/docs/de/htl/overview.html)-Skripte zum Rendern von dynamischem HTML. Sie können sich das Modul **ui.apps** als Zuordnung zur Struktur in der JCR vorstellen, jedoch in einem Format, das auf einem Dateisystem gespeichert und der Quellcodeverwaltung verpflichtet werden kann. Das Modul **ui.apps** enthält nur Code.

So erstellen Sie das folgende Modul:

1. Über die Befehlszeile. Navigieren Sie zum Ordner `ui.apps` (unter `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Führen Sie folgenden Befehl aus:

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigieren Sie zu [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten das `ui.apps`-Paket als erstes installiertes Paket sehen und es sollte einen aktuelleren Zeitstempel haben als eines der anderen Pakete.

   ![Ui.apps-Paket installiert](assets/project-setup/ui-apps-package.png)

1. Kehren Sie zur Befehlszeile zurück und führen Sie den folgenden Befehl aus (im Ordner `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Das Profil `autoInstallPackagePublish` dient zum Bereitstellen des Pakets auf einer Publish-Umgebung, die auf Port **4503** ausgeführt wird. Der obige Fehler wird erwartet, wenn eine AEM Instanz, die auf http://localhost:4503 ausgeführt wird, nicht gefunden werden kann.

1. Führen Sie schließlich den folgenden Befehl aus, um das `ui.apps`-Paket an Port **4504** bereitzustellen:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   Es wird erneut ein Buildfehler erwartet, wenn keine AEM Instanz auf Port **4504** verfügbar ist. Der Parameter `aem.port` wird in der POM-Datei unter `aem-guides-wknd/pom.xml` definiert.

Das Modul **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** ist genauso strukturiert wie das Modul **ui.apps**. Der einzige Unterschied besteht darin, dass das Modul **ui.content** den Inhalt **mutable** enthält. **&quot;** Mutablecontent&quot;bezieht sich im Wesentlichen auf Nicht-Code-Konfigurationen wie Vorlagen, Richtlinien oder Ordnerstrukturen, die in der Quellcodeverwaltung gespeichert sind,  **** aber auf einer AEM direkt geändert werden können. Dies wird im Kapitel zu Seiten und Vorlagen ausführlicher untersucht. Das Wichtigste dabei ist, dass dieselben Maven-Befehle, die zum Erstellen des Moduls **ui.apps** verwendet werden, zum Erstellen des Moduls **ui.content** verwendet werden können. Wiederholen Sie die oben genannten Schritte im Ordner **ui.content**.

### Ui.frontend module {#ui-frontend-module}

Das Modul **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** ist ein Maven-Modul, das tatsächlich ein [Webpack](https://webpack.js.org/)-Projekt ist. Dieses Modul ist als dediziertes Front-End-Build-System eingerichtet, das JavaScript- und CSS-Dateien ausgibt, die wiederum für AEM bereitgestellt werden. Mit dem Modul **ui.frontend** können Entwickler mit Sprachen wie [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), [npm](https://www.npmjs.com/) kodieren und die Ausgabe direkt in AEM integrieren.

Das Modul **ui.frontend** wird im Kapitel über clientseitige Bibliotheken und Frontend-Entwicklung ausführlicher behandelt. Schauen wir uns zunächst einmal an, wie es in das Projekt integriert ist.

1. Über die Befehlszeile. Navigieren Sie zum Ordner `ui.frontend` (unter `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. Führen Sie folgenden Befehl aus:

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   Beachten Sie die Zeilen wie `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`. Dies bedeutet, dass die kompilierten CSS- und JS-Dateien in den Ordner `ui.apps` kopiert werden.

1. Ansicht des geänderten Zeitstempels für die Datei `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`. Es sollte erst kürzlich aktualisiert werden, als die anderen Dateien im `ui.apps`-Modul.

   Im Gegensatz zu den anderen Modulen, die wir uns ansahen, stellt das Modul **ui.frontend** nicht direkt AEM bereit. Stattdessen werden CSS und JS in das Modul **ui.apps** kopiert und anschließend wird das Modul **ui.apps** AEM bereitgestellt. Wenn Sie sich die Buildreihenfolge des ersten Maven-Befehls ansehen, sehen Sie, dass **ui.frontend** immer *vor* **ui.apps** erstellt wird.

   Später werden wir die erweiterten Funktionen des Moduls **ui.frontend** und des eingebetteten Webpack-Entwicklungsservers für eine schnelle Entwicklung betrachten.

## Aufnahme der Kernkomponenten {#core-components}

Der Archetyp bettet [AEM Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) automatisch in das Projekt ein. Früher wurden beim Prüfen der bereitgestellten Pakete auf AEM mehrere Pakete im Zusammenhang mit den Core-Komponenten einbezogen. Kernkomponenten sind eine Reihe von Basiskomponenten, die die Entwicklung eines AEM Sites-Projekts beschleunigen sollen. Hauptkomponenten sind Open Source und verfügbar unter [GitHub](https://github.com/adobe/aem-core-wcm-components). Weitere Informationen darüber, wie Kernkomponenten im Projekt [enthalten sind, finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components).

1. Öffnen Sie mit Ihrem Lieblings-Texteditor `aem-guides-wknd/pom.xml`.

1. Suchen Sie nach `core.wcm.components.version`. Hier sehen Sie, welche Version der Core-Komponenten enthalten ist:

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > Der AEM Project Archetype wird eine Version der AEM Core-Komponenten enthalten, jedoch haben diese Projekte unterschiedliche Versionszyklen, und daher ist die enthaltene Version der Core-Komponenten möglicherweise nicht die neueste. Als Best Practice sollten Sie stets darauf achten, die neueste Version der Core-Komponenten zu nutzen. Neue Funktionen und Fehlerkorrekturen werden häufig aktualisiert. Die neuesten [Versionsinformationen finden Sie unter GitHub](https://github.com/adobe/aem-core-wcm-components/releases).

1. Wenn Sie nach unten zum Abschnitt `dependencies` blättern, sollten Sie die einzelnen Kernkomponentenabhängigkeiten sehen:

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## Source Control Management {#source-control}

Es empfiehlt sich immer, eine Form der Quellcodeverwaltung zu verwenden, um den Code in Ihrer Anwendung zu verwalten. Dieses Lernprogramm verwendet Git und GitHub. Es gibt mehrere Dateien, die von Maven und/oder der IDE der Wahl generiert werden, die vom SCM ignoriert werden sollten.

Maven erstellt einen Ordner für Zielgruppen, sobald Sie das Codepaket erstellen und installieren. Der Ordner und der Inhalt der Zielgruppe sollten von SCM ausgeschlossen werden.

Unter ui.apps werden auch viele .content.xml-Dateien angezeigt, die erstellt wurden. Diese XML-Dateien ordnen die Knotentypen und Eigenschaften von Inhalten zu, die in der JCR-Datei installiert sind. Diese Dateien sind kritisch und sollten **nicht** ignoriert werden.

Der AEM Projektarchiv generiert eine Beispieldatei `.gitignore`, die als Ausgangspunkt verwendet werden kann, für den Dateien sicher ignoriert werden können. Die Datei wird bei `<src>/aem-guides-wknd/.gitignore` generiert.

## Überprüfen {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihr erstes AEM Projekt erstellt!

### Nächste Schritte {#next-steps}

Verstehen Sie die zugrunde liegende Technologie einer Adobe Experience Manager (AEM)-Sites-Komponente mithilfe eines einfachen `HelloWorld`-Beispiels mit dem Lehrgang [Komponentengrundlagen](component-basics.md).
