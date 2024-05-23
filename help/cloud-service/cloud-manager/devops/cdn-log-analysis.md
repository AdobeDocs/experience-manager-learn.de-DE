---
title: Tools zur Analyse von CDN-Protokollen
description: Erfahren Sie mehr über die AEM Cloud Service-Tools zur Analyse von CDN-Protokollen, die Adobe bereitstellt, und darüber, wie sie Einblicke in Ihre CDN-Leistung und die AEM-Implementierung ermöglichen.
version: Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: ht
source-wordcount: '442'
ht-degree: 100%

---

# Tools zur Analyse von CDN-Protokollen

Informationen zu den _AEM Cloud Service-Tools zur Analyse von CDN-Protokollen_, die Adobe bietet, und dazu, wie sie Einblicke in Ihre CDN-Leistung und AEM-Implementierung ermöglichen.
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## Überblick

Die [AEM as a Cloud Service-Tools zur Analyse von CDN-Protokollen](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) bieten vorkonfigurierte Dashboards, die Sie in den [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html)- oder [ELK-Stapel](https://www.elastic.co/elastic-stack) für die Überwachung und Analyse Ihrer CDN-Protokolle in Echtzeit integrieren können.

Mit diesen Tools können Sie eine Echtzeit-Überwachung und eine proaktive Problemerkennung erreichen. So können Sie eine optimierte Inhaltsbereitstellung und angemessene Sicherheitsmaßnahmen gegen DoS- (Denial of Service) und DDoS-Angriffe (Distributed Denial of Service) sicherstellen.

## Wichtigste Funktionen

- Optimierte Protokollanalyse
- Echtzeit-Überwachung
- Nahtlose Integration
- Dashboards für Folgendes
   - Potenzielle Sicherheitsbedrohungen identifizieren
   - Schnelleres Endbenutzererlebnis

## Dashboard-Übersicht

Um die Protokollanalyse zu beschleunigen, stellt Adobe vorkonfigurierte Dashboards für Splunk- und ELK-Stapel bereit.

- **CDN-Cache-Trefferverhältnis**: Bietet Einblicke in die Gesamtanzahl der Cache-Treffer und die Gesamtanzahl der Anfragen nach HIT-, PASS- und MISS-Status. Außerdem werden die Top-URLs für HIT, PASS und MISS bereitgestellt.

  ![CDN-Cache-Trefferverhältnis](assets/CHR-dashboard.png)

- **CDN-Traffic-Dashboard**: Bietet Einblicke in den Traffic anhand von CDN- und Ursprungsanfragen, die Fehlerraten von 4xx und 5xx sowie nicht zwischengespeicherte Anfragen. Außerdem erhalten Sie maximal viele CND- und Ursprungsanfragen pro Sekunde pro Client-IP-Adresse und weitere Informationen zur Optimierung der CDN-Konfigurationen.

  ![CDN-Traffic-Dashboard](assets/Traffic-dashboard.png)

- **WAF-Dashboard**: Bietet Einblicke über analysierte, gekennzeichnete und blockierte Anfragen. Es bietet auch Top-Angriffe nach WAF-Kennzeichnungs-ID, die 100 wichtigsten Angreifer nach Client-IP, Land und Benutzeragent sowie weitere Einblicke zur Optimierung der WAF-Konfigurationen.

  ![WAF-Dashboard](assets/WAF-Dashboard.png)

## Splunk-Integration

Unternehmen, die [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) nutzen und die die AEMCS-Protokollweiterleitung an ihre Splunk-Instanzen aktiviert haben, können schnell vordefinierte Dashboards importieren. Diese Einrichtung erleichtert die beschleunigte Protokollanalyse und bietet praktische Einblicke, um AEM-Implementierungen zu optimieren und Sicherheitsbedrohungen wie DoS-Angriffe zu vermeiden.

Für den Einstieg können Sie das Handbuch [Splunk-Dashboards für die AEMCS-CDN-Protokollanalyse](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis) verwenden.


## ELK-Integration

Der [ELK-Stapel](https://www.elastic.co/elastic-stack), bestehend aus Elasticsearch, Logstash und Kibana, ist eine weitere leistungsstarke Option für die Protokollanalyse. Er ist nützlich für Organisationen, die keinen Zugriff auf ein Splunk-Setup oder Protokollweiterleitungsfunktionen haben. Das lokale Einrichten des ELK-Stapels ist unkompliziert. Das Tool stellt die Docker Compose-Datei bereit, um schnell loszulegen. Anschließend können Sie die vorkonfigurierten Dashboards importieren und die CDN-Protokolle aufnehmen, die mit Adobe Cloud Manager heruntergeladen werden.

Für den Einstieg können Sie das Handbuch [ELK-Docker-Container für die AEMCS-CDN-Protokollanalyse](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis) verwenden.
