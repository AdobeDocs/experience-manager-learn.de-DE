---
title: Erste Schritte mit AEM Sites – Projekt-Setup
description: Erstellen Sie ein Maven-Projekt mit mehreren Modulen, um den Code und die Konfigurationen für eine Site in Experience Manager zu verwalten.
version: 6.5, Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 578
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1684'
ht-degree: 100%

---

# Projekt-Setup {#project-setup}

In diesem Tutorial wird die Erstellung eines Maven-Projekts mit mehreren Modulen beschrieben, um den Code und die Konfigurationen für eine Adobe Experience Manager-Site zu verwalten.

## Voraussetzungen {#prerequisites}

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](./overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager lokal verfügbar ist und keine zusätzlichen Beispiel-/Demopakete installiert wurden (außer den erforderlichen Service Packs).

## Ziel {#objective}

1. Erfahren Sie, wie Sie mit einem Maven-Archetyp ein neues AEM-Projekt erstellen.
1. Machen Sie sich mit den verschiedenen Modulen vertraut, die vom AEM-Projektarchetyp generiert wurden, und erfahren Sie, wie sie zusammenarbeiten.
1. Erfahren Sie, wie AEM-Kernkomponenten in ein AEM-Projekt eingebunden sind.

## Was Sie erstellen werden {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

In diesem Kapitel erstellen Sie ein neues Projekt in Adobe Experience Manager mit dem [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype). Ihr AEM-Projekt enthält den vollständigen Code, die Inhalte und Konfigurationen, die für eine Sites-Implementierung nötig sind. Das in diesem Kapitel erstellte Projekt dient als Grundlage für die Implementierung der WKND-Site und wird in künftigen Kapiteln verwendet.

**Was ist ein Maven-Projekt?** - [Apache Maven](https://maven.apache.org/) ist ein Software-Management-Tool zum Erstellen von Projekten. Alle Implementierungen von *Adobe Experience Manager* verwenden Maven-Projekte zum Erstellen, Verwalten und Bereitstellen von benutzerdefiniertem Code für AEM.

**Was ist ein Maven-Archetyp?** – Ein [Maven-Archetyp](https://maven.apache.org/archetype/index.html) ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der AEM-Projektarchetyp hilft Ihnen, ein neues Projekt mit einem benutzerdefinierten Namespace zu generieren und dabei eine Projektstruktur zu verwenden, die Best Practices folgt und die Projektentwicklung erheblich beschleunigt.

## Erstellen eines Projekts {#create}

Es gibt mehrere Möglichkeiten zum Erstellen eines Maven-Multimodulprojekts für AEM. In diesem Tutorial wird der [Maven AEM-Projektarchetyp **35**](https://github.com/adobe/aem-project-archetype) verwendet. Cloud Manager stellt auch [einen UI-Assistenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=de) bereit, um mit der Erstellung eines AEM-Anwendungsprojekts zu beginnen. Das zugrunde liegende Projekt, das über die Benutzeroberfläche von Cloud Manager generiert wurde, weist dieselbe Struktur auf wie die direkte Verwendung des Archetyps.

>[!NOTE]
>
>Dieses Tutorial verwendet Version **35** des Archetyps. Es empfiehlt sich immer, bei der Generierung eines neuen Projekts die **neueste** Version des Archetyps zu verwenden.

Die nächsten Schritte werden mit einem UNIX®-basierten Befehlszeilen-Terminal durchgeführt, sollten aber bei Verwendung eines Windows-Terminals ähnlich sein.

1. Öffnen Sie ein Befehlszeilen-Terminal. Stellen Sie sicher, dass Maven installiert ist:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Navigieren Sie zu dem Ordner, in dem Sie das AEM-Projekt generieren möchten. Dies kann ein beliebiger Ordner sein, in dem Sie den Quellcode Ihres Projekts verwalten möchten. Beispiel: ein Verzeichnis mit dem Namen `code` unter dem Hauptverzeichnis des Benutzers bzw. der Benutzerin:

   ```shell
   $ cd ~/code
   ```

1. Fügen Sie Folgendes in die Befehlszeile ein, um [das Projekt im Batch-Modus zu generieren](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Um AEM 6.5.14+ zu verwenden, ersetzen Sie `aemVersion="cloud"` durch `aemVersion="6.5.14"`.
   >
   > Verwenden Sie außerdem immer die neueste `archetypeVersion` durch Verweis auf den [AEM-Projektarchetyp > Nutzung](https://github.com/adobe/aem-project-archetype#usage)

   Eine vollständige Liste der verfügbaren Eigenschaften zum Konfigurieren eines Projekts [finden Sie hier](https://github.com/adobe/aem-project-archetype#available-properties).

1. Die folgende Ordner- und Dateistruktur wird vom Maven-Archetyp in Ihrem lokalen Dateisystem generiert:

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
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Erstellen und Bereitstellen des Projekts {#build}

Erstellen Sie den Code des Projekts und stellen Sie ihn in einer lokalen Instanz von AEM bereit.

1. Stellen Sie sicher, dass eine Autoreninstanz von AEM lokal am Port **4502** ausgeführt wird.
1. Navigieren Sie in der Befehlszeile zum Projektverzeichnis `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Führen Sie den folgenden Befehl aus, um das gesamte Projekt zu erstellen und in AEM bereitzustellen:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Der Build dauert etwa eine Minute und sollte mit der folgenden Meldung enden:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Module des Projekts und stellt ein einziges Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket in einer AEM-Instanz bereitgestellt, die lokal am Port **4502** und mit den Anmeldeinformationen von `admin:admin` ausgeführt wird.

1. Navigieren Sie in Ihrer lokalen AEM-Instanz zu Package Manager: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten Pakete für `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` und `aem-guides-wknd.all` sehen.

1. Navigieren Sie zur Sites-Konsole: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Die WKND-Site ist eine der verfügbaren Sites. Sie enthält eine Site-Struktur mit einer US- und Sprach-Master-Hierarchie. Diese Site-Hierarchie basiert auf den Werten für `language_country` und `isSingleCountryWebsite` beim Generieren des Projekts mithilfe des Archetyps.

1. Öffnen Sie die Seite **US** `>` **English**, indem Sie die Seite auswählen und in der Menüleiste auf die Schaltfläche **Bearbeiten** klicken:

   ![Site-Konsole](assets/project-setup/aem-sites-console.png)

1. Der Startinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Die Grundlagen einer Komponente werden Sie im nächsten Kapitel kennenlernen.

   ![Inhalte zum Einstieg](assets/project-setup/start-home-page.png)

   *Vom Archetyp generierter Beispielinhalt*

## Prüfen des Projekts {#project-structure}

Das generierte AEM-Projekt besteht aus einzelnen Maven-Modulen mit jeweils einer anderen Rolle. Dieses Tutorial und die meisten Entwicklungsaktivitäten befassen sich mit diesen Modulen:

* [Core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=de) – Java-Code, hauptsächlich für Backend-Entwickler und -Entwicklerinnen.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de) – Enthält Quellcode für CSS, JavaScript, Sass, TypeScript, hauptsächlich für Frontend-Entwickler.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=de) – Enthält Komponenten- und Dialogfelddefinitionen, bettet kompilierte CSS- und JavaScript-Dateien als Client-Bibliotheken ein.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=de) – enthält strukturelle Inhalte und Konfigurationen wie bearbeitbare Vorlagen und Metadatenschemata (/content, /conf).

* **all** – Dies ist ein leeres Maven-Modul, das die oben genannten Module zu einem einzigen Paket kombiniert, das in einer AEM-Umgebung bereitgestellt werden kann.

![Maven-Projektdiagramm](assets/project-setup/project-pom-structure.png)

Weitere Informationen zu **allen** Maven-Modulen erhalten Sie in der [Dokumentation zu AEM-Projektarchetypen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de).

### Einbeziehung der Kernkomponenten {#core-components}

[AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) sind eine Reihe standardisierter WCM-Komponenten (Web Content Management) für AEM. Diese Komponenten bieten allgemeine Funktionen und werden für einzelne Projekte erstellt, angepasst und erweitert.

Die Umgebung für AEM as a Cloud Service enthält die neueste Version der [AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de). Daher sind in Projekte, die für AEM as a Cloud Service generiert werden, **keine** AEM-Kernkomponenten eingebettet.

Für mit AEM 6.5/6.4 generierte Projekte bettet der Archetyp automatisch [AEM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) in das Projekt ein. Es empfiehlt sich, AEM-Kernkomponenten für AEM 6.5/6.4 einzubetten, um sicherzustellen, dass die neueste Version mit Ihrem Projekt bereitgestellt wird. Weitere Informationen zur Verwendung von Kernkomponenten [im Projekt finden Sie hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=de#core-components).

## Versionsverwaltung {#source-control}

Es empfiehlt sich immer, irgendeine Form der Versionsverwaltung in Ihrer Anwendung zu nutzen. In diesem Tutorial werden Git und GitHub verwendet. Es gibt mehrere Dateien, die von Maven und/oder der IDE Ihrer Wahl generiert werden und die vom SCM ignoriert werden sollten.

Maven generiert bei jeder Erstellung und Installation des Code-Pakets einen Zielordner. Der Zielordner und die Inhalte sollten vom SCM ausgeschlossen werden.

Unter dem Modul `ui.apps` werden viele `.content.xml`-Dateien erstellt. Diese XML-Dateien ordnen die Knotentypen und Eigenschaften der im JCR installierten Inhalte zu. Diese Dateien sind wichtig und **dürfen nicht** ignoriert werden.

Der AEM-Projektarchetyp generiert eine beispielhafte `.gitignore`-Datei, die als Ausgangspunkt verwendet werden kann. Hierfür können Dateien sicher ignoriert werden. Die Datei wird unter `<src>/aem-guides-wknd/.gitignore` erstellt.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben Ihr erstes AEM-Projekt erstellt!

### Nächste Schritte {#next-steps}

Machen Sie sich im Tutorial [Grundlagen zu Komponenten](component-basics.md) durch ein einfaches `HelloWorld`-Beispiel mit der zugrunde liegenden Technologie einer Sites-Komponente in Adobe Experience Manager (AEM) vertraut.

## Erweiterte Maven-Befehle (Bonus) {#advanced-maven-commands}

Während der Entwicklung können Sie nur mit einem der Module arbeiten, um zu vermeiden, dass Sie das gesamte Projekt erstellen müssen, um Zeit zu sparen. Möglicherweise möchten Sie eine direkte Bereitstellung auf einer AEM-Veröffentlichungsinstanz oder auf einer Instanz von AEM durchführen, die nicht auf Port 4502 ausgeführt wird.

Im Folgenden werden einige zusätzliche Maven-Profile und -Befehle vorgestellt, die Ihnen bei der Entwicklung mehr Flexibilität bieten.

### Kernmodul {#core-module}

Das **[Kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=de)** modul enthält den gesamten mit dem Projekt verknüpften Java™-Code. Der Build vom **Kern** modul stellt ein OSGi-Bundle für AEM bereit. So erstellen Sie nur dieses Modul:

1. Navigieren Sie zum `core`-Ordner (unter `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Führen Sie den folgenden Befehl aus:

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

1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dies ist die OSGi-Web-Konsole. Sie enthält Informationen zu allen auf der AEM-Instanz installierten Bundles.

1. Schalten Sie die Sortierspalte **ID** um und Sie sollten sehen, dass das WKND-Paket installiert und aktiv ist.

   ![Kernpaket](assets/project-setup/wknd-osgi-console.png)

1. Sie können den „physischen“ Speicherort des JARs in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) sehen:

   ![CRXDE-Speicherort von JAR](assets/project-setup/jcr-bundle-location.png)

### Ui.apps- und Ui.content-Module {#apps-content-module}

Das Maven-Modul **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=de)** enthält den gesamten Rendering-Code, der für die unter `/apps` liegende Site benötigt wird. Dazu gehören CSS/JS-Inhalte, die in einem AEM-Format namens [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=de) gespeichert sind. Dazu gehören auch [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=de)-Skripte für die Darstellung von dynamischem HTML. Sie können sich das Modul **ui.apps** als eine Abbildung der Struktur im JCR vorstellen, jedoch in einem Format, das in einem Dateisystem gespeichert und in die Versionsverwaltung übertragen werden kann. Das Modul **ui.apps** enthält nur Code.

So erstellen Sie nur dieses Modul:

1. Über die Befehlszeile. Navigieren Sie zum Ordner `ui.apps` (unter `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Führen Sie den folgenden Befehl aus:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navigieren Sie zu [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten das Paket `ui.apps` als erstes installiertes Paket sehen und es sollte einen neueren Zeitstempel haben als alle anderen Pakete.

   ![Ui.apps-Paket installiert](assets/project-setup/ui-apps-package.png)

1. Kehren Sie zur Befehlszeile zurück und führen Sie den folgenden Befehl aus (im Ordner `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   Das Profil `autoInstallPackagePublish` ist für die Bereitstellung des Pakets in einer Veröffentlichungsumgebung vorgesehen, die auf Port **4503** läuft. Der obige Fehler wird angezeigt, wenn keine auf http://localhost:4503 ausgeführte AEM-Instanz gefunden werden kann.

1. Führen Sie schließlich den folgenden Befehl aus, um das Paket `ui.apps` auf Port **4504** bereitzustellen:

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

   Auch hier wird ein Build-Fehler angezeigt, wenn keine AEM-Instanz auf Port **4504** verfügbar ist. Der Parameter `aem.port` wird in der POM-Datei unter `aem-guides-wknd/pom.xml` definiert.

Das Modul **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=de)** ist auf die gleiche Weise strukturiert wie das Modul **ui.apps**. Der einzige Unterschied besteht darin, dass das Modul **ui.content** den sogenannten **veränderlichen** Inhalt enthält. **Veränderlicher** Inhalt bezieht sich im Wesentlichen auf Nicht-Code-Konfigurationen wie Vorlagen, Richtlinien oder Ordnerstrukturen, die in der Versionsverwaltung **gespeichert werden, aber** direkt auf einer AEM-Instanz geändert werden können. Dies wird im Kapitel zu Seiten und Vorlagen ausführlicher untersucht.

Die gleichen Maven-Befehle, die zur Erstellung des Moduls **ui.apps** verwendet wurden, können auch zur Erstellung des Moduls **ui.content** verwendet werden. Sie können die obigen Schritte im Ordner **ui.content** wiederholen.

## Fehlerbehebung

Wenn beim Generieren des Projekts mit dem AEM-Projektarchetyp Probleme auftreten, sehen Sie sich die Liste der [bekannten Probleme](https://github.com/adobe/aem-project-archetype#known-issues) und die Liste der offenen [Probleme](https://github.com/adobe/aem-project-archetype/issues) an.

## Nochmals herzlichen Glückwunsch! {#congratulations-bonus}

Sie haben haben sich auch das zusätzliche Infomaterial angesehen.

### Nächste Schritte {#next-steps-bonus}

Lernen Sie mit dem Tutorial zu den [Komponenten-Grundlagen](component-basics.md) anhand eines einfachen `HelloWorld`-Beispiels die einer Adobe Experience Manager (AEM) Sites-Komponente zugrundeliegende Technologie kennen.
