---
title: Debuggen von Dispatcher-Tools
description: Die Dispatcher-Tools bieten eine Container-Apache-Webserver-Umgebung, mit der AEM als lokaler Dispatcher des AEM Publish-Dienstes des Cloud Services simuliert werden können. Das Debuggen der Protokolle und Inhalte der Dispatcher-Tools kann entscheidend sein, um sicherzustellen, dass die End-to-End-AEM-Anwendung und die unterstützten Cache- und Sicherheitskonfigurationen korrekt sind.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: Entwicklung
role: Entwickler
level: Anfänger, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---


# Debuggen von Dispatcher-Tools

Die Dispatcher-Tools bieten eine Container-Apache-Webserver-Umgebung, mit der AEM als lokaler Dispatcher des AEM Publish-Dienstes des Cloud Services simuliert werden können.
Das Debuggen der Protokolle und Inhalte der Dispatcher-Tools kann entscheidend sein, um sicherzustellen, dass die End-to-End-AEM-Anwendung und die unterstützten Cache- und Sicherheitskonfigurationen korrekt sind.

>[!NOTE]
>
>Da Dispatcher-Tools auf Containern basieren, werden bei jedem Neustart vorherige Protokolle und Cache-Inhalte zerstört.

## Dispatcher Tools-Protokolle

Dispatcher Tools-Protokolle sind über den Befehl `stdout` oder `bin/docker_run` oder mit weiteren Details im Docker-Container unter `/etc/https/logs` verfügbar.

Anweisungen zum direkten Zugriff auf die Protokolle der Dispatcher-Tools finden Sie unter [Dispatcher logs](./logs.md#dispatcher-logs).

## Dispatcher Tools-Cache

### Zugriff auf Protokolle im Docker-Container

Der Dispatcher-Cache kann direkt auf den Docker-Container unter ` /mnt/var/www/html` zugreifen.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Kopieren der Docker-Protokolle in das lokale Dateisystem

Dispatcher-Protokolle können aus dem Docker-Container bei `/mnt/var/www/html` in das lokale Dateisystem kopiert werden, um sie mit Ihren Lieblings-Tools zu überprüfen. Beachten Sie, dass es sich hierbei um eine Point-in-Time-Kopie handelt und keine Echtzeit-Aktualisierungen für den Cache bereitgestellt werden.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

