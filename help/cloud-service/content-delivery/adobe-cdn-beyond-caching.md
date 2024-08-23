---
title: Adobe CDN - Erweiterte Funktionen, die über das Zwischenspeichern hinausgehen
description: Erfahren Sie mehr über die erweiterten Funktionen von Adobe CDN, die über das Caching hinausgehen, wie z. B. das Konfigurieren von Traffic im CDN, Einrichten von Token und Anmeldedaten, CDN-Fehlerseiten und mehr.
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: 10f9ca66a1669e1207237128469852ec7514d110
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---


# Adobe CDN - Erweiterte Funktionen, die über das Zwischenspeichern hinausgehen

Erfahren Sie mehr über die erweiterten Funktionen des Adobe Content Delivery Network (CDN), die über das Caching hinausgehen, z. B. Konfigurieren von Traffic im CDN, Einrichten von Token und Anmeldedaten, CDN-Fehlerseiten und mehr.

Neben der Zwischenspeicherung von Inhalten bietet Adobe CDN mehrere erweiterte Funktionen, mit denen Sie die Leistung Ihrer Website optimieren können. Zu diesen Funktionen gehören:

- Konfigurieren des Traffics auf dem CDN
- CDN-Anmeldeinformationen und Authentifizierung konfigurieren
- CDN-Fehlerseiten

Diese Funktionen sind **Self-Service**-Funktionen. In der Datei `cdn.yaml` Ihres AEM-Projekts konfiguriert und mithilfe der Cloud Manager-Konfigurationspipeline bereitgestellt.

## Konfigurieren des Traffics auf dem CDN

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit _Konfigurieren des Traffics am CDN_ beschrieben:

- **DoS-Angriffsprävention:** Adobe CDN absorbiert DoS-Angriffe im Netzwerk
-Ebene, wodurch verhindert wird, dass sie Ihren Herkunftsserver erreichen.
- **Ratenbegrenzung:** Um zu verhindern, dass Ihr Herkunftsserver mit zu vielen Anforderungen überlastet wird, können Sie die Ratenbegrenzung für das CDN konfigurieren.
- **Web-Anwendungs-Firewall (WAF):** Die WAF schützt Ihre Website vor allgemeinen Sicherheitslücken durch Webanwendungen wie SQL-Injection, Cross-Site-Scripting und mehr. Für die Verwendung dieser Funktion ist die Lizenz für Enhanced Security oder WAF-DDoS Protection erforderlich.
- **Anforderungsumwandlung:** Ändern Sie eingehende Anforderungen wie das Festlegen oder Aufheben von Kopfzeilen, das Ändern von Abfrageparametern, Cookies und mehr.
- **Reaktionsumwandlung:** Ändern Sie ausgehende Antworten, z. B. Festlegen oder Aufheben der Einstellung von Kopfzeilen.
- **Herkunftsauswahl:** Traffic wird basierend auf der Anforderungs-URL zu verschiedenen Herkunftsservern (Adobe und Nicht-Adobe) geleitet.
- **URL-Umleitung:** Umleitungsanfragen (HTTP 301/302) zu einer anderen absoluten oder relativen URL.

## CDN-Anmeldeinformationen und Authentifizierung konfigurieren

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit _Konfigurieren von CDN-Anmeldeinformationen und Authentifizierung_ beschrieben:

- **API-Token bereinigen**: Ermöglicht die Erstellung eines eigenen Bereinigungsschlüssels zum Bereinigen einer einzelnen oder Gruppe oder aller Ressourcen aus dem Cache.
- **Grundlegende Authentifizierung**: Ein einfacher Authentifizierungsmechanismus, wenn Sie den Zugriff auf Ihre Website oder einen Teil davon beschränken möchten. Meist im Rahmen verschiedener Überprüfungsprozesse erforderlich, bevor sie live geschaltet werden.
- **HTTP-Header-Validierung**: Wird verwendet, wenn ein kundenverwaltetes CDN Traffic an Adobe-CDN weiterleitet. Das Adobe CDN validiert die eingehende Anfrage basierend auf dem Header-Wert `X-AEM-Edge-Key` . Ermöglicht die Erstellung eines eigenen Werts für die Kopfzeile `X-AEM-Edge-Key` .

## CDN-Fehlerseiten

Im Folgenden werden die wichtigsten Funktionen im Zusammenhang mit _CDN-Fehlerseiten_ beschrieben:

- **Fehlerseiten mit Markenzeichen**: Zeigen Sie Ihren Benutzern eine Fehlerseite mit Branding im _unwahrscheinlichen Szenario_ an, wenn das Adobe-CDN nicht in der Lage ist, Ihren Herkunftsserver zu erreichen.

## Implementieren

Die Implementierung dieser erweiterten Funktionen umfasst zwei Schritte:

1. **CDN-Konfigurationsdatei aktualisieren**: Aktualisieren Sie die Datei `cdn.yaml` in Ihrem AEM-Projekt mit den erforderlichen Konfigurationen. Die Konfigurationen werden als Regeln hinzugefügt und folgen einer Regelsyntax. Die drei Hauptkomponenten der Regel: `name`, `when` und `action`.

2. **CDN-Konfigurationsdatei bereitstellen**: Stellen Sie die aktualisierte `cdn.yaml` -Datei mithilfe der Cloud Manager-Konfigurationspipeline bereit. Weitere Informationen finden Sie unter [Bereitstellen von Regeln über Cloud Manager](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Beispiel

Im folgenden Beispiel ist die WKND-Beispielsite so konfiguriert, dass die `/top3`-URL zu `/us/en/top3.html` umgeleitet wird.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Verwandte Tutorials

[Schutz von Websites mit Traffic-Filterregeln](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[CDN-Regel zur HTTP-Header-Überprüfung konfigurieren und bereitstellen](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Bereinigen des CDN-Cache](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Konfigurieren des Traffics am CDN](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Konfigurieren von CDN-Anmeldeinformationen und Authentifizierung](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

[Konfigurieren von CDN-Fehlerseiten](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)
