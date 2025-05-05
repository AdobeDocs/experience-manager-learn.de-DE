---
title: AEM Forms-Service-Anmeldeinformationen
description: Laden Sie die Service-Anmeldeinformationen von der AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '109'
ht-degree: 100%

---

# AEM Forms-Service-Anmeldeinformationen

Integrationen in AEM as a Cloud Service müssen in der Lage sein, sich sicher bei AEM zu authentifizieren. AEM Developer Console generiert Service-Anmeldeinformationen, die es externen Anwendungen, Systemen und Services ermöglichen, über HTTP programmgesteuert mit dem AEM Author- oder Publish-Service zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/3412600?quality=12&learn=on&captions=ger)

Die heruntergeladene Datei mit den Service-Anmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen „service_token.json“ gespeichert. Die Werte in der Datei „service_token“ werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffs-Token auszutauschen. Die Dienstprogrammklasse „GetServiceCredentials“ wird zum Abrufen der Eigenschaftswerte aus der Ressourcendatei „service_token.json“ verwendet.
