---
title: Debugging von AEM SDK mithilfe von Protokollen
description: Protokolle dienen als erste Anlaufstelle für das Debugging von AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM-Anwendung ab.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 423
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# Debugging von AEM SDK mithilfe von Protokollen

Durch den Zugriff auf die Protokolle des AEM SDK können entweder die lokalen Schnellstart-JARs des AEM SDK oder die Dispatcher-Tools wichtige Einblicke in das Debugging von AEM-Anwendungen bieten.

## AEM-Protokolle

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Protokolle dienen als erste Anlaufstelle für das Debugging von AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM-Anwendung ab. Adobe empfiehlt, die lokale Entwicklung und die Konfigurationen für die AEM as a Cloud Service Dev-Protokollierung so ähnlich wie möglich zu halten. Dadurch wird die Protokollsichtbarkeit für den lokalen Schnellstart des AEM SDK und Entwicklungsumgebungen von AEM as a Cloud Service normalisiert, was das Anpassen von Konfigurationen und Neuimplementierungen reduziert.

Der [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype) konfiguriert die Protokollierung auf der DEBUG-Ebene für die Java-Pakete Ihrer AEM-Anwendung für die lokale Entwicklung über die Sling Logger OSGi-Konfiguration, die Sie unter

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

finden, die in der Datei `error.log` protokolliert wird.

Wenn die Standardprotokollierung für die lokale Entwicklung nicht ausreicht, kann die Ad-hoc-Protokollierung über die Log Support-Web-Konsole in der lokalen AEM SDK Schnellstart-Konsole ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)) konfiguriert werden; es wird jedoch nicht empfohlen, Ad-hoc-Änderungen in Git beizubehalten, es sei denn, genau diese Protokollkonfigurationen sind auch in AEM as a Cloud Service-Entwicklungsumgebungen erforderlich. Beachten Sie, dass über die Log Support-Konsole vorgenommene Änderungen direkt im lokalen Schnellstart-Repository vom AEM SDK übernommen werden.

Java-Protokolleinträge können in der Datei `error.log` angezeigt werden:

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Oft ist es nützlich, `error.log` zu verfolgen, dessen Ausgabe an das Terminal gestreamt wird.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows erfordert [Tail-Anwendungen von Drittanbietern](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) oder die Verwendung des [Get-Content-Befehls von Powershell](https://stackoverflow.com/a/46444596/133936).

## Dispatcher-Protokolle

Dispatcher-Protokolle werden beim Aufruf von `bin/docker_run` auf stdout ausgegeben. Sie können jedoch auch direkt auf die Protokolle im Docker-Container zugreifen.

### Zugreifen auf Protokolle im Docker-Container{#dispatcher-tools-access-logs}

Auf Dispatcher-Protokolle kann direkt im Docker-Container unter `/etc/httpd/logs` zugegriffen werden.

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

_Die `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` muss durch die Ziel-Docker-Container-ID aus dem Befehl `docker ps` ersetzt werden._


### Kopieren der Docker-Protokolle auf das lokale Dateisystem{#dispatcher-tools-copy-logs}

Dispatcher-Protokolle können im Docker-Container unter `/etc/httpd/logs` kopiert werden, sodass Sie sie im lokalen Dateisystem mit Ihrem bevorzugten Log-Analyse-Tool überprüfen können. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt und keine Echtzeitaktualisierungen der Protokolle bereitgestellt werden.

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

_Die `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` muss durch die Ziel-Docker-Container-ID aus dem Befehl `docker ps` ersetzt werden._
