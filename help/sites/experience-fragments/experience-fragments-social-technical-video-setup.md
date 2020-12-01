---
title: Einrichten von Social-Beiträgen mit AEM Erlebnisfragmenten
description: Erlebnisfragmente ermöglichen es Marketingexperten, in AEM erstellte Erlebnisse auf Plattformen sozialer Medien zu veröffentlichen. Im folgenden Video werden die Einstellungen und Konfigurationen erläutert, die zum Veröffentlichen von Erlebnisfragmenten auf Facebook und Pinterest erforderlich sind.
sub-product: Sites, Content-Services
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# Einrichten von Social-Beiträgen mit Erlebnisfragmenten {#set-up-social-posting-with-experience-fragments}

Erlebnisfragmente ermöglichen es Marketingexperten, in AEM erstellte Erlebnisse auf Plattformen sozialer Medien zu veröffentlichen. Im folgenden Video werden die Einstellungen und Konfigurationen erläutert, die zum Veröffentlichen von Erlebnisfragmenten auf Facebook und Pinterest erforderlich sind.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Erlebnisfragmente]  - Einrichtung und Konfiguration von Social-Beiträgen für Facebook und Pinterest*

## Checkliste für die Konfiguration von Erlebnisfragmenten zum Posten auf Facebook und Pinterest

1. AEM Authoring-Instanz wird unter HTTPS ausgeführt
2. Facebook-Konto + Facebook Developer-App
3. Pinterest-Konto + Pinterest-Entwickler-App
4. [!UICONTROL AEM Cloud ] ServicesKonfiguration - Facebook
5. [!UICONTROL AEM Cloud ] ServicesKonfiguration - Pinterest
6. AEM Erlebnisfragment mit Cloud Services für Facebook + Pinterest
7. Erlebnisfragment-Variation mit Facebook-Vorlage
8. Erlebnisfragment-Variation mit Pinterest-Vorlage

## Erlebnisfragment-Umleitungs-URI

Dieser URI wird für Facebook- und Pinterest-Apps als Teil des Oauth-Flusses verwendet.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

