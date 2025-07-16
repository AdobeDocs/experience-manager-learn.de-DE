---
title: Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln
description: Erfahren Sie mehr über empfohlene Best Practices für die Konfiguration von Traffic-Filterregeln, einschließlich WAF-Regeln in AEM as a Cloud Service, um die Sicherheit zu erhöhen und Risiken zu minimieren.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln

Erfahren Sie mehr über empfohlene Best Practices für die Konfiguration von Traffic-Filterregeln, einschließlich WAF-Regeln in AEM as a Cloud Service, um die Sicherheit zu erhöhen und Risiken zu minimieren.

>[!IMPORTANT]
>
>Die in diesem Artikel beschriebenen Best Practices sind nicht vollständig und sollen nicht als Ersatz für Ihre eigenen Sicherheitsrichtlinien und -verfahren dienen.

## Allgemeine Best Practices

- Beginnen Sie mit dem [empfohlenen Satz](./overview.md#adobe-recommended-rules) von standardmäßigen Traffic-Filtern und WAF-Regeln, die von Adobe bereitgestellt werden, und passen Sie sie an die spezifischen Anforderungen Ihrer Anwendung und die Bedrohungslandschaft an.
- Arbeiten Sie mit Ihrem Sicherheits-Team zusammen, um zu ermitteln, welche Regeln dem Sicherheitszustand und den Compliance-Anforderungen Ihres Unternehmens entsprechen.
- Testen Sie immer neue oder aktualisierte Regeln in Entwicklungsumgebungen, bevor Sie sie in die Staging- und Produktionsumgebung übertragen.
- Beginnen Sie beim Deklarieren und Validieren von Regeln mit dem `action` Typ `log` , um das Verhalten zu beobachten, ohne den rechtmäßigen Traffic zu blockieren.
- Nur nach Analyse ausreichender Traffic-Daten und Bestätigung, dass keine gültigen Anfragen betroffen sind, können Sie von `log` nach `block` wechseln.
- Inkrementelle Einführung von Regeln unter Einbeziehung von Teams für Qualitätssicherung, Leistungs- und Sicherheitstests, um unbeabsichtigte Nebenwirkungen zu identifizieren.
- Regelmäßige Überprüfung und Analyse der Regelwirksamkeit mithilfe von [Dashboard-Tools](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Die Häufigkeit der Überprüfungen (täglich, wöchentlich, monatlich) sollte sich nach dem Traffic-Volumen und Risikoprofil Ihrer Site richten.
- Kontinuierliche Verfeinerung von Regeln auf der Grundlage neuer Bedrohungsdaten, Traffic-Verhalten und Auditergebnisse.

## Best Practices für Traffic-Filterregeln

- Verwenden Sie Adobe [Empfohlene Standard-Traffic](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)Filterregeln) als Grundlinie, die Regeln für Edge-, Origin Protection- und OFAC-basierte Einschränkungen enthält.
- Überprüfen Sie regelmäßig Warnhinweise und Protokolle, um Muster von Missbrauch oder Fehlkonfigurationen zu identifizieren.
- Passen Sie Schwellenwerte für Ratenbeschränkungen auf der Grundlage des Traffic-Musters und des Benutzerverhaltens Ihrer Anwendung an.

  In der folgenden Tabelle finden Sie Anleitungen zur Auswahl der Schwellenwerte:

  | Variante | Wert |
  | :--------- | :------- |
  | Ursprung | Nehmen Sie den höchsten Wert der maximalen Ursprungsanfragen pro IP/POP unter **normalen** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS-Angriffs) und erhöhen Sie ihn um ein Vielfaches |
  | Edge | Nehmen Sie den höchsten Wert der maximalen Edge-Anfragen pro IP/POP unter **normalen** Traffic-Bedingungen (d. h. nicht die Rate zum Zeitpunkt eines DDoS-Angriffs) und erhöhen Sie ihn um ein Vielfaches |

  Weitere Einzelheiten finden Sie [ Abschnitt „Auswählen ](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) Schwellenwerten“.

- Gehen Sie erst dann zu `block` Aktion, wenn Sie bestätigt haben, dass die `log`-Aktion keinen Einfluss auf den zulässigen Traffic hat.

## Best Practices für WAF-Regeln

- Beginnen Sie mit den Adobe [empfohlenen WAF-Regeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) die Regeln zum Blockieren bekannter schlechter IPs, zum Erkennen von DDoS-Angriffen und zum Abmildern des Bot-Missbrauchs enthalten.
- Die `ATTACK` WAF-Markierung sollte Sie vor potenziellen Bedrohungen warnen. Vergewissern Sie sich, dass keine Fehlalarme vorliegen, bevor Sie zu `block` wechseln.
- Wenn empfohlene WAF-Regeln bestimmte Bedrohungen nicht abdecken, sollten Sie ggf. benutzerdefinierte Regeln erstellen, die auf den individuellen Anforderungen Ihrer Anwendung basieren. Eine vollständige Liste der [WAF-Flags finden Sie ](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) der Dokumentation.

## Durchführungsbestimmungen

Erfahren Sie, wie Sie Traffic-Filterregeln und WAF-Regeln in AEM as a Cloud Service implementieren:

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
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
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM-Websites mithilfe von Adobe-empfohlenen Regeln für die Web Application Firewall (WAF) in AEM as a Cloud Service vor komplexen Bedrohungen wie DoS, DDoS und Bot-Missbrauch schützen.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">WAF aktivieren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Zusätzliche Ressourcen

- [Traffic-Filterregeln einschließlich WAF-Regeln](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Grundlagen zur Vermeidung von DoS/DDoS in AEM](https://experienceleague.adobe.com/de/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Sperren von DoS- und DDoS-Angriffen mithilfe von Traffic-Filterregeln](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

