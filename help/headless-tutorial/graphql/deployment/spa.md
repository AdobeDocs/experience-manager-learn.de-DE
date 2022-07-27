---
title: Bereitstellen von SPA für AEM GraphQL
description: Erfahren Sie SPA Bereitstellungsoptionen in Bezug auf AEM GraphQL, Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Bereitstellen eines SPA

In diesem Abschnitt wird ein Ansatz für die Bereitstellung von SPA (React, Vue, Angular usw.) beschrieben, der AEM GraphQL-APIs aufruft, um die Daten zu laden. Davor sollten wir jedoch die allgemeinen Artefakte verstehen, die bereitgestellt werden müssen.

**SPA App-Artifacts erstellen:**

Die vom SPA Framework erzeugten Dateien, in der Regel **HTML, CSS, JS**, auch statische Build-Artefakte genannt. Im Fall der React-App werden die Artefakte aus der `build` Verzeichnis und für die Vue-Artefakte aus `dist` Verzeichnis.
Die Anfrage an Ihre SPA (z. B. https://HOST/my-aem-spa.html) wird mit diesen Build-Artefakten bereitgestellt.

**AEM GraphQL-APIs:**

Dieser GraphQL-API-Endpunkt (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) in der AEM gehostet werden.

Zusammenfassend SPA die Implementierungsarchitektur aus zwei Teilen *1. SPA 2. AEM GraphQL-API-Ebene*, also überprüfen wir die Implementierungsoptionen für diese beiden Teile.


## Bereitstellungsoptionen

| Bereitstellungsoption | SPA URL | AEM GraphQL-API-URL | CORS-Konfiguration erforderlich? |
| ---------|---------- | ---------|---------- |
| **Gleiche Domäne** | https://**GAST**/my-aem-spa.html | https://**GAST**/graphql/execute.json/.. | ✘ |
| **Verschiedene Domäne** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/.. | ms |

**Dieselbe Domäne:**\
Beide *SPA und AEM GraphQL-API-Ebene* in dieser Option bereitgestellt, um **Gleiche Domäne**. Anfrage an SPA URI `/my-aem-spa.html` &amp; GraphQL-API-Schicht `/graphql/execute.json/` von exakt derselben Domäne bereitgestellt werden.

**Verschiedene Domäne:**\
Beide *SPA und AEM GraphQL-API-Ebene* in dieser Option bereitgestellt werden, um **Verschiedene Domäne**. Anfrage an SPA URI `/my-aem-spa.html` wird von einem **andere Domäne** als GraphQL-API-Schicht `/graphql/execute.json/` -Anfragen. Beachten Sie im Rahmen dieser Bereitstellungsoption Folgendes: [CORS konfigurieren](cors.md) auf der AEM Instanz.

>[!NOTE]
>
>Sie MÜSSEN CORS in AEM Instanz ordnungsgemäß konfigurieren. [Schritte hier sehen](cors.md).

### Bereitstellen auf derselben Domäne

Bei der Bereitstellung auf derselben Domäne kann die Domäne eine **primäre AEM** (Auf AEM Domäne) oder **primäre SPA** (Aus AEM Domäne) und eingehende SPA können AEM GraphQL-API-Anforderungen an eine der Bereitstellungskomponenten wie CDN ([Fastly](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD mit Reverse Proxy](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). Mit anderen Worten: Sie stellen die SPA Build-Artefakte und AEM GraphQL-APIs weiterhin auf verschiedenen Servern bereit, aber für Endbenutzer werden diese von einer einzelnen Domäne bereitgestellt und hinter der Szene an einen anderen Ziel- oder Herkunftsserver weitergeleitet.

Außerdem können SPA Build-Artefakte in AEM gehostet werden *jedoch nicht empfohlen.*

| Gleiche Domänenbereitstellung | CDN-Teilung | HTTPD + Reverse Proxy | AEM gehostete SPA Artefakte |
| ---------|---------- | ---------|---------- |
| **AEM** | ms | ms | ms |
| **OFF AEM Domain** | ms | ms | **Nicht zutreffend** |


**HTTPD + Reverse Proxy**

Die Beispielkonfiguration sieht wie folgt aus

>[!TIP]
>
> Die folgenden Konfigurationen sind Beispiele. Stellen Sie sicher, dass Sie sie an die Anforderungen Ihres Projekts anpassen.

AUF AEM DOMÄNE

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

OFF AEM Domain

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Bereitstellen auf einer anderen Domäne

In diesem Szenario werden SPA Build-Artefakte in einer anderen Domäne als die AEM GraphQL-API-Domäne bereitgestellt und für Endbenutzer werden sie von zwei separaten Domänen bereitgestellt, sodass [CORS-Konfiguration](cors.md) MUSS AEM.

**SPA von App-Anforderungen über verschiedene Domänen**

![Verschiedene Domain SPA Versand](assets/spa/different-domain-spa-delivery.png)


**CORS Response Header auf AEM GraphQL-API**

![CORS Response Header AEM GraphQL API](assets/spa/CORS-response-header-aem-graphql-api.png)


