---
title: Debugging von Dispatcher-Tools
description: Die Dispatcher-Tools bieten eine containerisierte Apache-Webserver-Umgebung, die verwendet werden kann, um den Dispatcher des AEM-Veröffentlichungsdienstes von AEM as a Cloud Service lokal zu simulieren. Das Debuggen der Protokolle und Cache-Inhalte des Dispatcher-Tools kann entscheidend sein, um sicherzustellen, dass die End-to-End-AEM-Anwendung und unterstützende Cache- und Sicherheitskonfigurationen korrekt sind.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 60
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '230'
ht-degree: 100%

---

# Debugging von Dispatcher-Tools

Die Dispatcher-Tools bieten eine containerisierte Apache-Webserver-Umgebung, die verwendet werden kann, um den Dispatcher des AEM-Veröffentlichungsdienstes von AEM as a Cloud Service lokal zu simulieren.

Das Debuggen der Protokolle und Cache-Inhalte des Dispatcher-Tools kann entscheidend sein, um sicherzustellen, dass die End-to-End-AEM-Anwendung und unterstützende Cache- und Sicherheitskonfigurationen korrekt sind.

>[!NOTE]
>
>Da die Dispatcher-Tools auf Containern basieren, werden bei jedem Neustart die vorherigen Protokolle und Cache-Inhalte zerstört.

## Dispatcher-Tools-Protokolle

Die Dispatcher-Tools-Protokolle sind über den `stdout`- oder den `bin/docker_run`-Befehl oder mit weiteren Details im Docker-Container unter `/etc/https/logs` verfügbar.

Siehe [Dispatcher-Protokolle](./logs.md#dispatcher-logs) für Anweisungen zum direkten Zugriff auf die Protokolle des Docker-Containers der Dispatcher-Tools.

## Dispatcher-Tools-Cache

### Zugreifen auf Protokolle im Docker-Container

Der Dispatcher-Cache kann direkt im Docker-Container unter ` /mnt/var/www/html` aufgerufen werden.

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

### Kopieren der Docker-Protokolle auf das lokale Dateisystem

Dispatcher-Protokolle können aus dem Docker-Container unter `/mnt/var/www/html` in das lokale Dateisystem zur Überprüfung mit Ihren bevorzugten Tools kopiert werden. Beachten Sie, dass es sich hierbei um eine temporäre Kopie handelt und keine Echtzeitaktualisierungen des Caches bereitgestellt werden.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
