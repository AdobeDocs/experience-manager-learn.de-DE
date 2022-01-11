---
title: Erste Schritte mit AEM Sites - Projekteinrichtung
seo-title: Getting Started with AEM Sites - Project Setup
description: Behandelt die Erstellung eines Maven-Multi-Module-Projekts zur Verwaltung des Codes und der Konfigurationen für eine AEM Site.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
source-git-commit: a366d485da3f473bd4c1ef31538231965acc825c
workflow-type: tm+mt
source-wordcount: '1843'
ht-degree: 4%

---

# Projekt-Setup {#project-setup}

In diesem Tutorial wird die Erstellung eines Maven-Multi-Module-Projekts beschrieben, um den Code und die Konfigurationen für eine Adobe Experience Manager-Site zu verwalten.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten eines [lokale Entwicklungsumgebung](./overview.md#local-dev-environment). Stellen Sie sicher, dass eine neue Instanz von Adobe Experience Manager lokal verfügbar ist und keine zusätzlichen Beispiel-/Demopakete installiert wurden (außer den erforderlichen Service Packs).

## Ziel {#objective}

1. Erfahren Sie, wie Sie mit einem Maven-Archetyp ein neues AEM-Projekt erstellen.
1. Machen Sie sich mit den verschiedenen Modulen vertraut, die vom AEM Projektarchetyp generiert wurden und wie sie zusammenarbeiten.
1. Erfahren Sie, wie AEM Kernkomponenten in einem AEM Projekt enthalten sind.

## Was Sie erstellen werden {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

In diesem Kapitel erstellen Sie ein neues Adobe Experience Manager-Projekt mit dem [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype). Ihr AEM-Projekt enthält den gesamten Code, den Inhalt und die Konfigurationen, die für eine Sites-Implementierung verwendet werden. Das in diesem Kapitel erstellte Projekt wird als Grundlage für die Implementierung der WKND-Site dienen und in künftigen Kapiteln aufbauen.

**Was ist ein Maven-Projekt?** - [Apache Maven](https://maven.apache.org/) ist ein Software-Management-Tool zum Erstellen von Projekten. *Alle Adobe Experience Manager* -Implementierungen verwenden Maven-Projekte zum Erstellen, Verwalten und Bereitstellen von benutzerdefiniertem Code zusätzlich zu AEM.

**Was ist ein Maven-Archetyp?** - A [Maven-Archetyp](https://maven.apache.org/archetype/index.html) ist eine Vorlage oder ein Muster zum Generieren neuer Projekte. Der Archetyp AEM Projekts ermöglicht es uns, ein neues Projekt mit einem benutzerdefinierten Namespace zu generieren und eine Projektstruktur einzufügen, die Best Practices einhält und unser Projekt erheblich beschleunigt.

## Projekt erstellen {#create}

Es gibt mehrere Optionen zum Erstellen eines Maven-Multi-Modul-Projekts für AEM. In diesem Tutorial wird die [Maven AEM Projektarchetyp **26**](https://github.com/adobe/aem-project-archetype). Cloud Manager auch [stellt einen UI-Assistenten bereit](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html) , um die Erstellung eines AEM Anwendungsprojekts zu starten. Das zugrunde liegende Projekt, das von der Cloud Manager-Benutzeroberfläche generiert wurde, weist dieselbe Struktur auf wie die direkte Verwendung des Archetyps.

>[!NOTE]
>
>Dieses Tutorial verwendet Version **26** des Archetyps. Es empfiehlt sich immer, die **latest** Version des Archetyps, um ein neues Projekt zu generieren.

Die nächsten Schritte werden mit einem UNIX-basierten Befehlszeilenterminal durchgeführt, sollten jedoch bei Verwendung eines Windows-Terminals ähnlich sein.

1. Öffnen Sie ein Befehlszeilen-Terminal. Stellen Sie sicher, dass Maven installiert ist:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Stellen Sie sicher, dass **adobe-public** profile ist aktiv, indem der folgende Befehl ausgeführt wird:

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

   Wenn Sie **not** sehen Sie die **adobe-public** Dies ist ein Hinweis darauf, dass das Adobe-Repository in Ihrer `~/.m2/settings.xml` -Datei. Lesen Sie die Schritte zur Installation und Konfiguration von Apache Maven in [eine lokale Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Navigieren Sie zu einem Ordner, in dem Sie das AEM Projekt generieren möchten. Dies kann ein beliebiger Ordner sein, in dem Sie den Quellcode Ihres Projekts verwalten möchten. Beispiel: ein Verzeichnis mit dem Namen `code` unter dem Basisverzeichnis des Benutzers:

   ```shell
   $ cd ~/code
   ```

1. Fügen Sie Folgendes in die Befehlszeile ein, um [Projekt im Batch-Modus generieren](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

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
   > Wenn AEM 6.5.5 oder höher ausgewählt ist, ersetzen Sie `aemVersion="cloud"` mit `aemVersion="6.5.5"`. Verwenden Sie bei der Zielgruppenbestimmung ab Version 6.4.8 `aemVersion="6.4.8"`.

   Eine vollständige Liste der verfügbaren Eigenschaften zum Konfigurieren eines Projekts [finden Sie hier .](https://github.com/adobe/aem-project-archetype#available-properties).

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

1. Stellen Sie sicher, dass eine Autoreninstanz AEM lokal am Port ausgeführt wird. **4502**.
1. Navigieren Sie in der Befehlszeile zum `aem-guides-wknd` Projektverzeichnis.

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

   Das Maven-Profil `autoInstallSinglePackage` kompiliert die einzelnen Module des Projekts und stellt ein einzelnes Paket in der AEM-Instanz bereit. Standardmäßig wird dieses Paket in einer AEM-Instanz bereitgestellt, die lokal am Port ausgeführt wird **4502** und mit den Anmeldeinformationen von `admin:admin`.

1. Navigieren Sie in Ihrer lokalen AEM-Instanz zu Package Manager : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten Pakete für `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`und `aem-guides-wknd.all`.

1. Navigieren Sie zur Sites-Konsole: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Die WKND-Site wird eine der Sites sein. Es wird eine Site-Struktur mit einer US- und Sprach-Master-Hierarchie enthalten. Diese Site-Hierarchie basiert auf den Werten für `language_country` und `isSingleCountryWebsite` beim Generieren des Projekts mithilfe des Archetyps.

1. Öffnen Sie die **USA** `>` **englisch** Seite durch Auswahl der Seite und Klicken auf **Bearbeiten** in der Menüleiste:

   ![Site-Konsole](assets/project-setup/aem-sites-console.png)

1. Starterinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Im nächsten Kapitel lernen Sie die Grundlagen einer Komponente kennen.

   ![Starter-Inhalt](assets/project-setup/start-home-page.png)

   *Vom Archetyp generierter Beispielinhalt*

## Inspect des Projekts {#project-structure}

Das generierte AEM Projekt besteht aus einzelnen Maven-Modulen mit jeweils einer anderen Rolle. Dieses Tutorial und ein Großteil der Entwicklung konzentrieren sich auf diese Module:

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java-Code, in erster Linie Back-End-Entwickler.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=de) - Enthält Quellcode für CSS, JavaScript, Sass, Type Script, hauptsächlich für Frontend-Entwickler.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) - Enthält Komponenten- und Dialogfelddefinitionen, bettet kompilierte CSS- und JavaScript-Dateien als Client-Bibliotheken ein.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) - enthält Strukturinhalte und Konfigurationen wie bearbeitbare Vorlagen, Metadatenschemata (/content, /conf).

* **all** - Dies ist ein leeres Maven-Modul, das die oben genannten Module zu einem einzigen Paket kombiniert, das in einer AEM Umgebung bereitgestellt werden kann.

![Maven-Projektdiagramm](assets/project-setup/project-pom-structure.png)

Siehe [Dokumentation zum AEM Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) Weitere Informationen finden Sie unter **all** Maven-Module.

### Aufnahme von Kernkomponenten {#core-components}

[AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de) sind eine Reihe standardisierter WCM-Komponenten (Web Content Management) für AEM. Diese Komponenten bieten einen Grundsatz an Funktionen und sind für die Formatierung, Anpassung und Erweiterung einzelner Projekte ausgelegt.

AEM as a Cloud Service Umgebungen enthalten die neueste Version von [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Daher erledigen Projekte, die AEM as a Cloud Service Projekte erstellt wurden **not** enthalten eine Einbettung AEM Kernkomponenten.

Für AEM 6.5/6.4 generierten Projekte wird der Archetyp automatisch eingebettet [AEM Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) im Projekt. Es empfiehlt sich, AEM 6.5/6.4 AEM Kernkomponenten einzubetten, um sicherzustellen, dass die neueste Version mit Ihrem Projekt bereitgestellt wird. Weitere Informationen zur Verwendung von Kernkomponenten [im Projekt enthalten sind, finden Sie hier .](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Quellcodeverwaltung {#source-control}

Es empfiehlt sich immer, eine Form der Quell-Code-Verwaltung in Ihrer Anwendung zu verwenden. In diesem Tutorial werden Git und GitHub verwendet. Es gibt mehrere Dateien, die von Maven und/oder der IDE Ihrer Wahl generiert werden und die vom SCM ignoriert werden sollten.

Maven erstellt bei jeder Erstellung und Installation des Code-Pakets einen Zielordner. Der Zielordner und die Inhalte sollten von SCM ausgeschlossen werden.

darunter `ui.apps` feststellen, dass viele `.content.xml` -Dateien erstellt werden. Diese XML-Dateien ordnen die Knotentypen und Eigenschaften des im JCR installierten Inhalts zu. Diese Dateien sind wichtig und sollten **not** ignoriert werden.

Der AEM Projektarchetyp generiert ein Beispiel `.gitignore` -Datei, die als Ausgangspunkt verwendet werden kann, für den Dateien sicher ignoriert werden können. Die Datei wird generiert unter `<src>/aem-guides-wknd/.gitignore`.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihr erstes AEM Projekt erstellt!

### Nächste Schritte {#next-steps}

Machen Sie sich mit der zugrunde liegenden Technologie einer Adobe Experience Manager (AEM) Sites-Komponente durch eine einfache `HelloWorld` Beispiel mit der [Komponentengrundlagen](component-basics.md) Tutorial.

## Erweiterte Maven-Befehle (Bonus) {#advanced-maven-commands}

Während der Entwicklung können Sie nur mit einem der Module arbeiten und vermeiden, das gesamte Projekt zu erstellen, um Zeit zu sparen. Sie können auch eine direkte Bereitstellung auf einer AEM-Veröffentlichungsinstanz oder möglicherweise auf einer Instanz von AEM durchführen, die nicht auf Port 4502 ausgeführt wird.

Als Nächstes werden wir uns einige zusätzliche Maven-Profile und -Befehle ansehen, die Sie für mehr Flexibilität bei der Entwicklung verwenden können.

### Kernmodul {#core-module}

Die **[core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** -Modul enthält den gesamten Java-Code, der mit dem Projekt verknüpft ist. Nach der Erstellung wird ein OSGi-Bundle für die AEM bereitgestellt. So erstellen Sie nur dieses Modul:

1. Navigieren Sie zur `core` Ordner (unter `aem-guides-wknd`):

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

1. Navigieren Sie zu [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Dies ist die OSGi-Web-Konsole und enthält Informationen zu allen Bundles, die auf der AEM-Instanz installiert sind.

1. Umschalten zwischen **ID** Sortierungsspalte, sollte das WKND-Bundle installiert und aktiv sein.

   ![Kernpaket](assets/project-setup/wknd-osgi-console.png)

1. Sie können die &quot;physische&quot;Position der JAR-Datei in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![CRXDE-Speicherort von JAR](assets/project-setup/jcr-bundle-location.png)

### Ui.apps- und Ui.content-Module {#apps-content-module}

Die **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** Das Maven-Modul enthält den gesamten Rendercode, der für die unten stehende Site benötigt wird. `/apps`. Dazu gehört auch CSS/JS, das in einem AEM namens [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). Dazu gehören auch [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=de) Skripte zum Rendern von dynamischem HTML. Sie können sich die **ui.apps** -Modul als Zuordnung zur Struktur im JCR, jedoch in einem Format, das in einem Dateisystem gespeichert und an die Quell-Code-Verwaltung übertragen werden kann. Die **ui.apps** -Modul enthält nur Code.

So erstellen Sie das Modul nur dieses:

1. In der Befehlszeile. Navigieren Sie zur `ui.apps` Ordner (unter `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Führen Sie den folgenden Befehl aus:

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

1. Navigieren Sie zu [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Sie sollten die `ui.apps` -Paket als erstes installiertes Paket und es sollte einen aktuelleren Zeitstempel als jedes andere Paket haben.

   ![installiertes Ui.apps-Paket](assets/project-setup/ui-apps-package.png)

1. Kehren Sie zur Befehlszeile zurück und führen Sie den folgenden Befehl aus (innerhalb der `ui.apps` Ordner):

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

   Das Profil `autoInstallPackagePublish` ist dazu bestimmt, das Paket in einer Veröffentlichungsumgebung bereitzustellen, die auf dem Port ausgeführt wird. **4503**. Der obige Fehler wird erwartet, wenn eine AEM Instanz, die auf http://localhost:4503 ausgeführt wird, nicht gefunden werden kann.

1. Führen Sie abschließend den folgenden Befehl aus, um die `ui.apps` Package on Port **4504**:

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

   Auch hier wird ein Build-Fehler erwartet, wenn keine AEM Instanz auf dem Port ausgeführt wird **4504** ist verfügbar. Der Parameter `aem.port` wird in der POM-Datei unter `aem-guides-wknd/pom.xml`.

Die **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** -Modul auf die gleiche Weise strukturiert wie das **ui.apps** -Modul. Der einzige Unterschied besteht darin, dass die **ui.content** enthält das so genannte **veränderlich** Inhalt. **Veränderlich** Inhalt bezieht sich im Wesentlichen auf Nicht-Code-Konfigurationen wie Vorlagen, Richtlinien oder Ordnerstrukturen, die in der Quell-Code-Verwaltung gespeichert sind **but** kann direkt in einer AEM-Instanz geändert werden. Dies wird im Kapitel über Seiten und Vorlagen ausführlicher untersucht.

Dieselben Maven-Befehle, die zum Erstellen der **ui.apps** -Modul zum Erstellen der **ui.content** -Modul. Sie können die oben genannten Schritte innerhalb der **ui.content** Ordner.
