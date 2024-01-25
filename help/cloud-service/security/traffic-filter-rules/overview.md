---
title: Schutz von Websites mit Traffic-Filterregeln (einschließlich WAF-Regeln)
description: Erfahren Sie mehr über Traffic-Filterregeln, einschließlich der Unterkategorie „WAF-Regeln“ (Web Application Firewall). Erstellen, Bereitstellen und Testen der Regeln. Analysieren Sie außerdem die Ergebnisse, um Ihre AEM-Sites zu schützen.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 170
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 100%

---

# Schutz von Websites mit Traffic-Filterregeln (einschließlich WAF-Regeln)

Informationen zu **Traffic-Filterregeln**, einschließlich der Unterkategorie **WAF-Regeln (Web Application Firewall)** in AEM as a Cloud Service (AEMCS). Erfahren Sie, wie Sie die Regeln erstellen, bereitstellen und testen können. Analysieren Sie außerdem die Ergebnisse, um Ihre AEM-Sites zu schützen.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Übersicht

Das Risiko von Sicherheitsverletzungen zu reduzieren, ist für jede Organisation eine der obersten Prioritäten. AEMCS bietet die Funktion für Traffic-Filterregeln, einschließlich WAF-Regeln, um Websites und Anwendungen zu schützen.

Traffic-Filterregeln werden im [integrierten CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=de) bereitgestellt und ausgewertet, bevor die Anfrage die AEM-Infrastruktur erreicht. Mit dieser Funktion können Sie die Sicherheit Ihrer Website erheblich verbessern und sicherstellen, dass nur legitime Anfragen Zugriff auf die AEM-Infrastruktur erhalten.

Dieses Tutorial führt Sie durch den Prozess der Erstellung, Bereitstellung, Prüfung und Analyse der Ergebnisse von Traffic-Filterregeln, einschließlich WAF-Regeln.

Weitere Informationen zu Traffic-Filterregeln finden Sie in [diesem Artikel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de).

>[!IMPORTANT]
>
> Eine Unterkategorie von Traffic-Filterregeln namens „WAF-Regeln“ erfordert eine WAF-DDoS-Schutzlizenz oder eine Lizenz für erweiterte Sicherheit.

Wir laden Sie ein, Feedback zu geben oder Fragen zu Traffic-Filterregeln zu stellen, indem Sie eine E-Mail an **aemcs-waf-adopter@adobe.com** senden.

## Nächster Schritt

Erfahren Sie, wie die Funktion [eingerichtet](./how-to-setup.md) wird, damit Sie Traffic-Filterregeln erstellen, bereitstellen und testen können. Lesen Sie mehr über die Einrichtung des Dashboard-Tools-Stack mit **Elasticsearch, Logstash und Kibana (ELK)**, um die Ergebnisse Ihrer AEMCS-CDN-Protokolle zu analysieren.


