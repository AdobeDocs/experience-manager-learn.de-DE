---
title: AEM Service-Anmeldedaten
description: Laden Sie die Anmeldedaten für den Dienst von AEM Developer Console herunter.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Dienstberechtigungen

Integrationen mit AEM as a Cloud Service müssen sich sicher bei AEM authentifizieren können. AEM Developer Console generiert Service-Anmeldeinformationen, die von externen Anwendungen, Systemen und Diensten verwendet werden, um programmgesteuert mit AEM Author- oder Publish-Diensten über HTTP zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Die heruntergeladene Datei mit den Dienstanmeldeinformationen wird in der bereitgestellten Eclipse als Ressourcendatei mit dem Namen service_token.json gespeichert. Die Werte in der Datei service_token werden verwendet, um das JWT zu generieren und das JWT gegen ein Zugriffstoken auszutauschen. Die Dienstprogrammklasse GetServiceCredentials wird verwendet, um die Eigenschaftswerte aus der Ressourcendatei service_token.json abzurufen.
