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
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 3%

---

# Schutz von Websites mit Traffic-Filterregeln (einschließlich WAF-Regeln)

Informationen zu **Traffic-Filterregeln**, einschließlich der Unterkategorie von **Web Application Firewall (WAF)-Regeln** in AEM as a Cloud Service (AEMCS). Erfahren Sie, wie Sie die Regeln erstellen, bereitstellen und testen können. Analysieren Sie außerdem die Ergebnisse, um Ihre AEM Sites zu schützen.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Übersicht

Die Verringerung des Risikos von Sicherheitsverletzungen ist für jede Organisation eine der obersten Prioritäten. AEMCS bietet die Funktion für Traffic-Filterregeln, einschließlich WAF-Regeln, um Websites und Anwendungen zu schützen.

Traffic-Filterregeln werden in der [integriertes CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=de) und werden ausgewertet, bevor die Anfrage die AEM-Infrastruktur erreicht. Mit dieser Funktion können Sie die Sicherheit Ihrer Website erheblich verbessern und sicherstellen, dass nur legitime Anfragen Zugriff auf die AEM-Infrastruktur erhalten.

Dieses Tutorial führt Sie durch den Prozess der Erstellung, Bereitstellung, Prüfung und Analyse der Ergebnisse von Traffic-Filterregeln, einschließlich WAF-Regeln.

Weitere Informationen zu Traffic-Filterregeln finden Sie in [diesem Artikel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Eine Unterkategorie von Traffic-Filterregeln namens &quot;WAF-Regeln&quot;erfordert eine WAF-DDoS-Schutzlizenz oder eine Lizenz für erweiterte Sicherheit.

Wir laden Sie ein, Feedback zu geben oder Fragen zu Traffic-Filterregeln per E-Mail zu stellen **aemcs-waf-adopter@adobe.com**.

## Nächster Schritt

Lernen [Einrichtung](./how-to-setup.md) die Funktion , damit Sie Traffic-Filterregeln erstellen, bereitstellen und testen können. Lesen Sie mehr über die Einrichtung der **Elasticsearch, Logstash und Kibana (ELK)** Dashboard-Tools stapeln, um die Ergebnisse Ihrer AEMCS-CDN-Protokolle zu analysieren.


