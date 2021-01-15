---
title: Debuggen AEM SDK mithilfe von Protokollen
description: Protokolle dienen als Frontline für das Debugging AEM Anwendungen, sind jedoch von einer angemessenen Anmeldung in der bereitgestellten AEM abhängig.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
translation-type: tm+mt
source-git-commit: 178ba3dbcb6f2050a9c56303bbabbcfcbead3e79
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Debuggen AEM SDK mithilfe von Protokollen

Durch Zugriff auf die Protokolle des AEM SDK können entweder die lokalen Schnellstart-Jar&#39;s des AEM SDK oder die Dispatcher Tools&#39; wichtige Einblicke in das Debugging AEM Anwendungen bieten.

## AEM Protokolle

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Protokolle dienen als Frontline für das Debugging AEM Anwendungen, sind jedoch von einer angemessenen Anmeldung in der bereitgestellten AEM abhängig. Adobe empfiehlt, die lokalen Entwicklungs- und AEM als Cloud Service-Dev-Protokollierungskonfigurationen so ähnlich wie möglich zu halten, da dadurch die Protokollsichtbarkeit beim lokalen Schnellstart des AEM SDK und AEM als Dev-Umgebung des Cloud Service normalisiert wird, wodurch das Konfigurations-Widget und die erneute Bereitstellung reduziert werden.

Der [AEM-Projektarchetype](https://github.com/adobe/aem-project-archetype) konfiguriert die Protokollierung auf DEBUG-Ebene für die Java-Pakete Ihrer AEM-Anwendung für die lokale Entwicklung über die Sling Logger OSGi-Konfiguration unter

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

, der sich bei `error.log` anmeldet.

Wenn die Standardprotokollierung für die lokale Entwicklung nicht ausreicht, kann die Ad-hoc-Protokollierung über AEM lokale Schnellstart-Protokollunterstützungs-Webkonsole von SDK konfiguriert werden, unter ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Es wird jedoch nicht empfohlen, dass Ad-hoc-Änderungen in Git beibehalten werden, es sei denn, diese Protokollkonfigurationen sind auch für AEM Cloud Service-Dev-Umgebung erforderlich. Beachten Sie, dass Änderungen über die Konsole Protokollunterstützung direkt im Repository des AEM SDKs für den lokalen Schnellstart beibehalten werden.

Java-Protokollanweisungen können in der Datei `error.log` Ansicht sein:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Oft ist es nützlich, die `error.log` zu &quot;schwinden&quot;, die ihre Ausgabe an das Terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Für Windows sind [Tail-Anwendungen von Drittanbietern](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) oder die Verwendung des Befehls [Get-Content von Powershell](https://stackoverflow.com/a/46444596/133936) erforderlich.

## Dispatcher-Protokolle

Dispatcher-Protokolle werden ausgegeben, um abzustürzen, wenn `bin/docker_run` aufgerufen wird. Protokolle können jedoch direkt mit in der Docker-Datei aufgerufen werden.

### Zugriff auf Protokolle im Container Docker{#dispatcher-tools-access-logs}

Dispatcher-Protokolle können direkt auf den Docker-Container unter `/etc/httpd/logs` zugreifen.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_Das  `<CONTAINER ID>` in  `docker exec -it <CONTAINER ID> /bin/sh` muss durch die Zielgruppe Docker CONTAINER-ID ersetzt werden, die im  `docker ps` Befehl aufgeführt ist._


### Kopieren der Docker-Protokolle in das lokale Dateisystem{#dispatcher-tools-copy-logs}

Dispatcher-Protokolle können aus dem Docker-Container unter `/etc/httpd/logs` in das lokale Dateisystem kopiert werden, um sie mit Ihrem bevorzugten Tool zur Analyse von Protokolldateien zu überprüfen. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt, die keine Aktualisierungen in Echtzeit für die Protokolle bereitstellt.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_Das  `<CONTAINER_ID>` in  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` muss durch die Zielgruppe Docker CONTAINER-ID ersetzt werden, die im  `docker ps` Befehl aufgeführt ist._
