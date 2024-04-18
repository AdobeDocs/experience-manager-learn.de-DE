---
title: Blockieren von DoS- und DoS-Angriffen mithilfe von Traffic-Filterregeln
description: Erfahren Sie, wie Sie DoS- und DDoS-Angriffe mithilfe von Traffic-Filterregeln im AEM as a Cloud Service bereitgestellten CDN blockieren.
version: Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-03-18T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
source-git-commit: 7f0f4d1b739cb63b96afc08eb31ab72a507c4722
workflow-type: tm+mt
source-wordcount: '1918'
ht-degree: 1%

---


# Blockieren von DoS- und DoS-Angriffen mithilfe von Traffic-Filterregeln

Erfahren Sie, wie Sie Denial-of-Service- (DoS-) und dezentrale Denial-of-Service-Angriffe (DDoS-Angriffe) mit **Traffic-Filter für Ratenbegrenzung** Regeln und andere Strategien auf dem AEM as a Cloud Service (AEMCS) verwalteten CDN. Diese Angriffe verursachen Traffic-Spitzen im CDN und möglicherweise im AEM Publish-Dienst (auch als Ursprung bekannt) und können die Reaktionsfähigkeit und Verfügbarkeit der Website beeinträchtigen.

Dieses Tutorial dient als Anleitung zu _wie Sie Ihre Traffic-Muster analysieren und die Ratenbegrenzung konfigurieren [Traffic-Filterregeln](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ um diese Angriffe abzumildern. In diesem Tutorial wird auch beschrieben, wie Sie [Warnhinweise konfigurieren](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) damit Sie benachrichtigt werden, wenn ein Verdacht auf einen Angriff besteht.

## Grundlegendes zum Schutz

Im Folgenden werden die standardmäßigen DDoS-Schutzmaßnahmen für Ihre AEM Website beschrieben:

- **Zwischenspeicherung:** Bei guten Caching-Richtlinien sind die Auswirkungen eines DDoS-Angriffs eingeschränkter, da das CDN verhindert, dass die meisten Anforderungen an den Ursprung gehen und die Leistung beeinträchtigt.
- **Automatische Skalierung:** Die Autoren- und Veröffentlichungsdienste AEM automatisch skaliert, um Traffic-Spitzen zu bewältigen, obwohl sie durch einen plötzlichen, massiven Anstieg des Traffics noch immer beeinträchtigt werden können.
- **Blocking:** Das Adobe-CDN blockiert Traffic an die Herkunft, wenn er eine Adobe-definierte Rate von einer bestimmten IP-Adresse pro CDN PoP (Point of Presence) überschreitet.
- **Warnhinweis:** Das Aktionszentrum sendet eine Benachrichtigung zu einer Traffic-Spitze bei der Warnung zur Herkunft, wenn der Traffic eine bestimmte Rate überschreitet. Dieser Warnhinweis wird ausgelöst, wenn der Traffic zu einem bestimmten CDN-PoP den Wert _Adobe-definiert_ Anfragerate pro IP-Adresse. Siehe [Warnhinweise zu Traffic-Filterregeln](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) für weitere Details.

Diese integrierten Schutzmaßnahmen sollten als Grundlage für die Fähigkeit eines Unternehmens betrachtet werden, die Auswirkungen eines DDoS-Angriffs auf die Leistung zu minimieren. Da jede Website unterschiedliche Leistungsmerkmale aufweist und möglicherweise Leistungseinbußen aufweist, bevor die Adobe-definierte Ratenbegrenzung erreicht ist, wird empfohlen, den Standardschutz um _Kundenkonfiguration_.

Sehen wir uns einige zusätzliche, empfohlene Maßnahmen an, die Kunden ergreifen können, um ihre Websites vor DDoS-Angriffen zu schützen:

- Declare **Traffic-Filterregeln für Ratenbegrenzung** um Traffic zu blockieren, der eine bestimmte Rate überschreitet, und zwar von einer einzelnen IP-Adresse aus pro PoP. Hierbei handelt es sich in der Regel um einen niedrigeren Schwellenwert als das von der Adobe definierte Ratenlimit.
- Konfigurieren **Warnungen** über Regeln zum Traffic-Filter für Ratenbegrenzungen durch eine &quot;Warnhinweis-Aktion&quot;festgelegt, sodass beim Auslösen der Regel eine Benachrichtigung für das Aktionszentrum gesendet wird.
- Verbessern der Cache-Abdeckung durch Deklarieren **Anfragetransformationen** , um Abfrageparameter zu ignorieren.

>[!NOTE]
>
>Die [Warnhinweise zu Traffic-Filterregeln](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) wurde noch nicht veröffentlicht. Um Zugriff über das Programm für frühe Anwender zu erhalten, senden Sie eine E-Mail an **<aemcs-waf-adopter@adobe.com>**.

### Varianten von Traffic-Regeln für Ratenbegrenzungen {#rate-limit-variations}

Es gibt zwei Varianten von Traffic-Regeln für Ratenbegrenzungen:

1. Edge - blockiert Anforderungen basierend auf der Rate des gesamten Traffics (einschließlich der Anforderungen, die aus dem CDN-Cache bereitgestellt werden können) für eine bestimmte IP-Adresse pro PoP.
1. Origin - blockiert Anfragen basierend auf der Traffic-Rate, die für die Herkunft, für eine bestimmte IP-Adresse und pro PoP bestimmt ist.

## Journey

Die folgenden Schritte zeigen den wahrscheinlichen Prozess, durch den Kunden ihre Websites schützen sollten.

1. Erkennen Sie die Notwendigkeit einer Traffic-Filterregel für die Ratenbegrenzung. Dies kann das Ergebnis einer Warnmeldung über eine vordefinierte Traffic-Spitze beim Ursprung sein, oder es kann sich um eine proaktive Entscheidung handeln, Vorsichtsmaßnahmen zu treffen, um das Risiko einer erfolgreichen DDoS zu verringern.
1. Analysieren Sie Traffic-Muster mithilfe eines Dashboards, wenn Ihre Site bereits live ist, um die optimalen Schwellenwerte für Ihre Traffic-Filterregeln für die Ratenbegrenzung zu ermitteln. Wenn Ihre Site noch nicht live ist, wählen Sie Werte basierend auf Ihren Traffic-Erwartungen aus.
1. Konfigurieren Sie mithilfe der Werte aus dem vorherigen Schritt Traffic-Filterregeln für die Ratenbegrenzung. Stellen Sie sicher, dass Sie die entsprechenden Warnhinweise aktivieren, damit Sie benachrichtigt werden, wenn der Schwellenwert erreicht ist.
1. Erhalten Sie Warnhinweise zu Traffic-Filterregeln, sobald es zu Traffic-Spitzen kommt. So erhalten Sie wertvolle Einblicke darüber, ob Ihr Unternehmen potenziell von böswilligen Akteuren angesprochen wird.
1. Gehen Sie bei Bedarf in Bezug auf die Warnung vor. Analysieren Sie den Traffic, um festzustellen, ob die Spitze legitime Anforderungen widerspiegelt und nicht einen Angriff. Erhöhen Sie die Schwellenwerte, wenn der Traffic legitim ist, oder senken Sie sie gegebenenfalls.

Der Rest dieses Tutorials führt Sie durch diesen Prozess.

## Erkennen der Notwendigkeit, Regeln zu konfigurieren {#recognize-the-need}

Wie bereits erwähnt, blockiert das Adobe standardmäßig den Traffic beim CDN, der eine bestimmte Rate überschreitet. Bei einigen Websites kann es jedoch vorkommen, dass die Leistung unter diesem Schwellenwert liegt. Daher sollten Regeln für Traffic-Filter für Ratenbegrenzungen konfiguriert werden.

Idealerweise würden Sie die Regeln konfigurieren, bevor Sie in die Produktion einsteigen. In der Praxis deklarieren viele Organisationen Regeln nur dann reaktiv, wenn sie auf eine Traffic-Spitze hingewiesen haben, die auf einen wahrscheinlichen Angriff hinweist.

Adobe sendet eine Traffic-Spitze bei der Ursprungswarnung als [Information über Aktionen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/actions-center) wenn ein standardmäßiger Schwellenwert für Traffic von einer einzelnen IP-Adresse überschritten wird, für einen bestimmten PoP. Wenn Sie einen solchen Warnhinweis erhalten haben, wird empfohlen, eine Traffic-Filterregel für die Ratenbegrenzung zu konfigurieren. Diese Standardwarnung unterscheidet sich von den Warnhinweisen, die von Kunden bei der Definition von Traffic-Filterregeln explizit aktiviert werden müssen, worüber Sie in einem zukünftigen Abschnitt erfahren werden.


## Analyse von Traffic-Mustern {#analyze-traffic}

Wenn Ihre Site bereits live ist, können Sie die Traffic-Muster mithilfe von CDN-Protokollen und einer der folgenden Methoden analysieren:

### ELK - Konfigurieren des Dashboard-Tools

Die **Elasticsearch, Logstash und Kibana (ELK)** Die von Adobe bereitgestellten Dashboard-Tools können zur Analyse der CDN-Protokolle verwendet werden. Diese Tools enthalten ein Dashboard, das die Traffic-Muster visualisiert, sodass Sie die optimalen Schwellenwerte für Ihre Traffic-Filterregeln für die Ratenbegrenzung leichter ermitteln können.

- Klonen Sie die [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) GitHub-Repository.
- Richten Sie die Tools ein, indem Sie den Anweisungen [Einrichten des ELK-Docker-Containers](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool?tab=readme-ov-file#how-to-set-up-the-elk-docker-container) Schritte.
- Importieren Sie im Rahmen der Einrichtung die `traffic-filter-rules-analysis-dashboard.ndjson` -Datei, um die Daten zu visualisieren. Die _CDN-Traffic_ Das Dashboard enthält Visualisierungen, die die maximale Anzahl von Anforderungen pro IP/POP an CDN Edge und Origin anzeigen.
- Aus dem [Cloud Manager](https://my.cloudmanager.adobe.com/)s _Umgebungen_ -Karte, laden Sie die CDN-Protokolle des AEMCS Publish-Dienstes herunter.

  ![Downloads von Cloud Manager-CDN-Protokollen](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Es kann bis zu 5 Minuten dauern, bis die neuen Anfragen in den CDN-Protokollen angezeigt werden.

### Splunk - Konfigurieren der Dashboard-Werkzeuge

Kunden, die [Splunk-Protokollweiterleitung aktiviert](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) kann ein neues Dashboard erstellen, um die Traffic-Muster zu analysieren. Die folgende XML-Datei hilft Ihnen beim Erstellen eines Dashboards in Splunk:

- [CDN - Traffic-Dashboard](./assets/traffic-dashboard.xml): Dieses Dashboard bietet Einblicke in die Traffic-Muster am CDN Edge und Origin. Sie enthält Visualisierungen, die die maximale Anzahl von Anforderungen pro IP/POP an CDN Edge und Origin anzeigen.

### Datensuche

Die folgenden Visualisierungen sind in den ELK- und Splunk-Dashboards verfügbar:

- **Edge RPS pro Client-IP und POP**: Diese Visualisierung zeigt die maximale Anzahl von Anfragen pro IP/POP **am CDN-Edge**. Der Spitzenwert in der Visualisierung gibt die maximale Anforderungsnummer an.

  **ELK-Dashboard**:
  ![ELK-Dashboard - Max. Anforderungen pro IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Splunk-Dashboard**:\
  ![Splunk-Dashboard - Max. Anforderungen pro IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **Ursprüngliche RPS pro Client-IP und POP**: Diese Visualisierung zeigt die maximale Anzahl von Anfragen pro IP/POP **des Ursprungs**. Der Spitzenwert in der Visualisierung gibt die maximale Anforderungsnummer an.

  **ELK-Dashboard**:
  ![ELK-Dashboard - Max. Herkunftsanforderungen pro IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Splunk-Dashboard**:
  ![Splunk-Dashboard - Max. Ausgangsanforderungen pro IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Auswählen von Schwellenwerten

Die Schwellenwerte für die Traffic-Filterregeln für die Ratenbegrenzung sollten auf der obigen Analyse basieren und sicherstellen, dass der legale Traffic nicht blockiert wird. In der folgenden Tabelle finden Sie Anleitungen zur Auswahl der Schwellenwerte:

| Variante | Wert |
| :--------- | :------- |
| Origin | Nehmen Sie den höchsten Wert der Max-Origin-Anforderungen pro IP/POP unter **normal** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS) und erhöhen Sie sie um ein Vielfaches |
| Edge | Nehmen Sie den höchsten Wert der Max. Edge-Anforderungen pro IP/POP unter **normal** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS) und erhöhen Sie sie um ein Vielfaches |

Das zu verwendende Mehrfach hängt von Ihren Erwartungen an normale Traffic-Spitzen aufgrund von organischem Traffic, Kampagnen und anderen Ereignissen ab. Ein Vielfaches zwischen 5 und 10 kann vernünftig sein.

Wenn Ihre Site noch nicht live ist, können keine Daten analysiert werden. Stellen Sie daher sicher, welche Werte für die Traffic-Filterregeln für die Ratenbegrenzung festgelegt werden sollten. Zum Beispiel:

| Variante | Wert |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origin | 100 |

## Regeln konfigurieren {#configure-rules}

Konfigurieren Sie die **Traffic-Filter für Ratenbegrenzung** Regeln in der AEM Ihres Projekts `/config/cdn.yaml` mit Werten, die auf der obigen Diskussion basieren. Wenden Sie sich bei Bedarf an Ihr Web Security-Team, um sicherzustellen, dass die Ratengrenzwerte angemessen sind und nicht den legitimen Traffic blockieren.

Siehe Abschnitt [Erstellen von Regeln in Ihrem AEM Projekt](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) für weitere Details.

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
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average            
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true   
          
```

Beachten Sie, dass sowohl die Ursprungs- als auch die Edge-Regeln deklariert werden und dass die Eigenschaft alert auf `true` sodass Sie Warnhinweise erhalten können, sobald die Schwelle erreicht ist, was wahrscheinlich auf einen Angriff hinweist.

>[!NOTE]
>
>Die _experimentell_ prefix_ vor experimental_alert wird entfernt, wenn die Warnhinweisfunktion veröffentlicht wird. Um dem frühen Adopter-Programm beizutreten, senden Sie eine E-Mail an **<aemcs-waf-adopter@adobe.com>**.

Es wird empfohlen, den Aktionstyp auf die anfängliche Protokollierung festzulegen, damit Sie den Traffic für einige Stunden oder Tage überwachen können, um sicherzustellen, dass der zulässige Traffic diese Raten nicht überschreitet. Wechseln Sie nach einigen Tagen in den Blockmodus.

Führen Sie die folgenden Schritte aus, um die Änderungen in Ihrer AEMCS-Umgebung bereitzustellen:

- Übertragen Sie die oben genannten Änderungen in Ihr Cloud Manager-Git-Repository.
- Stellen Sie die Änderungen mithilfe der Config Pipeline von Cloud Manager in der AEMCS-Umgebung bereit. Siehe [Bereitstellen von Regeln über Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) für weitere Details.
- So überprüfen Sie die **Traffic-Filterregel zur Ratenbegrenzung** wie erwartet funktioniert, können Sie einen Angriff simulieren, wie im Abschnitt [Angriffssimulation](#attack-simulation) Abschnitt. Begrenzen Sie die Anzahl der Anforderungen auf einen Wert, der über dem in der Regel festgelegten Ratenlimit liegt.

### Konfigurieren von Konvertierungsregeln für Anforderungen {#configure-request-transform-rules}

Zusätzlich zu den Traffic-Filterregeln für die Ratenbegrenzung wird empfohlen, [Anfragetransformationen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) , um die Abfrage-Parameter zu deaktivieren, die von der Anwendung nicht benötigt werden, um Möglichkeiten zu minimieren, den Cache durch Cache-Busting-Techniken zu umgehen. Wenn Sie beispielsweise nur `search` und `campaignId` -Abfrageparameter, kann die folgende Regel deklariert werden:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Warnungen zu Traffic-Filterregeln erhalten {#receiving-alerts}

Wie oben erwähnt, wenn die Traffic-Filterregel *experimental_alert: true*, wird ein Warnhinweis empfangen, wenn die Regel übereinstimmt.

## Maßnahmen bei Warnungen {#acting-on-alerts}

Manchmal ist der Warnhinweis informativ und gibt Ihnen ein Gefühl für die Häufigkeit von Angriffen. Es lohnt sich, Ihre CDN-Daten mithilfe des oben beschriebenen Dashboards zu analysieren und zu überprüfen, ob die Traffic-Spitze auf einen Angriff und nicht nur auf einen Anstieg des legitimen Traffic-Volumens zurückzuführen ist. Wenn dies der Fall ist, sollten Sie Ihre Schwelle erhöhen.

## Angriffssimulation{#attack-simulation}

In diesem Abschnitt werden Methoden zum Simulieren eines DoS-Angriffs beschrieben, mit dem Daten für die in diesem Tutorial verwendeten Dashboards generiert und überprüft werden können, ob konfigurierte Regeln Angriffe erfolgreich blockieren.

>[!CAUTION]
>
> Führen Sie diese Schritte nicht in einer Produktionsumgebung aus. Die folgenden Schritte dienen nur Simulationszwecken.
> 
>Wenn Sie einen Warnhinweis mit einer Traffic-Spitze erhalten haben, fahren Sie mit dem [Analyse von Traffic-Mustern](#analyzing-traffic-patterns) Abschnitt.

Um einen Angriff zu simulieren, Tools wie [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta), und andere können verwendet werden.

### Edge-Anforderungen

Verwenden Sie Folgendes [Vegeta](https://github.com/tsenart/vegeta) -Befehl können Sie viele Anfragen an Ihre Website richten:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

Der obige Befehl führt 120 Anfragen für 5 Sekunden aus und gibt einen Bericht aus. Wenn die Website nicht ratenbeschränkt ist, kann dies zu einer Traffic-Spitze führen.

### Ausgangsanforderungen

Um den CDN-Cache zu umgehen und Anforderungen an die Herkunft (AEM Veröffentlichungsdienst) zu stellen, können Sie der URL einen eindeutigen Abfrageparameter hinzufügen. Siehe Beispiel für ein Apache JMeter-Skript aus dem [Simulieren des DoS-Angriffs mithilfe des JMeter-Skripts](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

