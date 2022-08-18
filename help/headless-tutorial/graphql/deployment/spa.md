---
title: Bereitstellen von SPA für AEM GraphQL
description: Erfahren Sie mehr über Implementierungsüberlegungen für Einzelseiten-Apps (SPA) AEM Headless-Implementierungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 1%

---


# AEM Headless-SPA-Bereitstellungen


AEM Headless-Single-Page-App-Bereitstellungen (SPA) beinhalten JavaScript-basierte Anwendungen, die mit Frameworks wie React oder Vue erstellt wurden, die Inhalte in AEM Headless nutzen und mit ihnen interagieren.

Wenn Sie eine SPA bereitstellen, die auf Headless-Weise AEM interagiert, wird die SPA gehostet und über einen Webbrowser zugänglich gemacht.

## Hosten des SPA

Ein SPA besteht aus einer Sammlung nativer Webressourcen: **HTML, CSS und JavaScript**. Diese Ressourcen werden während der _build_ Prozess (z. B. `npm run build`) und auf einem Host für die Verwendung durch Endbenutzer bereitgestellt werden.

Es gibt verschiedene **Hosting** Optionen entsprechend den Anforderungen Ihres Unternehmens:

1. **Cloud-Anbieter** wie **Azure** oder **AWS**.

2. **On-Premise** Hosting in einem Unternehmen **Rechenzentrum**

3. **Frontend-Hosting-Plattformen** wie **AWS Amplify**, **Azure App Service**, **verfeinern**, **Heroku**, **Vercel**, usw.

## Bereitstellungskonfigurationen

Beim Hosten einer SPA, die mit Headless interagiert, sollten Sie vor allem berücksichtigen, ob der Zugriff auf die SPA über AEM Domäne (oder den Host) oder eine andere Domäne erfolgt.  Der Grund dafür ist, SPA Webanwendungen in Webbrowsern ausgeführt werden und daher Sicherheitsrichtlinien für Webbrowser unterliegen.

### Freigegebene Domäne

Eine SPA und eine AEM teilen Domänen, wenn beide von Endbenutzern aus derselben Domäne aufgerufen werden. Beispiel:

+ Der Zugriff auf AEM erfolgt über: `https://wknd.site/`
+ SPA ist über zugänglich `https://wknd.site/spa`

Da sowohl AEM als auch die SPA von derselben Domäne aus aufgerufen werden, ermöglichen Webbrowser dem SPA, XHR zu AEM Headless-Endpunkten zu machen, ohne CORS erforderlich zu sein, und die Freigabe von HTTP-Cookies (z. B. AEM `login-token` Cookie).

Wie SPA und AEM Traffic auf der freigegebenen Domäne geleitet wird, liegt bei Ihnen: CDN mit mehreren Ursprüngen, HTTP-Server mit Reverse-Proxy, Hosting des SPA direkt in AEM usw.

Im Folgenden finden Sie Bereitstellungskonfigurationen, die für SPA Produktionsbereitstellungen erforderlich sind, wenn sie auf derselben Domäne wie AEM gehostet werden.

| SPA stellt Verbindungen her | AEM Author | AEM Publish | AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ms | ms |
| Cross-Origin Resource Sharing (CORS) | ✘ | ✘ | ✘ |
| AEM Hosts | ✘ | ✘ | ✘ |

### Verschiedene Domänen

SPA und AEM haben unterschiedliche Domänen, wenn Endbenutzer von der jeweiligen Domäne aus auf sie zugreifen. Beispiel:

+ Der Zugriff auf AEM erfolgt über: `https://wknd.site/`
+ SPA ist über zugänglich `https://wknd-app.site/`

Da auf AEM und die SPA von verschiedenen Domänen aus zugegriffen wird, erzwingen Webbrowser Sicherheitsrichtlinien wie [Cross-Origin Resource Sharing (CORS)](./configurations/cors.md)und die Freigabe von HTTP-Cookies (z. B. AEM `login-token` Cookie).

Im Folgenden finden Sie Bereitstellungskonfigurationen, die für SPA Produktionsbereitstellungen erforderlich sind, wenn sie auf einer anderen Domäne als AEM gehostet werden.

| SPA stellt Verbindungen her | AEM-Autor | AEM-Veröffentlichung | AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ms | ms |
| [Cross-Origin Resource Sharing (CORS)](./configurations/cors.md) | ms | ms | ms |
| [AEM Hosts](./configurations/aem-hosts.md) | ms | ms | ms |

#### Beispiel SPA Bereitstellung auf verschiedenen Domänen

In diesem Beispiel wird der SPA in einer Netlify-Domäne (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) und der SPA verwendet AEM GraphQL-APIs aus der AEM-Veröffentlichungsdomäne (`https://publish-p65804-e666805.adobeaemcloud.com`). In den folgenden Screenshots wird die CORS-Anforderung hervorgehoben.

1. Der SPA wird von einer Netlify-Domäne bereitgestellt, führt jedoch einen XHR-Aufruf an AEM GraphQL-APIs in einer anderen Domäne durch. Diese Site-übergreifende Anforderung erfordert [CORS](./configurations/cors.md) auf AEM eingerichtet werden, um der Anforderung der Netlify-Domäne den Zugriff auf ihren Inhalt zu ermöglichen.

   ![SPA von SPA- und AEM-Hosts ](assets/spa/cors-requirement.png)

2. Überprüfen der XHR-Anfrage an die AEM GraphQL-API, `Access-Control-Allow-Origin` vorhanden ist, was dem Webbrowser anzeigt, dass AEM Anfrage von dieser Netlify-Domäne den Zugriff auf ihren Inhalt erlaubt.

   Wenn die AEM [CORS](./configurations/cors.md) fehlte oder die Netlify-Domäne nicht enthielt, schlug der Webbrowser die XHR-Anforderung fehl und meldete einen CORS-Fehler.

   ![CORS Response Header AEM GraphQL API](assets/spa/cors-response-headers.png)

## Beispiel einer Einzelseitenanwendung

Adobe bietet eine Beispiel-Einzelseitenanwendung, die in React codiert ist.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React-App" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React-App">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React-App">React-App</a></p>
               <p class="is-size-6">Eine Beispiel-Einzelseitenanwendung, geschrieben in React, die Inhalte von AEM Headless GraphQL-APIs verbraucht.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
