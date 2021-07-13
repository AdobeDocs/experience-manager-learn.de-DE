---
title: Social Posting mit AEM Experience Fragments einrichten
description: Mit Experience Fragments können Marketingexperten in AEM erstellte Erlebnisse auf Social-Media-Plattformen posten. Im folgenden Video erfahren Sie, wie Sie Experience Fragments in Facebook und Pinterest veröffentlichen können.
sub-product: Sites, Content-Services
feature: Experience Fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Content Management
role: Admin, Developer
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# Einrichten von Social-Beiträgen mit Experience Fragments {#set-up-social-posting-with-experience-fragments}

Mit Experience Fragments können Marketingexperten in AEM erstellte Erlebnisse auf Social-Media-Plattformen posten. Im folgenden Video erfahren Sie, wie Sie Experience Fragments in Facebook und Pinterest veröffentlichen können.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Experience Fragments]  - Einrichtung und Konfiguration von Social-Beiträgen in Facebook und Pinterest*

## Checkliste für die Konfiguration von Experience Fragments zum Posten in Facebook und Pinterest

1. Die AEM-Autoreninstanz wird unter HTTPS ausgeführt
2. Facebook-Konto und Facebook Developer App
3. Pinterest-Konto und Pinterest Developer App
4. [!UICONTROL AEM Cloud ] ServicesConfiguration - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguration - Pinterest
6. AEM Experience Fragment mit Cloud Services für Facebook und Pinterest
7. Experience Fragment-Variante mit Facebook-Vorlage
8. Experience Fragment-Variante mit Pinterest-Vorlage

## Experience Fragment-Umleitungs-URI

Dieser URI wird für Facebook- und Pinterest-Apps als Teil des OAuth-Flusses verwendet.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

