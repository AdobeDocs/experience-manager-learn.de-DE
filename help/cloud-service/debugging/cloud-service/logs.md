---
title: Protokolle
description: Protokolle sind als Cloud Service für das Debugging AEM Anwendungen in AEM fungieren, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: e2473a1584ccf315fffe5b93cb6afaed506fdbce
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 3%

---


# Debugging von AEM als Cloud Service mithilfe von Protokollen

Protokolle sind als Cloud Service für das Debugging AEM Anwendungen in AEM fungieren, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.

Die gesamte Protokollaktivität für den AEM-Dienst einer bestimmten Umgebung (Autoren-, Veröffentlichungs-/Veröffentlichungs-Dispatcher) wird in einer Protokolldatei konsolidiert, selbst wenn verschiedene Pods innerhalb dieses Dienstes die Protokollanweisungen generieren.

Pod-IDs werden in jeder Protokollanweisung bereitgestellt und ermöglichen das Filtern oder Sortieren von Protokollanweisungen. Pod-IDs haben das Format:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Beispiel: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Benutzerdefinierte Protokolldateien

AEM as a Cloud Services unterstützt keine benutzerdefinierten Protokolldateien, unterstützt jedoch die benutzerdefinierte Protokollierung.

Damit Java-Protokolle in AEM als Cloud Service verfügbar sind (über [Cloud Manager](#cloud-manager) oder [Adobe I/O CLI](#aio)), müssen benutzerdefinierte Protokollanweisungen in `error.log` geschrieben werden. Protokolle, die in benutzerdefinierte benannte Protokolle wie `example.log` geschrieben wurden, sind von AEM als Cloud Service nicht zugänglich.

Protokolle können mit einer OSGi-Konfigurationseigenschaft Sling LogManager in den `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`-Dateien der Anwendung in die `error.log` geschrieben werden.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Protokolle des AEM-Autoren- und Veröffentlichungsdienstes

Sowohl der AEM-Autoren- als auch der Veröffentlichungsdienst stellen AEM Laufzeitserver-Protokolle bereit:

+ `aemerror` ist das Java-Fehlerprotokoll (zu finden unter  `/crx-quickstart/logs/error.log` dem lokalen Schnellstart des AEM SDK). Im Folgenden finden Sie die [empfohlenen Protokollebenen](#log-levels) für benutzerdefinierte Logger nach Umgebungstyp:
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemaccess` listet HTTP-Anforderungen an den AEM-Dienst mit Details auf
+ `aemrequest` listet HTTP-Anforderungen an AEM Dienst und die entsprechende HTTP-Antwort auf

## AEM Publish Dispatcher-Protokolle

Nur der AEM Publish Dispatcher stellt Apache-Webserver- und Dispatcher-Protokolle bereit, da diese Aspekte nur in der AEM-Veröffentlichungsstufe und nicht in der AEM-Autorenstufe vorhanden sind.

+ `httpdaccess` listet HTTP-Anforderungen auf, die an den Apache-Webserver/Dispatcher des AEM-Dienstes gesendet wurden.
+ `httperror`  listet Protokollmeldungen vom Apache-Webserver auf und hilft beim Debugging unterstützter Apache-Module wie  `mod_rewrite`.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemdispatcher` listet Protokollmeldungen aus den Dispatcher-Modulen auf, einschließlich Filtern und Verarbeiten aus Cache-Nachrichten.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager ermöglicht den täglichen Download von Protokollen über die Aktion &quot;Protokolle herunterladen&quot;einer Umgebung.

![Cloud Manager - Download-Protokolle](./assets/logs/download-logs.png)

Diese Protokolle können über beliebige Protokollanalysewerkzeuge heruntergeladen und überprüft werden.

## Adobe I/O-CLI mit Cloud Manager-Plug-in{#aio}

Adobe Cloud Manager unterstützt den Zugriff auf AEM als Cloud Service-Logs über die [Adobe I/O-CLI](https://github.com/adobe/aio-cli) mit dem [Cloud Manager-Plugin für die Adobe I/O-CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Richten Sie zunächst [die Adobe I/O mit dem Cloud Manager-Plugin](../../local-development-environment/development-tools.md#aio-cli) ein.

Stellen Sie sicher, dass die relevante Programm-ID und die Umgebungs-ID identifiziert wurden, und verwenden Sie [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid), um die Protokolloptionen aufzulisten, die für [tail](#aio-cli-tail-logs)- oder [download](#aio-cli-download-logs) -Protokolle verwendet werden.

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

### Versandlogs{#aio-cli-tail-logs}

Adobe I/O CLI bietet die Möglichkeit, Protokolle in Echtzeit von AEM als Cloud Service mithilfe des Befehls [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) zu verfolgen. Das Tailing ist nützlich, um Echtzeit-Protokollaktivitäten zu beobachten, wenn Aktionen auf der AEM als Cloud Service-Umgebung ausgeführt werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere Befehlszeilen-Tools wie `grep` können gemeinsam mit `tail-logs` verwendet werden, um Protokolleinträge zu isolieren, die von Interesse sind, z. B.:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... zeigt nur Protokollanweisungen an, die von `com.example.MySlingModel` generiert wurden oder diese Zeichenfolge enthalten.

### Protokolle herunterladen{#aio-cli-download-logs}

Adobe I/O CLI bietet die Möglichkeit, Protokolle von AEM als Cloud Service mit dem Befehl [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days) herunterzuladen. Dies liefert dasselbe Endergebnis wie das Herunterladen der Protokolle aus der Cloud Manager-Web-Benutzeroberfläche, wobei der Unterschied darin besteht, dass der Befehl `download-logs` die Protokolle über Tage hinweg konsolidiert, basierend darauf, wie viele Tage Protokolle angefordert werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Grundlegendes zu Protokollen

Logs AEM als Cloud Service haben mehrere Pods, die Protokolleinträge in sie schreiben. Da mehrere AEM Instanzen in dieselbe Protokolldatei schreiben, ist es wichtig, zu verstehen, wie beim Debugging analysiert und Rauschen reduziert werden kann. Zur Erläuterung wird das folgende `aemerror`-Protokollfragment verwendet:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Mithilfe der Pod-IDs, dem Datenpunkt nach dem Datum und der Uhrzeit, können die Protokolle von Pod oder AEM Instanz im Dienst erfasst werden, wodurch die Verfolgung und das Verständnis der Codeausführung erleichtert werden.

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

Allgemeine Anleitungen der Adobe zu den Protokollebenen pro AEM als Cloud Service-Umgebung sind:

+ Lokale Entwicklung (AEM SDK): `DEBUG`
+ Entwicklung: `DEBUG`
+ Staging: `WARN`
+ Produktion: `ERROR`

Die beste Protokollebene für jeden Umgebungstyp wird mit AEM als Cloud Service festgelegt. Die Protokollebenen werden im Code beibehalten

+ Java-Protokollkonfigurationen werden in OSGi-Konfigurationen beibehalten
+ Apache-Webserver und Dispatcher-Protokollebenen im Dispatcher-Projekt

...und erfordern daher eine Implementierung, um sich zu ändern.

### Umgebungsspezifische Variablen zum Festlegen von Java-Protokollebenen

Eine Alternative zum Festlegen statischer bekannter Java-Protokollebenen für jede Umgebung besteht darin, AEM als umgebungsspezifische Variablen [a1/> des Cloud Service zu verwenden, um die Protokollebenen zu parametrisieren, sodass die Werte dynamisch über die Adobe I/O-CLI [mit dem Cloud Manager-Plug-in](#aio-cli) geändert werden können.](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)

Dazu müssen die OSGi-Protokollierungskonfigurationen aktualisiert werden, um die umgebungsspezifischen Variablenplatzhalter zu verwenden. [Standardwerte ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) für Protokollebenen sollten gemäß den  [Adobe-Empfehlungen](#log-levels) festgelegt werden. Beispiel:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Dieser Ansatz hat Nachteile, die berücksichtigt werden müssen:

+ [Eine begrenzte Anzahl von Umgebungsvariablen ist zulässig](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables), und das Erstellen einer Variablen zur Verwaltung der Protokollebene verwendet eine.
+ Umgebungsvariablen können nur programmgesteuert über [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) oder [Cloud Manager-HTTP-APIs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties) verwaltet werden.
+ Änderungen an Umgebungsvariablen müssen von einem unterstützten Tool manuell zurückgesetzt werden. Wenn Sie vergessen, eine Umgebung mit hohem Traffic, wie z. B. die Produktion, auf eine weniger ausführliche Protokollebene zurückzusetzen, werden die Protokolle möglicherweise überschwemmt und AEM Leistung beeinträchtigt.

_Umgebungsspezifische Variablen funktionieren nicht für Apache-Webserver- oder Dispatcher-Protokollkonfigurationen, da diese nicht über die OSGi-Konfiguration konfiguriert sind._
