---
title: Einrichten der Dispatcher-Tools für die AEM as a Cloud Service-Entwicklung
description: Die Dispatcher-Tools des AEM SDK erleichtern die lokale Entwicklung von Adobe Experience Manager (AEM)-Projekten, indem sie die lokale Installation, Ausführung und Fehlerbehebung von Dispatcher vereinfachen.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1695'
ht-degree: 95%

---

# Einrichten lokaler Dispatcher-Tools {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Lokale Dispatcher-Tools"
>abstract="Dispatcher ist ein integraler Bestandteil der Gesamtarchitektur von Experience Manager und sollte auch in der lokalen Entwicklungsumgebung vorhanden sein. Das AEM as a Cloud Service SDK enthält die empfohlene Dispatcher-Tool-Version, die die lokale Konfiguration, Validierung und Simulation von Dispatcher vereinfacht."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=de" text="Dispatcher in der Cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/de/aemcloud.html" text="Herunterladen des AEM as a Cloud Service SDK"

Der Dispatcher von Adobe Experience Manager (AEM) ist ein Apache-HTTP-Webservermodul, das eine Sicherheits- und Leistungsschicht zwischen dem CDN und der AEM-Veröffentlichungsebene bildet. Dispatcher ist ein integraler Bestandteil der Gesamtarchitektur von Experience Manager und sollte auch in der lokalen Entwicklungsumgebung vorhanden sein.

Das AEM as a Cloud Service SDK enthält die empfohlene Dispatcher-Tool-Version, die die lokale Konfiguration, Validierung und Simulation von Dispatcher vereinfacht. Die Dispatcher-Tools umfassen Folgendes:

+ einen Grundsatz von Apache HTTP-Webserver- und Dispatcher-Konfigurationsdateien, zu finden unter `.../dispatcher-sdk-x.x.x/src`
+ ein Konfigurations-Validator-CLI-Tool, zu finden unter `.../dispatcher-sdk-x.x.x/bin/validate`
+ ein CLI-Tool zur Konfigurationsgenerierung, zu finden unter `.../dispatcher-sdk-x.x.x/bin/validator`
+ ein CLI-Tool für die Konfigurationsbereitstellung, zu finden unter `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ eine unveränderliche Konfigurationsdatei, die das CLI-Tool überschreibt, zu finden unter `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ ein Docker-Bild, das den Apache-HTTP-Webserver mit dem Dispatcher-Modul ausführt

Beachten Sie, dass `~` als Abkürzung für das Benutzerverzeichnis verwendet wird. Unter Windows entspricht dies `%HOMEPATH%`.

>[!NOTE]
>
> Die Videos auf dieser Seite wurden mit macOS aufgezeichnet. Windows-Benutzende können dem Video folgen, müssen aber die entsprechenden Windows-Befehle der Dispatcher-Tools verwenden, die mit jedem Video mitgeliefert werden.

## Voraussetzungen

1. Windows-Benutzende müssen Windows 10 Professional (oder eine Version, die Docker unterstützt) verwenden
1. Installieren Sie [Experience Manager Veröffentlichungs-Schnellstart-JAR](./aem-runtime.md) auf dem lokalen Entwicklungsrechner.

