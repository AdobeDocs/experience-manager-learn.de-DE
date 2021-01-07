---
title: Einrichten von Entwicklungstools für AEM als Cloud Service-Entwicklung
description: Richten Sie eine lokale Entwicklungsmaschine mit allen grundlegenden Werkzeugen ein, die zur Entwicklung gegen AEM lokal erforderlich sind.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: debf13d8e376979548bcbf55f71661d8cb8eb823
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 2%

---


# Entwicklungstools einrichten

Für die Entwicklung von Adobe Experience Manager (AEM) ist ein minimaler Satz von Entwicklungs-Tools erforderlich, die auf dem Entwicklercomputer installiert und eingerichtet werden müssen. Diese Instrumente unterstützen die Entwicklung und den Aufbau AEM Projekte.

Beachten Sie, dass `~` als Kurzbezeichnung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

## Java installieren

Experience Manager ist eine Java-Anwendung und erfordert daher das Java SDK, um die Entwicklung und die AEM als Cloud Service-SDK zu unterstützen.

1. [Laden Sie das neueste Java 11 SDK herunter und installieren Sie es](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fdc jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=Liste&amp;p.offset=0&amp;p.limit=14)
1. Stellen Sie sicher, dass Java 11 SDK installiert ist, indem Sie den Befehl ausführen:
   + Windows: `java -version`
   + macOS/Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Homebrew installieren

_Die Verwendung von Homebrew ist optional, wird jedoch empfohlen._

Homebrew ist ein Open-Source-Paketmanager für macOS, Windows und Linux. Alle unterstützenden Tools können separat installiert werden. Homebrew bietet eine komfortable Möglichkeit, eine Vielzahl von Entwicklungstools zu installieren und zu aktualisieren, die für die Entwicklung von Experience Managern erforderlich sind.

