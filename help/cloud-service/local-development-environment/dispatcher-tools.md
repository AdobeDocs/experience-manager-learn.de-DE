---
title: Einrichten der Dispatcher Tools für die AEM als Cloud Service-Entwicklung
description: AEM Dispatcher Tools des SDK erleichtern die lokale Entwicklung von Adobe Experience Manager-Projekten (AEM), indem sie die lokale Installation, Ausführung und Fehlerbehebung von Dispatcher erleichtern.
sub-product: foundation
feature: Dispatcher, Entwicklertools
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1639'
ht-degree: 2%

---


# Einrichten lokaler Dispatcher Tools

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale Dispatcher-Tools"
>abstract="Der Dispatcher ist ein integraler Bestandteil der gesamten Experience Manager-Architektur und sollte Teil der lokalen Entwicklungseinrichtung sein. Das AEM as a Cloud Service SDK enthält die empfohlene Version der Dispatcher Tools, die die lokale Konfiguration, Validierung und Simulation des Dispatchers erleichtert."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher in der Cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM als Cloud Service-SDK herunterladen"

Der Dispatcher von Adobe Experience Manager (AEM) ist ein Apache HTTP-Webservermodul, das eine Sicherheits- und Leistungsschicht zwischen der CDN- und der AEM-Veröffentlichungsstufe bietet. Der Dispatcher ist ein integraler Bestandteil der gesamten Experience Manager-Architektur und sollte Teil der lokalen Entwicklungseinrichtung sein.

Das AEM as a Cloud Service SDK enthält die empfohlene Version der Dispatcher Tools, die die lokale Konfiguration, Validierung und Simulation des Dispatchers erleichtert. Die Dispatcher Tools umfassen Folgendes:

+ einen Grundsatz von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, der sich unter `.../dispatcher-sdk-x.x.x/src` befindet
+ ein Konfigurations-Validator-CLI-Tool unter `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ ein CLI-Tool zur Konfigurationsgenerierung, das sich unter `.../dispatcher-sdk-x.x.x/bin/validator` befindet
+ ein CLI-Konfigurationsimplementierungs-Tool unter `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ ein Docker-Bild, das den Apache HTTP-Webserver mit dem Dispatcher-Modul ausführt

Beachten Sie, dass `~` als Kurzbezeichnung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

>[!NOTE]
>
> Die Videos auf dieser Seite wurden auf macOS aufgezeichnet. Windows-Benutzer können folgen, aber die entsprechenden Windows-Befehle für die Dispatcher Tools verwenden, die in jedem Video bereitgestellt werden.

## Voraussetzungen

1. Windows-Benutzer müssen Windows 10 Professional verwenden
1. Installieren Sie [Experience Manager Publish Quickstart Jar](./aem-runtime.md) auf dem lokalen Entwicklungscomputer.
   + Installieren Sie optional die neueste [AEM Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases) im lokalen AEM-Veröffentlichungsdienst. Diese Website wird in diesem Tutorial zur Visualisierung eines funktionierenden Dispatchers verwendet.
1. Installieren und starten Sie die neueste Version von [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) auf dem lokalen Entwicklungscomputer.

## Herunterladen der Dispatcher Tools (als Teil des AEM SDK)

Das AEM as a Cloud Service SDK oder AEM SDK enthält die Dispatcher Tools zum lokalen Ausführen des Apache HTTP-Webservers mit dem Dispatcher-Modul für die Entwicklung sowie das kompatible QuickStart-JAR.

Wenn das AEM as a Cloud Service-SDK bereits auf [heruntergeladen wurde, um die lokale AEM-Laufzeitumgebung](./aem-runtime.md) einzurichten, muss es nicht erneut heruntergeladen werden.

1. Melden Sie sich mit Ihrer Adobe ID bei [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) an.
   + Ihre Adobe-Organisation __muss__ für AEM als Cloud Service zum Herunterladen der AEM als Cloud Service-SDK bereitgestellt werden.
1. Klicken Sie auf die letzte Ergebniszeile __AEM SDK__, die heruntergeladen werden soll.
   + Stellen Sie sicher, dass die Dispatcher Tools ab Version 2.0.29 des AEM SDK in der Download-Beschreibung aufgeführt sind.

## Extrahieren der Dispatcher Tools aus der AEM SDK-ZIP-Datei

>[!TIP]
>
> Windows-Benutzer dürfen keine Leerzeichen oder Sonderzeichen im Pfad zum Ordner haben, der die lokalen Dispatcher-Tools enthält. Wenn im Pfad Leerzeichen vorhanden sind, schlägt `docker_run.cmd` fehl.

Die Version der Dispatcher Tools unterscheidet sich von der des AEM SDK. Stellen Sie sicher, dass die Version der Dispatcher Tools über die AEM SDK-Version bereitgestellt wird, die der AEM als Cloud Service-Version entspricht.

