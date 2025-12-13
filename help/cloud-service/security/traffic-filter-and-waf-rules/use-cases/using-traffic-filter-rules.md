---
title: Schützen von AEM-Websites mit Standard-Traffic-Filterregeln
description: Erfahren Sie, wie Sie AEM-Websites mit von Adobe empfohlenen Standard-Traffic-Filterregeln in AEM as a Cloud Service vor Denial of Service(DoS)-Angriffen und Bot-Missbrauch schützen.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
exl-id: 5e235220-82f6-46e4-b64d-315f027a7024
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1780'
ht-degree: 100%

---

# Schützen von AEM-Websites mit Standard-Traffic-Filterregeln

Erfahren Sie, wie Sie AEM-Websites mit _von Adobe empfohlenen_ **Standard-Traffic-Filterregeln** in AEM as a Cloud Service vor Denial of Service(DoS)- und Distributed Denial of Service(DDoS)-Angriffen sowie Bot-Missbrauch schützen.


>[!VIDEO](https://video.tv.adobe.com/v/3469396/?quality=12&learn=on)

## Lernziele

- Prüfen der von Adobe empfohlenen Standard-Traffic-Filterregeln.
- Definieren, Implementieren und Testen der Regeln und Analysieren der Ergebnisse.
- Erfahren Sie, wann und wie Sie die Regeln basierend auf Traffic-Mustern optimieren müssen.
- Verwenden des AEM-Aktions-Centers zur Prüfung der von Regeln generierten Warnhinweise.

### Implementierung – Überblick

Zu den Implementierungsschritten gehören:

- Hinzufügen der Standard-Traffic-Filterregeln zur Datei `/config/cdn.yaml` des AEM WKND-Projekts.
- Übertragen und Pushen der Änderungen in das Cloud Manager-Git-Repository.
- Bereitstellen der Änderungen in der AEM-Umgebung mit der Konfigurations-Pipeline von Cloud Manager. 
- Testen der Regeln durch Simulieren eines DoS-Angriffs mit [Vegeta](https://github.com/tsenart/vegeta).
- Analysieren der Ergebnisse mit den AEMCS CDN-Protokollen und den ELK-Dashboard-Tools.

## Voraussetzungen

Bevor Sie fortfahren, vergewissern Sie sich, dass Sie die erforderlichen Schritte ausgeführt haben, wie im Tutorial [Einrichten von Traffic-Filter- und WAF-Regeln](../setup.md) beschrieben. Außerdem müssen Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) geklont und in Ihrer AEM-Umgebung bereitgestellt haben.

## Wichtigste Aktionen der Regeln

Bevor wir in die Details der Standard-Traffic-Filterregeln eintauchen, klären wir die wichtigsten Aktionen, die von diesen Regeln durchgeführt werden. Das Attribut `action` in jeder Regel definiert, wie der Traffic-Filter reagieren soll, wenn die Bedingungen erfüllt sind. Zu den Aktionen gehören:

- **Protokollieren**: Die Regeln protokollieren die Ereignisse für die Überwachung und Analyse, sodass Sie Traffic-Muster überprüfen und Schwellenwerte nach Bedarf anpassen können. Diese Aktion wird durch das Attribut `type: log` angegeben.

- **Warnhinweis**: Die Regeln lösen Warnhinweise aus, wenn die Bedingungen erfüllt sind, damit Sie potenzielle Probleme identifizieren können. Diese Aktion wird durch das Attribut `alert: true` angegeben.

- **Blockieren**: Die Regeln blockieren den Traffic, wenn die Bedingungen erfüllt sind, und verhindern den Zugriff auf Ihre AEM-Site. Diese Aktion wird durch das Attribut `action: block` angegeben.

## Prüfen und Definieren von Regeln

Von Adobe empfohlene Standard-Traffic-Filterregeln dienen als Grundlage für die Identifizierung potenziell schadhafter Traffic-Muster, indem Ereignisse wie die Überschreitung IP-basierter Ratenbegrenzungen protokolliert und Traffic aus bestimmten Ländern blockiert werden. Diese Protokolle unterstützen Teams dabei, Schwellenwerte zu validieren und fundierte Entscheidungen für den **Übergang zu Blockmodus**-Regeln zu treffen, ohne den legitimen Traffic zu beeinträchtigen.

Sehen wir uns die drei Standard-Traffic-Filterregeln an, die Sie der Datei `/config/cdn.yaml` des AEM WKND-Projekts hinzufügen sollten:

- **DoS am Edge verhindern**: Diese Regel erkennt potenzielle DoS-Angriffe (Denial of Service) am CDN-Edge, indem sie die Anzahl der Anforderungen pro Sekunde (RPS) von Client-IPs überwacht.
- **DoS am Ursprung verhindern**: Diese Regel erkennt potenzielle DoS-Angriffe (Denial of Service) am Ursprung, indem sie Abrufanfragen von Client-IPs überwacht.
- **OFAC-Länder blockieren**: Diese Regel blockiert den Zugriff von bestimmten Ländern, die unter die Einschränkungen des OFAC (Office of Foreign Assets Control) fallen.

### &#x200B;1. DoS am Edge verhindern

Diese Regel **sendet einen Warnhinweis**, wenn sie einen potenziellen DoS-Angriff (Denial of Service) im CDN erkennt. Das Kriterium zum Auslösen dieser Regel besteht darin, dass ein Client am Edge **500 Anfragen pro Sekunde** (gemittelt über 10 Sekunden) pro CDN-POP (Point of Presence) überschreitet.

**Alle** Anfragen werden gezählt und nach Client-IP gruppiert.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

Das Attribut `action` gibt an, dass die Regel die Ereignisse protokollieren und einen Warnhinweis ausgeben soll, wenn die Bedingungen erfüllt sind. Auf diese Weise können Sie potenzielle DoS-Angriffe überwachen, ohne legitimen Traffic zu blockieren. Ihr Ziel ist es jedoch, diese Regel schließlich in den Blockmodus zu überführen, sobald Sie die Traffic-Muster validiert und die Schwellenwerte angepasst haben.

### &#x200B;2. DoS am Ursprung verhindern

Diese Regel **sendet einen Warnhinweis** wenn sie einen potenziellen DoS-Angriff (Denial of Service) am Ursprung erkennt. Das Kriterium zum Auslösen dieser Regel besteht darin, dass ein Client **100 Anfragen pro Sekunde** (gemittelt über 10 Sekunden) pro Client-IP am Ursprung überschreitet.

**Abrufe** (Cache-Bypassing-Anfragen) werden gezählt und nach Client-IP gruppiert.

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

Das Attribut `action` gibt an, dass die Regel die Ereignisse protokollieren und einen Warnhinweis ausgeben soll, wenn die Bedingungen erfüllt sind. Auf diese Weise können Sie potenzielle DoS-Angriffe überwachen, ohne legitimen Traffic zu blockieren. Ihr Ziel ist es jedoch, diese Regel schließlich in den Blockmodus zu überführen, sobald Sie die Traffic-Muster validiert und die Schwellenwerte angepasst haben.

### &#x200B;3. OFAC-Länder blockieren

Diese Regel blockiert den Zugriff aus bestimmten Ländern, die unter [OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information)-Einschränkungen fallen.
Sie können die Länderliste bei Bedarf prüfen und ändern.

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

Das Attribut `action` gibt an, dass die Regel den Zugriff aus den angegebenen Länder blockieren soll. So verhindern Sie den Zugriff auf Ihre AEM-Site aus Regionen, die Sicherheitsrisiken darstellen können.

Die vollständige Datei `cdn.yaml` mit den oben genannten Regeln sieht wie folgt aus:

![WKND CDN-YAML-Regeln](../assets/use-cases/wknd-cdn-yaml-rules.png)

## Bereitstellen der Regeln

Gehen Sie zur Bereitstellung der Regeln wie folgt vor:

- Übernehmen Sie die Änderungen und pushen Sie sie in das Cloud Manager-Git-Repository.

- Implementieren Sie die Änderungen mit der [zuvor erstellten](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager-Konfigurations-Pipeline in der AEM-Entwicklungsumgebung.

  ![Cloud Manager-Konfigurations-Pipeline](../assets/use-cases/cloud-manager-config-pipeline.png)

## Testen der Regeln

Um die Effektivität der Standard-Traffic-Filterregeln zu überprüfen, simulieren Sie sowohl am **CDN-Edge** als auch am **Ursprung** hohen Anfrage-Traffic mit [Vegeta](https://github.com/tsenart/vegeta), einem vielseitigen Tool zum Testen der HTTP-Last.

- Testen Sie die DoS-Regel am Edge (Grenzwert: 500 Anfragen/s). Der folgende Befehl simuliert 15 Sekunden lang 200 Anfragen pro Sekunde, was den Edge-Schwellenwert (500 Anfragen/s) überschreitet.

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Vegeta-DoS-Angriff am Edge](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  Beachten Sie den Wert von *100%* für „Success“ und den Status-Code _200_ im obigen Bericht. Da die Regeln auf `log` und `alert` eingestellt sind, werden die Anfragen _nicht blockiert_, aber zu Überwachungs-, Analyse- und Warnzwecken protokolliert.

- Testen Sie die DoS-Regel am Ursprung (Grenzwert: 100 Anfragen/s). Der folgende Befehl simuliert 1 Sekunde lang 110 Abrufanfragen pro Sekunde, was den Ursprungsschwellenwert (100 Anfragen/s) überschreitet. Um die Cache-Umgehung von Anfragen zu simulieren, wird die Datei `targets.txt` mit eindeutigen Abfrageparametern erstellt, damit sichergestellt ist, dass jede Anfrage als Abrufanfrage behandelt wird.

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Vegeta-DoS-Angriff am Ursprung](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  Beachten Sie den Wert von *100%* für „Success“ und den Status-Code _200_ im obigen Bericht. Da die Regeln auf `log` und `alert` eingestellt sind, werden die Anfragen _nicht blockiert_, aber zu Überwachungs-, Analyse- und Warnzwecken protokolliert.

- Der Einfachheit halber wird die OFAC-Regel hier nicht getestet.

## Prüfen von Warnhinweisen

Warnhinweise werden generiert, wenn die Traffic-Filterregeln ausgelöst werden. Sie können diese Warnhinweise im [AEM-Aktionscenter](https://experience.adobe.com/aem/actions-center) prüfen.

![WKND AEM-Aktionscenter](../assets/use-cases/wknd-aem-action-center.png)

## Analysieren der Ergebnisse

Um die Ergebnisse der Traffic-Filterregeln zu analysieren, können Sie die AEMCS CDN-Protokolle und das ELK-Dashboard-Tool verwenden. Befolgen Sie die Anweisungen im Einrichtungsabschnitt [CDN-Protokollaufnahme](../setup.md#ingest-cdn-logs), um die CDN-Protokolle in den ELK-Stack aufzunehmen.

Im folgenden Screenshot sehen Sie die CDN-Protokolle der AEM-Entwicklungsumgebung, die in den ELK-Stack aufgenommen wurden.

![WKND CDN-Protokolle ELK](../assets/use-cases/wknd-cdn-logs-elk.png)

Im ELK-Programm sollte das **CDN-Traffic-Dashboard** die während der simulierten DoS-Angriffe aufgetretenen Spitzen am **Edge** und am **Ursprung** anzeigen.

Die beiden Panels _Edge RPS per Client IP and POP_ und _Origin RPS per Client IP and POP_ zeigen die Anfragen pro Sekunde (RPS) am Edge bzw. am Ursprung an, gruppiert nach Client-IP und Point of Presence (POP).

![WKND CDN Edge-Traffic-Dashboard](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

Sie können auch andere Panels im CDN-Traffic-Dashboard verwenden, um die Traffic-Muster zu analysieren, z. B. _Top Client IPs_, _Top Countries_ und _Top User Agents_. Diese Panels unterstützen Sie dabei, potenzielle Bedrohungen zu identifizieren und Ihre Traffic-Filterregeln entsprechend anzupassen.

### Splunk-Integration

Kundinnen und Kunden, die die [Splunk Log-Weiterleitung](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) aktiviert haben, können neue Dashboards erstellen, um Traffic-Muster zu analysieren. 

Um Dashboards in Splunk zu erstellen, folgen Sie den Schritten [Splunk-Dashboards für AEMCS CDN Log Analysis](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

Der folgende Screenshot zeigt ein Beispiel für ein Splunk-Dashboard, das die maximalen Ursprungs- und Edge-Anforderungen pro IP anzeigt, damit Sie potenzielle DoS-Angriffe identifizieren können.

![Splunk-Dashboard – Max. Anfragen pro IP am Ursprung und am Edge](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## Zeitpunkt und Methode zum Optimieren von Regeln

Sie möchten verhindern, dass legitimer Traffic blockiert wird, und gleichzeitig Ihre AEM-Site vor potenziellen Bedrohungen schützen. Die Standard-Traffic-Filterregeln sind so konzipiert, dass sie vor Bedrohungen warnen und diese protokollieren (und schließlich blockieren, wenn der Modus gewechselt wird), ohne den legitimen Traffic zu blockieren.

Gehen Sie wie folgt vor, um die Regeln zu optimieren:

- **Überwachen Sie Traffic-Muster**: Verwenden Sie die CDN-Protokolle und das ELK-Dashboard, um Traffic-Muster zu überwachen und Anomalien oder Traffic-Spitzen zu identifizieren.
- **Passen Sie Schwellenwerte an**: Passen Sie basierend auf den Traffic-Mustern die Schwellenwerte in den Regeln durch Erhöhen oder Verringern der Ratenbegrenzungen an, um sie besser auf Ihre spezifischen Anforderungen abzustimmen. Wenn Sie beispielsweise bemerken, dass legitimer Traffic die Warnhinweise ausgelöst hat, können Sie die Ratenbegrenzungen erhöhen oder die Gruppierungen anpassen.
In der folgenden Tabelle finden Sie Anleitungen zur Auswahl der Schwellenwerte:

  | Variante | Wert |
  | :--------- | :------- |
  | Ursprung | Nehmen Sie den höchsten Wert der maximalen Ursprungsanfragen pro IP/POP unter **normalen** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS-Angriffs) und erhöhen Sie ihn um ein Vielfaches |
  | Edge | Nehmen Sie den höchsten Wert der maximalen Edge-Anfragen pro IP/POP unter **normalen** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS-Angriffs) und erhöhen Sie ihn um ein Vielfaches |

  Weitere Informationen finden Sie im Abschnitt [Auswählen von Schwellenwerten](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values).

- **Wechseln zu Blockierungsregeln**: Nachdem Sie die Traffic-Muster validiert und die Schwellenwerte angepasst haben, sollten Sie die Regeln in den Blockmodus überführen.

## Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie AEM-Websites mit von Adobe empfohlenen Standard-Traffic-Filterregeln in AEM as a Cloud Service vor Denial of Service(DoS)- und Distributed Denial of Service(DDoS)-Angriffen sowie Bot-Missbrauch schützen.

## Empfohlene WAF-Regeln

Erfahren Sie, wie Sie die von Adobe empfohlenen WAF-Regeln implementieren, um Ihre AEM-Websites vor komplexen Bedrohungen zu schützen, die herkömmliche Sicherheitsmaßnahmen durch erweiterte Methoden umgehen.

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln">Schützen von AEM-Websites mit WAF-Traffic-Filterregeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mit den von Adobe empfohlenen Traffic-Filterregeln der Web Application Firewall (WAF) in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen.</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF aktivieren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Anwendungsfälle – jenseits der Standardregeln

Für komplexere Szenarien können Sie die folgenden Anwendungsfälle erkunden. Diese demonstrieren, wie Sie benutzerdefinierte Traffic-Filterregeln basierend auf bestimmten Unternehmensanforderungen implementieren:

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
                    <p class="is-size-6">Erfahren Sie, wie Sie sensible Anfragen überwachen, indem Sie sie mit Traffic-Filterregeln in AEM as a Cloud Service protokollieren.</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
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
                    <p class="is-size-6">Erfahren Sie, wie Sie den Zugriff durch Blockierung bestimmter Anfragen mit Traffic-Filterregeln in AEM as a Cloud Service einschränken.</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Weitere Informationen</span>
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
                    <p class="is-size-6">Erfahren Sie, wie Sie Anfragen durch Transformation mit Traffic-Filterregeln in AEM as a Cloud Service normalisieren.</p>
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

- [Empfohlene Anfangsregeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)
