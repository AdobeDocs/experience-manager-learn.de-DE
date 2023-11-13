---
title: Beispiele und Ergebnisanalyse für Traffic-Filterregeln, einschließlich WAF-Regeln
description: Lernen Sie verschiedene Traffic-Filterregeln kennen, darunter Beispiele für WAF-Regeln. Außerdem erfahren Sie, wie Sie die Ergebnisse mit den CDN-Protokollen von AEM as a Cloud Service (AEMCS) analysieren.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 100%

---

# Beispiele und Ergebnisanalyse für Traffic-Filterregeln, einschließlich WAF-Regeln

Erfahren Sie, wie Sie verschiedene Arten von Traffic-Filterregeln deklarieren und die Ergebnisse mit den CND-Protokollen und Dashboard-Tools von Adobe Experience Manager as a Cloud Service (AEMCS) analysieren.

In diesem Abschnitt werden praktische Beispiele für Traffic-Filterregeln, einschließlich WAF-Regeln, vorgestellt. Sie erfahren, wie Sie Anfragen basierend auf URI (oder Pfad), IP-Adresse, Anzahl der Anfragen und verschiedenen Angriffstypen protokollieren, zulassen und blockieren, indem Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) verwenden.

Darüber hinaus erfahren Sie, wie Sie Dashboard-Tools verwenden, die AEMCS-CDN-Protokolle erfassen, um wichtige Metriken über von Adobe bereitgestellten Beispiel-Dashboards zu visualisieren.

Um Ihre spezifischen Anfragen zu erfüllen, können Sie benutzerdefinierte Dashboards erweitern und erstellen, um tiefere Einblicke zu erhalten und die Regelkonfigurationen für Ihre AEM Sites zu optimieren.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Beispiele

Im Folgenden werden verschiedene Beispiele für Traffic-Filterregeln, einschließlich WAF-Regeln, vorgestellt. Vergewissern Sie sich, dass Sie den erforderlichen Einrichtungsprozess wie zuvor im Abschnitt [Einrichtung](./how-to-setup.md) abgeschlossen haben und dass Sie das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) geklont haben.

### Protokollierung von Anfragen

Beginnen Sie mit der **Protokollierung der Anfragen von WKND-Anmelde- und Abmeldepfaden** für den  Publish-Service von AEM.

- Fügen Sie die folgende Regel zur Datei `/config/cdn.yaml` des WKND-Projekts hinzu.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
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

