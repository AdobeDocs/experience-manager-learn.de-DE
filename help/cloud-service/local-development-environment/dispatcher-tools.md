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
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# Lokale Dispatcher-Tools einrichten

Der Dispatcher von Adobe Experience Manager (AEM) ist ein Apache HTTP-Webservermodul, das eine Sicherheits- und Leistungsebene zwischen der CDN- und der AEM Publish-Stufe bereitstellt. Dispatcher ist ein integraler Bestandteil der Gesamtarchitektur des Experience Managers und sollte Teil der lokalen Entwicklungskonzepte sein.

Das AEM als Cloud Service-SDK enthält die empfohlene Dispatcher-Tools-Version, die die Konfiguration, Validierung und Simulation von Dispatcher lokal erleichtert. Dispatcher-Tools bestehen aus:

+ einen Grundsatz der Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die sich in `.../dispatcher-sdk-x.x.x/src` befinden
+ einem CLI-Validator für die Konfiguration, der sich unter `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+) befindet
+ ein CLI-Tool zur Konfigurationsgenerierung, das sich unter `.../dispatcher-sdk-x.x.x/bin/validator` befindet
+ ein CLI-Tool für die Konfigurationsimplementierung, das sich unter `.../dispatcher-sdk-x.x.x/bin/docker_run` befindet
+ ein Dockerbild, das den Apache HTTP-Webserver mit dem Dispatcher-Modul ausführt

Beachten Sie, dass `~` als Kurzbezeichnung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

>[!NOTE]
>
> Die Videos auf dieser Seite wurden unter macOS aufgezeichnet. Windows-Benutzer können den Vorgang fortsetzen, jedoch die entsprechenden Dispatcher Tools Windows-Befehle verwenden, die mit jedem Video bereitgestellt werden.

## Voraussetzungen

1. Windows-Benutzer müssen Windows 10 Professional verwenden
1. Installieren Sie [Experience Manager Publish Quickstart Jar](./aem-runtime.md) auf dem lokalen Entwicklungscomputer.
   + Installieren Sie optional die neueste [AEM Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases) im lokalen AEM Publish-Dienst. Diese Website wird in diesem Lernprogramm verwendet, um einen funktionierenden Dispatcher zu visualisieren.
1. Installieren und Beginn der neuesten Version von [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) auf dem lokalen Entwicklungscomputer.

## Dispatcher-Tools herunterladen (als Teil des AEM SDK)

Die AEM als Cloud Service-SDK oder AEM SDK enthält die Dispatcher-Tools, die zum Ausführen des Apache HTTP-Webservers mit dem Dispatcher-Modul lokal zur Entwicklung verwendet werden, sowie die kompatible QuickStart-Jar.

Wenn die AEM als Cloud Service-SDK bereits auf [die lokale AEM Runtime](./aem-runtime.md) heruntergeladen wurde, muss sie nicht erneut heruntergeladen werden.

1. Melden Sie sich bei [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout Liste&amp;p.offset=0&amp;p.limit=1) mit Ihrem Adobe ID an
   + Ihre Adobe-Organisation __muss__ als Cloud Service bereitgestellt werden, um die AEM als Cloud Service-SDK herunterzuladen.
1. Klicken Sie auf die letzte __AEM SDK__-Ergebniszeile, die heruntergeladen werden soll.
   + Stellen Sie sicher, dass AEM Dispatcher Tools v2.0.29+ des SDK in der Downloadbeschreibung vermerkt ist.

## Extrahieren Sie die Dispatcher Tools aus der ZIP-Datei des AEM SDK

>[!TIP]
>
> Windows-Benutzer dürfen keine Leerzeichen oder Sonderzeichen im Pfad zu dem Ordner haben, der die lokalen Dispatcher-Tools enthält. Wenn Leerzeichen im Pfad vorhanden sind, schlägt `docker_run.cmd` fehl.

Die Version der Dispatcher Tools unterscheidet sich von der des AEM SDK. Vergewissern Sie sich, dass die Dispatcher-Tools über die AEM SDK-Version bereitgestellt werden, die mit der AEM als Cloud Service-Version übereinstimmt.

1. Dekomprimieren Sie die heruntergeladene Datei `aem-sdk-xxx.zip`
1. Entpacken Sie die Dispatcher-Tools in `~/aem-sdk/dispatcher`
   + Windows: Dekomprimieren Sie `aem-sdk-dispatcher-tools-x.x.x-windows.zip` nach `C:\Users\<My User>\aem-sdk\dispatcher` (erstellen Sie nach Bedarf fehlende Ordner).
   + macOS/Linux: Führen Sie das zugehörige Shell-Skript `aem-sdk-dispatcher-tools-x.x.x-unix.sh` aus, um die Dispatcher-Tools zu entpacken.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Beachten Sie, dass alle unten angegebenen Befehle davon ausgehen, dass der aktuelle Arbeitsordner den erweiterten Dispatcher Tools-Inhalt enthält.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Verstehen der Dispatcher-Konfigurationsdateien

>[!TIP]
> Experience Manager-Projekte, die aus dem AEM [Projekt Maven Archetype](https://github.com/adobe/aem-project-archetype) erstellt wurden, werden mit diesem Satz Dispatcher-Konfigurationsdateien vorausgefüllt. Daher müssen sie nicht aus dem Ordner Dispatcher Tools src kopiert werden.

Die Dispatcher-Tools bieten eine Reihe von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die das Verhalten für alle Umgebung, einschließlich der lokalen Entwicklung, definieren.

Diese Dateien sollen in ein Experience Manager-Maven-Projekt in den Ordner `dispatcher/src` kopiert werden, sofern sie noch nicht im Experience Manager-Maven-Projekt vorhanden sind.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

Eine vollständige Beschreibung der Konfigurationsdateien ist in den entpackten Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html` verfügbar.

