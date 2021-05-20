---
title: Verwenden AEM Experience Fragment-Angebote in Adobe Target
seo-title: Verwenden AEM Experience Fragment-Angebote in Adobe Target
description: Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketingexperten Inhalte kanalübergreifend nahtlos testen und personalisieren.
seo-description: Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketingexperten Inhalte kanalübergreifend nahtlos testen und personalisieren.
sub-product: content-services
feature: Experience Fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: 'Personalisierung '
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 11%

---


# Verwenden von Experience Fragment-Angeboten in Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 stellt den Personalisierungs-Workflow zwischen AEM und Target um. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebote an Adobe Target übermittelt werden. Damit können Marketingexperten Inhalte kanalübergreifend nahtlos testen und personalisieren.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Es wird empfohlen, die at.js-Client-Bibliothek zu verwenden. Es empfiehlt sich daher, Tag-Management-Lösungen wie Launch by Adobe, Adobe DTM oder beliebige Drittanbieter-Tag-Management-Lösungen zu verwenden, um Ihren Siteseiten Zielbibliotheken hinzuzufügen

>[!NOTE]
>
>AEM Experience Fragment-Angebote in Adobe Target sind auch als Feature Pack für AEM 6.3-Benutzer verfügbar. Im folgenden Abschnitt finden Sie Feature Packs und Abhängigkeiten.


* Adobe Experience Managers benutzerfreundlicher und leistungsstarker Mechanismus zur Inhaltserstellung sowie die künstliche Intelligenz (AI) und maschinelles Lernen von Adobe Target helfen Inhaltsautoren bei der Erstellung und Verwaltung von Inhalten für alle Kanäle an einem zentralen Ort. Mit der Möglichkeit, Experience Fragments als HTML-Angebote in Adobe Target zu exportieren, haben Marketingexperten jetzt mehr Flexibilität, mit diesen Angeboten ein personalisierteres Erlebnis zu erstellen, und können nun jedes von ihnen erstellte Erlebnis testen und skalieren.
* Der Hauptunterschied zwischen den HTML-Angeboten und Experience Fragment-Angeboten besteht darin, dass die Bearbeitung für die spätere Bearbeitung nur in AEM und dann mit Adobe Target synchronisiert werden kann
* Die auf den Experience Fragment-Ordner angewendete Target Cloud-Dienstkonfiguration erbt alle Experience Fragments, die direkt im übergeordneten Ordner erstellt wurden. Der untergeordnete Ordner übernimmt nicht die übergeordnete Cloud Services-Konfiguration.
* Um ein personalisiertes Angebot zu erstellen, können wir nun einfach die in AEM gespeicherten Inhalte nutzen.
* Sie können Target-Aktivitätstypen erstellen, einschließlich Sensei-gestützter Aktivitäten wie automatisierte Zuordnung, automatisches Targeting und Automated Personalization

## AEM 6.3 Feature Packs und Abhängigkeiten {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | Abhängigkeiten |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zu Experience Fragments](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Verwenden von Experience Fragments](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
