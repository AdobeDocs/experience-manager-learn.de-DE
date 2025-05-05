---
title: Erstellen einer Cloud Services-Konfiguration
description: Erstellen einer Datenquelle für die Verbindung mit Salesforce mithilfe der OAuth-Anmeldeinformationen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '75'
ht-degree: 100%

---

# Erstellen einer Datenquelle

Erstellen Sie eine REST-gesicherte Datenquelle mithilfe der Swagger-Datei, die Sie im vorherigen Schritt erstellt haben.

>[!VIDEO](https://video.tv.adobe.com/v/3447275?quality=12&learn=on&captions=ger)

| Einstellung | Wert |
|---------------------|-----------------------------------------------------------------|
| OAuth-URL | https://login.salesforce.com/services/oauth2/authorize |
| Autorisierungsumfang | api chatter_api full id openid refresh_token visualforce web |
| URL für aktualisiertes Token | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL des Zugriffs-Tokens | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Die Domain-Namen der URL für das Aktualisierungs- und Zugriffs-Token müssen geändert werden, damit sie Ihren Salesforce-Kontoeinstellungen entsprechen**