- Implementieren Sie die Änderungen in der AEM-Entwicklungsumgebung mithilfe der Konfigurationspipeline `Dev-Config` von Cloud Manager, die Sie [zuvor erstellt](how-to-setup.md#deploy-rules-through-cloud-manager) haben.

  ![Cloud Manager-Konfigurations-Pipeline](./assets/cloud-manager-config-pipeline.png)

- Testen Sie die Regel, indem Sie sich bei der WKND-Site Ihres Programms im Publish-Service anmelden und abmelden (z. B. `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Sie können `asmith/asmith` als Benutzernamen und Kennwort verwenden.

  ![WKND-Anmeldung](./assets/wknd-login.png)

#### Analysieren{#analyzing}

Im Folgenden analysieren wir die Ergebnisse der Regel `publish-auth-requests`, indem wir die AEMCS-CDN-Protokolle aus Cloud Manager herunterladen und die [Dashboard-Tools](how-to-setup.md#analyze-results-using-elk-dashboard-tool) verwenden, die Sie im vorherigen Kapitel eingerichtet haben.

- Laden Sie auf der Karte **Umgebungen** von [Cloud Manager](https://my.cloudmanager.adobe.com/) die CDN-Protokolle des **Publish-Services** von AEMCS herunter.

  ![Downloads von Cloud Manager-CDN-Protokollen](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Es kann bis zu 5 Minuten dauern, bis die neuen Anfragen in den CDN-Protokollen angezeigt werden.

- Kopieren Sie die heruntergeladene Protokolldatei (beispielsweise `publish_cdn_2023-10-24.log` im folgenden Screenshot) in den Ordner `logs/dev` des Elastic-Dashboard-Tool-Projekts.

  ![Ordner für ELK-Tool-Protokolle](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Aktualisieren Sie Seite des Elastic-Dashboard-Tools.
   - Bearbeiten Sie im Abschnitt **Globaler Filter** den Filter `aem_env_name.keyword` und wählen Sie den Wert der `dev`-Umgebung aus.

     ![Globaler Filter für das ELK-Tool](./assets/elk-tool-global-filter.png)

   - Um das Zeitintervall zu ändern, klicken Sie auf das Kalendersymbol oben rechts und wählen Sie das gewünschte Zeitintervall aus.

     ![Zeitintervall des ELK-Tools](./assets/elk-tool-time-interval.png)

- Überprüfen Sie die Bedienfelder **Analysierte Anfragen**, **Gekennzeichnete Anfragen**, und **Gekennzeichnete Anfragen – Details** im aktualisierten Dashboard. Bei übereinstimmenden CDN-Protokolleinträgen sollten die Werte der Client-IP (cli_ip), des Hosts, der URL, der Aktion (waf_action) und des Regelnamens (waf_match) jedes Eintrags angezeigt werden.

  ![ELK-Tool-Dashboard](./assets/elk-tool-dashboard.png)


### Blockieren von Anfragen

In diesem Beispiel fügen wir eine Seite in einem _internen_ Ordner im Pfad `/content/wknd/internal` im bereitgestellten WKND-Projekt hinzu. Deklarieren Sie dann eine Traffic-Filterregel, die Traffic zu Unterseiten von allen anderen Adressen als einer angegebenen IP-Adresse, die Ihrer Organisation entspricht (z. B. ein Unternehmens-VPN), **blockiert**.

Sie können entweder eine eigene interne Seite erstellen (z. B. `demo-page.html`) oder das [angehängte Paket](./assets/demo-internal-pages-package.zip) verwenden.

- Fügen Sie die folgende Regel in der Datei `/config/cdn.yaml` des WKND-Projekts hinzu:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

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

- Implementieren Sie die Änderungen in der AEM Entwicklungsumgebung mithilfe der [früher erstellten](how-to-setup.md#deploy-rules-through-cloud-manager) Konfigurationspipeline `Dev-Config` in Cloud Manager.

- Testen Sie die Regel, indem Sie auf die interne Seite der WKND-Site zugreifen, zum Beispiel `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, oder mithilfe des folgenden CURL-Befehls:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Wiederholen Sie den obigen Schritt sowohl von der in der Regel verwendeten IP-Adresse als auch von einer anderen IP-Adresse (z. B. über Ihr Mobiltelefon).

#### Analysieren

Um die Ergebnisse der Regel `block-internal-paths` zu analysieren, führen Sie dieselben Schritte aus wie im [früheren Beispiel](#analyzing) beschrieben.

Diesmal sollten Sie jedoch die **blockierten Anfragen** und die entsprechenden Werte in den Spalten Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) sehen.

![ELK-Tool-Dashboard – Blockierte Anfragen](./assets/elk-tool-dashboard-blocked.png)


### Vermeiden von DoS-Angriffen

Wir **vermeiden DoS-Angriffe** durch das Blockieren von Anfragen von einer IP-Adresse, die 100 Anfragen pro Sekunde stellt. Die Adresse wird 5 Minuten lang blockiert.

- Fügen Sie die folgende [Traffic-Filterregel zur Ratenbegrenzung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de#ratelimit-structure) in der Datei `/config/cdn.yaml` des WKND-Projekts hinzu.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block     
```

>[!WARNING]
>
>In Ihrer Produktionsumgebung können Sie mit Ihrem Team für Web-Sicherheit zusammenarbeiten, um die entsprechenden Werte für `rateLimit` zu bestimmen.

- Übertragen, pushen und implementieren Sie Änderungen wie in den [früheren Beispielen](#logging-requests) erwähnt.

- Um den DoS-Angriff zu simulieren, verwenden Sie den folgenden [Vegeta](https://github.com/tsenart/vegeta)-Befehl.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Dieser Befehl sendet 120 Anfragen in 5 Sekunden und gibt einen Bericht aus. Wie Sie sehen können, beträgt die Erfolgsrate 32,5 %. Für den Rest wird ein 406-HTTP-Antwort-Code empfangen, der zeigt, dass der Traffic blockiert wurde.

  ![Vegeta-DoS-Angriff](./assets/vegeta-dos-attack.png)

#### Analysieren

Um die Ergebnisse der Regel `prevent-dos-attacks` zu analysieren, führen Sie dieselben Schritte aus wie im [früheren Beispiel](#analyzing) beschrieben.

Diesmal sollten Sie viele **blockierte Anfragen** und die entsprechenden Werten in den Spalten Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) sehen.

![ELK-Tool-Dashboard – DoS-Anfrage](./assets/elk-tool-dashboard-dos.png)

Außerdem zeigen die Bedienfelder zu den **Top 100 der Angriffe nach Client-IP, Land und Benutzeragent** zusätzliche Details an, die zur weiteren Optimierung der Regelkonfiguration verwendet werden können.

![ELK-Tool-Dashboard – DoS Top 100 Anfragen](./assets/elk-tool-dashboard-dos-top-100.png)

### WAF-Regeln

Die Beispiele für Traffic-Filterregeln können bisher von allen Kundinnen und Kunden von „Sites and Forms“ konfiguriert werden.

Als Nächstes sollten wir das Erlebnis einer Kundin oder eines Kunden untersuchen, die bzw. der eine Enhanced Security- oder WAF-DDoS-Schutzlizenz erworben hat, mit der erweiterte Regeln konfiguriert werden können, um Websites vor komplexeren Angriffen zu schützen.

Bevor Sie fortfahren, aktivieren Sie den WAF-DDoS-Schutz für Ihr Programm, wie in der Dokumentation zu Traffic-Filterregeln beschrieben. [Einrichtungsschritte](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de#setup).

#### Ohne WAF-Flags

Lassen Sie uns zunächst das Erlebnis sehen, noch bevor WAF-Regeln deklariert werden. Wenn das WAF-DDoS in Ihrem Programm aktiviert ist, protokolliert Ihr CDN standardmäßig alle Übereinstimmungen mit bösartigem Traffic, sodass Sie über die richtigen Informationen verfügen, um geeignete Regeln zu finden.

Beginnen wir damit, die WKND-Site anzugreifen, ohne eine WAF-Regel hinzuzufügen (oder die Eigenschaft `wafFlags`) und analysieren Sie die Ergebnisse.

- Um einen Angriff zu simulieren, verwenden Sie den folgenden [Nikto](https://github.com/sullo/nikto)-Befehl, der in 6 Minuten ca. 700 schädliche Anfragen sendet.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulation von Nikto-Angriffen](./assets/nikto-attack.png)

  Weitere Informationen zur Angriffssimulation finden Sie in der Dokumentation zu [Nikto – Scan-Optimierung](https://github.com/sullo/nikto/wiki/Scan-Tuning), in der Sie erfahren, wie Sie den Typ von Testangriffen angeben, die ein- oder ausgeschlossen werden sollen.

##### Analysieren

Um die Ergebnisse der Angriffsimulation zu analysieren, führen Sie dieselben Schritte aus wie im [früheren Beispiel](#analyzing) beschrieben.

Diesmal sollten Sie jedoch die **gekennzeichneten Anfragen** und die entsprechenden Werte in den Spalten Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) sehen. Mithilfe dieser Informationen können Sie die Ergebnisse analysieren und die Regelkonfiguration optimieren.

![ELK-Tool-Dashboard – WAF Gekennzeichnete Anfrage](./assets/elk-tool-dashboard-waf-flagged.png)

Beachten Sie, dass die Bedienfelder **Verteilung von WAF-Flags** und **Top-Angriffe** zusätzliche Details anzeigen, die zur weiteren Optimierung der Regelkonfiguration verwendet werden können.

![ELK-Tool-Dashboard – WAF-Flags Angriffe Anfrage](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ELK-Tool-Dashboard – WAF Top-Angriffe Anfrage](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Mit WAF-Flags

Fügen wir nun eine WAF-Regel hinzu, die die Eigenschaft `wafFlags` als Teil der Eigenschaft `action` enthält und **die simulierten Angriffsanfragen blockiert**.

Aus der Syntaxperspektive ähneln die WAF-Regeln den zuvor erkannten Regeln. Die Eigenschaft `action` verweist auf einen oder mehrere `wafFlags`-Werte. Weitere Informationen über `wafFlags` finden Sie im Abschnitt [WAF-Flags-Liste](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de#waf-flags-list).

- Fügen Sie die folgende Regel in der Datei `/config/cdn.yaml` des WKND-Projekts hinzu. Beachten Sie, dass die Regel `block-waf-flags` einige der WAF-Flags enthält, die in den Dashboard-Tools angezeigt wurden, wenn sie mit simuliertem schädlichen Traffic angegriffen wurden. Tatsächlich ist es empfehlenswert, im Laufe der Zeit Protokolle zu analysieren, um zu bestimmen, welche neuen Regeln deklariert werden sollten, wenn sich die Bedrohungslandschaft entwickelt.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
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
            - SIGSCI-IP
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
```

- Übertragen, pushen und implementieren Sie Änderungen wie in den [früheren Beispielen](#logging-requests) erwähnt.

- Um einen Angriff zu simulieren, verwenden Sie denselben [Nikto](https://github.com/sullo/nikto)-Befehl wie zuvor.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analysieren

Wiederholen Sie die Schritte wie im [früheren Beispiel](#analyzing) beschrieben.

Dieses Mal sollten Einträge unter **blockierte Anfragen** und die entsprechenden Werte in den Spalten Client-IP (cli_ip), Host, URL, Aktion (waf_action) und Regelname (waf_match) zu sehen sein.

![ELK-Tool-Dashboard – WAF Blockierte Anfrage](./assets/elk-tool-dashboard-waf-blocked.png)

Außerdem zeigen die Bedienfelder **WAF-Flags-Verteilung** und **Top-Angriffe** zusätzliche Details an.

![ELK-Tool-Dashboard – WAF-Flags Angriffe Anfrage](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ELK-Tool-Dashboard – WAF Top-Angriffe Anfrage](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Umfassende Analyse

In vorherigen Abschnitt _Analyse_ haben Sie erfahren, wie Sie die Ergebnisse bestimmter Regeln mithilfe des Dashboard-Tools analysieren können. Sie können die Analyseergebnisse mithilfe anderer Dashboard-Bedienfelder weiter untersuchen, darunter:


- analysierte, gekennzeichnete und blockierte Anfragen
- Verteilung der WAF-Flags über einen bestimmten Zeitraum
- ausgelöste Traffic-Filterregeln im Zeitverlauf
- Top-Angriffe nach WAF-Flag-ID
- Traffic-Filter mit der häufigsten Auslösung
- Die 100 wichtigsten Ursprünge von Angriffen nach Client-IP, Land und Benutzeragent

![ELK-Tool-Dashboard – Umfassende Analyse](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![ELK-Tool-Dashboard – Umfassende Analyse](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Nächster Schritt

Machen Sie sich mit empfohlenen [Best Practices](./best-practices.md) vertraut, um das Risiko von Sicherheitsverletzungen zu reduzieren.

## Zusätzliche Ressourcen

[Syntax für Traffic-Filterregeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de#rules-syntax)

[CDN-Protokollformat](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=de#cdn-log-format)
