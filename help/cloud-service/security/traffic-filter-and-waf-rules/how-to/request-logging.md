---
title: Überwachen sensibler Anfragen
description: Erfahren Sie, wie Sie sensible Anfragen überwachen, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service protokollieren.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 34%

---

# Überwachen sensibler Anfragen

Erfahren Sie, wie Sie sensible Anfragen überwachen, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service protokollieren.

Die Protokollierung ermöglicht die Beobachtung von Traffic-Mustern, ohne die Endbenutzer oder Services zu beeinträchtigen, und ist ein wichtiger erster Schritt vor der Implementierung von Sperrregeln.

In diesem Tutorial wird gezeigt, wie **Anfragen von WKND-Anmelde- und -Abmeldepfaden)** den AEM-Veröffentlichungs-Service protokollieren.

## Warum und wann Anfragen protokolliert werden sollten

Die Protokollierung spezifischer Anfragen ist eine risikoarme, wertvolle Vorgehensweise, um zu verstehen, wie Benutzende - und potenziell böswillige Akteure - mit Ihrer AEM-Anwendung interagieren. Dies ist besonders vor der Durchsetzung von Blockierungsregeln nützlich, da Sie so die Möglichkeit haben, Ihren Sicherheitszustand zu verbessern, ohne den rechtmäßigen Datenverkehr zu unterbrechen.

Häufige Szenarien für die Protokollierung sind:

- Validieren der Auswirkungen und der Reichweite einer Regel, bevor sie in den `block` Modus hochgestuft wird.
- Überwachen von Anmelde-/Abmeldepfaden und Authentifizierungsendpunkten auf ungewöhnliche Muster oder Brute-Force-Versuche.
- Tracking des hochfrequenten Zugriffs auf API-Endpunkte für potenziellen Missbrauch oder DoS-Aktivitäten.
- Festlegung von Grundlinien für das Verhalten von Bots vor Anwendung strengerer Kontrollen
- Stellen Sie im Falle von Sicherheitsvorfällen forensische Daten bereit, um die Art des Angriffs und die betroffenen Ressourcen zu verstehen.

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung abgeschlossen haben, wie im Tutorial [Einrichten von Traffic-Filtern und WAF-Regeln](../setup.md) beschrieben. Außerdem müssen Sie das [AEM WKND Sites-Projekt geklont und in ](https://github.com/adobe/aem-guides-wknd) AEM-Umgebung bereitgestellt haben.

## Beispiel: Protokollieren von WKND-Anmelde- und Abmeldeanfragen

In diesem Beispiel erstellen Sie eine Traffic-Filterregel, um Anfragen zu protokollieren, die an die WKND-Anmelde- und -Abmeldepfade im AEM-Veröffentlichungs-Service gesendet werden. Dies hilft Ihnen bei der Überwachung von Authentifizierungsversuchen und der Erkennung potenzieller Sicherheitsprobleme.

- Fügen Sie die folgende Regel zur Datei `/config/cdn.yaml` des WKND-Projekts hinzu.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
    - name: publish-auth-requests
      when:
        allOf:
          - reqProperty: tier
            matches: publish
          - reqProperty: path
            in:
              - /system/sling/login/j_security_check
              - /system/sling/logout
      action: log   
```

- Übertragen und pushen Sie die Änderungen in das Cloud Manager-Git-Repository.

- Stellen Sie die Änderungen mithilfe der Cloud Manager-Konfigurations-Pipeline ([ erstellt) in der AEM-](../setup.md#deploy-rules-using-adobe-cloud-manager) bereit.

- Testen Sie die Regel, indem Sie sich an der WKND-Site Ihres Programms anmelden und von ihr abmelden (z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Sie können `asmith/asmith` als Benutzernamen und Kennwort verwenden.

  ![WKND-Anmeldung](../assets/how-to/wknd-login.png)

## Analysieren

Analysieren wir die Ergebnisse der `publish-auth-requests`, indem wir die AEMCS-CDN-Protokolle aus Cloud Manager herunterladen und das [AEMCS CDN Log Analysis Tool“ ](../setup.md#setup-the-elastic-dashboard-tool).

- Laden Sie auf der Karte **Umgebungen** von [Cloud Manager](https://my.cloudmanager.adobe.com/) die CDN-Protokolle des **Publish-Services** von AEMCS herunter.

  ![Downloads von Cloud Manager-CDN-Protokollen](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Es kann bis zu 5 Minuten dauern, bis die neuen Anfragen in den CDN-Protokollen angezeigt werden.

- Kopieren Sie die heruntergeladene Protokolldatei (beispielsweise `publish_cdn_2023-10-24.log` im folgenden Screenshot) in den Ordner `logs/dev` des Elastic-Dashboard-Tool-Projekts.

  ![Ordner für ELK-Tool-Protokolle](../assets/how-to/elk-tool-logs-folder.png)

- Aktualisieren Sie Seite des Elastic-Dashboard-Tools.
   - Bearbeiten Sie im Abschnitt **Globaler Filter** den Filter `aem_env_name.keyword` und wählen Sie den Wert der `dev`-Umgebung aus.

     ![Globaler Filter für das ELK-Tool](../assets/how-to/elk-tool-global-filter.png)

   - Um das Zeitintervall zu ändern, klicken Sie auf das Kalendersymbol oben rechts und wählen Sie das gewünschte Zeitintervall aus.

     ![Zeitintervall des ELK-Tools](../assets/how-to/elk-tool-time-interval.png)

- Überprüfen Sie die Bedienfelder **Analysierte Anfragen**, **Gekennzeichnete Anfragen**, und **Gekennzeichnete Anfragen – Details** im aktualisierten Dashboard. Bei übereinstimmenden CDN-Protokolleinträgen sollten die Werte der Client-IP (cli_ip), des Hosts, der URL, der Aktion (waf_action) und des Regelnamens (waf_match) jedes Eintrags angezeigt werden.

  ![ELK-Tool-Dashboard](../assets/how-to/elk-tool-dashboard.png)

