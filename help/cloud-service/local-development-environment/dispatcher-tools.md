---
title: Einrichten von Dispatcher-Tools für AEM als Cloud Service-Entwicklung
description: AEM Dispatcher Tools von SDK erleichtern die lokale Entwicklung von Adobe Experience Manager (AEM)-Projekten, indem sie die Installation, Ausführung und Fehlerbehebung von Dispatcher lokal erleichtern.
sub-product: Stiftung
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 2%

---


# Lokale Dispatcher-Tools einrichten

Der Dispatcher von Adobe Experience Manager (AEM) ist ein Apache HTTP-Webservermodul, das eine Sicherheits- und Leistungsebene zwischen der CDN- und der AEM Publish-Stufe bereitstellt. Dispatcher ist ein integraler Bestandteil der Gesamtarchitektur des Experience Managers und sollte Teil der lokalen Entwicklungskonzepte sein.

Das AEM als Cloud Service-SDK enthält die empfohlene Dispatcher-Tools-Version, die die Konfiguration, Validierung und Simulation von Dispatcher lokal erleichtert. Dispatcher-Tools bestehen aus:

+ einen Grundsatz der Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die sich unter `.../dispatcher-sdk-x.x.x/src`
+ einem CLI-Validator für die Konfiguration unter `.../dispatcher-sdk-x.x.x/bin/validator`
+ ein CLI-Tool für die Konfigurationsimplementierung, das sich unter `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ ein Dockerbild, das den Apache HTTP-Webserver mit dem Dispatcher-Modul ausführt

Beachten Sie, dass dies als Kurzschrift für das Benutzerverzeichnis verwendet `~` wird. Unter Windows entspricht dies `%HOMEPATH%`.

>[!NOTE]
>
> Die Videos auf dieser Seite wurden unter macOS aufgezeichnet. Windows-Benutzer können den Vorgang fortsetzen, jedoch die entsprechenden Dispatcher Tools Windows-Befehle verwenden, die mit jedem Video bereitgestellt werden.

## Voraussetzungen

1. Windows-Benutzer müssen Windows 10 Professional verwenden
1. Installieren Sie [Experience Manager Publish QuickStart](./aem-runtime.md) auf dem lokalen Entwicklungscomputer.
   + Installieren Sie optional die neueste [AEM Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases) im lokalen AEM Publish-Dienst. Diese Website wird in diesem Lernprogramm verwendet, um einen funktionierenden Dispatcher zu visualisieren.
1. Installieren und Beginn der neuesten Version von [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) auf dem lokalen Entwicklungscomputer.

## Dispatcher-Tools herunterladen (als Teil des AEM SDK)

Die AEM als Cloud Service-SDK oder AEM SDK enthält die Dispatcher-Tools, die zum Ausführen des Apache HTTP-Webservers mit dem Dispatcher-Modul lokal zur Entwicklung verwendet werden, sowie die kompatible QuickStart-Jar.

Wenn die AEM als Cloud Service-SDK bereits heruntergeladen wurde, um die lokale AEM [einzurichten](./aem-runtime.md), muss sie nicht erneut heruntergeladen werden.

1. Log in to [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) with your Adobe ID
   + Beachten Sie, dass Ihre Adobe-Organisation AEM als Cloud Service zum Herunterladen der AEM als Cloud Service-SDK bereitgestellt werden __muss__ .
1. Navigieren Sie zur Registerkarte &quot; __AEM als Cloud Service__ &quot;
1. Sortieren nach __Veröffentlichungsdatum__ in __Absteigend__ Reihenfolge
1. Klicken Sie auf die letzte __AEM SDK__ -Ergebniszeile
1. Überprüfen und akzeptieren Sie den EULA und klicken Sie auf die Schaltfläche __Herunterladen__
1. Stellen Sie sicher, dass AEM Dispatcher Tools v2.0.21+ des SDK verwendet wird.

## Extrahieren Sie die Dispatcher Tools aus der ZIP-Datei des AEM SDK

>[!TIP]
>
> Windows-Benutzer dürfen keine Leerzeichen oder Sonderzeichen im Pfad zu dem Ordner haben, der die lokalen Dispatcher-Tools enthält. Wenn Leerzeichen im Pfad vorhanden sind, schlägt der `docker_run.cmd` Fehler fehl.

Die Version der Dispatcher Tools unterscheidet sich von der des AEM SDK. Vergewissern Sie sich, dass die Dispatcher-Tools über die AEM SDK-Version bereitgestellt werden, die mit der AEM als Cloud Service-Version übereinstimmt.

1. Unzip the downloaded `aem-sdk-xxx.zip` file
1. Entpacken Sie die Dispatcher-Werkzeuge in `~/aem-sdk/dispatcher`
   + Windows: Entpacken `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (Erstellen fehlender Ordner nach Bedarf)
   + macOS/Linux: Führen Sie das zugehörige Shell-Skript aus, `aem-sdk-dispatcher-tools-x.x.x-unix.sh` um die Dispatcher-Tools zu entpacken.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Beachten Sie, dass alle unten angegebenen Befehle davon ausgehen, dass der aktuelle Arbeitsordner den erweiterten Dispatcher Tools-Inhalt enthält.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)