## Konfigurationen überprüfen

Optional können die Dispatcher- und Apache-Webserverkonfigurationen (über `httpd -t`) mit dem Skript `validate` validiert werden (nicht zu verwechseln mit der ausführbaren Datei `validator`).

+ Nutzung:
   + Windows: `bin\validate src`
   + macOS/Linux: `./bin/validate ./src`

## Dispatcher lokal ausführen

Um den Dispatcher lokal auszuführen, müssen die Dispatcher-Konfigurationsdateien mit dem CLI-Tool der Dispatcher Tools generiert werden.`validator`

+ Nutzung:
   + Windows: `bin\validator full -d out src`
   + macOS/Linux: `./bin/validator full -d ./out ./src`

Mit diesem Befehl werden die Konfigurationen in einen mit dem Apache HTTP-Webserver des Docker-Containers kompatiblen Dateisatz übertragen.

Nach der Generierung werden die implementierten Konfigurationen verwendet, um Dispatcher lokal im Docker-Container auszuführen. Es ist wichtig sicherzustellen, dass die neuesten Konfigurationen mit der `validate` __und__-Ausgabe unter Verwendung der `-d`-Option des Validators validiert wurden.

+ Nutzung:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS/Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host` kann auf `host.docker.internal` eingestellt werden, ein spezieller DNS-Name Docker stellt im Container bereit, der auf die IP des Hostcomputers aufgelöst wird. Wenn `host.docker.internal` nicht aufgelöst wird, lesen Sie bitte den Abschnitt [Fehlerbehebung](#troubleshooting-host-docker-internal) weiter unten.

So können Sie beispielsweise den Dispatcher-Docker-Container mit den von den Dispatcher-Tools bereitgestellten Standardkonfigurationsdateien Beginn haben:

1. Generieren Sie das `deployment-folder` mit dem Namen `out` nach Konvention bei jeder Konfigurationsänderung von Grund auf:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS/Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Re-)Beginn Dispatcher Docker Container, der den Pfad zum Bereitstellungsordner angibt:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS/Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Der AEM als Veröffentlichungsdienst des Cloud Service-SDK, der lokal auf Port 4503 ausgeführt wird, ist über Dispatcher unter `http://localhost:8080` verfügbar.

Um Dispatcher-Tools für die Dispatcher-Konfiguration eines Experience Manager-Projekts auszuführen, generieren Sie einfach den Ordner `deployment-folder` mit dem Projektordner `dispatcher/src`.

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

Dispatcher-Protokolle sind während der lokalen Entwicklung hilfreich, um zu verstehen, ob und warum HTTP-Anforderungen blockiert werden. Die Protokollierungsstufe kann festgelegt werden, indem der Ausführung von `docker_run` die Umgebung vorangestellt wird.

Dispatcher-Tools-Protokolle werden beim Ausführen von `docker_run` an den Standard ausgegeben.

Zu den nützlichen Parametern für das Debugging von Dispatcher gehören:

