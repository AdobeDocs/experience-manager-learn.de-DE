---
title: Einrichten der Entwicklungs-Tools für die AEM as a Cloud Service-Entwicklung
description: Richten Sie einen lokalen Entwicklungs-Computer mit allen Grundlinien-Tools ein, die für die lokale Entwicklung mit AEM erforderlich sind.
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3592
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 100%

---

# Einrichten von Entwicklungs-Tools {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Einrichten von Entwicklungs-Tools"
>abstract="Für Entwicklungsaufgaben in Adobe Experience Manager (AEM) müssen nur wenige Entwicklungs-Tools auf dem Computer der Entwicklungsperson installiert und eingerichtet werden. Zu diesen Tools gehören u. a. Java, Maven, Adobe I/O CLI und eine Entwicklungsumgebung (IDE)."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=de" text="Entwicklungsrichtlinien"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=de" text="Entwicklungsgrundlagen"

Für Entwicklungsaufgaben in Adobe Experience Manager (AEM) müssen nur wenige Entwicklungs-Tools auf dem Computer des Entwicklers bzw. der Entwicklerin installiert und eingerichtet werden. Diese Tools die Entwicklung und den Aufbau von AEM.

Beachten Sie, dass `~` als Abkürzung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

## Installieren von Java

Experience Manager ist eine Java-Applikation und erfordert daher das Java-SDK, um die Entwicklung und das AEM as a Cloud Service-SDK zu unterstützen.

