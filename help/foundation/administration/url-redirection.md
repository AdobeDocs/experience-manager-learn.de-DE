---
title: URL-Umleitungen
description: Erfahren Sie mehr über die verschiedenen Optionen, um URL-Umleitungen in AEM vorzunehmen.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 3cc9b4fa0a30d36638a8c28a73663ffa455ba4a3
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 67%

---

# URL-Umleitungen

Die Umleitung von URLs ist ein gängiger Aspekt der Website-Vorgänge. Architektinnen, Architekten und Admins müssen die beste Lösung für die Verwaltung der URL-Umleitungen finden, die Flexibilität bietet und die Bereitstellungszeit für schnelle Umleitungen verkürzt.

Stellen Sie sicher, dass Sie mit [AEM (6.x) alias AEM Classic](https://experienceleague.adobe.com/en/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) und der [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/overview/architecture)-Infrastruktur vertraut sind. Die wichtigsten Unterschiede sind:

1. AEM as a Cloud Service verfügt über ein [integriertes CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn). Kundinnen und Kunden können jedoch ein CDN (BYOCDN) vor einem von AEM verwalteten CDN bereitstellen.
1. AEM 6.x enthält kein von AEM verwaltetes CDN, auf Kundenseite muss also selbst eines bereitgestellt werden – unabhängig davon, ob es sich um On-Premise oder Adobe Managed Services (AMS) handelt.

Die anderen AEM-Dienste (AEM Author/Publish und Dispatcher) sind ansonsten von der Konzeption her ähnlich wie AEM 6.x und AEM as a Cloud Service.

Es gibt folgende URL-Umleitungslösungen in AEM:

|                                                   | Verwalteter und bereitgestellter AEM-Projekt-Code | Änderungen können durch das Marketing-/Content-Team vorgenommen werden | Kompatibel mit AEM as a Cloud Service | Wenn eine Weiterleitungsausführung erfolgt |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Bei Edge über AEM verwaltetes CDN](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge/CDN (integriert) |
| [Bei Edge über eigene CDN (BYOCDN)](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN (BYOCDN) |
| [Apache `mod_rewrite`-Regeln als Dispatcher-Konfiguration](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons – Redirect Map Manager](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons – Redirect Manager](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [Die `Redirect`-Seiteneigenschaft](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Lösungsoptionen

Die folgenden Lösungsoptionen sind in der Reihenfolge ihrer Nähe zum Browser der Website-Besuchenden aufgeführt.

### Bei Edge über AEM verwaltetes CDN {#at-edge-via-aem-managed-cdn}

Diese Option steht nur AEM as a Cloud Service Kunden zur Verfügung.

Die [AEM-verwaltetes CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) bietet eine Umleitungslösung auf Edge-Ebene, wodurch die Rundfahrten zum Ursprung reduziert werden. Die [Clientseitige Umleitungen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors) -Funktion können Sie die Umleitungsregeln im AEM-Projektcode konfigurieren und mithilfe der [Konfigurations-Pipeline](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager). Die CDN-Konfigurationsdatei (`cdn.yaml`) darf nicht größer sein als 100 KB.

Die Verwaltung von Weiterleitungen auf Edge- oder CDN-Ebene hat Leistungsvorteile.

### Bei Edge über Ihr eigenes CDN

Einige CDN-Dienste bieten Umleitungs-Lösungen auf Edge-Ebene und reduzieren so Rundfahrten zum Ursprung. Weitere Informationen finden Sie unter [Akamai Edge Redirector](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector) und [AWS CloudFront Functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Wenden Sie sich an Ihren CDN-Dienstanbieter, wenn Sie Fragen zur Umleitungsfunktion auf Edge-Ebene haben.

Die Verwaltung von Umleitungen auf Edge- oder CDN-Ebene hat Leistungsvorteile, sie werden jedoch nicht im Rahmen von AEM, sondern vielmehr diskreter Projekte verwaltet. Ein klar definierter Prozess zur Verwaltung und Einführung von Umleitungsregeln ist entscheidend, um Probleme zu vermeiden.


### `mod_rewrite`-Apache-Modul

Eine gängige Lösung ist die Verwendung des [Apache-Moduls „mod_rewrite“](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Der [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype) bietet eine Dispatcher-Projektstruktur für das [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure)- und [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure)-Projekt. Die standardmäßigen (unveränderlichen) und benutzerdefinierten Neuschreibungsregeln werden im Ordner `conf.d/rewrites` definiert und die Rewrite-Engine ist für `virtualhosts` aktiviert, die den Port `80` über die Datei `conf.d/dispatcher_vhost.conf` überwachen. Eine Beispielimplementierung ist im [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites) verfügbar.

As a Cloud Service werden diese Umleitungsregeln AEM als Teil AEM Codes verwaltet und über Cloud Manager bereitgestellt [Web-Tier-Konfigurationspipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines) oder [Vollstack-Pipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines). Somit ist Ihr AEM projektspezifischer Prozess für die Verwaltung, Bereitstellung und Verfolgung der Umleitungsregeln im Spiel.

Die meisten CDN-Dienste speichern die HTTP-301- und -302-Umleitungen abhängig von ihren `Cache-Control`- oder `Expires`-Headern im Cache. Dies hilft, den Roundtrip nach der ersten Umleitung zu vermeiden, die von Apache/Dispatcher stammt.


### ACS AEM Commons

Es gibt zwei Funktionen in [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/), um URL-Umleitungen zu verwalten. Hinweis: ACS AEM Commons ist ein von der Community verwaltetes Open-Source-Projekt und wird nicht von Adobe unterstützt.

#### Redirect Map Manager

[Umleitungs-Map-Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) hilft AEM 6.x-Administratoren bei der einfachen Wartung und Veröffentlichung [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) Dateien ohne direkten Zugriff auf den Apache-Webserver oder ohne Neustart des Apache-Webservers. Diese Funktion ermöglicht Benutzern das Erstellen, Aktualisieren und Löschen von Umleitungsregeln aus einer Konsole in AEM, ohne die Hilfe des Entwicklungsteams oder einer AEM Bereitstellung zu benötigen. Redirect Map Manager ist **NICHT mit AEM as a Cloud Service kompatibel**.

#### Redirect Manager

[Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) ermöglicht es Benutzenden in AEM, Umleitungen von AEM einfach zu verwalten und zu veröffentlichen. Die Implementierung basiert auf dem Java™-Servlet-Filter, es besteht also ein typischer JVM-Ressourcenverbrauch. Diese Funktion beseitigt auch die Abhängigkeit vom AEM-Entwicklungs-Team und von den AEM-Bereitstellungen. Redirect Manager ist mit **AEM as a Cloud Service** und **AEM 6.x** kompatibel. Während die ursprüngliche umgeleitete Anfrage den AEM Publish-Dienst erreichen muss, um den Cache 301/302 (in den meisten Fällen) des CDNs 301/302 zu generieren, sodass nachfolgende Anforderungen an den Edge/CDN umgeleitet werden können.

### Die `Redirect`-Seiteneigenschaft

Die vorkonfigurierte `Redirect`-Seiteneigenschaft der [Registerkarte „Erweitert“](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html) ermöglicht es Inhaltsautorinnen und Inhaltsautoren, den Umleitungsort für die aktuelle Seite zu definieren. Diese Lösung eignet sich am besten für seitenbasierte Umleitungsszenarien und verfügt nicht über einen zentralen Ort, an dem die Seitenumleitungen angezeigt und verwaltet werden können.

## Welche Lösung eignet sich zur Implementierung?

Im Folgenden finden Sie einige Kriterien zur Bestimmung der richtigen Lösung. Darüber hinaus sollte der IT- und Marketing-Prozess Ihrer Organisation bei der Auswahl der richtigen Lösung helfen.

1. Aktivierung des Marketing-Teams oder der Superuser zur Verwaltung von Umleitungsregeln ohne das AEM-Entwicklungs-Team und die AEM-Bereitstellungen
1. Der Prozess zum Verwalten, Überprüfen, Nachverfolgen und Zurücksetzen der Änderungen oder Risikominderung
1. Verfügbarkeit von _Fachkenntnissen in Sachfragen_ für die Lösung **bei Edge über den CDN-Dienst**.