1. Terminal öffnen
1. Überprüfen Sie, ob Homebrew bereits installiert ist, indem Sie den Befehl ausführen: `brew --version`.
1. Wenn Homebrew nicht installiert ist, installieren Sie Homebrew
   + [Homebrew auf macOS installieren](https://brew.sh/)
      + Für Homebrew unter macOS ist [Xcode](https://apps.apple.com/us/app/xcode/id497799835) oder [Befehlszeilenwerkzeuge](https://developer.apple.com/download/more/) erforderlich, kann über folgenden Befehl installiert werden:
         + `xcode-select --install`
   + [Installieren von Homebrew unter Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Installieren von Homebrew unter Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Überprüfen Sie, ob Homebrew installiert ist, indem Sie den Befehl ausführen: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Wenn Sie Homebrew verwenden, befolgen Sie die Anweisungen unter __Installation mit Homebrew__ in den folgenden Abschnitten. Wenn Sie mit Homebrew __nicht__ arbeiten, installieren Sie die Werkzeuge unter den OS-spezifischen Links.

## Git installieren

[Geben Sie ](https://git-scm.com/) das Quellcodeverwaltungssystem an, das von  [Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html) verwendet wird und ist daher für die Entwicklung erforderlich.

+ Installieren von Git mithilfe von Homebrew
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Führen Sie den Befehl aus: `brew install git`
   1. Überprüfen Sie mithilfe des Befehls, ob Git installiert ist: `git --version`
+ Oder laden Sie Git (macOS, Linux oder Windows) herunter und installieren Sie es.
   1. [Herunterladen und Installieren von Git](https://git-scm.com/downloads)
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Überprüfen Sie mithilfe des Befehls, ob Git installiert ist: `git --version`

![Git](./assets/development-tools/git.png)

## Node.js (und npm){#node-js} installieren

[Node.](https://nodejs.org) jsis eine JavaScript-Laufzeitumgebung, die zum Arbeiten mit den Front-End-Assets des  __ui.__ frontendsub-Projekts eines AEM Projekts verwendet wird. Node.js wird mit [npm](https://www.npmjs.com/) verteilt, ist der defacto-Paketmanager Node.js, der zum Verwalten von JavaScript-Abhängigkeiten verwendet wird.

+ Installieren von Node.js mithilfe von Homebrew
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Führen Sie den Befehl aus: `brew install node`
   1. Überprüfen Sie mithilfe des Befehls, ob Node.js installiert ist: `node -v`
   1. Überprüfen Sie mithilfe des Befehls, ob npm installiert ist: `npm -v`
+ Oder laden Sie Node.js (macOS, Linux oder Windows) herunter und installieren Sie es.
   1. [Node.js herunterladen und installieren](https://nodejs.org/en/download/)
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Überprüfen Sie mithilfe des Befehls, ob Node.js installiert ist: `node -v`
   1. Überprüfen Sie mithilfe des Befehls, ob npm installiert ist: `npm -v`

![Node.js und npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Projekt Archetype](https://github.com/adobe/aem-project-archetype)-basierte AEM Projekte installieren eine isolierte Version von Node.js zur Buildzeit. Es ist gut, die Version des lokalen Entwicklungssystems synchron (oder nahe an) mit den Node.js- und NPM-Versionen zu halten, die in der Reactor pom.xml Ihres AEM Maven-Projekts angegeben sind.
>
>In diesem Beispiel [AEM Projektreaktor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) finden Sie Informationen zum Speicherort der Build-Versionen von Node.js und npm.

## Maven installieren

Apache Maven ist das Open-Source-Java-Befehlszeilenwerkzeug zum Erstellen AEM Projekte, die aus dem AEM Projekt Maven Archetype generiert wurden. Alle wichtigen IDEs ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio-Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/) usw.) integrierte Unterstützung von Maven.

+ Maven mithilfe von Homebrew installieren
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Führen Sie den Befehl aus: `brew install maven`
   1. Überprüfen Sie mithilfe des Befehls, ob Maven installiert ist: `mvn -v`
+ Oder laden Sie Maven (macOS, Linux oder Windows) herunter und installieren Sie es.
   1. [Maven herunterladen](https://maven.apache.org/download.cgi)
   1. [Maven installieren](https://maven.apache.org/install.html)
   1. Öffnen Sie das Terminal/die Eingabeaufforderung
   1. Überprüfen Sie mithilfe des Befehls, ob Maven installiert ist: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Adobe I/O CLI einrichten{#aio-cli}

Die [Adobe I/O CLI](https://github.com/adobe/aio-cli) oder `aio` bietet Befehlszeilenzugriff auf eine Vielzahl von Adoben-Diensten, einschließlich [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) und [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Die Adobe I/O CLI spielt eine wesentliche Rolle bei der Entwicklung AEM als Cloud Service, da sie Entwicklern Folgendes ermöglicht:

+ Datenprotokolle von AEM als Cloud Services-Services
+ Verwalten von Cloud Manager-Pipelines über die CLI

### Adobe I/O CLI installieren

1. Vergewissern Sie sich, dass [Node.js installiert ist, da die Adobe I/O-CLI ein npm-Modul ist.](#node-js)
   + Führen Sie `node --version` zur Bestätigung aus
1. Führen Sie `npm install -g @adobe/aio-cli` aus, um das `aio` npm-Modul global zu installieren.

### Einrichten des Adobe I/O CLI Cloud Manager-Plugins{#aio-cloud-manager}

Mit dem Adobe I/O Cloud Manager-Plugin kann die AIO-CLI über den Befehl `aio cloudmanager` mit Adobe Cloud Manager interagieren.

1. Führen Sie `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` aus, um das [AIO Cloud Manager-Plugin](https://github.com/adobe/aio-cli-plugin-cloudmanager) zu installieren.

### Einrichten des Adobe I/O CLI Asset compute-Plugins{#aio-asset-compute}

Mit dem Adobe I/O Cloud Manager-Plugin kann die AIO-CLI Asset compute-Mitarbeiter über den Befehl `aio asset-compute` generieren und ausführen.

1. Führen Sie `aio plugins:install @adobe/aio-cli-plugin-asset-compute` aus, um das [aio-Asset compute-Plug-in](https://github.com/adobe/aio-cli-plugin-asset-compute) zu installieren.

### Einrichten der Adobe I/O CLI-Authentifizierung

Damit die Adobe I/O-CLI mit Cloud Manager kommunizieren kann, muss in der Adobe I/O Console eine Cloud Manager-Integration erstellt werden. Damit die Authentifizierung erfolgreich durchgeführt werden kann, müssen Anmeldeinformationen eingeholt werden.

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. Melden Sie sich bei [console.adobe.io](https://console.adobe.io) an
1. Stellen Sie sicher, dass Ihr Unternehmen, das das Cloud Manager-Produkt enthält, mit dem eine Verbindung hergestellt werden soll, im Adobe Org Switcher aktiv ist.
1. Neues [Adobe I/O-Programm ](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) erstellen oder öffnen
   + Adobe I/O Console Programme sind einfach organisatorische Integrationsgruppierungen, die Erstellung oder Verwendung und das vorhandene Programm, je nach Verwaltung Ihrer Integrationen
   + Wenn Sie ein neues Projekt erstellen, wählen Sie bei entsprechender Aufforderung &quot;Leeres Projekt&quot;(im Gegensatz zu &quot;Aus Vorlage erstellen&quot;)
   + Adobe I/O Console-Programm unterscheiden sich von Cloud Manager-Programmen
1. Neue Cloud Manager-API-Integration mit dem Profil &quot;Developer - Cloud Service&quot; erstellen
1. Die Anmeldeinformationen für das Dienstkonto (JWT) müssen mit der Adobe I/O-CLI [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) gefüllt werden
1. Laden Sie die Datei `config.json` in die Adobe I/O-CLI
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. Laden Sie die Datei `private.key` in die Adobe I/O-CLI
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Starten Sie die Ausführung von Befehlen[ für Cloud Manager über die Adobe I/O-Befehlszeilenschnittstelle.](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)

## Einrichten der Entwicklungs-IDE

AEM Entwicklung besteht in erster Linie aus der Java- und Front-End-Entwicklung (JavaScript, CSS usw.) und dem XML-Management. Die folgenden IDEs sind die beliebtesten für AEM Entwicklung.

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEAist eine leistungsstarke IDE für Java-Entwicklung. IntelliJ IDEA ist in zwei Geschmacksrichtungen erhältlich: eine kostenlose Community Edition und eine kommerzielle (bezahlte) Ultimate Version. Die kostenlose Community-Version ist ausreichend für AEM Entwicklung, aber die Ultimate [erweitert ihren Funktionssatz](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [IntelliJ-IDEA herunterladen](https://www.jetbrains.com/idea/download)
+ [Repo-Tool herunterladen](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio-Code

__[Visual Studio Code](https://code.visualstudio.com/)__  (VS Code) ist ein kostenloses Open-Source-Tool für Front-End-Entwickler. Visual Studio-Code kann so eingerichtet werden, dass die Inhaltssynchronisierung mit AEM mithilfe eines Adobe-Tools, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__, integriert wird.

Visual Studio-Code ist die ideale Wahl für Front-End-Entwickler, die in erster Linie Front-End-Code erstellen. JavaScript, CSS und HTML. Während VS-Code Java-Unterstützung über [Erweiterungen](https://code.visualstudio.com/docs/java/java-tutorial) besitzt, fehlen möglicherweise einige der erweiterten Funktionen, die Java-spezifisch bereitgestellt werden.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio-Code herunterladen](https://code.visualstudio.com/Download)
+ [Repo-Tool herunterladen](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Herunterladen der Erweiterung &quot;VS-Code&quot;](https://aemfed.io/)
+ [Download AEM Synchronisierungs-VS-Code-Erweiterung](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDEs ist eine beliebte IDEs für die Java-Entwicklung und unterstützt das   __[AEM Developer ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsplug-in von Adobe, das eine In-IDE-Anleitung zum Authoring und zum Synchronisieren von JCR-Inhalten mit einer lokalen AEM-Instanz bietet.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse herunterladen](https://www.eclipse.org/ide/)
+ [Eclipse Dev Tools herunterladen](https://eclipse.adobe.com/aem/dev-tools/)
