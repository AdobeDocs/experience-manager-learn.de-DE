---
title: Protokolle
description: Protokolle dienen in AEM as a Cloud Service als zentrale Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: eb669d1e2493d9b4a973314ab1323764920ba220
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 3%

---

# Debugging AEM as a Cloud Service mithilfe von Protokollen

Protokolle dienen in AEM as a Cloud Service als zentrale Anlaufstelle für das Debugging AEM Anwendungen, hängen jedoch von einer angemessenen Protokollierung in der bereitgestellten AEM ab.

Die gesamte Protokollaktivität für den AEM-Dienst einer bestimmten Umgebung (Autoren-, Veröffentlichungs-/Veröffentlichungs-Dispatcher) wird in einer Protokolldatei konsolidiert, selbst wenn verschiedene Pods innerhalb dieses Dienstes die Protokollanweisungen generieren.

Pod-IDs werden in jeder Protokollanweisung bereitgestellt und ermöglichen das Filtern oder Sortieren von Protokollanweisungen. Pod-IDs haben das Format:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Beispiel: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Benutzerdefinierte Protokolldateien

AEM as a Cloud Services unterstützt keine benutzerdefinierten Protokolldateien, unterstützt jedoch die benutzerdefinierte Protokollierung.

Für Java-Protokolle, die in AEM as a Cloud Service verfügbar sein sollen (über [Cloud Manager](#cloud-manager) oder [Adobe I/O CLI](#aio)), müssen benutzerdefinierte Protokollanweisungen geschrieben werden. `error.log`. Protokolle, die in benutzerdefinierte benannte Protokolle geschrieben wurden, z. B. `example.log`, ist nicht von AEM as a Cloud Service zugänglich.

Protokolle können in die `error.log` Verwendung einer OSGi-Konfigurationseigenschaft des Sling LogManager in der Anwendungskonfiguration `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` Dateien.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Protokolle des AEM-Autoren- und Veröffentlichungsdienstes

Sowohl der AEM-Autoren- als auch der Veröffentlichungsdienst stellen AEM Laufzeitserver-Protokolle bereit:

+ `aemerror` ist das Java-Fehlerprotokoll (zu finden unter `/crx-quickstart/logs/error.log` im lokalen Schnellstart des AEM SDK). Im Folgenden finden Sie die [empfohlene Protokollebenen](#log-levels) für benutzerdefinierte Logger nach Umgebungstyp:
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemaccess` listet HTTP-Anforderungen an den AEM-Dienst mit Details auf
+ `aemrequest` listet HTTP-Anforderungen an AEM Dienst und die entsprechende HTTP-Antwort auf

## AEM Publish Dispatcher-Protokolle

Nur der AEM Publish Dispatcher stellt Apache-Webserver- und Dispatcher-Protokolle bereit, da diese Aspekte nur in der AEM-Veröffentlichungsstufe und nicht in der AEM-Autorenstufe vorhanden sind.

+ `httpdaccess` listet HTTP-Anforderungen auf, die an den Apache-Webserver/Dispatcher des AEM-Dienstes gesendet wurden.
+ `httperror`  listet Protokollmeldungen vom Apache-Webserver auf und hilft beim Debugging unterstützter Apache-Module, wie `mod_rewrite`.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`
+ `aemdispatcher` listet Protokollmeldungen aus den Dispatcher-Modulen auf, einschließlich Filtern und Verarbeiten aus Cache-Nachrichten.
   + Entwicklung: `DEBUG`
   + Staging: `WARN`
   + Produktion: `ERROR`

## Cloud Manager {#cloud-manager}

Adobe Cloud Manager ermöglicht den täglichen Download von Protokollen über die Aktion &quot;Protokolle herunterladen&quot;einer Umgebung.

![Cloud Manager - Download-Protokolle](./assets/logs/download-logs.png)

Diese Protokolle können über beliebige Protokollanalysewerkzeuge heruntergeladen und überprüft werden.

## Adobe I/O-CLI mit Cloud Manager-Plug-in{#aio}

Adobe Cloud Manager unterstützt den Zugriff auf AEM as a Cloud Service Protokolle über [Adobe I/O CLI](https://github.com/adobe/aio-cli) mit dem [Cloud Manager-Plug-in für die Adobe I/O-CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Erstens: [Einrichten der Adobe I/O mit dem Cloud Manager-Plug-in](../../local-development-environment/development-tools.md#aio-cli).

Stellen Sie sicher, dass die entsprechende Programm-ID und die Umgebungs-ID identifiziert wurden, und verwenden Sie [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) zum Auflisten der Protokolloptionen, mit denen [Longtail](#aio-cli-tail-logs) oder [herunterladen](#aio-cli-download-logs) Protokolle.

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

Adobe I/O CLI bietet die Möglichkeit, Protokolle in Echtzeit von AEM as a Cloud Service zu verfolgen, indem die [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) Befehl. Das Tracking ist nützlich, um Echtzeit-Protokollaktivitäten zu beobachten, während Aktionen in der AEM as a Cloud Service Umgebung ausgeführt werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere Befehlszeilenwerkzeuge, z. B. `grep` kann zusammen mit `tail-logs` zur Isolierung von Protokolleinträgen mit Interesse, z. B.:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... zeigt nur Protokollanweisungen an, die aus `com.example.MySlingModel` oder diese Zeichenfolge enthalten.

### Protokolle herunterladen{#aio-cli-download-logs}

Adobe I/O CLI bietet die Möglichkeit, Protokolle von AEM as a Cloud Service herunterzuladen, indem die [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Dies liefert dasselbe Endergebnis wie das Herunterladen der Protokolle über die Cloud Manager-Web-Benutzeroberfläche, wobei der Unterschied darin besteht, dass `download-logs` -Befehl konsolidiert Protokolle über Tage, basierend darauf, wie viele Tage Protokolle angefordert werden.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Grundlegendes zu Protokollen

Logs in AEM as a Cloud Service haben mehrere Pods, die Protokolleinträge in sie schreiben. Da mehrere AEM Instanzen in dieselbe Protokolldatei schreiben, ist es wichtig, zu verstehen, wie beim Debugging analysiert und Rauschen reduziert werden kann. Im Folgenden wird erläutert, `aemerror` wird ein Protokollausschnitt verwendet:

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

Die allgemeinen Leitlinien der Adobe zu den Protokollebenen AEM as a Cloud Service Umgebungen sind:

+ Lokale Entwicklung (AEM SDK): `DEBUG`
+ Entwicklung: `DEBUG`
+ Staging: `WARN`
+ Produktion: `ERROR`

Die beste Protokollebene für jeden Umgebungstyp wird mit AEM as a Cloud Service festgelegt, die Protokollebenen werden im Code beibehalten

+ Java-Protokollkonfigurationen werden in OSGi-Konfigurationen beibehalten
+ Apache-Webserver und Dispatcher-Protokollebenen im Dispatcher-Projekt

...und erfordern daher eine Implementierung, um sich zu ändern.

### Umgebungsspezifische Variablen zum Festlegen von Java-Protokollebenen

Eine Alternative zum Festlegen statischer bekannter Java-Protokollebenen für jede Umgebung besteht darin, AEM als Cloud Service [Umgebungsspezifische Variablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) zur Parameter der Protokollebenen, sodass die Werte dynamisch über die [Adobe I/O-CLI mit Cloud Manager-Plug-in](#aio-cli).

Dazu müssen die OSGi-Protokollierungskonfigurationen aktualisiert werden, um die umgebungsspezifischen Variablenplatzhalter zu verwenden. [Standardwerte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) für die Protokollierungsstufen sollte gemäß [Empfehlungen der Adobe](#log-levels). Zum Beispiel:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Dieser Ansatz hat Nachteile, die berücksichtigt werden müssen:

+ [Eine begrenzte Anzahl von Umgebungsvariablen ist zulässig.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)und das Erstellen einer Variablen zur Verwaltung der Protokollebene verwendet eine.
+ Umgebungsvariablen können nur programmgesteuert über [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) oder [Cloud Manager-HTTP-APIs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Änderungen an Umgebungsvariablen müssen von einem unterstützten Tool manuell zurückgesetzt werden. Wenn Sie vergessen, eine Umgebung mit hohem Traffic, wie z. B. die Produktion, auf eine weniger ausführliche Protokollebene zurückzusetzen, werden die Protokolle möglicherweise überschwemmt und AEM Leistung beeinträchtigt.

_Umgebungsspezifische Variablen funktionieren nicht für Apache-Webserver- oder Dispatcher-Protokollkonfigurationen, da diese nicht über die OSGi-Konfiguration konfiguriert sind._
