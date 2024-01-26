---
title: AEM Service-Anmeldeinformationen
description: Laden Sie die Service-Anmeldeinformationen von der AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 458
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 100%

---

# Service-Anmeldeinformationen

Integrationen in AEM as a Cloud Service müssen in der Lage sein, sich sicher bei AEM zu authentifizieren. Developer Console von AEM generiert Service-Anmeldeinformationen, die von externen Anwendungen, Systemen und Services verwendet werden, um programmgesteuert mit dem AEM Author- oder Publish-Service über HTTP zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Die heruntergeladene Datei mit den Service-Anmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen „service_token.json“ gespeichert. Die Werte in der Datei „service_token“ werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffs-Token auszutauschen. Die Dienstprogrammklasse „GetServiceCredentials“ wird zum Abrufen der Eigenschaftswerte aus der Ressourcendatei „service_token.json“ verwendet.
