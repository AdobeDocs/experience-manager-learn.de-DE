---
title: Adobe CDN – Erweiterte Funktionen, die über das Caching hinausgehen
description: Erfahren Sie mehr über die erweiterten Funktionen von Adobe CDN, die über das Caching hinausgehen, wie z. B. das Konfigurieren von Traffic im CDN, das Einrichten von Token und Anmeldedaten, CDN-Fehlerseiten und mehr.
version: Experience Manager as a Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '546'
ht-degree: 100%

---

# Adobe CDN – Erweiterte Funktionen, die über das Caching hinausgehen

Erfahren Sie mehr über die erweiterten Funktionen des Adobe Content Delivery Network (CDN), die über das Caching hinausgehen, z. B. das Konfigurieren von Traffic im CDN, das Einrichten von Token und Anmeldeinformationen, CDN-Fehlerseiten und mehr.

Neben dem Caching von Inhalten bietet Adobe CDN mehrere erweiterte Funktionen, mit denen Sie die Leistung Ihrer Website optimieren können. Zu diesen Funktionen gehören:

- Konfigurieren von Traffic im CDN
- Konfigurieren von CDN-Anmeldeinformationen und einer Authentifizierung
- CDN-Fehlerseiten

Diese Funktionen sind **Self-Service**-Funktionen. Sie werden in der Datei `cdn.yaml` Ihres AEM-Projekts konfiguriert und mithilfe der Cloud Manager-Konfigurations-Pipeline bereitgestellt.

>[!VIDEO](https://video.tv.adobe.com/v/3440283?quality=12&learn=on&captions=ger)

## Konfigurieren von Traffic im CDN

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit _Konfigurieren von Traffic im CDN_ beschrieben:

- **DoS-Angriffsprävention:** Das Adobe CDN fängt DoS-Angriffe auf der Netzwerkebene ab und hindert sie daran, Ihren Herkunfts-Server zu erreichen.
- **Ratenbegrenzung:** Um zu verhindern, dass Ihr Herkunfts-Server mit zu vielen Anfragen überlastet wird, können Sie die Ratenbegrenzung für das CDN konfigurieren.
- **Web Application Firewall (WAF):** Die WAF schützt Ihre Website vor allgemeinen Sicherheitslücken durch Web-Anwendungen wie SQL-Injection, Cross-Site-Scripting und mehr. Für die Verwendung dieser Funktion ist die Lizenz für erweiterte Sicherheit oder eine WAF-DDoS-Schutzlizenz erforderlich.
- **Anfrageumwandlung:** Ändern eingehender Anfragen durch Festlegen oder Aufheben von Headern, Ändern von Abfrageparametern, Cookies und mehr.
- **Reaktionsumwandlung:** Ändern ausgehender Antworten durch Festlegen oder Aufheben von Headern.
- **Herkunftsauswahl:** Weiterleiten von Traffic je nach Anfrage-URL zu verschiedenen Herkunfts-Servern (Adobe und Nicht-Adobe).
- **URL-Umleitung:** Umleiten von Anfragen (HTTP 301/302) zu einer anderen absoluten oder relativen URL.

## Konfigurieren von CDN-Anmeldeinformationen und einer Authentifizierung

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit dem _Konfigurieren von CDN-Anmeldeinformationen und der Authentifizierung_ beschrieben:

- **Bereinigungs-API-Token**: Ermöglicht die Erstellung eines eigenen Bereinigungsschlüssels zum Bereinigen einer einzelnen Ressource oder einer Gruppe aller Ressourcen aus dem Cache.
- **Grundlegende Authentifizierung**: Ein einfacher Authentifizierungsmechanismus, wenn Sie den Zugriff auf Ihre Website oder einen Teil davon beschränken möchten. Meist im Rahmen verschiedener Überprüfungsprozesse erforderlich, bevor etwas live geschaltet wird.
- **HTTP-Header-Validierung**: Wird verwendet, wenn ein kundenseitig verwaltetes CDN Traffic an das Adobe-CDN weiterleitet. Das Adobe-CDN validiert die eingehende Anfrage basierend auf dem Wert des Headers `X-AEM-Edge-Key`. Ermöglicht Ihnen die Erstellung eines eigenen Werts für den Header `X-AEM-Edge-Key`.

## CDN-Fehlerseiten

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit _CDN-Fehlerseiten_ beschrieben:

- **Fehlerseiten mit Branding**: Zeigen Sie Ihren Benutzenden eine Fehlerseite mit Branding an für das _unwahrscheinliche Szenario_, dass das Adobe-CDN nicht in der Lage ist, Ihren Herkunfts-Server zu erreichen.

## Vorgehensweise bei der Implementierung

Die Implementierung dieser erweiterten Funktionen umfasst zwei Schritte:

1. **Aktualisieren der CDN-Konfigurationsdatei**: Aktualisieren Sie die Datei `cdn.yaml` in Ihrem AEM-Projekt mit den erforderlichen Konfigurationen. Die Konfigurationen werden als Regeln hinzugefügt und folgen einer Regelsyntax. Die drei Hauptkomponenten einer Regel sind: `name`, `when` und `action`.

2. **Bereitstellen der CDN-Konfigurationsdatei**: Stellen Sie die aktualisierte Datei `cdn.yaml` mithilfe der Cloud Manager-Konfigurations-Pipeline bereit. Weitere Informationen finden Sie unter [Bereitstellen der Regeln über Cloud Manager](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Beispiel

Im folgenden Beispiel ist die Beispiel-Site von WKND so konfiguriert, dass die URL `/top3` zu `/us/en/top3.html` umgeleitet wird.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Ähnliche Tutorials

[Schutz von Websites mit Traffic-Filterregeln](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Konfigurieren und Bereitstellen der CDN-Regel für die HTTP-Header-Validierung](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Bereinigen des CDN-Cache](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Konfigurieren von CDN-Fehlerseiten](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[Konfigurieren von Traffic im CDN](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Konfigurieren von CDN-Anmeldeinformationen und der Authentifizierung](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

