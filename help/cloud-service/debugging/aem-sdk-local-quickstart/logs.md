---
title: Debugging AEM SDK mithilfe von Protokollen
description: Protokolle dienen als erste Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Entwicklung
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 3%

---


# Debugging AEM SDK mithilfe von Protokollen

Durch den Zugriff auf die Protokolle des AEM SDK können entweder die lokalen Schnellstart-JARs des AEM SDK oder die Dispatcher Tools wichtige Einblicke in das Debugging AEM Anwendungen bieten.

## AEM Logs

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Protokolle dienen als erste Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab. Adobe empfiehlt, die lokale Entwicklung und AEM als Cloud Service-Protokollierungskonfigurationen so ähnlich wie möglich zu halten, da dadurch die Protokollanzeige für den lokalen Schnellstart des AEM SDK und AEM als Entwicklungsumgebungen eines Cloud Service normalisiert wird, wodurch das Konfigurations-Widget und die Neubereitstellung reduziert werden.

Der [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) konfiguriert die Protokollierung auf DEBUG-Ebene für die Java-Pakete Ihrer AEM-Anwendung für die lokale Entwicklung über die Sling Logger-OSGi-Konfiguration unter

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

, der sich zum `error.log` meldet.

Wenn die Standardprotokollierung für die lokale Entwicklung nicht ausreichend ist, kann die Ad-hoc-Protokollierung über AEM lokale Schnellstart-Web-Konsole Protokollunterstützung des SDK unter ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)) konfiguriert werden. Es wird jedoch nicht empfohlen, Ad-hoc-Änderungen in Git zu persistieren, es sei denn, diese Protokollkonfigurationen sind auch in AEM Entwicklungsumgebungen des Cloud Service erforderlich. Beachten Sie, dass Änderungen über die Konsole &quot;Log Support&quot;direkt im Repository des lokalen Schnellstarts des AEM SDK persistiert werden.

Java-Protokollanweisungen können in der Datei `error.log` angezeigt werden:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Oft ist es nützlich, die `error.log` zu &quot;schwingen&quot;, die ihre Ausgabe an das Terminal streamt.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows erfordert [Tail-Anwendungen von Drittanbietern](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) oder die Verwendung des Befehls [Get-Content von Powershell](https://stackoverflow.com/a/46444596/133936).

## Dispatcher-Protokolle

Dispatcher-Protokolle werden ausgegeben, um abzustürzen, wenn `bin/docker_run` aufgerufen wird. Protokolle können jedoch direkt mit in den Docker-enthalten sein.

### Zugriff auf Protokolle im Docker-Container{#dispatcher-tools-access-logs}

Dispatcher-Protokolle können direkt im Docker-Container unter `/etc/httpd/logs` aufrufen.

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

_Die  `<CONTAINER ID>` in  `docker exec -it <CONTAINER ID> /bin/sh` muss durch die Ziel-Docker-CONTAINER-ID ersetzt werden, die vom  `docker ps` Befehl aufgeführt wird._


### Kopieren der Docker-Protokolle in das lokale Dateisystem{#dispatcher-tools-copy-logs}

Dispatcher-Protokolle können mit Ihrem bevorzugten Protokollanalysewerkzeug aus dem Docker-Container unter `/etc/httpd/logs` in das lokale Dateisystem kopiert werden, um sie zu überprüfen. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt und keine Echtzeitaktualisierungen der Protokolle bereitgestellt werden.

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

_Die  `<CONTAINER_ID>` in  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` muss durch die Ziel-Docker-CONTAINER-ID ersetzt werden, die vom  `docker ps` Befehl aufgeführt wird._
