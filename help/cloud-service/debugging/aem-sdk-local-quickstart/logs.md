---
title: Debugging AEM SDK mithilfe von Protokollen
description: Protokolle dienen als erste Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Debugging AEM SDK mithilfe von Protokollen

Durch den Zugriff auf die Protokolle des AEM SDK können entweder die lokalen Schnellstart-JARs des AEM SDK oder die Dispatcher Tools wichtige Einblicke in das Debugging AEM Anwendungen bieten.

## AEM Logs

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Protokolle dienen als erste Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab. Adobe empfiehlt, die lokale Entwicklung und die Konfigurationen für die as a Cloud Service Dev-Protokollierung so ähnlich wie möglich zu halten, da dadurch die Protokollsichtbarkeit für den lokalen Schnellstart des AEM SDK und AEM Entwicklungsumgebungen von as a Cloud Service normalisiert wird, was das Anpassen und die Neuimplementierung reduziert.

Die [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) konfiguriert die Protokollierung auf DEBUG-Ebene für die Java-Pakete Ihrer AEM-Anwendung für die lokale Entwicklung über die Sling Logger OSGi-Konfiguration unter

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

, der sich bei der `error.log`.

Wenn die Standardprotokollierung für die lokale Entwicklung nicht ausreicht, kann die Ad-hoc-Protokollierung über die Web-Konsole &quot;Log Support&quot;AEM SDK in der lokalen Schnellstart-Konsole ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), es wird jedoch nicht empfohlen, dass Ad-hoc-Änderungen in Git persistiert werden, es sei denn, diese Protokollkonfigurationen sind auch in AEM as a Cloud Service Entwicklungsumgebungen erforderlich. Beachten Sie, dass Änderungen über die Konsole &quot;Log Support&quot;direkt im Repository des lokalen Schnellstarts des AEM SDK persistiert werden.

Java-Protokolleinträge können im `error.log` Datei:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Oft ist es nützlich, die `error.log` , das seine Ausgabe an das Terminal streamt.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows erfordert [Tail-Anwendungen von Drittanbietern](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) oder die Verwendung [Get-Content-Befehl von Powershell](https://stackoverflow.com/a/46444596/133936).

## Dispatcher-Protokolle

Dispatcher-Protokolle werden ausgegeben, wenn `bin/docker_run` aufgerufen wird, können jedoch Protokolle direkt mit in dem Docker-enthalten sein.

### Zugriff auf Protokolle im Docker-Container{#dispatcher-tools-access-logs}

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

_Die `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` muss durch die Ziel-Docker-CONTAINER-ID ersetzt werden, die aus der Liste `docker ps` Befehl._


### Kopieren der Docker-Protokolle in das lokale Dateisystem{#dispatcher-tools-copy-logs}

Dispatcher-Protokolle können aus dem Docker-Container unter kopiert werden. `/etc/httpd/logs` in das lokale Dateisystem zur Überprüfung mit Ihrem bevorzugten Log-Analyse-Tool. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt und keine Echtzeitaktualisierungen der Protokolle bereitgestellt werden.

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

_Die `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` muss durch die Ziel-Docker-CONTAINER-ID ersetzt werden, die aus der Liste `docker ps` Befehl._
