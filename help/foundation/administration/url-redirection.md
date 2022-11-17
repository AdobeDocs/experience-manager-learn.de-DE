---
title: URL-Umleitungen
description: Erfahren Sie mehr über die verschiedenen Optionen, um URL-Umleitungen in AEM durchzuführen.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 3%

---


# URL-Umleitungen

Die Umleitung von URLs ist ein gängiger Aspekt im Rahmen des Website-Vorgangs. Architekten und Administratoren müssen die beste Lösung für die Verwaltung der URL-Umleitungen finden, die Flexibilität bieten und die Bereitstellungszeit für schnelle Umleitungen verkürzen.

Vergewissern Sie sich, dass Sie mit dem [AEM 6.x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) und [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) Infrastrukturen. Die wichtigsten Unterschiede sind:

1.  AEM as a Cloud Service hat [integriertes CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=de)Kunden können jedoch ein CDN (BYOCDN) vor AEM verwalteten CDN bereitstellen.
1.  AEM 6.x, unabhängig davon, ob On-Premise oder Adobe Managed Services (AMS) kein AEM verwaltetes CDN enthält und Kunden ihre eigenen mitbringen müssen.

Die anderen AEM-Dienste (AEM Author/Publish und Dispatcher) sind von der Konzeption her zwischen AEM 6.x und AEM as a Cloud Service.

AEM URL-Umleitungslösungen lauten wie folgt:

|  | Verwaltetes und bereitgestelltes AEM Projektcode | Fähigkeit, Änderungen durch das Marketing-/Content-Team vorzunehmen | AEM als Cloud Service-kompatibel | Wenn eine Weiterleitungsausführung erfolgt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Bei Edge über bringen Sie Ihr eigenes CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ms | Edge/CDN |
| [Apache `mod_rewrite` Regeln als Dispatcher-Konfiguration ](#apache-mod_rewrite-module) | ms | ✘ | ms | Dispatcher |
| [ACS Commons - Umleitungs-Map-Manager](#redirect-map-manager) | ✘ | ms | ✘ | Dispatcher |
| [ACS Commons - Umleitungs-Manager](#redirect-manager) | ✘ | ms | ms | AEM |


## Lösungsoptionen

Im Folgenden finden Sie Lösungsoptionen, um näher am Browser des Website-Besuchers zu sein.

### Bei Edge über eigene CDN

Einige CDN-Dienste bieten Umleitungs-Lösungen auf Edge-Ebene, wodurch die Rundfahrten zum Ursprung reduziert werden. Siehe [Akamai Edge-Weiterleitung](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront-Funktionen](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Wenden Sie sich an Ihren CDN-Dienstanbieter, wenn Sie Fragen zur Umleitungsfunktion auf Edge-Ebene haben.

Die Verwaltung von Weiterleitungen auf Edge- oder CDN-Ebene hat Leistungsvorteile, sie werden jedoch nicht im Rahmen AEM, sondern eher diskreter Projekte verwaltet. Um Probleme zu vermeiden, ist ein gut durchdachter Prozess zur Verwaltung und Einführung von Umleitungsregeln von entscheidender Bedeutung.


### Apache `mod_rewrite` Modul

Eine gängige Lösung verwendet [Apache Module mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Die [AEM Projektarchetyp](https://github.com/adobe/aem-project-archetype) stellt eine Dispatcher-Projektstruktur für beide bereit [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) und [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) Projekte. Die standardmäßigen (unveränderlichen) und benutzerdefinierten Neuschreibungsregeln werden im `conf.d/rewrites` und die Rewrite-Engine für `virtualhosts` , der den Port überwacht `80` via `conf.d/dispatcher_vhost.conf` -Datei. Eine Beispielimplementierung ist im Abschnitt [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

as a Cloud Service werden diese Umleitungsregeln AEM als Teil AEM Codes verwaltet und über Cloud Manager bereitgestellt [Web-Tier-Konfigurationspipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) oder [Vollstack-Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Somit ist Ihr AEM projektspezifischer Prozess für die Verwaltung, Bereitstellung und Verfolgung der Umleitungsregeln im Spiel.

Die meisten CDN-Dienste speichern die HTTP 301- und 302-Weiterleitungen je nach ihren `Cache-Control` oder `Expires` Kopfzeilen. Auf diese Weise können Sie die Umleitung nach der ersten Umleitung, die von Apache/Dispatcher stammt, vermeiden.


### ACS AEM Commons

Es gibt zwei Funktionen in [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) um URL-Umleitungen zu verwalten. Bitte beachten Sie, dass ACS AEM Commons ein von der Community verwaltetes Open-Source-Projekt ist und nicht von Adobe unterstützt wird.

#### Umleitungs-Map-Manager

[Umleitungs-Map-Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) ermöglicht AEM 6.x-Administratoren die einfache Wartung und Veröffentlichung [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) Dateien ohne direkten Zugriff auf den Apache-Webserver oder ohne Neustart des Apache-Webservers. Diese Funktion ermöglicht Benutzern das Erstellen, Aktualisieren und Löschen von Umleitungsregeln aus einer Konsole in AEM, ohne die Hilfe des Entwicklungsteams oder einer AEM Bereitstellung zu benötigen. Umleitungs-Map-Manager ist **AEM as a Cloud Service Kompatibilität**.

#### Umleitungs-Manager

[Umleitungs-Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) ermöglicht es Benutzern in AEM, Umleitungen einfach von AEM zu verwalten und zu veröffentlichen. Die Implementierung basiert auf dem Java™-Servlet-Filter und ist daher ein typischer JVM-Ressourcenverbrauch. Diese Funktion beseitigt auch die Abhängigkeit vom AEM Entwicklungsteam und den AEM Bereitstellungen. Der Umleitungs-Manager ist beides **AEM as a Cloud Service** und **AEM 6.x** kompatibel. Beachten Sie, dass die ursprüngliche umgeleitete Anfrage zwar von seinem AEM-Veröffentlichungsdienst generiert werden muss, um den 301/302 zu generieren, die meisten CDNs jedoch standardmäßig den Cache 301/302, sodass nachfolgende Anfragen an den Edge/CDN weitergeleitet werden können.


## Welche Lösung eignet sich zur Implementierung?

Im Folgenden finden Sie einige Kriterien zur Bestimmung der richtigen Lösung. Darüber hinaus sollte der IT- und Marketing-Prozess Ihres Unternehmens bei der Auswahl der richtigen Lösung helfen.

1. Aktivierung des Marketing-Teams oder der Superuser zur Verwaltung von Weiterleitungsregeln ohne das AEM Entwicklungsteam und AEM Veröffentlichungs- und Bereitstellungszyklen.
1. Der Prozess zur Überprüfung, Verfolgung und Wiederherstellung der Änderungen oder Risikominderung.
1. Verfügbarkeit _Fachkenntnisse in Sachfragen_ für **Bei Edge über CDN-Dienst** Lösung.

