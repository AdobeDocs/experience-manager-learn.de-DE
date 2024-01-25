---
title: Zwischenspeicherung in AEM as a Cloud Service
description: Allgemeiner Überblick über die Zwischenspeicherung in AEM as a Cloud Service.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 71
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# Zwischenspeicherung in AEM as a Cloud Service

In AEM as a Cloud Service ist das Verständnis der Zwischenspeicherung von entscheidender Bedeutung. Die Zwischenspeicherung umfasst die Speicherung und Wiederverwendung zuvor abgerufener Daten, um die Systemeffizienz zu verbessern und die Ladezeiten zu reduzieren. Dieser Mechanismus beschleunigt die Bereitstellung von Inhalten erheblich, steigert die Leistung von Websites und optimiert das Benutzererlebnis.

AEM as a Cloud Service verfügt über mehrere Zwischenspeicherungsebenen und Strategien, die sich zwischen Author- und Publish-Service unterscheiden.

![Übersicht über die Zwischenspeicherung in AEM as a Cloud Service](./assets/overview/all.png){align="center"}

## AEM-Zwischenspeicherung

AEM as a Cloud Service verfügt über eine robuste, konfigurierbare, mehrschichtige Zwischenspeicherungsstrategie, einschließlich eines CDN, AEM Dispatcher und optional eines kundenverwalteten CDN. Die Zwischenspeicherung über mehrere Ebenen kann für eine optimale Leistung fein abgestimmt werden, sodass AEM nur die besten Erlebnisse bereitstellt. AEM hat unterschiedliche Anliegen bezüglich der Zwischenspeicherung für die Author- und Publish-Services. Erkunden Sie die Zwischenspeicherungsstrategien für jeden der nachfolgenden Services.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="AEM-Publish-Service" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Zwischenspeicherung beim AEM-Publish-Service">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Zwischenspeicherung beim AEM-Publish-Service">Zwischenspeicherung beim AEM-Publish-Service</a></p>
            <p class="is-size-6">Der AEM-Publish-Service verwendet ein verwaltetes CDN und AEM Dispatcher, um die Web-Erfahrungen der Endbenutzenden zu optimieren.</p>
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
                <a href="./author.md" title="Zwischenspeicherung des AEM-Author-Service" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Zwischenspeicherung des AEM-Author-Service">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Zwischenspeicherung des AEM-Author-Service">Zwischenspeicherung des AEM-Author-Service</a></p>
                <p class="is-size-6">Der AEM-Author-Service verwendet ein verwaltetes CDN, um optimierte Autorenerlebnisse bereitzustellen.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
</a>
            </div>
            </div>
        </div>
    </div>
</div>
