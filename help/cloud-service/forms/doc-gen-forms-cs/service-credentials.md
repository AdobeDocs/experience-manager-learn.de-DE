---
title: AEM Forms-Service-Anmeldeinformationen
description: Laden Sie die Service-Anmeldeinformationen von der AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 458
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 100%

---

# AEM Forms-Service-Anmeldeinformationen

Integrationen in AEM as a Cloud Service müssen in der Lage sein, sich sicher bei AEM zu authentifizieren. AEM Developer Console generiert Service-Anmeldeinformationen, die es externen Anwendungen, Systemen und Services ermöglichen, über HTTP programmgesteuert mit dem AEM Author- oder Publish-Service zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Die heruntergeladene Datei mit den Service-Anmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen „service_token.json“ gespeichert. Die Werte in der Datei „service_token“ werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffs-Token auszutauschen. Die Dienstprogrammklasse „GetServiceCredentials“ wird zum Abrufen der Eigenschaftswerte aus der Ressourcendatei „service_token.json“ verwendet.
