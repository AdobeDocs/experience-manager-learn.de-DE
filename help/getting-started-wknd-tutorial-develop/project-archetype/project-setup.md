---
title: Erste Schritte mit AEM Sites - Projekteinrichtung
seo-title: Erste Schritte mit AEM Sites - Projekteinrichtung
description: Behandelt die Erstellung eines Maven-Multi-Module-Projekts zur Verwaltung des Codes und der Konfigurationen für eine AEM Site.
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: AEM-Projektarchetyp
topic: Content Management, Entwicklung
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1888'
ht-degree: 5%

---


# Projekt-Setup {#project-setup}

In diesem Tutorial wird die Erstellung eines Maven-Multi-Module-Projekts beschrieben, um den Code und die Konfigurationen für eine Adobe Experience Manager-Site zu verwalten.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](../overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager lokal verfügbar ist und keine zusätzlichen Beispiel-/Demopakete installiert wurden (außer den erforderlichen Service Packs).

## Vorgabe {#objective}

1. Erfahren Sie, wie Sie mit einem Maven-Archetyp ein neues AEM-Projekt erstellen.
1. Machen Sie sich mit den verschiedenen Modulen vertraut, die vom AEM Projektarchetyp generiert wurden und wie sie zusammenarbeiten.
1. Erfahren Sie, wie AEM Kernkomponenten in einem AEM Projekt enthalten sind.

## Was Sie erstellen werden {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In diesem Kapitel erstellen Sie ein neues Adobe Experience Manager-Projekt mit dem AEM [Projektarchetyp](https://github.com/adobe/aem-project-archetype). Ihr AEM-Projekt enthält den gesamten Code, den Inhalt und die Konfigurationen, die für eine Sites-Implementierung verwendet werden. Das in diesem Kapitel erstellte Projekt wird als Grundlage für die Implementierung der WKND-Site dienen und in künftigen Kapiteln aufbauen.

**Was ist ein Maven-Projekt?** -  [Apache ](https://maven.apache.org/) Mavenis ist ein Software-Management-Tool zum Erstellen von Projekten. *Alle Adobe Experience* Manager-Implementierungen verwenden Maven-Projekte zum Erstellen, Verwalten und Bereitstellen von benutzerdefiniertem Code zusätzlich zu AEM.

**Was ist ein Maven-Archetyp?** - Ein  [Maven-](https://maven.apache.org/archetype/index.html) Archetyp ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der Archetyp AEM Projekts ermöglicht es uns, ein neues Projekt mit einem benutzerdefinierten Namespace zu generieren und eine Projektstruktur einzufügen, die Best Practices einhält und unser Projekt erheblich beschleunigt.

## Projekt erstellen {#create}

Es gibt mehrere Optionen zum Erstellen eines Maven-Multi-Modul-Projekts für AEM. In diesem Tutorial wird der AEM [Maven-Projektarchetyp **26**](https://github.com/adobe/aem-project-archetype) verwendet. Cloud Manager bietet außerdem [einen UI-Assistenten](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html), um die Erstellung eines AEM Anwendungsprojekts zu starten. Das zugrunde liegende Projekt, das von der Cloud Manager-Benutzeroberfläche generiert wurde, weist dieselbe Struktur auf wie die direkte Verwendung des Archetyps.

>[!NOTE]
>
>In diesem Tutorial wird die Version **26** des Archetyps verwendet. Es empfiehlt sich immer, die **neueste** Version des Archetyps zu verwenden, um ein neues Projekt zu erstellen.

Die nächsten Schritte werden mit einem UNIX-basierten Befehlszeilenterminal durchgeführt, sollten jedoch bei Verwendung eines Windows-Terminals ähnlich sein.

1. Öffnen Sie ein Befehlszeilen-Terminal. Stellen Sie sicher, dass Maven installiert ist:

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

   Wenn Sie **not** die **adobe-public** anzeigen, ist dies ein Hinweis darauf, dass der Adobe-Repo in Ihrer `~/.m2/settings.xml`-Datei nicht ordnungsgemäß referenziert wird. Lesen Sie die Schritte zur Installation und Konfiguration von Apache Maven in [einer lokalen Entwicklungsumgebung](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Navigieren Sie zu einem Ordner, in dem Sie das AEM Projekt generieren möchten. Dies kann ein beliebiger Ordner sein, in dem Sie den Quellcode Ihres Projekts verwalten möchten. Beispiel: ein Verzeichnis mit dem Namen `code` unter dem Basisverzeichnis des Benutzers:

   ```shell
   $ cd ~/code
   ```

1. Fügen Sie Folgendes in die Befehlszeile zu [Erstellen Sie das Projekt im Batch-Modus](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=26 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Bei der Zielgruppenbestimmung AEM 6.5.5+ `aemVersion="cloud"` durch `aemVersion="6.5.5"` ersetzen. Verwenden Sie bei der Zielgruppenbestimmung ab Version 6.4.8 `aemVersion="6.4.8"`.

   Eine vollständige Liste der verfügbaren Eigenschaften zum Konfigurieren eines Projekts [finden Sie hier](https://github.com/adobe/aem-project-archetype#available-properties).

1. Die folgende Ordner- und Dateistruktur wird vom Maven-Archetyp auf Ihrem lokalen Dateisystem generiert:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Projekt bereitstellen und erstellen {#build}

Erstellen Sie den Projektcode und stellen Sie ihn auf einer lokalen Instanz von AEM bereit.

1. Stellen Sie sicher, dass Sie über eine Autoreninstanz von AEM verfügen, die lokal am Port **4502** ausgeführt wird.
1. Navigieren Sie über die Befehlszeile zum Projektverzeichnis `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Führen Sie den folgenden Befehl aus, um das gesamte Projekt zu erstellen und AEM bereitzustellen:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Der Build dauert etwa eine Minute und sollte mit der folgenden Meldung enden:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Module des Projekts und stellt ein einzelnes Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket auf einer AEM-Instanz bereitgestellt, die lokal auf Port **4502** und mit den Anmeldedaten von `admin:admin` ausgeführt wird.

1. Navigieren Sie in Ihrer lokalen AEM-Instanz zu Package Manager : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten Pakete für `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` und `aem-guides-wknd.all` sehen.

1. Navigieren Sie zur Sites-Konsole: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Die WKND-Site wird eine der Sites sein. Es wird eine Site-Struktur mit einer US- und Sprach-Master-Hierarchie enthalten. Diese Site-Hierarchie basiert auf den Werten für `language_country` und `isSingleCountryWebsite` beim Generieren des Projekts mit dem Archetyp.

1. Öffnen Sie die Seite **US** `>` **English**, indem Sie die Seite auswählen und in der Menüleiste auf die Schaltfläche **Bearbeiten** klicken:

   ![Site-Konsole](assets/project-setup/aem-sites-console.png)

1. Starterinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Im nächsten Kapitel lernen Sie die Grundlagen einer Komponente kennen.

   ![Starter-Inhalt](assets/project-setup/start-home-page.png)

   *Vom Archetyp generierter Beispielinhalt*

## Inspect das Projekt {#project-structure}

Das generierte AEM Projekt besteht aus einzelnen Maven-Modulen mit jeweils einer anderen Rolle. Dieses Tutorial und ein Großteil der Entwicklung konzentrieren sich auf diese Module:

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  - Java-Code, in erster Linie Back-End-Entwickler.
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  - Enthält Quellcode für CSS, JavaScript, Sass, Type Script, hauptsächlich für Frontend-Entwickler.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  - Enthält Komponenten- und Dialogfelddefinitionen, bettet kompilierte CSS- und JavaScript-Dateien als Client-Bibliotheken ein.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  - enthält Strukturinhalte und Konfigurationen wie bearbeitbare Vorlagen, Metadatenschemata (/content, /conf).

* **all**  - dies ist ein leeres Maven-Modul, das die oben genannten Module zu einem einzigen Paket kombiniert, das in einer AEM Umgebung bereitgestellt werden kann.

![Maven-Projektdiagramm](assets/project-setup/project-pom-structure.png)

Weitere Informationen zu den Maven-Modulen finden Sie in der [AEM Dokumentation zum Projektarchetyp](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) , um mehr über **alle** zu erfahren.

### Aufnahme von Kernkomponenten {#core-components}

[AEM Kernkomponenten ](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html) sind eine Reihe standardisierter Web Content Management (WCM)-Komponenten für AEM. Diese Komponenten bieten einen Grundsatz an Funktionen und sind für die Formatierung, Anpassung und Erweiterung einzelner Projekte ausgelegt.

AEM as a Cloud Service-Umgebungen enthalten die neueste Version von [AEM Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Daher enthalten für AEM als Cloud Service generierte Projekte **nicht** eine Einbettung AEM Kernkomponenten.

Für AEM 6.5/6.4 generierten Projekte bettet der Archetyp [AEM Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) automatisch in das Projekt ein. Es empfiehlt sich, AEM 6.5/6.4 AEM Kernkomponenten einzubetten, um sicherzustellen, dass die neueste Version mit Ihrem Projekt bereitgestellt wird. Weitere Informationen dazu, wie Kernkomponenten [im Projekt enthalten sind, finden Sie hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Quellcodeverwaltung {#source-control}

Es empfiehlt sich immer, eine Form der Quell-Code-Verwaltung in Ihrer Anwendung zu verwenden. In diesem Tutorial werden Git und GitHub verwendet. Es gibt mehrere Dateien, die von Maven und/oder der IDE Ihrer Wahl generiert werden und die vom SCM ignoriert werden sollten.

Maven erstellt bei jeder Erstellung und Installation des Code-Pakets einen Zielordner. Der Zielordner und die Inhalte sollten von SCM ausgeschlossen werden.

Beachten Sie unter `ui.apps`, dass viele `.content.xml` Dateien erstellt werden. Diese XML-Dateien ordnen die Knotentypen und Eigenschaften des im JCR installierten Inhalts zu. Diese Dateien sind wichtig und sollten **nicht** ignoriert werden.

Der AEM Projektarchetyp generiert eine Beispieldatei `.gitignore` , die als Ausgangspunkt verwendet werden kann, für die Dateien sicher ignoriert werden können. Die Datei wird unter `<src>/aem-guides-wknd/.gitignore` erstellt.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihr erstes AEM Projekt erstellt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit der zugrunde liegenden Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch ein einfaches `HelloWorld`-Beispiel im Tutorial [Komponentengrundlagen](component-basics.md) vertraut.

## Erweiterte Maven-Befehle (Bonus) {#advanced-maven-commands}

Während der Entwicklung können Sie nur mit einem der Module arbeiten und vermeiden, das gesamte Projekt zu erstellen, um Zeit zu sparen. Sie können auch eine direkte Bereitstellung auf einer AEM-Veröffentlichungsinstanz oder möglicherweise auf einer Instanz von AEM durchführen, die nicht auf Port 4502 ausgeführt wird.

Als Nächstes werden wir uns einige zusätzliche Maven-Profile und -Befehle ansehen, die Sie für mehr Flexibilität bei der Entwicklung verwenden können.

### Kernmodul {#core-module}

Das Modul **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** enthält den gesamten mit dem Projekt verknüpften Java-Code. Nach der Erstellung wird ein OSGi-Bundle für die AEM bereitgestellt. So erstellen Sie nur dieses Modul:

1. Navigieren Sie zum Ordner `core` (unter `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Führen Sie folgenden Befehl aus:

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dies ist die OSGi-Web-Konsole und enthält Informationen zu allen Bundles, die auf der AEM-Instanz installiert sind.

1. Schalten Sie die Sortierungsspalte **Id** um. Das WKND-Bundle sollte installiert und aktiv sein.

   ![Kernpaket](assets/project-setup/wknd-osgi-console.png)

1. Sie können den &quot;physischen&quot;Speicherort der JAR-Datei in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) sehen:

   ![CRXDE-Speicherort von JAR](assets/project-setup/jcr-bundle-location.png)

### Ui.apps- und Ui.content-Module {#apps-content-module}

Das Maven-Modul **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** enthält den gesamten Rendercode, der für die Site unter `/apps` benötigt wird. Dazu gehört auch CSS/JS, das im AEM Format [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html) gespeichert wird. Dazu gehören auch [HTL](https://docs.adobe.com/content/help/de-DE/experience-manager-htl/using/overview.html)-Skripte zum Rendern von dynamischem HTML. Sie können sich das Modul **ui.apps** als Zuordnung zur Struktur im JCR vorstellen, jedoch in einem Format, das auf einem Dateisystem gespeichert und an die Quell-Code-Verwaltung übertragen werden kann. Das Modul **ui.apps** enthält nur Code.

So erstellen Sie das Modul nur dieses:

1. In der Befehlszeile. Navigieren Sie zum Ordner `ui.apps` (unter `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Führen Sie folgenden Befehl aus:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigieren Sie zu [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten das `ui.apps`-Paket als erstes installiertes Paket sehen und es sollte einen aktuelleren Zeitstempel als eines der anderen Pakete haben.

   ![installiertes Ui.apps-Paket](assets/project-setup/ui-apps-package.png)

1. Kehren Sie zur Befehlszeile zurück und führen Sie den folgenden Befehl aus (im Ordner `ui.apps` ):

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

   Das Profil `autoInstallPackagePublish` ist für die Bereitstellung des Pakets in einer Veröffentlichungsumgebung vorgesehen, die auf Port **4503** ausgeführt wird. Der obige Fehler wird erwartet, wenn eine AEM Instanz, die auf http://localhost:4503 ausgeführt wird, nicht gefunden werden kann.

1. Führen Sie schließlich den folgenden Befehl aus, um das `ui.apps`-Paket auf Port **4504** bereitzustellen:

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

   Auch hier wird ein Build-Fehler erwartet, wenn keine AEM Instanz verfügbar ist, die auf Port **4504** ausgeführt wird. Der Parameter `aem.port` wird in der POM-Datei unter `aem-guides-wknd/pom.xml` definiert.

Das Modul **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** ist auf dieselbe Weise strukturiert wie das Modul **ui.apps**. Der einzige Unterschied besteht darin, dass das Modul **ui.content** Inhalte enthält, die als **veränderlicher** -Inhalt bezeichnet werden. **** Veränderlicher Inhalt bezieht sich im Wesentlichen auf Nicht-Code-Konfigurationen wie Vorlagen, Richtlinien oder Ordnerstrukturen, die in der Quell-Code-Verwaltung gespeichert sind,  **** aber direkt auf einer AEM-Instanz geändert werden können. Dies wird im Kapitel über Seiten und Vorlagen ausführlicher untersucht.

Mit denselben Maven-Befehlen, die zum Erstellen des Moduls **ui.apps** verwendet werden, können Sie das Modul **ui.content** erstellen. Wiederholen Sie die oben genannten Schritte im Ordner **ui.content** .
