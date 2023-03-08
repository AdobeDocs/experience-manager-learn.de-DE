---
title: Einrichten der Dispatcher Tools für die AEM as a Cloud Service Entwicklung
description: AEM Dispatcher Tools des SDK erleichtern die lokale Entwicklung von Adobe Experience Manager-Projekten (AEM), indem sie die lokale Installation, Ausführung und Fehlerbehebung von Dispatcher erleichtern.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: 370e15fdd96f1c33bc50ee72066381bec40d82c3
workflow-type: tm+mt
source-wordcount: '1593'
ht-degree: 3%

---

# Einrichten lokaler Dispatcher Tools {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale Dispatcher-Tools"
>abstract="Der Dispatcher ist ein integraler Bestandteil der gesamten Experience Manager-Architektur und sollte Teil der lokalen Entwicklungseinrichtung sein. Das AEM as a Cloud Service SDK enthält die empfohlene Version der Dispatcher Tools, die die Konfiguration, Validierung und Simulation des Dispatchers lokal erleichtert."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=de" text="Dispatcher in der Cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Herunterladen AEM as a Cloud Service SDK"

Der Dispatcher von Adobe Experience Manager (AEM) ist ein Apache HTTP-Webservermodul, das eine Sicherheits- und Leistungsschicht zwischen der CDN- und der AEM-Veröffentlichungsstufe bietet. Der Dispatcher ist ein integraler Bestandteil der gesamten Experience Manager-Architektur und sollte Teil der lokalen Entwicklungseinrichtung sein.

Das AEM as a Cloud Service SDK enthält die empfohlene Version der Dispatcher Tools, die die Konfiguration, Validierung und Simulation des Dispatchers lokal erleichtert. Die Dispatcher Tools umfassen Folgendes:

+ einen Grundsatz von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, zu finden unter `.../dispatcher-sdk-x.x.x/src`
+ ein Konfigurations-Validator-CLI-Tool, zu finden unter `.../dispatcher-sdk-x.x.x/bin/validate`
+ ein CLI-Tool zur Konfigurationsgenerierung, das sich unter `.../dispatcher-sdk-x.x.x/bin/validator`
+ ein CLI-Tool für die Konfigurationsbereitstellung, das sich unter `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ eine unveränderliche Konfigurationsdatei, die das CLI-Tool überschreibt, unter `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ ein Docker-Bild, das den Apache HTTP-Webserver mit dem Dispatcher-Modul ausführt

Beachten Sie Folgendes: `~` wird als Kurzbezeichnung für das Benutzerverzeichnis verwendet. Unter Windows entspricht dies dem `%HOMEPATH%`.

>[!NOTE]
>
> Die Videos auf dieser Seite wurden auf macOS aufgezeichnet. Windows-Benutzer können folgen, aber die entsprechenden Windows-Befehle für die Dispatcher Tools verwenden, die in jedem Video bereitgestellt werden.

## Voraussetzungen

1. Windows-Benutzer müssen Windows 10 Professional (oder eine Version, die Docker unterstützt) verwenden
1. Installieren [Schnellstart-JAR für Experience Manager-Veröffentlichung](./aem-runtime.md) auf der lokalen Entwicklungsmaschine.

