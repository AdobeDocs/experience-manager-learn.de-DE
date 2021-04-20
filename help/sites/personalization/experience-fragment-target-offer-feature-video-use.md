---
title: Verwenden von AEM Experience Fragment-Angeboten in Adobe Target
seo-title: Verwenden von AEM Experience Fragment-Angeboten in Adobe Target
description: Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.
seo-description: Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.
sub-product: content-services
feature: Experience Fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalization
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# Verwenden von Experience Fragment-Angeboten in Adobe Target{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager 6.4 stellt den Personalisierungsarbeitsablauf zwischen AEM und Zielgruppe vor. Erlebnisse, die in AEM erstellt wurden, können jetzt direkt als HTML-Angebot an Adobe Target übermittelt werden. Marketingexperten können Inhalte nahtlos über verschiedene Kanal hinweg testen und personalisieren.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Es wird empfohlen, die at.js-Client-Bibliothek zu verwenden. Es empfiehlt sich, Tag-Management-Lösungen wie Launch by Adobe, Adobe DTM oder eine Drittanbieter-Tag-Management-Lösung zu verwenden, um Ihren Siteseiten Zielgruppe-Bibliotheken hinzuzufügen.

>[!NOTE]
>
>AEM Experience Fragment-Angebot in Adobe Target sind auch als Feature Pack für AEM 6.3 verfügbar. Im folgenden Abschnitt finden Sie Funktionspakete und Abhängigkeiten.


* Adobe Experience Managers benutzerfreundlicher und leistungsstarker Content-Erstellungsmechanismus sowie Adobe Targets künstliche Intelligenz (AI) und maschinelles Lernen unterstützen Autoren bei der Erstellung und Verwaltung von Inhalten für alle Kanal an einem zentralen Ort. Mit der Möglichkeit, Erlebnisfragmente als HTML-Angebot in Adobe Target zu exportieren, haben Marketingexperten jetzt mehr Flexibilität, um mit diesen Angeboten ein personalisierteres Erlebnis zu erstellen, und können nun jedes von ihnen erstellte Erlebnis testen und skalieren.
* Der Hauptunterschied zwischen den HTML-Angeboten und den Experience Fragment-Angeboten besteht darin, dass die Bearbeitung für die spätere Bearbeitung nur in AEM und dann mit Adobe Target synchronisiert werden kann
* Die auf den Experience Fragment-Ordner angewendete Konfiguration des Zielgruppe Cloud-Dienstes erbt alle Erlebnisfragmente, die direkt im übergeordneten Ordner erstellt wurden. Die Konfiguration der übergeordneten Cloud-Dienste wird nicht vom untergeordneten Ordner übernommen.
* Um ein personalisiertes Angebot zu erstellen, können wir jetzt auf einfache Weise in AEM gespeicherte Inhalte nutzen.
* Sie können verschiedene Aktivitäten der Zielgruppe erstellen, einschließlich der Sensei-basierten Aktivitäten wie &quot;Autom. zuordnen&quot;, &quot;Auto-Zielgruppe&quot;und &quot;Automated Personalization&quot;

## AEM 6.3 Feature Packs und Abhängigkeiten {#aem-feature-packs-and-dependencies}

| AEM 6.3 Feature Pack | Abhängigkeiten |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Zusätzliche Ressourcen {#additional-resources}

* [Dokumentation zu Erlebnisfragmenten](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Verwenden von Experience Fragments](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
