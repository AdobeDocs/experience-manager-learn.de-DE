---
title: Erstellen einer Acrobat Sign-Cloud-Konfiguration – Cloud-Service
description: Erstellen Sie eine Integration von AEM Forms und Acrobat Sign mithilfe der Cloud-Service-Konfiguration.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 100%

---

# Erstellen einer Acrobat Sign-Cloud-Konfiguration

Mit der Cloud-Service-Konfiguration in AEM können Sie eine Integration zwischen AEM und anderen Cloud-Anwendungen erstellen.

Das folgende Video führt Sie durch die Schritte, die zum Erstellen der Cloud-Service-Konfiguration zur Integration von AEM in Acrobat Sign erforderlich sind

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Fehlerbehebung

Wenn beim Konfigurieren der Abobe Sign-Cloud-Konfiguration ein Fehler auftritt, können Sie die folgenden Schritte zur Fehlerbehebung ausführen.
* Stellen Sie sicher, dass die in der Acrobat Sign-API-Anwendung angegebene Umleitungs-URL das folgende Format aufweist:
&lt;Instanzname>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;Container>.
Beispiel: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. „FormsCS“ ist der Name des Containers, in dem die Cloud-Konfiguration enthalten sein wird.
* Vergewissern Sie sich, dass die oAuth-URL korrekt ist.
* Überprüfen Sie Client-ID und Client-Geheimnis.
* Probieren Sie den Inkognito-Fenstermodus aus.

