---
title: Einschränken des Zugriffs
description: Erfahren Sie, wie Sie den Zugriff einschränken, indem Sie bestimmte Anfragen mithilfe von Traffic-Filterregeln in AEM as a Cloud Service blockieren.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# Einschränken des Zugriffs

Erfahren Sie, wie Sie den Zugriff einschränken, indem Sie bestimmte Anfragen mithilfe von Traffic-Filterregeln in AEM as a Cloud Service blockieren.

In diesem Tutorial wird gezeigt, wie **Anfragen an interne Pfade von öffentlichen IPs** AEM-Veröffentlichungs-Service blockieren können.

## Warum und wann Anfragen blockiert werden sollten

Die Blockierung des Traffics hilft bei der Durchsetzung von Sicherheitsrichtlinien des Unternehmens, indem unter bestimmten Bedingungen der Zugriff auf sensible Ressourcen oder URLs verhindert wird. Im Vergleich zur Protokollierung ist die Blockierung eine strengere Aktion und sollte verwendet werden, wenn Sie sicher sind, dass Traffic von bestimmten Quellen nicht autorisiert oder unerwünscht ist.

Häufige Szenarien, in denen eine Blockierung angemessen ist, umfassen:

- Beschränken des Zugriffs auf `internal` oder `confidential` Seiten auf interne IP-Bereiche (z. B. hinter einem Unternehmens-VPN)
- Sperren von Bot-Traffic, automatisierten Scannern oder Bedrohungsakteuren, die durch IP oder Geolokalisierung identifiziert werden.
- Verhindern des Zugriffs auf veraltete oder ungesicherte Endpunkte während gestaffelter Migrationen.
- Einschränken des Zugriffs auf Authoring-Tools oder Admin-Routen in Veröffentlichungsebenen.

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung abgeschlossen haben, wie im Tutorial [Einrichten von Traffic-Filtern und WAF-Regeln](../setup.md) beschrieben. Außerdem haben Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt.

## Beispiel: Blockieren interner Pfade von öffentlichen IPs

In diesem Beispiel konfigurieren Sie eine Regel, um den externen Zugriff auf eine interne WKND-Seite, z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, von öffentlichen IP-Adressen aus zu blockieren. Nur Benutzer innerhalb eines vertrauenswürdigen IP-Bereichs (z. B. ein Unternehmens-VPN) können auf diese Seite zugreifen.

Sie können entweder eine eigene interne Seite erstellen (z. B. `demo-page.html`) oder das [angehängte Paket](../assets/how-to/demo-internal-pages-package.zip) verwenden.

- Fügen Sie die folgende Regel zur Datei `/config/cdn.yaml` des WKND-Projekts hinzu.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- Übertragen und pushen Sie die Änderungen in das Cloud Manager-Git-Repository.

- Stellen Sie die Änderungen mithilfe der Cloud Manager-Konfigurations-Pipeline ([ erstellt) in der AEM-](../setup.md#deploy-rules-using-adobe-cloud-manager) bereit.

- Testen Sie die Regel, indem Sie auf die interne Seite der WKND-Site zugreifen, zum Beispiel `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, oder mithilfe des folgenden CURL-Befehls:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Wiederholen Sie den obigen Schritt sowohl von der in der Regel verwendeten IP-Adresse als auch von einer anderen IP-Adresse (z. B. über Ihr Mobiltelefon).

## Analysieren

Gehen Sie zur Analyse der Ergebnisse der `block-internal-paths`-Regel wie im Einrichtungs[Tutorial beschrieben vor](../setup.md#cdn-logs-ingestion)

Sie sollten die **Blockierte Anfragen** und die entsprechenden Werte in den Spalten Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) sehen.

![ELK-Tool-Dashboard – Blockierte Anfragen](../assets/how-to/elk-tool-dashboard-blocked.png)