+ Installieren Sie optional die neueste [AEM-Referenz-Website](https://github.com/adobe/aem-guides-wknd/releases) auf dem lokalen AEM-Veröffentlichungs-Service. Diese Website wird in diesem Tutorial zur Visualisierung eines funktionierenden Dispatchers verwendet.

1. Installieren und starten Sie die neueste Version von [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) auf dem lokalen Entwicklungsrechner.

## Herunterladen der Dispatcher-Tools (als Teil des AEM SDK)

Das AEM as a Cloud Service-SDK oder AEM SDK enthält die Dispatcher-Tools, die zum lokalen Ausführen des Apache-HTTP-Webservers mit dem Dispatcher-Modul für die Entwicklung verwendet werden, und das kompatible Schnellstart-JAR.

Wenn das AEM as a Cloud Service-SDK bereits heruntergeladen wurde, um [die lokale AEM-Laufzeitumgebung einzurichten](./aem-runtime.md), muss es nicht erneut heruntergeladen werden.

1. Melden Sie sich unter [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) mit Ihrer Adobe-ID an.
   + Ihre Adobe-Organisation __muss__ für AEM as a Cloud Service bereitgestellt werden, um das AEM as a Cloud Service-SDK herunterladen zu können
1. Klicken Sie auf die neueste __AEM SDK__-Ergebniszeile zum Herunterladen

## Extrahieren Sie die Dispatcher-Tools aus der AEM SDK-ZIP-Datei

>[!TIP]
>
> Windows-Benutzende dürfen keine Leerzeichen oder Sonderzeichen im Pfad zum Ordner haben, der die lokalen Dispatcher-Tools enthält. Wenn Leerzeichen im Pfad vorhanden sind, schlägt `docker_run.cmd` fehl.

Die Version der Dispatcher-Tools unterscheidet sich von der des AEM SDK. Stellen Sie sicher, dass die Version der Dispatcher-Tools über die AEM SDK-Version bereitgestellt wird, die der AEM as a Cloud Service-Version entspricht.

1. Entpacken Sie die heruntergeladene Datei `aem-sdk-xxx.zip`.
1. Entpacken Sie die Dispatcher-Tools in `~/aem-sdk/dispatcher`.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Entpacken `aem-sdk-dispatcher-tools-x.x.x-windows.zip` in `C:\Users\<My User>\aem-sdk\dispatcher` (Erstellen fehlender Ordner nach Bedarf).

>[!TAB Linux]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Bei allen unten angegebenen Befehlen wird davon ausgegangen, dass das aktuelle Arbeitsverzeichnis die erweiterten Inhalte der Dispatcher-Tools enthält.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*In diesem Video wird macOS zur Veranschaulichung verwendet. Mit den entsprechenden Windows-/Linux-Befehlen werden ähnliche Ergebnisse erzielt.*

## Grundlegendes zu den Dispatcher-Konfigurationsdateien

>[!TIP]
> Experience Manager-Projekte, die mit dem [AEM-Projekt-Maven-Archetyp](https://github.com/adobe/aem-project-archetype) erstellt wurden, sind mit diesem Satz von Dispatcher-Konfigurationsdateien vorausgefüllt, sodass ein Kopieren aus dem Dispatcher-Tools-Quellordner nicht erforderlich ist.

Die Dispatcher-Tools bieten eine Reihe von Apache-HTTP-Webserver- und Dispatcher-Konfigurationsdateien, die das Verhalten für alle Umgebungen, einschließlich der lokalen Entwicklung, definieren.

Diese Dateien sollten in ein Experience Manager-Maven-Projekt in den Ordner `dispatcher/src` kopiert werden, wenn sie nicht bereits im Experience Manager-Maven-Projekt vorhanden sind.

Eine vollständige Beschreibung der Konfigurationsdateien ist in den entpackten Dispatcher-Tools als `dispatcher-sdk-x.x.x/docs/Config.html` verfügbar.

## Überprüfen der Konfigurationen

Optional können die Konfigurationen des Dispatchers und des Apache-Webservers (über `httpd -t`) mit dem Skript `validate` (nicht zu verwechseln mit der ausführbaren Datei `validator`) validiert werden. Das `validate`-Skript bietet eine bequeme Möglichkeit, die [drei Phasen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=de) des `validator` auszuführen.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Lokales Ausführen des Dispatchers

Der AEM Dispatcher wird lokal mit Docker für die `src`-Dispatcher- und Apache-Webserver-Konfigurationsdateien ausgeführt.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Die `docker_run_hot_reload` ausführbare Datei wird bevorzugt `docker_run` beim erneuten Laden von Konfigurationsdateien, während sie geändert werden, ohne dass sie manuell beendet und neu gestartet werden müssen `docker_run`. Alternativ: `docker_run` kann verwendet werden, es ist jedoch ein manuelles Beenden und Neustart erforderlich `docker_run` wenn Konfigurationsdateien geändert werden.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

Die `docker_run_hot_reload` ausführbare Datei wird bevorzugt `docker_run` beim erneuten Laden von Konfigurationsdateien, während sie geändert werden, ohne dass sie manuell beendet und neu gestartet werden müssen `docker_run`. Alternativ: `docker_run` kann verwendet werden, es ist jedoch ein manuelles Beenden und Neustart erforderlich `docker_run` wenn Konfigurationsdateien geändert werden.

>[!ENDTABS]

Das `<aem-publish-host>` kann auf `host.docker.internal` gesetzt werden, einen speziellen DNS-Namen, den Docker im Container bereitstellt und der in die IP des Host-Rechners aufgelöst wird. Wenn `host.docker.internal` nicht aufgelöst werden kann, lesen Sie bitte den Abschnitt [Fehlerbehebung](#troubleshooting-host-docker-internal) weiter unten.

Um beispielsweise den Dispatcher-Docker-Container mit den Standardkonfigurationsdateien zu starten, die von den Dispatcher-Tools bereitgestellt werden:

Starten Sie den Dispatcher-Docker-Container, indem Sie den Pfad zum src-Ordner der Dispatcher-Konfiguration angeben:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

Der Publish-Service des AEM as a Cloud Service-SDK, der lokal auf Port 4503 ausgeführt wird, ist über den Dispatcher unter `http://localhost:8080` verfügbar.

Um die Dispatcher Tools für die Dispatcher-Konfiguration eines Experience Manager-Projekts auszuführen, verweisen Sie auf den `dispatcher/src`-Ordner Ihres Projekts.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Dispatcher-Tools-Protokolle

Dispatcher-Protokolle sind bei der lokalen Entwicklung hilfreich, um zu verstehen, ob und warum HTTP-Anfragen blockiert werden. Die Protokollebene kann festgelegt werden, indem bei der Ausführung von `docker_run` Umgebungsparameter vorangestellt werden.

Die Dispatcher-Tools-Protokolle werden an die Standardausgabe ausgegeben, wenn `docker_run` ausgeführt wird.

Nützliche Parameter zum Debuggen des Dispatchers sind:

+ `DISP_LOG_LEVEL=Debug` setzt die Protokollierung des Dispatcher-Moduls auf die Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` setzt die Protokollierung des Webserver-Neuschreibungsmoduls der Apache-HTTP auf die Debug-Ebene
   + Der Standardwert ist: `Warn`
+ `DISP_RUN_MODE` legt den „Ausführungsmodus“ der Dispatcher-Umgebung fest und lädt die entsprechenden Dispatcher-Konfigurationsdateien dazu.
   + Standardwert ist `dev`
+ Gültige Werte: `dev`, `stage` oder `prod`

Ein oder mehrere Parameter können an `docker_run` weitergegeben werden. 

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Zugriff auf Protokolldateien

Auf Apache-Webserver- und AEM Dispatcher-Protokolle kann direkt im Docker-Container zugegriffen werden:

+ [Zugreifen auf Protokolle im Docker-Container](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Kopieren der Docker-Protokolle auf das lokale Dateisystem](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Zeitpunkt zur Aktualisierung der Dispatcher-Tools{#dispatcher-tools-version}

Die Dispatcher-Tools erhalten im Gegensatz zu Experience Manager seltener Versionserhöhungen. Daher sind für die Dispatcher-Tools weniger Aktualisierungen in der lokalen Entwicklungsumgebung erforderlich.

Es wird die Dispatcher-Tools-Version aus dem Lieferumfang des AEM as a Cloud Service SDK empfohlen, das mit der Experience Manager as a Cloud Service-Version übereinstimmt. Die AEM as a Cloud Service-Version finden Sie über [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Umgebungen__, pro Umgebung unter __AEM-Version__ angegeben

![Experience Manager-Version](./assets/dispatcher-tools/aem-version.png)

*Beachten Sie, dass die Dispatcher-Tools-Version nicht mit der Experience Manager-Version übereinstimmt.*

## Aktualisieren der Apache- und Dispatcher-Basiskonfigurationen

Die Apache- und Dispatcher-Basiskonfigurationen werden regelmäßig erweitert und mit der AEM as a Cloud Service SDK-Version veröffentlicht. Die Best Practise liegt darin, die erweiterten Basiskonfigurationen in Ihr AEM-Projekt zu integrieren, um [lokale Validierungs](#validate-configurations)- sowie Cloud Manager-Pipeline-Fehler zu vermeiden. Aktualisieren Sie sie mithilfe des `update_maven.sh`-Skripts aus dem Ordner `.../dispatcher-sdk-x.x.x/bin`.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Dieses Video verwendet macOS zu Illustrationszwecken. Mit den entsprechenden Windows-/Linux-Befehlen werden ähnliche Ergebnisse erzielt.*


Angenommen, Sie haben früher ein AEM-Projekt über den [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype) mit zu dieser Zeit aktuellen Apache- und Dispatcher-Basiskonfigurationen erstellt. Mithilfe dieser Basiskonfigurationen wurden Ihre projektspezifischen Konfigurationen durch Wiederverwenden und Kopieren von Dateien wie `*.vhost`, `*.conf`, `*.farm` und `*.any` aus den Ordnern `dispatcher/src/conf.d` und `dispatcher/src/conf.dispatcher.d` erstellt. Ihre lokalen Dispatcher-Validierungs- und Cloud Manager-Pipelines haben einwandfrei funktioniert.

Unterdessen wurden die Apache- und Dispatcher-Basiskonfigurationen aus verschiedenen Gründen erweitert, z. B. mit neuen Funktionen, Sicherheitskorrekturen und Optimierungen. Sie werden über eine neuere Version der Dispatcher-Tools als Teil der AEM as a Cloud Service-Version veröffentlicht.

Wenn Sie nun Ihre projektspezifischen Dispatcher-Konfigurationen für die neueste Version der Dispatcher-Tools validieren, schlagen sie fehl. Um dies zu beheben, müssen die Basiskonfigurationen mithilfe der folgenden Schritte aktualisiert werden:

+ Überprüfen Sie, ob die Validierung für die neueste Version der Dispatcher-Tools fehlschlägt.

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Aktualisieren Sie die unveränderlichen Dateien mit dem Skript `update_maven.sh`.

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

+ Überprüfen Sie die aktualisierten unveränderlichen Dateien wie `dispatcher_vhost.conf`, `default.vhost` und `default.farm` und nehmen Sie bei Bedarf relevante Änderungen an Ihren benutzerdefinierten Dateien vor, die von diesen Dateien stammen.

+ Validieren Sie die Konfigurationen erneut. Sie sollten nicht mehr fehlschlagen.

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

+ Übergeben Sie nach der lokalen Überprüfung der Änderungen die aktualisierten Konfigurationsdateien.

## Fehlerbehebung

### „docker_run“ führt zur Meldung „Warten, bis host.docker.internal verfügbar ist“{#troubleshooting-host-docker-internal}

`host.docker.internal` ist ein Host-Name für den Docker-Container, der zum Host aufgelöst wird. Per docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> Ab Docker 18.03 wird empfohlen, eine Verbindung zum speziellen DNS-Namen „host.docker.internal“ herzustellen, der zu der vom Host verwendeten internen IP-Adresse aufgelöst wird.

Gehen Sie wie folgt vor, wenn `bin/docker_run src host.docker.internal:4503 8080` zur Meldung __Warten, bis host.docker.internal verfügbar ist__ führt:

1. Stellen Sie sicher, dass die installierte Docker-Version 18.03 oder höher ist.
1. Möglicherweise haben Sie einen lokalen Rechner eingerichtet, der die Registrierung/Auflösung des Namens `host.docker.internal` verhindert. Verwenden Sie stattdessen Ihre lokale IP.

>[!BEGINTABS]

>[!TAB macOS]

+ Führen Sie `ifconfig` über das Terminal aus und notieren Sie sich die __inet__-Host-IP-Adresse (normalerweise das Gerät __en0__).
+ Führen Sie dann `docker_run` unter Verwendung dieser Host-IP-Adresse aus: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Führen Sie über die Eingabeaufforderung `ipconfig` aus und notieren Sie sich die __IPv4-Adresse__ des Host-Computers.
+ Führen Sie dann `docker_run` unter Verwendung dieser IP-Adresse aus: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux]

+ Führen Sie `ifconfig` über das Terminal aus und notieren Sie sich die __inet__-Host-IP-Adresse (normalerweise das Gerät __en0__).
+ Führen Sie dann `docker_run` unter Verwendung dieser Host-IP-Adresse aus: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Beispiel für einen Fehler

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
+ [Docker-Download](https://www.docker.com/)
+ [Download der AEM-Referenz-Website (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Experience Manager-Dispatcher-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=de)
