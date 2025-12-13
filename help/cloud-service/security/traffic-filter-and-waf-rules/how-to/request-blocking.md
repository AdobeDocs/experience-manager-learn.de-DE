---
title: Einschränken des Zugriffs
description: Erfahren Sie, wie Sie den Zugriff durch Blockierung bestimmter Anfragen mit Traffic-Filterregeln in AEM as a Cloud Service einschränken.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 100%

---

# Einschränken des Zugriffs

Erfahren Sie, wie Sie den Zugriff durch Blockierung bestimmter Anfragen mit Traffic-Filterregeln in AEM as a Cloud Service einschränken.

In diesem Tutorial wird gezeigt, wie Sie mit dem AEM Publish-Service **Anfragen an interne Pfade von öffentlichen IPs blockieren**.

## Gründe und Zeitpunkt für die Blockierung von Anfragen

Die Blockierung des Traffics hilft bei der Durchsetzung von Sicherheitsrichtlinien im Unternehmen, indem der Zugriff auf sensible Ressourcen oder URLs unter bestimmten Bedingungen verhindert wird. Im Vergleich zur Protokollierung ist die Blockierung eine strengere Aktion und sollte verwendet werden, wenn Sie sicher sind, dass Traffic von bestimmten Quellen nicht autorisiert oder nicht erwünscht ist.

Häufige Szenarien, in denen eine Blockierung angemessen ist, umfassen:

- Beschränken des Zugriffs auf als `internal` oder `confidential` eingestufte Seiten auf interne IP-Bereiche (z. B. hinter einem Unternehmens-VPN)
- Sperren von Bot-Traffic, automatischen Scannern oder Bedrohungsakteuren, die über die IP-Adresse oder Geolokalisierung identifiziert werden.
- Verhindern des Zugriffs auf veraltete oder ungesicherte Endpunkte während gestaffelter Migrationen.
- Einschränken des Zugriffs auf Authoring-Tools oder Admin-Routen in Veröffentlichungsebenen.

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung wie im Tutorial [Einrichten von Traffic-Filter- und WAF-Regeln](../setup.md) beschrieben abgeschlossen haben. Außerdem müssen Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt haben.

## Beispiel: Blockieren interner Pfade für öffentliche IPs

In diesem Beispiel konfigurieren Sie eine Regel, um den externen Zugriff auf eine interne WKND-Seite, z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, von öffentlichen IP-Adressen aus zu blockieren. Nur Benutzende innerhalb eines vertrauenswürdigen IP-Bereichs (z. B. Unternehmens-VPN) können auf diese Seite zugreifen.

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

- Übernehmen Sie die Änderungen und pushen Sie sie in das Cloud Manager-Git-Repository.

- Implementieren Sie die Änderungen mit der [zuvor erstellten](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager-Konfigurations-Pipeline in der AEM-Entwicklungsumgebung.

- Testen Sie die Regel, indem Sie auf die interne Seite der WKND-Site zugreifen, zum Beispiel `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, oder mithilfe des folgenden CURL-Befehls:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Wiederholen Sie den obigen Schritt sowohl von der in der Regel verwendeten IP-Adresse als auch von einer anderen IP-Adresse (z. B. über Ihr Mobiltelefon).

## Analysieren

Um die Ergebnisse der Regel `block-internal-paths` zu analysieren, führen Sie dieselben Schritte aus wie im [Einrichtungs-Tutorial](../setup.md#cdn-logs-ingestion) beschrieben.

Diesmal sollten Sie die **blockierten Anfragen** und die entsprechenden Werte in den Spalten für Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) sehen.

![ELK-Tool-Dashboard – Blockierte Anfragen](../assets/how-to/elk-tool-dashboard-blocked.png)
