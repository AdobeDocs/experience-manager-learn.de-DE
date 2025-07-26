---
title: Schützen von AEM-Websites mithilfe von WAF-Regeln
description: Erfahren Sie, wie Sie AEM-Websites mithilfe von Adobe-empfohlenen Regeln für die Web Application Firewall (WAF) in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
badgeLicense: label="Erfordert eine Lizenz" type="positive" before-title="true"
jira: KT-18308
thumbnail: null
exl-id: b87c27e9-b6ab-4530-b25c-a98c55075aef
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 7%

---

# Schützen von AEM-Websites mithilfe von WAF-Regeln

Erfahren Sie, wie Sie AEM-Websites mithilfe von _von Adobe empfohlenen_ Web Application Firewall (WAF)-Regeln in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS **DDoS** Bot-Missbrauch schützen.

Die komplexen Angriffe zeichnen sich durch hohe Anforderungsraten, komplexe Muster und den Einsatz fortschrittlicher Techniken zur Umgehung herkömmlicher Sicherheitsmaßnahmen aus.

>[!IMPORTANT]
>
> WAF-Traffic-Filterregeln erfordern eine zusätzliche Lizenz für **WAF-DDoS Protection** oder **Enhanced Security**. Standardmäßige Traffic-Filterregeln sind standardmäßig für Kunden von Sites und Forms verfügbar.