+ `DISP_LOG_LEVEL=Debug` legt die Protokollierung des Dispatcher-Moduls auf Debug-Ebene fest
   + Der Standardwert ist: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` legt die Apache HTTP-Webserver-Umschreibungsmodul-Protokollierung auf Debug-Ebene fest
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

*In diesem Video wird macOS zur Veranschaulichung verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Wann werden die Dispatcher-Tools aktualisiert?{#dispatcher-tools-version}

Dispatcher Tools-Versionen werden weniger häufig als der Experience Manager inkrementiert, sodass Dispatcher Tools weniger Updates in der Umgebung für die lokale Entwicklung erfordern.

Die empfohlene Dispatcher Tools-Version ist die Version, die im Lieferumfang des AEM als Cloud Service-SDK enthalten ist, das dem Experience Manager als Cloud Service-Version entspricht. Die Version von AEM als Cloud Service finden Sie unter [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Umgebung__ pro Umgebung, die durch das  __AEM__ Releaselabel angegeben wird

![Experience Manager-Version](./assets/dispatcher-tools/aem-version.png)

_Beachten Sie, dass die Dispatcher Tools-Version selbst nicht mit der Experience Manager-Version übereinstimmt._

## Fehlerbehebung

### Die Ergebnisse von docker_run lauten in &#39;Warten, bis host.docker.internal verfügbar ist&#39; message{#troubleshooting-host-docker-internal}

`host.docker.internal` ist ein Hostname, der dem Docker-enthält, der zum Host aufgelöst wird. Pro docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> Ab Docker 18.03 empfehlen wir, eine Verbindung zum speziellen DNS-Namen host.docker.internal herzustellen, der zu der vom Host verwendeten internen IP-Adresse aufgelöst wird.

Wenn `bin/docker_run out host.docker.internal:4503 8080` die Meldung __Wartet, bis host.docker.internal verfügbar ist__, dann:

1. Stellen Sie sicher, dass die installierte Version von Docker 18.03 oder höher ist.
2. Möglicherweise haben Sie einen lokalen Computer eingerichtet, der die Registrierung/Auflösung des Namens `host.docker.internal` verhindert. Verwenden Sie stattdessen Ihre lokale IP.
   + Windows:
      + Führen Sie in der Eingabeaufforderung `ipconfig` aus und notieren Sie die __IPv4-Adresse__ des Hostcomputers.
      + Führen Sie dann `docker_run` mit dieser IP-Adresse aus:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS/Linux:
      + Führen Sie in Terminal `ifconfig` aus und zeichnen Sie die IP-Adresse des Hosts __inet__ auf, normalerweise das __en0__-Gerät.
      + Führen Sie dann `docker_run` unter Verwendung der Host-IP-Adresse aus:
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

Beim Ausführen von `docker_run.cmd` wird ein Fehler mit dem Wortlaut __* Fehler angezeigt: Bereitstellungsordner nicht gefunden:__. Dies geschieht normalerweise, weil sich im Pfad Leerzeichen befinden. Entfernen Sie nach Möglichkeit die Leerzeichen im Ordner oder verschieben Sie den Ordner `aem-sdk` in einen Pfad, der keine Leerzeichen enthält.

Windows-Benutzerordner sind beispielsweise häufig `<First name> <Last name>` mit einem Leerzeichen dazwischen. Im Beispiel unter dem Ordner `...\My User\...` enthält ein Leerzeichen, das die Ausführung der lokalen Dispatcher Tools&#39; `docker_run` unterbricht. Wenn sich die Leerzeichen in einem Windows-Benutzerordner befinden, versuchen Sie nicht, diesen Ordner umzubenennen, da er Windows beschädigt, sondern verschieben Sie stattdessen den Ordner `aem-sdk` an einen neuen Speicherort, den Ihr Benutzer vollständig ändern darf. Beachten Sie, dass Anweisungen, bei denen davon ausgegangen wird, dass sich der Ordner `aem-sdk` im Basisordner des Benutzers befindet, an den neuen Speicherort angepasst werden müssen.

#### Beispielfehler

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run schlägt unter Windows{#troubleshooting-windows-compatible} nicht Beginn

Die Ausführung von `docker_run` unter Windows kann zu dem folgenden Fehler führen, wodurch der Start von Dispatcher verhindert wird. Dies ist ein gemeldetes Problem mit Dispatcher unter Windows und wird in einer zukünftigen Version behoben.

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