+ Optional können Sie die neueste Version installieren [AEM](https://github.com/adobe/aem-guides-wknd/releases) im lokalen AEM-Veröffentlichungsdienst. Diese Website wird in diesem Tutorial zur Visualisierung eines funktionierenden Dispatchers verwendet.

1. Installieren und starten Sie die neueste Version von [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) auf dem lokalen Entwicklungscomputer.

## Herunterladen der Dispatcher Tools (als Teil des AEM SDK)

Das AEM as a Cloud Service SDK oder AEM SDK enthält die Dispatcher Tools, die zum lokalen Ausführen des Apache HTTP-Webservers mit dem Dispatcher-Modul für die Entwicklung verwendet werden, und das kompatible QuickStart-JAR.

Wenn das AEM as a Cloud Service SDK bereits heruntergeladen wurde in [Einrichten der lokalen AEM-Laufzeit](./aem-runtime.md), muss sie nicht erneut heruntergeladen werden.

1. Anmelden bei [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) mit Adobe ID
   + Ihre Adobe __must__ für AEM as a Cloud Service Download des as a Cloud Service AEM SDK bereitgestellt werden.
1. Klicken Sie auf die neueste __AEM SDK__ Ergebniszeile zum Herunterladen

## Extrahieren der Dispatcher Tools aus der AEM SDK-ZIP-Datei

>[!TIP]
>
> Windows-Benutzer dürfen keine Leerzeichen oder Sonderzeichen im Pfad zum Ordner haben, der die lokalen Dispatcher-Tools enthält. Wenn im Pfad Leerzeichen vorhanden sind, wird die `docker_run.cmd` fehlschlägt.

Die Version der Dispatcher Tools unterscheidet sich von der des AEM SDK. Stellen Sie sicher, dass die Version der Dispatcher Tools über die AEM SDK-Version bereitgestellt wird, die der AEM as a Cloud Service Version entspricht.

1. Entpacken Sie die heruntergeladene Datei `aem-sdk-xxx.zip` file
1. Entpacken Sie die Dispatcher Tools in `~/aem-sdk/dispatcher`

+ Windows: Entpacken `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (Erstellen fehlender Ordner nach Bedarf)
+ macOS Linux®: Ausführen des zugehörigen Shell-Skripts `aem-sdk-dispatcher-tools-x.x.x-unix.sh` Entpacken der Dispatcher-Tools
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Bei allen unten angegebenen Befehlen wird davon ausgegangen, dass der aktuelle Arbeitsordner die erweiterten Inhalte der Dispatcher Tools enthält.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*In diesem Video wird macOS zu Veranschaulichungszwecken verwendet. Die entsprechenden Windows/Linux-Befehle können verwendet werden, um ähnliche Ergebnisse zu erzielen*

## Grundlegendes zu den Dispatcher-Konfigurationsdateien

>[!TIP]
> Experience Manager-Projekte, die aus dem [AEM Projektarchetyp Maven](https://github.com/adobe/aem-project-archetype) vorausgefüllt sind, sind dieser Satz von Dispatcher-Konfigurationsdateien vorausgefüllt, sodass es nicht erforderlich ist, sie aus dem Ordner &quot;Dispatcher Tools src&quot;zu kopieren.

Die Dispatcher Tools bieten eine Reihe von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die das Verhalten für alle Umgebungen, einschließlich der lokalen Entwicklung, definieren.

Diese Dateien sollen in ein Experience Manager-Maven-Projekt in die `dispatcher/src` -Ordner, falls sie noch nicht im Experience Manager-Maven-Projekt vorhanden sind.

Eine vollständige Beschreibung der Konfigurationsdateien ist in den entpackten Dispatcher Tools als `dispatcher-sdk-x.x.x/docs/Config.html`.

## Konfigurationen überprüfen

Optional können die Dispatcher- und Apache-Webserverkonfigurationen (über `httpd -t`) mithilfe der `validate` Skript (nicht zu verwechseln mit der `validator` ausführbar). Die `validate` -Skript bietet eine bequeme Möglichkeit, die [drei Phasen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) des `validator`.

+ Verwendung:
   + Windows: `bin\validate src`
   + macOS Linux®: `./bin/validate.sh ./src`

## Dispatcher lokal ausführen

AEM Dispatcher mit Docker lokal ausgeführt wird. `src` Dispatcher- und Apache-Webserver-Konfigurationsdateien.

+ Verwendung:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux®: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Die `<aem-publish-host>` kann auf `host.docker.internal`, stellt ein spezieller DNS-Name Docker im Container bereit, der in die IP-Adresse des Hostcomputers aufgelöst wird. Wenn die Variable `host.docker.internal` löst nicht auf, siehe [Fehlerbehebung](#troubleshooting-host-docker-internal) unten.

Um beispielsweise den Dispatcher-Docker-Container mit den Standardkonfigurationsdateien zu starten, die von den Dispatcher Tools bereitgestellt werden:

Starten Sie den Dispatcher Docker-Container, der den Pfad zum Ordner Dispatcher Configuration src angibt:

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux®: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

Der Veröffentlichungsdienst des AEM as a Cloud Service SDK, der lokal auf Port 4503 ausgeführt wird, ist über den Dispatcher unter `http://localhost:8080`.

Um die Dispatcher Tools für die Dispatcher-Konfiguration eines Experience Manager-Projekts auszuführen, verweisen Sie auf die `dispatcher/src` Ordner.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux®:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Dispatcher Tools-Protokolle

Dispatcher-Protokolle sind bei der lokalen Entwicklung hilfreich, um zu verstehen, ob und warum HTTP-Anforderungen blockiert werden. Die Protokollebene kann festgelegt werden, indem der `docker_run` mit Umgebungsparametern.

Die Dispatcher Tools-Protokolle werden an den Standard ausgegeben, wenn `docker_run` ausgeführt wird.

Nützliche Parameter zum Debugging des Dispatchers sind:

+ `DISP_LOG_LEVEL=Debug` Setzt die Protokollierung des Dispatcher-Moduls auf Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` setzt die Protokollierung des Apache HTTP-Webserver-Neuschreibungsmoduls auf Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `DISP_RUN_MODE` legt den &quot;Ausführungsmodus&quot;der Dispatcher-Umgebung fest und lädt die entsprechenden Ausführungsmodi Dispatcher-Konfigurationsdateien.
   + Standardwert ist `dev`
+ Gültige Werte: `dev`, `stage`oder `prod`

Ein oder mehrere Parameter können an übergeben werden. `docker_run`

+ Windows:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux®:

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### Zugriff auf Protokolldateien

Der Zugriff auf Apache-Webserver und AEM Dispatcher-Protokolle erfolgt direkt im Docker-Container:

+ [Zugriff auf Protokolle im Docker-Container](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Kopieren der Docker-Protokolle in das lokale Dateisystem](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Aktualisieren der Dispatcher Tools{#dispatcher-tools-version}

Die Versionen der Dispatcher Tools werden weniger häufig erhöht als der Experience Manager. Daher erfordern die Dispatcher Tools weniger Updates in der lokalen Entwicklungsumgebung.

Die empfohlene Dispatcher Tools-Version ist diejenige, die im Lieferumfang des AEM as a Cloud Service SDK enthalten ist, das mit der as a Cloud Service Version des Experience Managers übereinstimmt. Die Version AEM as a Cloud Service finden Sie unter [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Umgebungen__, pro Umgebung, die von der __AEM__ label

![Experience Manager-Version](./assets/dispatcher-tools/aem-version.png)

*Beachten Sie, dass die Dispatcher Tools-Version nicht mit der Experience Manager-Version übereinstimmt.*

## So aktualisieren Sie den Grundliniensatz der Apache- und Dispatcher-Konfigurationen

Der Grundsatz der Apache- und Dispatcher-Konfiguration wird regelmäßig verbessert und mit der AEM as a Cloud Service SDK-Version veröffentlicht. Es empfiehlt sich, die grundlegenden Konfigurationsverbesserungen in Ihr AEM-Projekt zu integrieren und zu vermeiden. [lokale Validierung](#validate-configurations) und Cloud Manager-Pipeline-Fehler. Aktualisieren Sie sie mithilfe des `update_maven.sh` Skript aus `.../dispatcher-sdk-x.x.x/bin` Ordner.

Angenommen, Sie haben ein AEM Projekt in der Vergangenheit mit [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype), waren die grundlegenden Apache- und Dispatcher-Konfigurationen aktuell. Mithilfe dieser Grundlinienkonfigurationen wurden Ihre projektspezifischen Konfigurationen durch Wiederverwendung erstellt und die Dateien wie `*.vhost`, `*.conf`, `*.farm` und `*.any` von `dispatcher/src/conf.d` und `dispatcher/src/conf.dispatcher.d` Ordner. Ihre lokalen Dispatcher-Validierungs- und Cloud Manager-Pipelines funktionierten einwandfrei.

Unterdessen wurden die grundlegenden Apache- und Dispatcher-Konfigurationen aus verschiedenen Gründen verbessert, z. B. aus neuen Funktionen, Sicherheitskorrekturen und Optimierung. Sie werden über eine neuere Version der Dispatcher Tools als Teil der AEM as a Cloud Service Version veröffentlicht.

Wenn Sie jetzt Ihre projektspezifischen Dispatcher-Konfigurationen mit der neuesten Version der Dispatcher Tools überprüfen, schlagen sie fehl. Um dies zu beheben, müssen die Basiskonfigurationen mithilfe der folgenden Schritte aktualisiert werden:

+ Überprüfen, ob die Überprüfung mit der neuesten Version der Dispatcher Tools fehlschlägt

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ Aktualisieren Sie die unveränderlichen Dateien mit dem `update_maven.sh` script

   ```shell
   $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Updating dispatcher configuration at folder 
   running in 'extract' mode
   running in 'extract' mode
   reading immutable file list from /etc/httpd/immutable.files.txt
   preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
   ...
   immutable files extraction COMPLETE
   fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
   Cloud manager validator 2.0.53
   ```

+ Überprüfen Sie die aktualisierten unveränderlichen Dateien wie `dispatcher_vhost.conf`, `default.vhost`und `default.farm` und nehmen bei Bedarf relevante Änderungen an Ihren benutzerdefinierten Dateien vor, die aus diesen Dateien abgeleitet werden.

+ Überprüfen Sie die Konfigurationen neu. Sie sollten

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Bestätigen Sie nach der lokalen Überprüfung der Änderungen die aktualisierten Konfigurationsdateien.

## Fehlerbehebung

### docker_run führt die Meldung &#39;Warten, bis host.docker.internal verfügbar ist&#39; aus.{#troubleshooting-host-docker-internal}

Die `host.docker.internal` ist ein Hostname, der dem Docker-enthält bereitgestellt wird und zum Host aufgelöst wird. Pro docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> Ab Docker 18.03 wird empfohlen, eine Verbindung zum speziellen DNS-Namen host.docker.internal herzustellen, der zu der vom Host verwendeten internen IP-Adresse aufgelöst wird

Wann `bin/docker_run src host.docker.internal:4503 8080` Ergebnisse in der Nachricht __Warten, bis host.docker.internal verfügbar ist__, dann:

1. Stellen Sie sicher, dass die installierte Version von Docker 18.03 oder höher ist.
2. Möglicherweise verfügen Sie über einen lokalen Computer, der die Registrierung/Auflösung der `host.docker.internal` name. Verwenden Sie stattdessen Ihre lokale IP.
   + Windows:
   + Führen Sie über die Eingabeaufforderung Folgendes aus: `ipconfig`, und zeichnen Sie die __IPv4-Adresse__ des Hostcomputers.
   + Dann ausführen `docker_run` unter Verwendung dieser IP-Adresse:
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux®:
   + Führen Sie vom Terminal aus `ifconfig` und den Host aufzeichnen __inet__ IP-Adresse, normalerweise die __en0__ Gerät.
   + Dann ausführen `docker_run` Verwendung der Host-IP-Adresse:
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Beispielfehler

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Zusätzliche Ressourcen

+ [AEM SDK herunterladen](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker herunterladen](https://www.docker.com/)
+ [AEM Referenz-Website (WKND) herunterladen](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager Dispatcher-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=de)
