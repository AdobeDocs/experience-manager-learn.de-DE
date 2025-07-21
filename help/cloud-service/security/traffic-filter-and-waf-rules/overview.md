---
title: Überblick - Schutz von AEM-Websites
description: Erfahren Sie, wie Sie Ihre AEM-Websites mithilfe von Traffic-Filterregeln vor DoS, DDoS und bösartigem Traffic schützen können, einschließlich der Unterkategorie der WAF-Regeln (Web Application Firewall) in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---

# Überblick - Schutz von AEM-Websites

Erfahren Sie, wie Sie Ihre AEM-Websites mithilfe von **Traffic-Filterregeln** einschließlich der Unterkategorie „Web Application Firewall (WAF)“ in AEM as a Cloud Service vor Denial of Service (DoS), Distributed Denial of Service (DDoS), bösartigem Traffic **komplexen Angriffen**.

Außerdem erfahren Sie mehr über die Unterschiede zwischen standardmäßigen Traffic-Filterregeln und WAF-Traffic-Filterregeln, wann sie verwendet werden und wie Sie mit von Adobe empfohlenen Regeln beginnen.

>[!IMPORTANT]
>
> WAF-Traffic-Filterregeln erfordern eine zusätzliche Lizenz für **WAF-DDoS Protection** oder **Enhanced Security**. Standardmäßige Traffic-Filterregeln sind standardmäßig für Kunden von Sites und Forms verfügbar.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Einführung in die Traffic-Sicherheit in AEM as a Cloud Service

AEM as a Cloud Service nutzt eine integrierte CDN-Ebene, um die Bereitstellung Ihrer Website zu schützen und zu optimieren. Eine der wichtigsten Komponenten der CDN-Ebene ist die Möglichkeit, Traffic-Regeln zu definieren und durchzusetzen. Diese Regeln dienen als Schutzschild, um Ihre Website vor Missbrauch, Missbrauch und Angriffen zu schützen - ohne Leistungseinbußen.

Die Traffic-Sicherheit ist für die Aufrechterhaltung der Betriebszeit, den Schutz sensibler Daten und die Gewährleistung eines nahtlosen Erlebnisses für rechtmäßige Benutzer von entscheidender Bedeutung. AEM bietet zwei Kategorien von Sicherheitsregeln:

- **Standard-Traffic-Filterregeln**
- **Traffic-Filterregeln für Web Application Firewall (WAF)**

Die Regelsätze helfen Kunden dabei, gängige und komplexe Internet-Bedrohungen zu verhindern, den Lärm von böswilligen oder sich falsch verhalten Clients zu reduzieren und die Beobachtbarkeit durch Anforderungsprotokollierung, Blockierung und Mustererkennung zu verbessern.

## Unterschied zwischen Standard- und WAF-Traffic-Filterregeln

| Funktion | Standard-Traffic-Filterregeln | WAF-Traffic-Filterregeln |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Zweck | Missbrauch wie DoS, DoS, Scraping oder Bot-Aktivität verhindern | Erkennen und reagieren Sie auf ausgefeilte Angriffsmuster (z. B. OWASP Top 10), die auch vor Bots schützen |
| Beispiele | Ratenbegrenzung, Geoblocking, User-Agent-Filterung | SQL-Injection, XSS, bekannte Angriff-IPs |
| Flexibilität | Hochgradig über YAML konfigurierbar | Hochgradig über YAML konfigurierbar, mit vordefinierten WAF-Flags |
| Empfohlener Modus | Mit `log` beginnen und dann in den `block` wechseln | Beginnen Sie mit dem `block` für `ATTACK-FROM-BAD-IP` WAF-Flag und dem `log` für `ATTACK` WAF-Flag und wechseln Sie dann für beide in den `block` |
| Bereitstellung | Wird in YAML definiert und über die Cloud Manager-Konfigurations-Pipeline bereitgestellt | Wird in YAML mit `wafFlags` definiert und über die Cloud Manager-Konfigurations-Pipeline bereitgestellt |
| Lizenzierung | In Sites- und Forms-Lizenzen enthalten | **Erfordert WAF-DDoS-Schutz oder erweiterte Sicherheitslizenz** |

Die standardmäßigen Traffic-Filterregeln sind nützlich, um geschäftsspezifische Richtlinien zu erzwingen, z. B. Ratenbeschränkungen oder die Blockierung bestimmter Regionen, sowie die Blockierung des Traffics basierend auf Anfrageeigenschaften und Kopfzeilen wie IP-Adresse, Pfad oder Benutzeragent.
Die WAF-Traffic-Filterregeln bieten dagegen einen umfassenden proaktiven Schutz für bekannte Web-Exploits und Angriffsvektoren und verfügen über erweiterte Intelligenz, um falsch-positive Ergebnisse zu begrenzen (d. h. rechtmäßigen Traffic zu blockieren).
Um beide Regeltypen zu definieren, verwenden Sie die YAML-Syntax. Weitere Informationen finden Sie unter [Syntax von Traffic](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax)Filterregeln“.

## Wann und warum sie verwendet werden sollten

**Standard-Traffic-Filterregeln verwenden** wenn:

- Sie möchten organisationsspezifische Einschränkungen anwenden, z. B. die Drosselung der IP-Rate.
- Sie kennen bestimmte Muster (z. B. bösartige IP-Adressen, Regionen, Header), die gefiltert werden müssen.

**Verwenden von WAF-Traffic-Filterregeln** wenn:

- Sie wünschen einen umfassenden **proaktiven Schutz** vor weit verbreiteten bekannten Angriffsmustern (z. B. Injektion, Protokollmissbrauch) sowie bekannten böswilligen IPs, die aus erfahrenen Datenquellen gesammelt wurden.
- Sie möchten böswillige Anfragen ablehnen, während Sie die Wahrscheinlichkeit einschränken, rechtmäßigen Traffic zu blockieren.
- Sie möchten den Aufwand zum Schutz vor häufigen und komplexen Bedrohungen durch einfache Konfigurationsregeln begrenzen.