1. Dekomprimieren Sie die heruntergeladene Datei `aem-sdk-xxx.zip` .
1. Entpacken Sie die Dispatcher Tools in `~/aem-sdk/dispatcher`
   + Windows: Dekomprimieren Sie `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (erstellen Sie fehlende Ordner nach Bedarf).
   + macOS/Linux: Führen Sie das zugehörige Shell-Skript `aem-sdk-dispatcher-tools-x.x.x-unix.sh` aus, um die Dispatcher Tools zu entpacken.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Beachten Sie, dass alle unten angegebenen Befehle davon ausgehen, dass der aktuelle Arbeitsordner die erweiterten Inhalte der Dispatcher Tools enthält.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*In diesem Video wird macOS zu Veranschaulichungszwecken verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Grundlegendes zu den Dispatcher-Konfigurationsdateien

>[!TIP]
> Aus dem AEM [Projektarchetyp](https://github.com/adobe/aem-project-archetype) erstellte Experience Manager-Projekte werden mit diesem Satz von Dispatcher-Konfigurationsdateien vorausgefüllt, sodass keine Kopie vom Ordner &quot;Dispatcher Tools src&quot;erforderlich ist.

Die Dispatcher Tools bieten eine Reihe von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die das Verhalten für alle Umgebungen, einschließlich der lokalen Entwicklung, definieren.

Diese Dateien sollen in ein Experience Manager-Maven-Projekt in den Ordner `dispatcher/src` kopiert werden, sofern sie noch nicht im Experience Manager-Maven-Projekt vorhanden sind.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*In diesem Video wird macOS zu Veranschaulichungszwecken verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

Eine vollständige Beschreibung der Konfigurationsdateien ist in den entpackten Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html` verfügbar.

## Konfigurationen überprüfen

Optional können die Dispatcher- und Apache-Webserverkonfigurationen (über `httpd -t`) mithilfe des Skripts `validate` validiert werden (nicht zu verwechseln mit der ausführbaren Datei `validator`).

+ Nutzung:
   + Windows: `bin\validate src`
   + macOS/Linux: `./bin/validate.sh ./src`

## Dispatcher lokal ausführen

Um den Dispatcher lokal auszuführen, müssen die Dispatcher-Konfigurationsdateien mit dem CLI-Tool der Dispatcher Tools `validator` generiert werden.

+ Nutzung:
   + Windows: `bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

Mit diesem Befehl werden die Konfigurationen in einen Dateisatz übertragen, der mit dem Apache HTTP-Webserver des Docker-Containers kompatibel ist.

Nach der Erstellung werden die verschobenen Konfigurationen verwendet, um den Dispatcher lokal im Docker-Container auszuführen. Es ist wichtig sicherzustellen, dass die neuesten Konfigurationen mit der Ausgabe `validate` __und__ unter Verwendung der Option `-d` des Validators validiert wurden.

+ Nutzung:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host` kann auf `host.docker.internal` gesetzt werden, ein spezieller DNS-Name, den Docker im Container bereitstellt, der auf die IP-Adresse des Hostcomputers aufgelöst wird. Wenn `host.docker.internal` nicht aufgelöst wird, lesen Sie den Abschnitt [Fehlerbehebung](#troubleshooting-host-docker-internal) weiter unten.

Um beispielsweise den Dispatcher-Docker-Container mit den Standardkonfigurationsdateien zu starten, die von den Dispatcher Tools bereitgestellt werden:

1. Generieren Sie `deployment-folder` mit dem Namen `out` nach Konvention bei jeder Änderung einer Konfiguration von Grund auf neu:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Neu) Starten Sie den Dispatcher Docker-Container, der den Pfad zum Bereitstellungsordner angibt:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Der Veröffentlichungsdienst des AEM as a Cloud Service SDK, der lokal auf Port 4503 ausgeführt wird, ist über den Dispatcher unter `http://localhost:8080` verfügbar.

Um die Dispatcher Tools für die Dispatcher-Konfiguration eines Experience Manager-Projekts auszuführen, generieren Sie einfach den Ordner `deployment-folder` unter Verwendung des Ordners `dispatcher/src` des Projekts.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*In diesem Video wird macOS zu Veranschaulichungszwecken verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Dispatcher Tools-Protokolle

Dispatcher-Protokolle sind bei der lokalen Entwicklung hilfreich, um zu verstehen, ob und warum HTTP-Anforderungen blockiert werden. Die Protokollebene kann festgelegt werden, indem der Ausführung von `docker_run` Umgebungsparameter vorangestellt werden.

Dispatcher Tools-Protokolle werden bei Ausführung von `docker_run` an den Standard ausgegeben.

Nützliche Parameter zum Debugging des Dispatchers sind:

