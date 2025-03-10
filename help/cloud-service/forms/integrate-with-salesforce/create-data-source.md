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
workflow-type: ht
source-wordcount: '75'
ht-degree: 100%

---

# Erstellen einer Datenquelle

Erstellen Sie eine REST-gesicherte Datenquelle mithilfe der Swagger-Datei, die Sie im vorherigen Schritt erstellt haben.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Einstellung | Wert |
|---------------------|-----------------------------------------------------------------|
| OAuth-URL | https://login.salesforce.com/services/oauth2/authorize |
| Autorisierungsumfang | api chatter_api full id openid refresh_token visualforce web |
| URL für aktualisiertes Token | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL des Zugriffs-Tokens | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Die Domain-Namen der URL für das Aktualisierungs- und Zugriffs-Token müssen geändert werden, damit sie Ihren Salesforce-Kontoeinstellungen entsprechen**