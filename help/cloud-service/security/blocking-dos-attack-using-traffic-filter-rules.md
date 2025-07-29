---
title: Blockieren von DoS-, DDoS- und komplexen Angriffen mit Traffic-Filterregeln
description: Erfahren Sie, wie Sie DoS-, DDoS- und komplexe Angriffe mit Traffic-Filterregeln in AEM as a Cloud Service blockieren.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '609'
ht-degree: 100%

---

# Blockieren von DoS-, DDoS- und komplexen Angriffen mit Traffic-Filterregeln

Erfahren Sie, wie Sie Denial of Service(DoS)-, Distributed Denial of Service(DDoS)- und komplexe Angriffe mit **Traffic-Filterregeln** im verwalteten CDN AEM as a Cloud Service (AEMCS) blockieren. 

Diese Angriffe führen zu Traffic-Spitzen beim CDN und möglicherweise beim AEM Publish-Service (auch bekannt als Ursprung) und können die Reaktionsfähigkeit und Verfügbarkeit der Website beeinträchtigen.

Dieser Artikel bietet einen Überblick über die Standardschutzmechanismen für Ihre AEM-Website und darüber, wie Sie diese Schutzmechanismen durch Kundenkonfiguration erweitern können. Außerdem wird beschrieben, wie Sie Traffic-Muster analysieren und Standard-Traffic-Filterregeln konfigurieren, um diese Angriffe abzuwehren.

## Standardschutzmechanismen in AEM as a Cloud Service

Im Folgenden werden die standardmäßigen DDoS-Schutzmaßnahmen für Ihre AEM-Website beschrieben:

- **Caching:** Bei guten Caching-Richtlinien sind die Auswirkungen eines DDoS-Angriffs geringer, da das CDN die meisten Anfragen daran hindert, an den Ursprung zu gehen und die Leistung zu beeinträchtigen.
- **Automatische Skalierung:** Die Author- und Publish-Services von AEM verwenden die automatische Skalierung, um Traffic-Spitzen zu bewältigen, können aber immer noch durch einen plötzlichen, massiven Anstieg des Traffics beeinträchtigt werden.
- **Blockieren:** Das Adobe-CDN blockiert den Traffic zum Ursprung, wenn er eine von Adobe definierte Rate von einer bestimmten IP-Adresse pro CDN PoP (Point of Presence) überschreitet.
- **Warnhinweis:** Das Aktionszentrum sendet eine Benachrichtigung zu einer Traffic-Spitze, wenn der Traffic eine bestimmte Rate überschreitet. Dieser Alarm wird ausgelöst, wenn der Traffic zu einem bestimmten CDN PoP eine von _Adobe definierte_ Anfragerate pro IP-Adresse überschreitet. Weitere Informationen finden Sie unter [Warnhinweise zu Traffic-Filterregeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts).

Diese integrierten Schutzmechanismen sollten als Grundlage für die Fähigkeit eines Unternehmens betrachtet werden, die Auswirkungen eines DDoS-Angriffs auf die Leistung zu minimieren. Da jede Website unterschiedliche Leistungsmerkmale aufweist und es zu einer Leistungsverschlechterung kommen kann, bevor die von Adobe definierte Ratenbegrenzung erreicht wird, wird empfohlen, die Standardschutzmechanismen durch _Kundenkonfiguration_ zu erweitern.

## Erweitern des Schutzes mit Traffic-Filterregeln

Sehen wir uns einige zusätzliche, empfohlene Maßnahmen an, die kundenseitig ergriffen werden können, um ihre Websites vor DDoS-Angriffen zu schützen:

- Implementieren Sie die von Adobe empfohlenen [Standard-Traffic-Filterregeln](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md), um potenziell schädliche Traffic-Muster zu identifizieren, indem Sie verdächtiges Verhalten protokollieren und darauf hinweisen.
- Verwenden Sie das Add-on **WAF-DDoS-Schutz** oder **Erweiterte Sicherheit** und implementieren Sie die von Adobe empfohlenen [WAF-Traffic-Filterregeln](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md), um sich vor komplexen Angriffen zu schützen, einschließlich solcher, die erweiterte Protokoll- oder Payload-basierte Methoden verwenden.
- Erhöhen Sie die Cache-Abdeckung, indem Sie [Anfragetransformationen](./traffic-filter-and-waf-rules/how-to/request-transformation.md) so konfigurieren, dass sie Abfrageparameter ignorieren.

## Erste Schritte

In den folgenden Tutorials erfahren Sie, wie Sie von Adobe empfohlene Regeln konfigurieren, um Angriffe abzuwehren.

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="Einrichten von Traffic-Filterregeln, einschließlich WAF-Regeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="Einrichten von Traffic-Filterregeln, einschließlich WAF-Regeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="Einrichten von Traffic-Filterregeln, einschließlich WAF-Regeln">Einrichten von Traffic-Filterregeln, einschließlich WAF-Regeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie die Ergebnisse von Traffic-Filterregeln, einschließlich WAF-Regeln, erstellen, bereitstellen, testen und analysieren.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Jetzt starten</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="Schützen von AEM-Websites mit Standard-Traffic-Filterregeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="Schützen von AEM-Websites mit Standard-Traffic-Filterregeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Schützen von AEM-Websites mit Standard-Traffic-Filterregeln">Schützen von AEM-Websites mit Standard-Traffic-Filterregeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mit von Adobe empfohlenen Standard-Traffic-Filterregeln in AEM as a Cloud Service vor DoS, DDoS und Bot-Missbrauch schützen.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Regeln anwenden</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Schützen von AEM-Websites mit WAF-Traffic-Filterregeln">Schützen von AEM-Websites mit WAF-Traffic-Filterregeln</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mit den von Adobe empfohlenen Traffic-Filterregeln der Web Application Firewall (WAF) in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF aktivieren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
