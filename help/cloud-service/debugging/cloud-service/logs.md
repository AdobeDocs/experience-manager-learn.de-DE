---
title: Protokolle
description: Protokolle dienen als erste Anlaufstelle für das Debugging von AEM Anwendungen in AEM as a Cloud Service, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM-Anwendung ab.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 277
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 98%

---

# Debugging von AEM as a Cloud Service mithilfe von Protokollen

Protokolle dienen als erste Anlaufstelle für das Debugging von AEM Anwendungen in AEM as a Cloud Service, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM-Anwendung ab.

Die gesamte Protokollaktivität für den AEM-Service einer bestimmten Umgebung (Author, Publish/Publish Dispatcher) wird in einer einzigen Protokolldatei konsolidiert, selbst wenn verschiedene Pods innerhalb dieses Service die Protokollanweisungen generieren.

Pod-IDs werden in jeder Protokollanweisung bereitgestellt und ermöglichen das Filtern oder Sortieren von Protokollanweisungen. Pod-IDs haben folgendes Format:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Beispiel: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Benutzerdefinierte Protokolldateien

AEM as a Cloud Service unterstützt zwar keine benutzerdefinierten Protokolldateien, dafür aber die benutzerdefinierte Protokollierung.

Damit Java-Protokolle in AEM as a Cloud Service verfügbar sind (über [Cloud Manager](#cloud-manager) oder [Adobe I/O-CLI](#aio)), müssen benutzerdefinierte Protokollanweisungen erstellt werden. `error.log`. Auf Protokolle, die in benutzerdefinierte benannte Protokolle geschrieben werden, z. B. `example.log`, kann von AEM as a Cloud Service aus nicht zugegriffen werden.

Protokolle können unter Verwendung einer Sling LogManager-OSGi-Konfigurationseigenschaft in den Dateien `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` der Anwendung in `error.log` geschrieben werden.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Protokolle des AEM Author- und Publish-Service

Sowohl der AEM Author- als auch der AEM Publish-Service stellen AEM-Runtime-Server-Protokolle bereit:

+ `aemerror` ist das Java-Fehlerprotokoll (zu finden unter `/crx-quickstart/logs/error.log` im lokalen Schnellstart des AEM SDK). Folgendes sind die [empfohlenen Protokollebenen](#log-levels) für benutzerdefinierte Logger nach Umgebungstyp:
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemaccess` listet HTTP-Anfragen an den AEM-Service mit Details auf.
+ `aemrequest` listet HTTP-Anfragen an den AEM-Service und die entsprechenden HTTP-Antworten auf.

## AEM Publish Dispatcher-Protokolle

Nur AEM Publish Dispatcher stellt Apache-Webserver- und Dispatcher-Protokolle bereit, da diese Aspekte nur auf AEM Publish-Ebene und nicht auf AEM Author-Ebene vorhanden sind.

+ `httpdaccess` listet HTTP-Anfragen auf, die an den Apache-Webserver/Dispatcher des AEM-Service gestellt wurden.
+ `httperror` listet Protokollmeldungen vom Apache-Webserver auf und hilft beim Debugging unterstützter Apache-Module wie `mod_rewrite`.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemdispatcher` listet Protokollmeldungen aus den Dispatcher-Modulen auf, einschließlich Filtern und Bereitstellen aus Cache-Nachrichten.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager ermöglicht den tageweisen Download von Protokollen über die Aktion „Protokolle herunterladen“ einer Umgebung.

![Cloud Manager – Herunterladen von Protokollen](./assets/logs/download-logs.png)

Diese Protokolle können über beliebige Protokollanalyse-Tools heruntergeladen und überprüft werden.

## Adobe I/O-CLI mit Cloud Manager-Plug-in{#aio}

Adobe Cloud Manager unterstützt den Zugriff auf AEM as a Cloud Service-Protokolle über die [Adobe I/O-CLI](https://github.com/adobe/aio-cli) mit dem [Cloud Manager-Plug-in für die Adobe I/O-CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

[Richten Sie zunächst Adobe I/O mit dem Cloud Manager-Plug-in ein](../../local-development-environment/development-tools.md#aio-cli).

Stellen Sie sicher, dass die entsprechende Programm-ID und Umgebungs-ID identifiziert wurden, und listen Sie mit [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) die Protokolloptionen auf, die zum [Tailing](#aio-cli-tail-logs) oder [Herunterladen](#aio-cli-download-logs) von Protokollen verwendet werden.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Tailing von Protokollen{#aio-cli-tail-logs}

Die Adobe I/O-CLI bietet mithilfe des Befehls [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) die Möglichkeit zum Echtzeit-Tailing von Protokollen von AEM as a Cloud Service. Das Tailing ist nützlich, um Echtzeit-Protokollaktivitäten zu beobachten, während Aktionen in der AEM as a Cloud Service-Umgebung ausgeführt werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere Befehlszeilenwerkzeuge wie `grep` können zusammen mit `tail-logs` zur Isolierung relevanter Protokolleinträgen verwendet werden, z. B.:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

Es lassen sich aber auch nur die Protokollanweisungen anzeigen, die von `com.example.MySlingModel` generiert wurden oder diese Zeichenfolge enthalten.

### Herunterladen von Protokollen{#aio-cli-download-logs}

Die Adobe I/O-CLI bietet die Möglichkeit, Protokolle von AEM as a Cloud Service mithilfe des Befehls [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days) herunterzuladen. Das Endergebnis ist dabei dasselbe wie beim Herunterladen der Protokolle über die Cloud Manager-Web-Benutzeroberfläche, mit folgendem Unterschied: Über den Befehl `download-logs` werden Protokolle tageübergreifend konsolidiert, ausgehend davon, wie viele Protokolltage angefordert werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Grundlegendes zu Protokollen

Protokolle in AEM as a Cloud Service verfügen über mehrere Pods, durch die Protokolleinträge in die Protokolle geschrieben werden. Da mehrere AEM-Instanzen in dieselbe Protokolldatei schreiben, ist es wichtig, zu verstehen, wie beim Debugging eine Analyse durchgeführt und Rauschen reduziert werden kann. Zur Erläuterung wird das folgende `aemerror`-Protokoll-Snippet verwendet:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Mithilfe der Pod-IDs, dem Datenpunkt nach dem Datum und der Uhrzeit, können die Protokolle nach Pod oder AEM-Instanz im Service zusammengestellt werden, sodass die Ausführung des Codes einfacher verfolgt und nachvollzogen werden kann.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Empfohlene Protokollebenen{#log-levels}

Adobe empfiehlt im Allgemeinen die folgenden Protokollebenen nach AEM as a Cloud Service-Umgebung:

+ Lokale Entwicklung (AEM SDK): `DEBUG`
+ Entwicklung: `DEBUG`
+ Staging: `WARN`
+ Produktion: `ERROR`

Die optimale Protokollebene für jeden Umgebungstyp wird mit AEM as a Cloud Service festgelegt. Die Protokollebenen werden dabei im Code beibehalten.

+ Java-Protokollkonfigurationen werden in OSGi-Konfigurationen beibehalten.
+ Apache-Webserver- und Dispatcher-Protokollebenen werden im Dispatcher-Projekt erfasst.

Daher ist zur Änderung eine Bereitstellung erforderlich.

### Umgebungsspezifische Variablen zum Festlegen von Java-Protokollebenen

Eine Alternative zum Festlegen bekannter, statischer Java-Protokollebenen für jede Umgebung besteht darin, Protokollebenen mithilfe [umgebungsspezifischer Variablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#environment-specific-configuration-values) von AEM als Cloud Service zu parametrisieren, sodass die Werte dynamisch über die [Adobe I/O-CLI mit dem Cloud Manager-Plug-in](#aio-cli) geändert werden können.

Die OSGi-Protokollierungskonfigurationen müssen aktualisiert werden, um die umgebungsspezifischen Variablenplatzhalter verwenden zu können. Die [Standardwerte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#default-values) für die Protokollierungsebenen sollten gemäß den [Adobe-Empfehlungen](#log-levels) festgelegt werden. Zum Beispiel:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Dieser Ansatz hat Nachteile, die berücksichtigt werden müssen:

+ [Es ist nur eine begrenzte Anzahl von Umgebungsvariablen zulässig](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#number-of-variables), und beim Erstellen einer Variablen zur Verwaltung der Protokollebene wird eine verwendet.
+ Umgebungsvariablen können programmgesteuert über [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=de), [ADOBE I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), und [Cloud Manager-HTTP-APIs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#cloud-manager-api-format-for-setting-properties).
+ Änderungen an Umgebungsvariablen müssen von einem unterstützten Tool manuell zurückgesetzt werden. Wird eine Umgebung mit hohem Traffic, z. B. die Produktionsumgebung, nicht auf eine weniger ausführliche Protokollebene zurückgesetzt, werden die Protokolle möglicherweise mit Einträgen überschwemmt und die AEM-Leistung beeinträchtigt.

_Umgebungsspezifische Variablen funktionieren nicht bei Apache-Webserver- oder Dispatcher-Protokollkonfigurationen, da diese nicht über die OSGi-Konfiguration konfiguriert sind._
