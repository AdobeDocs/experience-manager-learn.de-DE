---
title: Erstellen einer Cloud Services-Konfiguration
description: Erstellen einer Datenquelle für die Verbindung mit Salesforce mithilfe der OAuth-Anmeldeinformationen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: ce22dd482417a54d222165deaf485ff69c2856b7
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 41%

---

# Erstellen einer Datenquelle

Erstellen Sie eine REST-gestützte Datenquelle mithilfe der im vorherigen Schritt erstellten Swagger-Datei.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Einstellung | Wert |
|---------------------|-----------------------------------------------------------------|
| OAuth-URL | https://login.salesforce.com/services/oauth2/authorize |
| Autorisierungsumfang | API chatter_api Vollständige OpenID-Aktualisierungs-Token VisualForce Web |
| URL für aktualisierten Token | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| Zugriffstoken-URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Die Domain-Namen der URL für das Aktualisierungs- und Zugriffstoken müssen sich ändern, damit sie mit Ihren Salesforce-Kontoeinstellungen übereinstimmen**