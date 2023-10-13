---
title: Zwischenspeicherung des AEM-Autorendienstes
description: Allgemeine Übersicht über die Zwischenspeicherung AEM as a Cloud Service Autorendiensts.
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 3%

---

# AEM Author

AEM Autor hat aufgrund der hochdynamischen und berechtigungssensiblen Inhalte, für die er dient, eingeschränkte Zwischenspeicherung. Im Allgemeinen wird es nicht empfohlen, die Zwischenspeicherung für AEM Author anzupassen, sondern sich stattdessen auf die von Adobe bereitgestellten Cache-Konfigurationen zu verlassen, um ein leistungsfähiges Erlebnis sicherzustellen.

![Übersichtsdiagramm zur Zwischenspeicherung AEM Autoren](./assets/author/author-all.png){align="center"}

Beim Anpassen der Zwischenspeicherung in AEM Autoreninstanz wird davon abgeraten, zu verstehen, dass AEM -Autor zwar über ein Adobe-verwaltetes CDN verfügt, aber über keinen AEM Dispatcher verfügt. Beachten Sie, dass alle AEM Dispatcher-Konfigurationen in AEM -Autoreninstanz ignoriert werden, da sie keinen AEM Dispatcher haben.

## CDN

AEM -Autorendienst verwendet ein CDN, sein Zweck besteht jedoch darin, die Bereitstellung von Produktressourcen zu verbessern, und sollte nicht umfassend konfiguriert werden, sondern so, wie es ist.

![Übersichtsdiagramm zur AEM-Zwischenspeicherung](./assets/author/author-cdn.png){align="center"}

Das CDN für die AEM-Autoreninstanz befindet sich zwischen dem Endbenutzer, normalerweise ein Marketing-Experte oder Inhaltsautor, und dem AEM Autor. Es werden unveränderliche Dateien zwischengespeichert, z. B. statische Assets, die das AEM Authoring-Erlebnis ermöglichen, und keine erstellten Inhalte.

AEM CDN des Autors speichert verschiedene Arten von Ressourcen zwischen, die von Interesse sein können, darunter eine [anpassbare TTL für beständige Abfragen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances)und ein [lange TTL in benutzerdefinierten Client-Bibliotheken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### Standardmäßige Cache-Lebensdauer

Die folgenden kundenorientierten Ressourcen werden vom AEM-Autor-CDN zwischengespeichert und haben die folgende standardmäßige Cache-Lebensdauer:

| Inhaltstyp | Standard-CDN-Cache-Lebensdauer |
|:------------ |:---------- |
| [Beständige Abfragen (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 Minute |
| [Client-Bibliotheken (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 Tage |
| [Alles andere](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Nicht zwischengespeichert |


## AEM Dispatcher

AEM -Autorendienst enthält AEM Dispatcher nicht und verwendet nur die [CDN](#cdn) zum Zwischenspeichern.