Gemeinsam bieten diese Regeln eine umfassende Verteidigungsstrategie, die es AEM as a Cloud Service-Kunden ermöglicht, sowohl proaktive als auch reaktive Maßnahmen zum Schutz ihrer digitalen Eigenschaften zu ergreifen.

## Von Adobe empfohlene Regeln

Adobe bietet empfohlene Regeln für standardmäßige Traffic-Filter- und WAF-Traffic-Filterregeln, mit denen Sie Ihre AEM-Sites schnell sichern können.

- **Standard-Traffic-Filterregeln** (standardmäßig verfügbar): Beheben gängiger Missbrauchsszenarien wie DoS-, DDoS- und Bot-Angriffe gegen **CDN-Edge**, **Herkunft** oder Traffic aus sanktionierten Ländern.\
  Beispiele dafür sind:
   - Ratenbegrenzende IPs, die mehr als 500 Anfragen/Sekunde _am CDN-Edge_
   - Ratenbegrenzende IPs, die mehr als 100 Anfragen/Sekunde _am Ursprung_
   - Sperren des Datenverkehrs aus Ländern, die vom Office of Foreign Assets Control (OFAC) aufgeführt werden

- **WAF-Traffic-Filterregeln** (Add-on-Lizenz erforderlich): Bietet zusätzlichen Schutz vor anspruchsvollen Bedrohungen, einschließlich [OWASP Top Ten](https://owasp.org/www-project-top-ten/)-Bedrohungen wie SQL Injection, Cross-Site Scripting (XSS) und anderen Angriffen auf Web-Anwendungen.
Beispiele dafür sind:
   - Blockieren von Anfragen von bekannten fehlerhaften IP-Adressen
   - Protokollieren oder Blockieren von verdächtigen Anfragen, die als Angriffe gekennzeichnet sind

>[!TIP]
>
> Wenden Sie zunächst die von **Adobe empfohlenen Regeln an** um von den Sicherheitskenntnissen und laufenden Updates von Adobe zu profitieren. Wenn Ihr Unternehmen bestimmte Risiken oder Sonderfälle hat oder falsch-positive Ergebnisse feststellt (Blockierung von legitimem Traffic), können Sie **benutzerdefinierte Regeln** definieren oder den Standardsatz entsprechend Ihren Anforderungen erweitern.

## Erste Schritte

Erfahren Sie, wie Sie Traffic-Filterregeln, einschließlich WAF-Regeln, in AEM as a Cloud Service definieren, bereitstellen, testen und analysieren, indem Sie das Setup-Handbuch und die folgenden Anwendungsfälle befolgen. Dadurch erhalten Sie Hintergrundwissen, damit Sie später die von Adobe empfohlenen Regeln sicher anwenden können.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln">Einrichten von Traffic-Filterregeln einschließlich WAF-Regeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie das Setup für das Erstellen, Bereitstellen, Testen und Analysieren der Ergebnisse von Traffic-Filterregeln, einschließlich WAF-Regeln, einrichten.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Jetzt beginnen</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Einrichtungshandbuch für von Adobe empfohlene Regeln

Dieses Handbuch enthält Schritt-für-Schritt-Anweisungen zum Einrichten und Bereitstellen der von Adobe empfohlenen Standard-Traffic-Filter- und WAF-Traffic-Filterregeln in Ihrer AEM as a Cloud Service-Umgebung.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Schützen von AEM-Websites mithilfe standardmäßiger Traffic-Filterregeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Schützen von AEM-Websites mithilfe standardmäßiger Traffic-Filterregeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Schützen von AEM-Websites mithilfe standardmäßiger Traffic-Filterregeln">Schutz von AEM-Websites mithilfe standardmäßiger Traffic-Filterregeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mithilfe von Adobe-empfohlenen Standard-Traffic-Filterregeln in AEM as a Cloud Service vor DoS, DDoS und Bot-Missbrauch schützen können.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Regeln anwenden</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Schützen von AEM-Websites mithilfe von WAF-Regeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Schützen von AEM-Websites mithilfe von WAF-Regeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Schützen von AEM-Websites mithilfe von WAF-Regeln">Schützen von AEM-Websites mithilfe von WAF-Regeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mithilfe der von Adobe empfohlenen Traffic-Filterregeln der Web Application Firewall (WAF) in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF aktivieren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Erweiterte Anwendungsfälle

Bei komplexeren Szenarien können Sie die folgenden Anwendungsfälle untersuchen, die zeigen, wie Sie benutzerdefinierte Traffic-Filterregeln basierend auf bestimmten Geschäftsanforderungen implementieren:

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="Überwachen sensibler Anfragen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Überwachen sensibler Anfragen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Überwachen sensibler Anfragen">Überwachen sensibler Anfragen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie sensible Anfragen überwachen, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service protokollieren.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="Einschränken des Zugriffs" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Einschränken des Zugriffs"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Einschränken des Zugriffs">Einschränken des Zugriffs</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie den Zugriff einschränken, indem Sie bestimmte Anfragen mithilfe von Traffic-Filterregeln in AEM as a Cloud Service blockieren.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="Normalisieren von Anfragen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Normalisieren von Anfragen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalisieren von Anfragen">Normalisieren von Anfragen</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Anfragen normalisieren können, indem Sie sie mithilfe von Traffic-Filterregeln in AEM as a Cloud Service transformieren.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Zusätzliche Ressourcen

- [Traffic-Filterregeln einschließlich WAF-Regeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