1. [Laden Sie die neueste Java SDK 11-Version herunter und installieren Sie sie](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Prüfen Sie, ob das Java 11 SDK installiert ist, indem Sie den folgenden Befehl ausführen:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Installation von Homebrew

_Die Verwendung von Homebrew ist optional, wird jedoch empfohlen._

Homebrew ist ein Open-Source-Package Manager für macOS, Windows und Linux. Alle unterstützenden Tools können separat installiert werden. Homebrew bietet eine praktische Möglichkeit, eine Vielzahl von Entwicklungs-Tools zu installieren und zu aktualisieren, die für die Entwicklung von Experience Manager erforderlich sind.

1. Öffnen Sie Ihr Terminal
1. Überprüfen Sie, ob Homebrew bereits installiert ist, indem Sie den folgenden Befehl ausführen: `brew --version`
1. Wenn Homebrew nicht installiert ist, installieren Sie Homebrew

>[!BEGINTABS]

>[!TAB macOS]

[](https://brew.sh/)Homebrew auf macOS erfordert [Xcode](https://apps.apple.com/us/app/xcode/id497799835) oder [Command Line Tools](https://developer.apple.com/download/more/), die über den folgenden Befehl installiert werden können:

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Installieren von Homebrew unter Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Installieren von Homebrew unter Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Stellen Sie sicher, dass Homebrew installiert ist, indem Sie den folgenden Befehl ausführen: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Wenn Sie Homebrew verwenden, folgen Sie den Anweisungen __Installation mit Homebrew__ in den folgenden Abschnitten. Wenn Sie Homebrew __nicht__ verwenden, installieren Sie die Tools über die betriebssystemspezifischen Links.

## Installieren von Git

[Git](https://git-scm.com/) ist das Versionsverwaltungssystem, das von [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=de) verwendet wird, und ist daher für die Entwicklung erforderlich.

>[!BEGINTABS]

>[!TAB Installieren von Git mithilfe von Homebrew]

1. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
1. Führen Sie den folgenden Befehl aus: `$ brew install git`
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass Git installiert ist: `$ git --version`

>[!TAB Herunterladen und Installieren von Git]

1. [Git herunterladen und installieren](https://git-scm.com/downloads)
1. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass Git installiert ist: `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Installieren von Node.js (und npm){#node-js}

[Node.js](https://nodejs.org) ist eine JavaScript-Laufzeitumgebung, die für die Arbeit mit den Frontend-Assets des Unterprojekts __ui.frontend__ eines AEM-Projekts verwendet wird. Node.js wird mit [npm](https://www.npmjs.com/) verteilt, dem defacto Package Manager von Node.js, der verwendet wird, um JavaScript-Abhängigkeiten zu verwalten.

>[!BEGINTABS]

>[!TAB Installieren von Node.js mithilfe von Homebrew]

1. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
1. Führen Sie den folgenden Befehl aus: `$ brew install node`
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass Node.js installiert ist: `$ node -v`
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass npm installiert ist: `$ npm -v`

>[!TAB Herunterladen und Installieren von Node.js]

1. [Node.js herunterladen und installieren](https://nodejs.org/de/download/)
2. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
3. Stellen Sie mithilfe des folgenden Befehls sicher, dass Node.js installiert ist: `$ node -v`
4. Stellen Sie mithilfe des folgenden Befehls sicher, dass npm installiert ist: `$ npm -v`

>[!ENDTABS]

![Node.js und npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype)-basierte AEM-Projekte installieren eine isolierte Version von Node.js zum Zeitpunkt der Erstellung. Es ist gut, die Version des lokalen Entwicklungssystems mit den Node.js- und npm-Versionen, die in der Reactor pom.xml Ihres AEM Maven-Projekts angegeben sind, zu synchronisieren (oder nahe daran zu halten).
>
>Im folgenden Beispiel [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) finden Sie weitere Informationen, wo die Node.js- und npm-Build-Versionen zu finden sind.

## Installieren von Maven

Apache Maven ist das Open-Source-Java-Befehlszeilenwerkzeug, mit dem AEM-Projekte erstellt werden, die aus dem AEM-Projekt-Maven-Archetyp generiert wurden. Alle gängigen IDEs ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), usw.) haben integrierte Maven-Unterstützung.


>[!BEGINTABS]

>[!TAB Installieren von Maven mithilfe von Homebrew]

1. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
1. Führen Sie den folgenden Befehl aus: `$ brew install maven`
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass Maven installiert ist: `$ mvn -v`

>[!TAB Herunterladen und Installieren von Maven]

1. [Maven herunterladen](https://maven.apache.org/download.cgi)
1. [Maven installieren](https://maven.apache.org/install.html)
1. Öffnen Sie Ihr Terminal/Ihre Eingabeaufforderung
1. Stellen Sie mithilfe des folgenden Befehls sicher, dass Maven installiert ist: `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Einrichten der Adobe I/O-CLI{#aio-cli}

Die [Adobe I/O-CLI](https://github.com/adobe/aio-cli), oder `aio`, bietet Befehlszeilenzugriff auf eine Vielzahl von Adobe-Diensten, einschließlich [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) und [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Die Adobe I/O-CLI spielt eine wesentliche Rolle bei der Entwicklung auf AEM as a Cloud Service, da sie Entwicklerinnen und Entwicklern Folgendes ermöglicht:

+ Protokollverfolgung von AEM as a Cloud Services
+ Verwalten von Cloud Manager-Pipelines über die CLI
+ Bereitstellen in [Schnelle Entwicklungsumgebungen von AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=de)

### Installieren der Adobe I/O-CLI

1. Stellen Sie sicher, dass [Node.js installiert ist](#node-js), da die Adobe I/O-CLI ein npm-Modul ist
   + Führen Sie `node --version` zur Bestätigung aus
1. Führen Sie `npm install -g @adobe/aio-cli` aus, um das npm-Modul `aio` global zu installieren

### Einrichten des Cloud Manager-Plug-ins der Adobe I/O-CLI{#aio-cloud-manager}

Das Cloud Manager-Plug-in von Adobe I/O ermöglicht der AIO-CLI die Interaktion mit Adobe Cloud Manager über den Befehl `aio cloudmanager`.

1. Führen Sie `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` aus, um das [AIO-Cloud Manager Plug-in](https://github.com/adobe/aio-cli-plugin-cloudmanager) zu installieren.

#### Einrichten der Adobe I/O CLI-Authentifizierung

Damit Adobe I/O-CLI mit Cloud Manager kommunizieren kann, muss eine [Cloud Manager-Integration in der Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager) erstellt werden, und es müssen Anmeldeinformationen zur erfolgreichen Authentifizierung eingeholt werden.

1. Melden Sie sich bei [console.adobe.io](https://console.adobe.io) an.
1. Stellen Sie sicher, dass Ihr Unternehmen, das das Cloud Manager-Produkt enthält, mit dem eine Verbindung hergestellt werden soll, im Adobe-Organisations-Umschalter aktiv ist.
1. Erstellen Sie ein neues oder öffnen Sie ein bestehendes [Adobe I/O-Programm](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O Console-Programme sind einfach organisatorische Gruppierungen von Integrationen. Erstellen Sie ein Programm oder verwenden Sie ein vorhandenes Programm, je nachdem, wie Sie Ihre Integrationen verwalten möchten.
   + Wenn Sie ein neues Projekt erstellen, wählen Sie bei entsprechender Aufforderung „Leeres Projekt“ aus (vs. „Aus Vorlage erstellen“)
   + Adobe I/O Console-Programme unterscheiden sich von Cloud Manager-Programmen
1. Erstellen Sie eine neue Cloud Manager-API-Integration mit dem Profil „Entwickler – Cloud Service“.
1. Rufen Sie die Anmeldeinformationen für das Dienstkonto (JWT) ab, die zum Auffüllen der Datei [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) der Adobe I/O-CLI erforderlich sind.
1. Laden Sie die `config.json`-Datei in die Adobe I/O-CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Laden Sie die `private.key`-Datei in die Adobe I/O-CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Beginnen Sie mit der [Ausführung von Befehlen](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) für Cloud Manager über die Adobe I/O-CLI.

### Einrichten des Plug-ins der schnellen Entwicklungsumgebung von AEM{#rde}

Das Plug-in der schnellen Entwicklungsumgebung von AEM ermöglicht der AIO-CLI die Interaktion mit den [schnellen Entwicklungsumgebungen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=de) von AEM as a Cloud Service über den Befehl `aio aem:rde`.

1. Führen Sie `aio plugins:install @adobe/aio-cli-plugin-aem-rde` aus, um das [AEM-Plug-in für schnelle Entwicklungsumgebungen](https://github.com/adobe/aio-cli-plugin-aem-rde) zu installieren.

### Einrichten des Asset Compute-Plug-ins für Adobe I/O-CLI{#aio-asset-compute}

Mit dem Adobe I/O Cloud Manager-Plug-in kann die AIO-CLI Asset Compute-Sekundäre über den Befehl `aio asset-compute` generieren und ausführen.

1. Führen Sie `aio plugins:install @adobe/aio-cli-plugin-asset-compute` aus, um das [AIO-Asset Compute-Plug-in](https://github.com/adobe/aio-cli-plugin-asset-compute) zu installieren.

## Einrichten der Entwicklungs-IDE

Die AEM-Entwicklung besteht in erster Linie aus der Java- und Frontend-Entwicklung (JavaScript, CSS usw.) sowie der XML-Verwaltung. Im Folgenden finden Sie die beliebtesten IDEs für die AEM-Entwicklung.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ ist eine leistungsstarke IDE für die Java-Entwicklung. IntelliJ IDEA gibt es in zwei Varianten, einer kostenlosen Community-Edition und einer kommerziellen (kostenpflichtigen) Ultimate-Version. Die kostenlose Community-Version ist für die AEM-Entwicklung ausreichend, die Ultimate-Version [erweitert jedoch den Funktionsumfang](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [IntelliJ IDEA herunterladen](https://www.jetbrains.com/idea/download)
+ [Repo-Tool herunterladen](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio-Code](https://code.visualstudio.com/)__ (VS Code) ist ein kostenloses Open-Source-Tool für Frontend-Entwicklerinnen und -Entwickler. Visual Studio Code kann mit Hilfe eines Adobe-Tools __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__ für die Integration der Inhaltssynchronisierung mit AEM eingerichtet werden.

Visual Studio Code ist die ideale Wahl für Frontend-Entwicklerinnen und -Entwickler, die hauptsächlich Frontend-Code erstellen: JavaScript, CSS und HTML. VS Code bietet zwar Java-Unterstützung über [Erweiterungen](https://code.visualstudio.com/docs/java/java-tutorial), doch fehlen ihm möglicherweise einige der fortgeschrittenen Funktionen, die von Java-spezifischen Programmen bereitgestellt werden.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Visual Studio Code herunterladen](https://code.visualstudio.com/Download)
+ [Repo-Tool herunterladen](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Eingebettete VS Code-Erweiterung herunterladen](https://aemfed.io/)
+ [AEM Sync VS Code-Erweiterung herunterladen](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ ist eine beliebte IDE für die Java-Entwicklung und unterstützt das von Adobe bereitgestellte __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=de)__-Plug-in, das eine IDE-GUI für das Authoring und die Synchronisierung von JCR-Inhalten mit einer lokalen AEM-Instanz bietet.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Eclipse herunterladen](https://www.eclipse.org/ide/)
+ [Eclipse Dev Tools herunterladen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=de)