*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Verstehen der Dispatcher-Konfigurationsdateien

>[!TIP]
> Experience Manager-Projekte, die aus dem [AEM Projekt Maven Archetype](https://github.com/adobe/aem-project-archetype) erstellt wurden, sind mit diesem Satz von Dispatcher-Konfigurationsdateien vorausgefüllt. Daher müssen sie nicht aus dem Ordner Dispatcher Tools src kopiert werden.

Die Dispatcher-Tools bieten eine Reihe von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die das Verhalten für alle Umgebung, einschließlich der lokalen Entwicklung, definieren.

Diese Dateien sollen in ein Experience Manager-Maven-Projekt kopiert werden, sofern sie nicht bereits im Experience Manager-Maven-Projekt `dispatcher/src` vorhanden sind.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)
*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

Eine vollständige Beschreibung der Konfigurationsdateien finden Sie in den entpackten Dispatcher Tools wie `dispatcher-sdk-x.x.x/docs/Config.html`.

## Dispatcher lokal ausführen

Um den Dispatcher lokal auszuführen, müssen die Dispatcher-Konfigurationsdateien, die für die Konfiguration verwendet werden, mit dem CLI-Tool der Dispatcher-Tools `validator` validiert werden.

+ Nutzung:
   + Windows: `bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

Die Validierung erfolgt in zweifacher Hinsicht:

+ Validiert die Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien auf Richtigkeit
+ Wandelt die Konfigurationen in einen mit dem Apache HTTP-Webserver des Docker-Containers kompatiblen Dateisatz um.

Nach der Validierung werden die implementierten Konfigurationen verwendet, um Dispatcher lokal im Docker-Container auszuführen. Es ist wichtig sicherzustellen, dass die neuesten Konfigurationen überprüft __und__ mit der `-d` Option des Validators ausgegeben wurden.

+ Nutzung:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Die `aem-publish-host` kann auf `host.docker.internal`, ein spezieller DNS-Name Docker in dem Container, der aufgelöst wird auf die IP des Hostcomputers. Falls er `host.docker.internal` nicht auflöst, lesen Sie bitte den Abschnitt [Fehlerbehebung](#troubleshooting-host-docker-internal) unten.

So können Sie beispielsweise den Dispatcher-Docker-Container mit den von den Dispatcher-Tools bereitgestellten Standardkonfigurationsdateien Beginn haben:

1. Generieren Sie den `deployment-folder`nach `out` Konventionen benannten Code bei jeder Konfigurationsänderung von Grund auf:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Re-)Beginn Dispatcher Docker Container, der den Pfad zum Bereitstellungsordner angibt:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Der AEM als Veröffentlichungsdienst des Cloud Service-SDK, der lokal auf Port 4503 ausgeführt wird, steht über Dispatcher unter `http://localhost:8080`.

Um Dispatcher-Tools für die Dispatcher-Konfiguration eines Experience Manager-Projekts auszuführen, generieren Sie einfach den Ordner `deployment-folder` mit dem `dispatcher/src` Projektordner.

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
*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Dispatcher Tools-Protokolle

Dispatcher-Protokolle sind während der lokalen Entwicklung hilfreich, um zu verstehen, ob und warum HTTP-Anforderungen blockiert werden. Die Protokollierungsstufe kann durch Präfix der Ausführung von `docker_run` mit Umgebung-Parametern festgelegt werden.

Dispatcher-Tools-Protokolle werden beim Ausführen an den Standard ausgegeben `docker_run` .

Zu den nützlichen Parametern für das Debugging von Dispatcher gehören:

