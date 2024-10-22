---
title: URL-Umleitungen
description: Erfahren Sie mehr über die verschiedenen Optionen, um URL-Umleitungen in AEM vorzunehmen.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2024-10-22T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 9e093e87c8c369ddd750be4a7dc30e2bf86495d5
workflow-type: tm+mt
source-wordcount: '878'
ht-degree: 95%

---

# URL-Umleitungen

Die Umleitung von URLs ist ein gängiger Aspekt der Website-Vorgänge. Architektinnen, Architekten und Admins müssen die beste Lösung für die Verwaltung der URL-Umleitungen finden, die Flexibilität bietet und die Bereitstellungszeit für schnelle Umleitungen verkürzt.

Stellen Sie sicher, dass Sie mit [AEM (6.x) alias AEM Classic](https://experienceleague.adobe.com/de/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) und der [AEM as a Cloud Service](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/overview/architecture)-Infrastruktur vertraut sind. Die wichtigsten Unterschiede sind:

1. AEM as a Cloud Service verfügt über ein [integriertes CDN](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn). Kundinnen und Kunden können jedoch ein CDN (BYOCDN) vor einem von AEM verwalteten CDN bereitstellen.
1. AEM 6.x enthält kein von AEM verwaltetes CDN, auf Kundenseite muss also selbst eines bereitgestellt werden – unabhängig davon, ob es sich um On-Premise oder Adobe Managed Services (AMS) handelt.

Die anderen AEM-Dienste (AEM Author/Publish und Dispatcher) sind ansonsten von der Konzeption her ähnlich wie AEM 6.x und AEM as a Cloud Service.

Es gibt folgende URL-Umleitungslösungen in AEM:

|                                                   | Verwalteter und bereitgestellter AEM-Projekt-Code | Änderungen können durch das Marketing-/Content-Team vorgenommen werden | Kompatibel mit AEM as a Cloud Service | Wenn eine Weiterleitungsausführung erfolgt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Am Edge über AEM-verwaltetes CDN](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integriert) |
| [Am Edge über Ihr eigenes CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite`-Regeln als Dispatcher-Konfiguration](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons – Redirect Map Manager](#redirect-map-manager) | ✘ | ✔ | ✔ | Dispatcher |
| [ACS Commons – Redirect Manager](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [Die `Redirect`-Seiteneigenschaft](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Lösungsoptionen

Die folgenden Lösungsoptionen sind in der Reihenfolge ihrer Nähe zum Browser der Website-Besuchenden aufgeführt.

### Am Edge über AEM-verwaltetes CDN {#at-edge-via-aem-managed-cdn}

Diese Option steht nur AEM as a Cloud Service-Kundinnen und -Kunden zur Verfügung.

Das [AEM-verwaltete CDN](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) bietet eine Weiterleitungslösung auf Edge-Ebene, wodurch Rundreisen zum Ursprung reduziert werden. Die Funktion [Client-seitige Weiterleitungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) ermöglicht Ihnen, die Weiterleitungsregeln im AEM-Projekt-Code zu konfigurieren und mithilfe der [Konfigurations-Pipeline](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) zu implementieren. Die CDN-Konfigurationsdatei (`cdn.yaml`) darf nicht größer sein als 100 KB.

Die Verwaltung von Weiterleitungen auf Edge- oder CDN-Ebene hat Leistungsvorteile.

### Am Edge über Ihr eigenes CDN

Einige CDN-Dienste bieten Weiterleitungslösungen auf Edge-Ebene, wodurch Rundreisen zum Ursprung reduziert werden. Weitere Informationen finden Sie unter [Akamai Edge Redirector](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector) und [AWS CloudFront Functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Wenden Sie sich an Ihren CDN-Dienstanbieter, wenn Sie Fragen zur Umleitungsfunktion auf Edge-Ebene haben.

Die Verwaltung von Umleitungen auf Edge- oder CDN-Ebene hat Leistungsvorteile, sie werden jedoch nicht im Rahmen von AEM, sondern vielmehr diskreter Projekte verwaltet. Um Probleme zu vermeiden, ist ein gut definierter Prozess zur Verwaltung und Einführung von Weiterleitungsregeln von entscheidender Bedeutung.


### `mod_rewrite`-Apache-Modul

Eine gängige Lösung ist die Verwendung des [Apache-Moduls „mod_rewrite“](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Der [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype) bietet eine Dispatcher-Projektstruktur für das [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure)- und [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure)-Projekt. Die standardmäßigen (unveränderlichen) und benutzerdefinierten Neuschreibungsregeln werden im Ordner `conf.d/rewrites` definiert und die Rewrite-Engine ist für `virtualhosts` aktiviert, die den Port `80` über die Datei `conf.d/dispatcher_vhost.conf` überwachen. Eine Beispielimplementierung ist im [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites) verfügbar.

In AEM as a Cloud Service werden diese Weiterleitungsregeln als Teil des AEM-Codes verwaltet und über die [Konfigurations-Pipeline für die Web-Ebene](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) oder die [Full Stack Pipeline](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) von Cloud Manager bereitgestellt. Somit ist Ihr AEM-projektspezifischer Prozess für die Verwaltung, Bereitstellung und Verfolgung der Weiterleitungsregeln mit im Spiel.

Die meisten CDN-Dienste speichern die HTTP-301- und -302-Umleitungen abhängig von ihren `Cache-Control`- oder `Expires`-Headern im Cache. Auf diese Weise können Sie die Rundreise nach der ersten Weiterleitung von Apache/vom Dispatcher vermeiden.


### ACS AEM Commons

Es gibt zwei Funktionen in [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/), um URL-Umleitungen zu verwalten. Hinweis: ACS AEM Commons ist ein von der Community verwaltetes Open-Source-Projekt und wird nicht von Adobe unterstützt.

#### Redirect Map Manager

Mit dem Umleitungs-Map-Manager ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) können AEM Administratoren einfach [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html)-Dateien verwalten und veröffentlichen, ohne direkt auf den Apache-Webserver zugreifen zu müssen oder einen Neustart des Apache-Webservers erforderlich zu machen. [ Mit dieser Funktion können Benutzende Weiterleitungsregeln über eine Konsole in AEM erstellen, aktualisieren und löschen, ohne dabei auf Hilfe durch das Entwicklungs-Team oder eine AEM-Bereitstellung angewiesen zu sein. Der Umleitungs-Manager ist sowohl mit **AEM as a Cloud Service** als auch mit **AEM 6.x** kompatibel.

#### Redirect Manager

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) ermöglicht es Benutzenden in AEM, Umleitungen von AEM einfach zu verwalten und zu veröffentlichen. Die Implementierung basiert auf dem Java™-Servlet-Filter, es besteht also ein typischer JVM-Ressourcenverbrauch. Diese Funktion beseitigt auch die Abhängigkeit vom AEM-Entwicklungs-Team und von den AEM-Bereitstellungen. Redirect Manager ist mit **AEM as a Cloud Service** und **AEM 6.x** kompatibel. Während die ursprüngliche, weitergeleitete Anfrage auf den AEM Publish-Service abzielen muss, speichern (die meisten) CDNs 301/302 im Cache, um 301/302 zu generieren, sodass eine Edge-/CDN-Weiterleitung nachfolgender Anfragen möglich ist.

### Die `Redirect`-Seiteneigenschaft

Die vorkonfigurierte `Redirect`-Seiteneigenschaft der [Registerkarte „Erweitert“](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html?lang=de) ermöglicht es Inhaltsautorinnen und Inhaltsautoren, den Umleitungsort für die aktuelle Seite zu definieren. Diese Lösung eignet sich am besten für seitenbasierte Umleitungsszenarien und verfügt nicht über einen zentralen Ort, an dem die Seitenumleitungen angezeigt und verwaltet werden können.

## Welche Lösung eignet sich zur Implementierung?

Im Folgenden finden Sie einige Kriterien zur Bestimmung der richtigen Lösung. Darüber hinaus sollte der IT- und Marketing-Prozess Ihrer Organisation bei der Auswahl der richtigen Lösung helfen.

1. Aktivierung des Marketing-Teams oder der Superuser zur Verwaltung von Umleitungsregeln ohne das AEM-Entwicklungs-Team und die AEM-Bereitstellungen
1. Der Prozess zum Verwalten, Überprüfen, Nachverfolgen und Zurücksetzen der Änderungen oder Risikominderung
1. Verfügbarkeit von _Fachkenntnissen in Sachfragen_ für die Lösung **bei Edge über den CDN-Dienst**.
