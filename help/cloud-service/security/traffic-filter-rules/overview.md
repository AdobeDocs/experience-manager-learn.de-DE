---
title: Schutz von Websites mit Traffic-Filterregeln (einschließlich WAF-Regeln)
description: Erfahren Sie mehr über Traffic-Filter-Regeln, einschließlich der Unterkategorie der WAF-Regeln (Web Application Firewall). Erstellen, Bereitstellen und Testen der Regeln. Analysieren Sie außerdem die Ergebnisse, um Ihre AEM Sites zu schützen.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 4%

---


# Schutz von Websites mit Traffic-Filterregeln (einschließlich WAF-Regeln)

Informationen zu **Traffic-Filterregeln**, einschließlich der Unterkategorie von **Web Application Firewall (WAF)-Regeln** in AEM as a Cloud Service (AEMCS). Erfahren Sie, wie Sie die Regeln erstellen, bereitstellen und testen können. Analysieren Sie außerdem die Ergebnisse, um Ihre AEM Sites zu schützen.

## Übersicht

Die Verringerung des Risikos von Sicherheitsverletzungen ist für jede Organisation eine der obersten Prioritäten. AEMCS bietet die Funktion für Traffic-Filterregeln, einschließlich WAF-Regeln, um Websites und Anwendungen zu schützen.

Traffic-Filterregeln werden in der [integriertes CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=de) und werden ausgewertet, bevor die Anfrage die AEM-Infrastruktur erreicht. Mit dieser Funktion können Sie die Sicherheit Ihrer Website erheblich verbessern und sicherstellen, dass nur legitime Anfragen Zugriff auf die AEM-Infrastruktur erhalten.

Dieses Tutorial führt Sie durch den Prozess der Erstellung, Bereitstellung, Prüfung und Analyse der Ergebnisse von Traffic-Filterregeln, einschließlich WAF-Regeln.

Weitere Informationen zu Traffic-Filterregeln finden Sie in [diesem Artikel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> Eine Unterkategorie von Traffic-Filterregeln, die &quot;WAF-Regeln&quot;genannt werden, erfordert eine WAF-DDoS-Schutzlizenz


## Nächster Schritt

Lernen [Einrichtung](./how-to-setup.md) die Funktion , damit Sie Traffic-Filterregeln erstellen, bereitstellen und testen können. Lesen Sie mehr über die Einrichtung der **Elasticsearch, Logstash und Kibana (ELK)** Dashboard-Tools stapeln, um die Ergebnisse Ihrer AEMCS-CDN-Protokolle zu analysieren.



