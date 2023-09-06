---
title: AEM as a Cloud Service Zwischenspeicherung
description: Allgemeine Übersicht über AEM as a Cloud Service Zwischenspeicherung.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---


# AEM as a Cloud Service Zwischenspeicherung

In AEM as a Cloud Service ist das Verständnis der Zwischenspeicherung von entscheidender Bedeutung. Das Caching umfasst die Speicherung und Wiederverwendung zuvor abgerufener Daten, um die Systemeffizienz zu verbessern und die Ladezeiten zu reduzieren. Dieser Mechanismus beschleunigt die Bereitstellung von Inhalten erheblich, steigert die Leistung von Websites und optimiert das Benutzererlebnis.

AEM as a Cloud Service verfügt über mehrere Zwischenspeicherungsebenen und Strategien, die sich zwischen den Autoren- und Veröffentlichungsdiensten unterscheiden.

![Übersicht über die as a Cloud Service Zwischenspeicherung AEM](./assets/overview/all.png){align="center"}

## AEM Zwischenspeicherung

AEM as a Cloud Service verfügt über eine robuste, konfigurierbare mehrschichtige Zwischenspeicherungsstrategie, einschließlich eines CDN, AEM Dispatcher und optional eines kundenverwalteten CDN. Das Zwischenspeichern über mehrere Ebenen kann zur Leistungsoptimierung optimiert werden, sodass AEM nur die besten Erlebnisse bereitstellt. AEM hat für die Autoren- und Veröffentlichungsdienste unterschiedliche Caching-Bedenken. Untersuchen Sie die folgenden Caching-Strategien für jeden Dienst.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM Publish-Dienst" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Zwischenspeicherung des AEM Veröffentlichungsdienstes">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Zwischenspeicherung des AEM Veröffentlichungsdienstes">Zwischenspeicherung des AEM Veröffentlichungsdienstes</a></p>
            <p class="is-size-6">AEM Veröffentlichungsdienst verwendet ein verwaltetes CDN und AEM Dispatcher, um die Weberfahrungen der Endbenutzer zu optimieren.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="Zwischenspeicherung des AEM-Autorendienstes" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Zwischenspeicherung des AEM-Autorendienstes">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Zwischenspeicherung des AEM-Autorendienstes">Zwischenspeicherung des AEM-Autorendienstes</a></p>
                <p class="is-size-6">AEM -Autorendienst verwendet ein verwaltetes CDN, um optimierte Authoring-Erlebnisse bereitzustellen.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
