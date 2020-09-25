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
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# Debuggen AEM SDK mithilfe von Protokollen

Durch Zugriff auf die Protokolle des AEM SDK können entweder die lokalen Schnellstart-Jar&#39;s des AEM SDK oder die Dispatcher Tools&#39; wichtige Einblicke in das Debugging AEM Anwendungen bieten.

## AEM Protokolle

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Protokolle dienen als Frontline für das Debugging AEM Anwendungen, sind jedoch von einer angemessenen Anmeldung in der bereitgestellten AEM abhängig. Adobe empfiehlt, die lokalen Entwicklungs- und AEM als Cloud Service-Dev-Protokollierungskonfigurationen so ähnlich wie möglich zu halten, da dadurch die Protokollsichtbarkeit beim lokalen Schnellstart des AEM SDK und AEM als Dev-Umgebung des Cloud Service normalisiert wird, wodurch das Konfigurations-Widget und die erneute Bereitstellung reduziert werden.

Der [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) konfiguriert die Protokollierung auf DEBUG-Ebene für die Java-Pakete Ihrer AEM-Anwendung für die lokale Entwicklung über die Sling Logger OSGi-Konfiguration unter

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

, der sich bei der `error.log`.

Wenn die Standardprotokollierung für die lokale Entwicklung nicht ausreicht, kann die Ad-hoc-Protokollierung über AEM lokale Schnellstart-Protokollunterstützungs-Webkonsole unter ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)) konfiguriert werden. Es wird jedoch nicht empfohlen, Ad-hoc-Änderungen in Git beizubehalten, es sei denn, diese Protokollkonfigurationen werden auch auf AEM als Cloud Service-Dev-Umgebung benötigt. Beachten Sie, dass Änderungen über die Konsole Protokollunterstützung direkt im Repository des AEM SDKs für den lokalen Schnellstart beibehalten werden.

Java-Protokollanweisungen können als Ansicht in der `error.log` Datei verwendet werden:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Oft ist es nützlich, die &quot;Tail&quot; die Ausgabe `error.log` , die ihre Ausgabe an das Terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows erfordert [Drittanbieter-Tail-Anwendungen](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) oder die Verwendung des Get-Content-Befehls [von](https://stackoverflow.com/a/46444596/133936)Powershell.

## Dispatcher-Protokolle

Dispatcher-Protokolle werden ausgegeben, um beim Aufrufen abzustürzen, Protokolle können jedoch direkt mit in der Docker-Datei aufgerufen werden. `bin/docker_run`

### Zugriff auf Protokolle im Docker-Container

Dispatcher-Protokolle können direkt im Docker-Container unter `/etc/httpd/logs`.

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

### Kopieren der Docker-Protokolle in das lokale Dateisystem

Dispatcher-Protokolle können aus dem Docker-Container in `/etc/httpd/logs` das lokale Dateisystem kopiert werden, um sie mit Ihrem Lieblings-Log-Analyse-Tool zu überprüfen. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt, die keine Aktualisierungen in Echtzeit für die Protokolle bereitstellt.

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