+ `DISP_LOG_LEVEL=Debug` legt die Protokollierung des Dispatcher-Moduls auf Debug-Ebene fest
   + Der Standardwert ist: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` legt die Apache HTTP-Webserver-Umschreibungsmodul-Protokollierung auf Debug-Ebene fest
   + Der Standardwert ist: `Warn`
+ `DISP_RUN_MODE` legt den &quot;Ausführungsmodus&quot;der Dispatcher-Umgebung fest und lädt die entsprechenden Ausführungsmodi Dispatcher-Konfigurationsdateien.
   + Standardwert ist `dev`
+ Gültige Werte: `dev`, `stage`oder `prod`

Ein oder mehrere Parameter können an `docker_run`

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
*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Aktualisieren der Dispatcher-Tools{#dispatcher-tools-version}

Dispatcher Tools-Versionen werden weniger häufig als der Experience Manager inkrementiert, sodass Dispatcher Tools weniger Updates in der Umgebung für die lokale Entwicklung erfordern.

Die empfohlene Dispatcher Tools-Version ist die Version, die im Lieferumfang des AEM als Cloud Service-SDK enthalten ist, das dem Experience Manager als Cloud Service-Version entspricht. Die AEM als Cloud Service finden Sie über [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Umgebung__ pro Umgebung, die in der __AEM__ -Versionshinweise angegeben ist

![Experience Manager-Version](./assets/dispatcher-tools/aem-version.png)

_Beachten Sie, dass die Dispatcher Tools-Version selbst nicht mit der Experience Manager-Version übereinstimmt._

## Fehlerbehebung

### docker_run gibt die Meldung &#39;Warten, bis host.docker.internal verfügbar ist&#39; aus.{#troubleshooting-host-docker-internal}

`host.docker.internal` ist ein Hostname, der dem Docker-enthält, der zum Host aufgelöst wird. Pro docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Ab Docker 18.03 empfehlen wir, eine Verbindung zum speziellen DNS-Namen host.docker.internal herzustellen, der zu der vom Host verwendeten internen IP-Adresse aufgelöst wird.

Wenn, wenn `bin/docker_run out host.docker.internal:4503 8080` die Meldung __Wartet, bis host.docker.internal verfügbar__ ist, dann:

1. Stellen Sie sicher, dass die installierte Version von Docker 18.03 oder höher ist.
2. Möglicherweise haben Sie einen lokalen Computer eingerichtet, der die Registrierung/Auflösung des `host.docker.internal` Namens verhindert. Verwenden Sie stattdessen Ihre lokale IP.
   + Windows:
      + Führen Sie an der Eingabeaufforderung die `ipconfig`IPv4-Adresse __des Hosts aus__ und zeichnen Sie sie auf.
      + Führen Sie dann `docker_run` mit dieser IP-Adresse aus:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + Führen Sie von Terminal aus die Host- `ifconfig` Int __-IP-Adresse aus__ und zeichnen Sie sie auf, normalerweise das __en0__ -Gerät.
      + Führen Sie die Ausführung dann `docker_run` unter Verwendung der Host-IP-Adresse aus:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Beispielfehler

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run führt zum &#39;**-Fehler: Bereitstellungsordner nicht gefunden

Bei Ausführung `docker_run.cmd`wird ein Fehler mit dem Wert __** angezeigt: Bereitstellungsordner nicht gefunden:__. Dies geschieht normalerweise, weil sich im Pfad Leerzeichen befinden. Entfernen Sie nach Möglichkeit die Leerzeichen im Ordner oder verschieben Sie den `aem-sdk` Ordner in einen Pfad, der keine Leerzeichen enthält.

Beispielsweise sind Windows-Benutzerordner häufig `<First name> <Last name>`mit einem Leerzeichen dazwischen. Im Beispiel unten `...\My User\...` enthält der Ordner ein Leerzeichen, das die Ausführung der lokalen Dispatcher Tools unterbricht `docker_run` . Wenn sich die Leerzeichen in einem Windows-Benutzerordner befinden, versuchen Sie nicht, diesen Ordner umzubenennen, da er Windows beschädigt. Verschieben Sie stattdessen den `aem-sdk` Ordner an einen neuen Speicherort, den Ihr Benutzer vollständig ändern darf. Beachten Sie, dass Anweisungen, bei denen davon ausgegangen wird, dass sich der `aem-sdk` Ordner im Basisordner des Benutzers befindet, an den neuen Speicherort angepasst werden müssen.

#### Beispielfehler

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run kann unter Windows nicht Beginn werden{#troubleshooting-windows-compatible}

Wird `docker_run` unter Windows ausgeführt, kann der folgende Fehler auftreten, sodass Dispatcher nicht gestartet werden kann. Dies ist ein gemeldetes Problem mit Dispatcher unter Windows und wird in einer zukünftigen Version behoben.

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
+ [Die AEM-Referenz-Website (WKND) herunterladen](https://github.com/adobe/aem-guides-wknd/releases)
+ [Dokumentation zum Experience Manager-Dispatcher](https://docs.adobe.com/content/help/de-DE/experience-manager-dispatcher/using/dispatcher.html)