+ `DISP_LOG_LEVEL=Debug` Setzt die Protokollierung des Dispatcher-Moduls auf Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` setzt die Protokollierung des Apache HTTP-Webserver-Neuschreibungsmoduls auf Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `DISP_RUN_MODE` legt den &quot;Ausführungsmodus&quot;der Dispatcher-Umgebung fest und lädt die entsprechenden Ausführungsmodi Dispatcher-Konfigurationsdateien.
   + Standardwert ist `dev`
+ Gültige Werte: `dev`, `stage` oder `prod`

Ein oder mehrere Parameter können an `docker_run` übergeben werden

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS/Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*In diesem Video wird macOS zu Veranschaulichungszwecken verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

### Zugriff auf Protokolldateien

Der Zugriff auf Apache-Webserver und AEM Dispatcher-Protokolle erfolgt direkt im Docker-Container:

+ [Zugriff auf Protokolle im Docker-Container](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Kopieren der Docker-Protokolle in das lokale Dateisystem](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Aktualisieren der Dispatcher Tools{#dispatcher-tools-version}

Die Versionen der Dispatcher Tools werden weniger häufig erhöht als der Experience Manager. Daher erfordern die Dispatcher Tools weniger Updates in der lokalen Entwicklungsumgebung.

Die empfohlene Dispatcher Tools-Version ist diejenige, die im Lieferumfang des AEM als Cloud Service-SDK enthalten ist, das dem Experience Manager als Cloud Service-Version entspricht. Die Version von AEM as a Cloud Service finden Sie über [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Umgebungen__, pro durch das  __AEM__ Releasedatum angegebener Umgebung

![Experience Manager-Version](./assets/dispatcher-tools/aem-version.png)

_Beachten Sie, dass die Dispatcher Tools-Version selbst nicht mit der Experience Manager-Version übereinstimmt._

## Fehlerbehebung

### docker_run führt zu &#39;Warten, bis host.docker.internal verfügbar ist&#39; message{#troubleshooting-host-docker-internal}

`host.docker.internal` ist ein Hostname, der dem Docker-enthält bereitgestellt wird und zum Host aufgelöst wird. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Ab Docker 18.03 wird empfohlen, eine Verbindung zum speziellen DNS-Namen host.docker.internal herzustellen, der zu der vom Host verwendeten internen IP-Adresse aufgelöst wird

Wenn `bin/docker_run out host.docker.internal:4503 8080` die Meldung __Warten, bis host.docker.internal verfügbar ist__, dann:

1. Stellen Sie sicher, dass die installierte Version von Docker 18.03 oder höher ist.
2. Möglicherweise verfügen Sie über einen lokalen Rechner, der die Registrierung/Auflösung des Namens `host.docker.internal` verhindert. Verwenden Sie stattdessen Ihre lokale IP.
   + Windows:
      + Führen Sie in der Eingabeaufforderung `ipconfig` aus und zeichnen Sie die __IPv4-Adresse__ des Hostcomputers auf.
      + Führen Sie dann `docker_run` mit dieser IP-Adresse aus:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + Führen Sie vom Terminal aus `ifconfig` aus und zeichnen Sie die IP-Adresse des Hosts __inet__ auf, normalerweise das Gerät __en0__.
      + Führen Sie dann `docker_run` mithilfe der Host-IP-Adresse aus:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Beispielfehler

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run führt zum Fehler &#39;**: Bereitstellungsordner nicht gefunden&quot;

Wenn Sie `docker_run.cmd` ausführen, wird ein Fehler angezeigt, der den Fehler __** liest: Bereitstellungsordner nicht gefunden:__. Dies geschieht normalerweise, weil sich im Pfad Leerzeichen befinden. Entfernen Sie nach Möglichkeit die Leerzeichen im Ordner oder verschieben Sie den Ordner `aem-sdk` in einen Pfad, der keine Leerzeichen enthält.

Beispielsweise sind Windows-Benutzerordner oft `<First name> <Last name>`, wobei ein Leerzeichen dazwischen steht. Im folgenden Beispiel enthält der Ordner `...\My User\...` ein Leerzeichen, das die Ausführung der lokalen Dispatcher Tools &quot;`docker_run`&quot;unterbricht. Wenn sich die Leerzeichen in einem Windows-Benutzerordner befinden, versuchen Sie nicht, diesen Ordner umzubenennen, da er Windows beschädigt, sondern verschieben Sie stattdessen den Ordner `aem-sdk` an einen neuen Speicherort, an den Ihr Benutzer berechtigt ist, Änderungen vollständig vorzunehmen. Beachten Sie, dass Anweisungen, bei denen davon ausgegangen wird, dass sich der Ordner `aem-sdk` im Basisverzeichnis des Benutzers befindet, an den neuen Speicherort angepasst werden müssen.

#### Beispielfehler

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run kann unter Windows{#troubleshooting-windows-compatible} nicht gestartet werden

Wenn Sie `docker_run` unter Windows ausführen, kann der folgende Fehler auftreten, was den Start des Dispatchers verhindert. Dies ist ein gemeldetes Problem mit dem Dispatcher unter Windows und wird in einer zukünftigen Version behoben.

#### Beispielfehler

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Zusätzliche Ressourcen

+ [AEM SDK herunterladen](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker herunterladen](https://www.docker.com/)
+ [AEM Referenz-Website (WKND) herunterladen](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher-Dokumentation](https://docs.adobe.com/content/help/de-DE/experience-manager-dispatcher/using/dispatcher.html)
