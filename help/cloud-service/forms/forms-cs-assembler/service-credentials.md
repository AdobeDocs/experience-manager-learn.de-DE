---
title: AEM Service-Anmeldeinformationen
description: Laden Sie die Service-Anmeldeinformationen von der AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '106'
ht-degree: 100%

---

# Service-Anmeldeinformationen

Integrationen in AEM as a Cloud Service müssen in der Lage sein, sich sicher bei AEM zu authentifizieren. Developer Console von AEM generiert Service-Anmeldeinformationen, die von externen Anwendungen, Systemen und Services verwendet werden, um programmgesteuert mit dem AEM Author- oder Publish-Service über HTTP zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/3412600?quality=12&learn=on&captions=ger)

Die heruntergeladene Datei mit den Service-Anmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen „service_token.json“ gespeichert. Die Werte in der Datei „service_token“ werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffs-Token auszutauschen. Die Dienstprogrammklasse „GetServiceCredentials“ wird zum Abrufen der Eigenschaftswerte aus der Ressourcendatei „service_token.json“ verwendet.