>[!VIDEO](https://video.tv.adobe.com/v/3469439/?quality=12&learn=on&captions=ger)

## Lernziele

- Überprüfen Sie die von Adobe empfohlenen WAF-Regeln.
- Definieren, Bereitstellen, Testen und Analysieren der Ergebnisse der Regeln.
- Verstehen, wann und wie die Regeln auf der Grundlage der Ergebnisse verfeinert werden können.
- Erfahren Sie, wie Sie mit dem AEM-Aktionscenter von den Regeln generierte Warnhinweise überprüfen können.

### Implementierungsübersicht

Zu den Implementierungsschritten gehören:

- Hinzufügen der WAF-Regeln zur `/config/cdn.yaml` des AEM-WKND-Projekts.
- Übertragen und Übertragen der Änderungen in das Cloud Manager-Git-Repository.
- Bereitstellen der Änderungen in der AEM-Umgebung mithilfe der Cloud Manager-Konfigurations-Pipeline.
- Testen der Regeln durch Simulieren eines DDoS-Angriffs mit [Nikto](https://github.com/sullo/nikto/wiki).
- Analyse der Ergebnisse mithilfe der AEMCS CDN-Protokolle und des ELK-Dashboard-Tools

## Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass Sie die erforderliche Einrichtung abgeschlossen haben, wie im Tutorial [Einrichten von Traffic-Filtern und WAF-Regeln](../setup.md) beschrieben. Außerdem haben Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt.

## Regeln überprüfen und definieren

Die von Adobe empfohlenen Regeln der Web Application Firewall (WAF) sind unverzichtbar, um AEM-Websites vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch zu schützen. Komplexe Angriffe zeichnen sich oft durch hohe Anforderungsraten, komplexe Muster und den Einsatz fortschrittlicher Techniken (protokoll- oder payload-basierte Angriffe) aus, um herkömmliche Sicherheitsmaßnahmen zu umgehen.

Sehen wir uns drei empfohlene WAF-Regeln an, die zur `cdn.yaml` im AEM WKND-Projekt hinzugefügt werden sollten:

### &#x200B;1. Blockieren Sie Angriffe von bekannten bösartigen IPs

Diese Regel **blockiert** Anfragen, die sowohl verdächtig aussehen *als auch* von IP-Adressen stammen, die als bösartig gekennzeichnet sind. Da beide Kriterien erfüllt sind, können wir davon ausgehen, dass das Risiko falsch positiver Ergebnisse (Blockierung legitimen Traffics) sehr gering ist. Die bekannten fehlerhaften IPs werden auf der Grundlage von Bedrohungsdaten-Feeds und anderen Quellen identifiziert.

Das `ATTACK-FROM-BAD-IP` WAF-Flag wird verwendet, um diese Anfragen zu identifizieren. Sie aggregiert mehrere der WAF-Flags [hier aufgelistet](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list).

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: attacks-from-bad-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: block
        wafFlags:
          - ATTACK-FROM-BAD-IP
```

### &#x200B;2. Globale Protokollierung (und spätere Blockierung) von Angriffen von beliebigen IP-Adressen

Diese Regel **Protokolle** Anforderungen, die als potenzielle Angriffe identifiziert werden, auch wenn die IP-Adressen nicht in den Feeds für Bedrohungsinformationen gefunden werden.

Das `ATTACK` WAF-Flag wird verwendet, um diese Anfragen zu identifizieren. Ähnlich wie die `ATTACK-FROM-BAD-IP`,   Aggregiert mehrere WAF-Flags.

Diese Anfragen sind wahrscheinlich bösartig, aber da die IP-Adressen in den Feeds für Bedrohungsinformationen nicht identifiziert werden, kann es ratsam sein, im `log` statt im Blockierungsmodus zu starten. Analysieren Sie die Protokolle auf falsch positive Ergebnisse und stellen Sie nach der Validierung **sicher, dass Sie die Regel in den `block` Modus**.

```yaml
...
    - name: attacks-from-any-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: log
        alert: true
        wafFlags:
          - ATTACK
```

Alternativ können Sie den `block`-Modus sofort verwenden, wenn Sie aufgrund Ihrer Geschäftsanforderungen keinen bösartigen Traffic zulassen möchten.

Diese empfohlenen WAF-Regeln bieten eine zusätzliche Sicherheitsebene gegen bekannte und neue Bedrohungen.

![WKND-WAF-Regeln](../assets/use-cases/wknd-cdn-yaml-waf-rules.png)

## Migration zu den neuesten von Adobe empfohlenen WAF-Regeln

Vor der Einführung der Flags `ATTACK-FROM-BAD-IP` und `ATTACK` WAF (im Juli 2025) wurden die folgenden WAF-Regeln empfohlen. Sie enthielten eine Liste spezifischer WAF-Flags zum Blockieren von Anfragen, die bestimmten Kriterien entsprachen, wie `SANS`, `TORNODE`, `NOUA` usw.

```yaml
...
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
...
```

Die obige Regel ist weiterhin gültig, aber es wird empfohlen, zu den neuen Regeln zu migrieren, die die Flags `ATTACK-FROM-BAD-IP` und `ATTACK` WAF verwenden _sofern Sie die `wafFlags` noch nicht an Ihre Geschäftsanforderungen angepasst haben_.

Sie können zur Übereinstimmung mit den Best Practices zu den neuen Regeln migrieren, indem Sie die folgenden Schritte ausführen:

- Überprüfen Sie die vorhandenen WAF-Regeln in Ihrer `cdn.yaml`, die dem obigen Beispiel ähneln können. Vergewissern Sie sich, dass es keine auf Ihre Geschäftsanforderungen zugeschnittene Anpassung der `wafFlags` gibt.

- Ersetzen Sie Ihre bestehenden WAF-Regeln durch die neuen von Adobe empfohlenen WAF-Regeln, die die Flags `ATTACK-FROM-BAD-IP` und `ATTACK` verwenden. Stellen Sie sicher, dass sich alle Regeln im Blockmodus befinden.

Wenn Sie die `wafFlags` zuvor angepasst haben, können Sie weiterhin zu diesen neuen Regeln migrieren, tun dies jedoch sorgfältig, um sicherzustellen, dass alle Anpassungen in die überarbeiteten Regeln übernommen werden.

Die Migration sollte Ihnen helfen, Ihre WAF-Regeln zu vereinfachen und gleichzeitig zuverlässigen Schutz vor komplexen Bedrohungen zu bieten. Die neuen Vorschriften sollen wirksamer und leichter zu handhaben sein.


## Regeln bereitstellen

Gehen Sie wie folgt vor, um die oben genannten Regeln bereitzustellen:

- Übertragen und pushen Sie die Änderungen in das Cloud Manager-Git-Repository.

- Stellen Sie die Änderungen mithilfe der Cloud Manager-Konfigurations-Pipeline ([ erstellt) in der AEM-](../setup.md#deploy-rules-using-adobe-cloud-manager) bereit.

  ![Cloud Manager-Konfigurations-Pipeline](../assets/use-cases/cloud-manager-config-pipeline.png)

## Testregeln

Um die Effektivität der WAF-Regeln zu überprüfen, simulieren Sie einen Angriff mit [Nikto](https://github.com/sullo/nikto), einem Webserver-Scanner, der Sicherheitslücken und Fehlkonfigurationen erkennt. Mit dem folgenden Befehl werden SQL-Injection-Angriffe auf die AEM-WKND-Website Trigger, die durch die WAF-Regeln geschützt ist.

```shell
$./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
```

![Simulation von Nikto-Angriffen](../assets/use-cases/nikto-attack.png)

Weitere Informationen zur Angriffssimulation finden Sie in der Dokumentation zu [Nikto – Scan-Optimierung](https://github.com/sullo/nikto/wiki/Scan-Tuning), in der Sie erfahren, wie Sie den Typ von Testangriffen angeben, die ein- oder ausgeschlossen werden sollen.

## Warnungen überprüfen

Warnhinweise werden generiert, wenn die Traffic-Filterregeln ausgelöst werden. Sie können diese Warnhinweise im [AEM-Aktionscenter](https://experience.adobe.com/aem/actions-center) überprüfen.

![WKND AEM-Aktionscenter](../assets/use-cases/wknd-aem-action-center.png)

## Ergebnisse analysieren

Um die Ergebnisse der Traffic-Filterregeln zu analysieren, können Sie die AEMCS-CDN-Protokolle und das ELK-Dashboard-Tool verwenden. Befolgen Sie die Anweisungen im Abschnitt [CDN-Protokollaufnahme](../setup.md#ingest-cdn-logs) Einrichtung , um die CDN-Protokolle in den ELK-Stack aufzunehmen.

Im folgenden Screenshot sehen Sie die CDN-Protokolle der AEM-Entwicklungsumgebung, die in den ELK-Stack aufgenommen wurden.

![WKND CDN Logs ELK](../assets/use-cases/wknd-cdn-logs-elk-waf.png)

Innerhalb des ELK-Programms sollte im **WAF-Dashboard** Folgendes angezeigt werden
Markierte Anfragen und entsprechende Werte in den Spalten Client-IP (CLI_IP), Host, URL, Aktion (WAF_ACTION) und Regelname (WAF_MATCH).

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged.png)

Außerdem zeigen die Bedienfelder **WAF-Flags-Verteilung** und **Top-Angriffe** zusätzliche Details an.

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-2.png)

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-3.png)

### Splunk-Integration

Kundinnen und Kunden, die die [Splunk Log-Weiterleitung](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) aktiviert haben, können neue Dashboards erstellen, um Traffic-Muster zu analysieren. 

Um Dashboards in Splunk zu erstellen, folgen Sie den Schritten [Splunk-Dashboards für AEMCS CDN Log Analysis](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

## Wann und wie Regeln verfeinert werden

Sie möchten verhindern, dass rechtmäßiger Traffic blockiert wird, und gleichzeitig Ihre AEM-Websites vor komplexen Bedrohungen schützen. Die empfohlenen WAF-Regeln sind als Ausgangspunkt für Ihre Sicherheitsstrategie konzipiert.

Gehen Sie wie folgt vor, um die Regeln zu verfeinern:

- **Traffic-Muster überwachen**: Verwenden Sie die CDN-Protokolle und das ELK-Dashboard, um Traffic-Muster zu überwachen und Anomalien oder Traffic-Spitzen zu identifizieren. Achten Sie auf die Bedienfelder _WAF Flags Distribution_ und _Top-_ im ELK-Dashboard, um die Arten von Angriffen zu verstehen, die erkannt werden.
- **wafFlags anpassen**: Wenn `ATTACK` Flags zu häufig ausgelöst werden oder
Um den Angriffsvektor zu optimieren, können Sie benutzerdefinierte Regeln mit bestimmten WAF-Flags erstellen. Eine vollständige Liste der [WAF-Flags finden Sie ](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) der Dokumentation. Erwägen Sie, zunächst neue benutzerdefinierte Regeln im `log` auszuprobieren.
- **Zu Blockierungsregeln wechseln**: Nachdem Sie die Traffic-Muster validiert und die WAF-Flags angepasst haben, können Sie ggf. zu Blockierungsregeln wechseln.

## Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie AEM-Websites mithilfe von Adobe-empfohlenen Regeln für die Web Application Firewall (WAF) vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen können.

## Anwendungsfälle - über Standardregeln hinaus

Bei komplexeren Szenarien können Sie die folgenden Anwendungsfälle untersuchen, die zeigen, wie Sie benutzerdefinierte Traffic-Filterregeln basierend auf bestimmten Geschäftsanforderungen implementieren:

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="Überwachen sensibler Anfragen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="Überwachen sensibler Anfragen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="Überwachen sensibler Anfragen">Überwachen sensibler Anfragen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie sensible Anfragen überwachen, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service protokollieren.</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="Einschränken des Zugriffs" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="Einschränken des Zugriffs"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="Einschränken des Zugriffs">Einschränken des Zugriffs</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie den Zugriff einschränken, indem Sie bestimmte Anfragen mithilfe von Traffic-Filterregeln in AEM as a Cloud Service blockieren.</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="Normalisieren von Anfragen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="Normalisieren von Anfragen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="Normalisieren von Anfragen">Normalisieren von Anfragen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Anfragen normalisieren können, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service transformieren.</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Zusätzliche Ressourcen

- [Empfohlene Starterregeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)
- [Liste der WAF-Flags](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)
